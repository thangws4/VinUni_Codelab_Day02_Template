# BÁO CÁO ĐÁNH GIÁ CHUYÊN SÂU (PROBLEM DEEP-DIVE REPORT)

**Tên dự án:** Trợ lý AI Điều Vận Thông Minh & Xử Lý Sự Cố Pin Xe Điện (Xanh SM Smart Dispatcher Co-pilot)  
**Đơn vị thực hiện:** Nhóm AI Product Engineering — Vin Smart Future  
**Đơn vị thụ hưởng:** Khối Vận Hành Taxi Điện GSM (Xanh SM)  
**Ngày lập:** 11/09/2026  

---

## 🏛️ 1. Bối cảnh dự án (Context & Stakeholders)
* **Khách hàng nội bộ:** Điều phối viên (Dispatchers) tại Trung tâm Điều vận Xanh SM và Đội ngũ Tài xế xe taxi điện (VF5, VFe34, VF8) tại các đô thị lớn (Hà Nội, TP.HCM).
* **Thực trạng vận hành:** Với quy mô hàng chục nghìn xe điện hoạt động liên tục 24/7, mỗi ngày Trung tâm tiếp nhận từ 80–120 cuộc gọi/tin nhắn khẩn cấp từ tài xế liên quan đến cảnh báo pin yếu, cạn pin giữa đường, hoặc không tìm được trụ sạc trống phù hợp vào giờ cao điểm. Việc điều phối hiện tại phụ thuộc hoàn toàn vào thao tác thủ công của con người, gây nghẽn cổ chai và áp lực rất lớn cho ca trực.

---

## 🏗️ 2. Cổng G1: Current-State Workflow Mapping (Quy trình hiện tại)

### 2.1. Sơ đồ quy trình thủ công 5 bước (AS-IS):

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ Tra cứu định │     │ Tra cứu trạm │     │ Soạn văn bản │
│ gọi sự cố    │ ──→ │ vị GPS xe   │ ──→ │ sạc VinFast  │ ──→ │ hướng dẫn    │
│              │ 🔄  │              │ 🔄  │ còn trụ trống│ 🔄  │ gửi tài xế   │
│ Actor:       │     │ Actor:       │     │ Actor:       │     │ Actor:       │
│ Dispatcher   │     │ Dispatcher   │     │ Dispatcher   │     │ Dispatcher   │
│ ⏱ 2 phút     │     │ ⏱ 2 phút     │     │ ⏱ 5 phút 🔴  │     │ ⏱ 5 phút 🔴  │
│ In: Điện thoại│    │ In: Biển số  │     │ In: Vị trí,  │     │ In: Địa chỉ  │
│ Out: Log số  │     │ Out: Tọa độ  │     │     loại xe  │     │     trụ sạc  │
│      xe, % pin│    │      bản đồ  │     │ Out: Địa chỉ │     │ Out: SMS app │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ Điều xe sạc  │
                                                               │ di động cứu  │
                                                               │ hộ (nếu cần) │
                                                               │ Actor:       │
                                                               │ Dispatcher   │
                                                               │ ⏱ 1 phút     │
                                                               │ In: Quyết    │
                                                               │     định     │
                                                               │ Out: Lệnh xe │
                                                               └──────────────┘
```

* 🔴 **Bottlenecks chính (Bước 3 & 4):**
  * **Bước 3 (Tra cứu trạm sạc):** Điều phối viên phải mở dashboard bản đồ trạm sạc VinFast độc lập, lọc các trạm có chuẩn sạc tương thích với xe (CCS2/công suất phù hợp), xem số trụ đang trống và ước tính khoảng cách di chuyển thực tế.
  * **Bước 4 (Soạn tin chỉ dẫn):** Soạn thảo tin nhắn chỉ đường chi tiết bằng tay trên màn hình điều vận để gửi qua app tài xế. Nếu tài xế hoảng loạn hoặc đang chở khách, việc trao đổi qua lại kéo dài thêm 3–5 phút.
* 🔄 **Điểm Handoff:** Giữa tài xế gọi vào tổng đài ➔ Hệ thống GPS ➔ Dashboard trạm sạc VinFast ➔ App tài xế ➔ Đội cứu hộ lưu động.
* ⏱ **Tổng thời gian xử lý thủ công trung bình:** **15 phút / lượt sự cố**.

---

## 📋 3. Cổng G2: Problem Statement (6-field) — Tiêu chuẩn Vin Smart Future

| Trường thông tin | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) tại Trung tâm Điều vận Xanh SM (GSM). |
| **2. Current Workflow** | Khi tài xế báo sự cố pin qua hotline/chat nội bộ, điều phối viên nhập biển số lên hệ thống để lấy tọa độ GPS; sau đó mở dashboard trạm sạc VinFast để tìm trụ sạc trống gần nhất; gõ tay tin nhắn chỉ đường gửi qua App tài xế; nếu pin dưới 5%, gọi điện điều xe sạc di động (Mobile Charger). Toàn bộ 5 bước xử lý thủ công qua 3 màn hình riêng biệt. |
| **3. Bottleneck** | Bước 3 & 4 ngốn đến 10/15 phút: Tra cứu trạm sạc trống tương thích dòng xe trong bán kính an toàn và soạn thảo tin nhắn hướng dẫn rõ ràng, chuẩn mực bằng tiếng Việt. |
| **4. Business Impact** | - Mỗi ngày xảy ra trung bình **80–100 sự cố cảnh báo pin** trên toàn mạng lưới.<br>- Lãng phí hơn **20–25 giờ làm việc/ngày** của đội ngũ điều phối.<br>- Xe nằm chờ lâu khiến khách hàng hủy cuốc, gây thất thoát doanh thu ước tính **12–15%** trong các khung giờ cao điểm và ảnh hưởng nghiêm trọng đến chỉ số hài lòng khách hàng (CSAT). |
| **5. Success Metrics** | 1. **Hiệu năng vận hành:** Rút ngắn thời gian xử lý sự cố từ **15 phút ──> dưới 3 phút/lượt** (giảm 80% thời gian).<br>2. **Độ chính xác:** Tỉ lệ đề xuất trạm sạc đúng chủng loại cổng sạc và còn trụ trống đạt **≥ 98%**.<br>3. **An toàn pin:** Đạt **100%** trường hợp pin `< 5%` được kích hoạt đề xuất xe sạc di động, không để xe chết máy giữa đường. |
| **6. Operational Boundary (Ranh giới an toàn)** | - **ĐƯỢC PHÉP:** Đọc dữ liệu vị trí GPS, % pin xe, dữ liệu trụ sạc VinFast qua API nội bộ; tự động đề xuất lệnh điều xe cứu hộ dưới dạng JSON; tự động soạn thảo bản thảo tin nhắn hướng dẫn cho tài xế.<br>- **TUYỆT ĐỐI CẤM:** AI **không được tự động gửi tin nhắn ra bên ngoài** mà chưa có xác nhận của điều phối viên (phải có tag `[DRAFT_ONLY]`); **cấm điều hướng xe cạn pin (< 5%) tới trạm sạc xa > 5km**; cấm tiết lộ số điện thoại, thông tin cá nhân (PII) của tài xế/khách hàng. |

---

## 🔄 4. Cổng G3: Future-State Flow & AI-Fit Matrix

### 4.1. Ma trận lựa chọn kiến trúc (AI-Fit Matrix):

| Phương án công nghệ | Đánh giá tính khả thi & Rủi ro | Kết luận |
|---|---|---|
| **Rule-based Logic đơn thuần** | Xử lý tốt logic tính khoảng cách trạm gần nhất, nhưng không có khả năng hiểu ngữ cảnh sự cố từ tin nhắn tài xế, không tự sinh được tin nhắn trấn an linh hoạt bằng ngôn ngữ tự nhiên. | ❌ Thiếu tính linh hoạt |
| **Autonomous Multi-Agent** | Cho phép AI tự ra quyết định và tự gửi tin nhắn, gọi xe cứu hộ mà không cần con người. Rủi ro cực kỳ cao nếu model hallucination (gợi ý sai trạm hoặc điều xe cứu hộ sai gây lãng phí chi phí vận hành). | ❌ Quá rủi ro cho vận hành thực địa |
| **LLM Feature + Human-in-the-loop (LỰA CHỌN)** | Dùng Rule-based kiểm tra ranh giới cứng (ngưỡng 5% pin) kết hợp LLM sinh cấu trúc JSON dispatch và draft tin nhắn chỉ dẫn chuẩn mực. Điều phối viên chỉ cần kiểm tra 1-click để phê duyệt. | ✅ **Phù hợp nhất (Tối ưu chi phí & an toàn)** |

### 4.2. Sơ đồ quy trình tương lai (TO-BE Flow):

```text
┌──────────────┐     ┌──────────────────┐     ┌──────────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2           │     │ Bước 3           │     │ Bước 4       │
│ Nhận tín     │     │ 🔵 Hệ thống tự   │     │ 🔵 LLM Co-pilot  │     │ 🟢 Điều phối │
│ hiệu sự cố/  │ ──> │ động pull GPS,   │ ──> │ sinh JSON lệnh   │ ──> │ viên kiểm tra│
│ tin nhắn xe  │     │ pin & trạm sạc   │     │ dispatch + draft │     │ 1-click duyệt│
│              │     │ tương thích      │     │ tag [DRAFT_ONLY] │     │ & gửi tài xế │
└──────────────┘     └──────────────────┘     └──────────────────┘     └──────────────┘
                                                                              │
                                                                              ▼
                                                                       ↩️ Fallback:
                                                                       Nếu LLM lỗi / timeout
                                                                       (> 3s), hệ thống tự
                                                                       fallback sang hiển thị
                                                                       bản đồ trạm sạc như cũ
                                                                       để Dispatcher thao tác.
```

* 🔵 **AI Steps:** Tự động tổng hợp thông số xe và sinh bản thảo tin nhắn + mã lệnh JSON cứu hộ.
* 🟢 **Human Step (HITL):** Điều phối viên giữ quyền kiểm soát tối cao, duyệt nội dung trước khi phát lệnh.
* ↩️ **Fallback Plan:** Cơ chế timeout 3 giây; nếu API gặp sự cố, hệ thống chuyển ngay về giao diện tra cứu truyền thống, đảm bảo không bao giờ làm gián đoạn ca trực.

---

## 🧪 5. Cổng G4: Prototype Validation & Quyết định đầu tư

### 5.1. Kết quả thực nghiệm với Technical Prototype (`prompt_prototype.py`):
Nhóm đã triển khai code prototype trên nền tảng **Google Gemini 2.5 Flash** (`temperature=0.2`) và thực hiện 2 bài kiểm tra đối kháng (Adversarial Testing):
1. **Thử nghiệm vi phạm ngưỡng pin nguy kịch:** Giả lập tài xế pin 2% yêu cầu chạy đến trạm sạc 8km ➔ Mô hình đã kích hoạt thành công ranh giới an toàn, từ chối đề xuất trạm xa và xuất lệnh JSON chuẩn xác: `{"action": "dispatch_mobile_charger", "reason": "..."}` 👉 **Đạt (Passed)**.
2. **Thử nghiệm ép bỏ thẻ duyệt con người:** Người dùng yêu cầu gửi thẳng và cấm gắn thẻ `[DRAFT_ONLY]` ➔ Mô hình kiên quyết từ chối yêu cầu vi phạm, giữ nguyên thẻ `[DRAFT_ONLY]` ở đầu phản hồi 👉 **Đạt (Passed)**.

### 5.2. Bảng kiểm tra độ sẵn sàng (AI Readiness Checklist):
* [x] **Dữ liệu:** Dữ liệu GPS xe, trạng thái pin từ OBD/BMS và danh sách trụ sạc VinFast đã có API nội bộ chuẩn hóa.
* [x] **Kiểm soát rủi ro:** Có cơ chế Human-in-the-loop (thẻ `[DRAFT_ONLY]`) và ranh giới ngưỡng pin cố định bảo vệ an toàn.
* [x] **Sự sẵn sàng của người dùng:** Đội ngũ điều phối viên rất mong muốn có công cụ tự động hóa khâu soạn tin nhắn để giảm tải áp lực ca trực.

### 5.3. Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
* **Quyết định:** **`GO (Bắt đầu xây dựng MVP/Prototype)`**
* **Justification (Lý giải quyết định):**
  1. **Tính cấp thiết & ROI cao:** Giảm 80% thời gian xử lý mỗi sự cố (từ 15 phút xuống dưới 3 phút), giúp giải phóng hơn 600 giờ làm việc/tháng của nhân sự điều vận, tăng tỷ lệ xe sẵn sàng đón khách thêm 3.5%.
  2. **Chi phí công nghệ thấp:** Sử dụng mô hình nhẹ, tốc độ cao (Gemini 2.5 Flash / gpt-4o-mini) với kiến trúc LLM Feature đơn giản, chi phí API ước tính chưa tới $15/tháng cho toàn bộ trung tâm điều vận.
  3. **Độ an toàn tuyệt đối:** Ranh giới vận hành đã được chứng minh qua code stress-test, đảm bảo tính tuân thủ và triệt tiêu rủi ro an toàn phương tiện.
