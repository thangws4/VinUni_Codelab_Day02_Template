# 02-deep-dive-report.md — Báo Cáo Phân Tích Sâu (Deep-Dive Report)

**Dự án:** Xanh SM Intelligent Battery Dispatcher & Rescue Co-Pilot  
**Đơn vị thực hiện:** Khối Công nghệ Vin Smart Future & Khối Vận hành Xanh SM (GSM)  
**Tác giả:** Trần Anh Quân (AI Product Engineer)  
**Trạng thái đề xuất:** [x] **GO**  [ ] NOT YET  [ ] NO-GO  

---

## 🏛️ 1. Bối cảnh & Lý do lựa chọn bài toán

Tại Trung tâm Điều vận Xanh SM (Hà Nội & TP.HCM), mỗi ngày đội xe taxi điện tiếp nhận hàng nghìn cuốc xe di chuyển liên tục. Trong quá trình vận hành, hiện tượng xe bị sụt pin bất ngờ vào giờ cao điểm hoặc gần cạn kiệt (< 5%) trên đường đón/trả khách diễn ra với tần suất khoảng **70–90 ca/ngày**.

Hiện tại, quy trình xử lý hoàn toàn thủ công khiến mỗi sự cố ngốn **15 phút** xử lý của Điều phối viên (Dispatcher), tăng thời gian chờ của tài xế lên 20–30 phút, gây áp lực tâm lý nặng nề và làm thất thoát khoảng **15% doanh thu** tiềm năng trong khung giờ vàng. Dự án này được thiết kế để tự động hóa khâu tra cứu, tổng hợp và đề xuất giải pháp sạc/cứu hộ an toàn tức thì.

---

## 🏗️ 2. Current-State Workflow Mapping (Quy trình hiện tại)

Quy trình 5 bước thủ công của Điều phối viên khi tiếp nhận sự cố pin:

```text
┌────────────────┐     ┌────────────────┐     ┌────────────────┐     ┌────────────────┐
│ Bước 1         │     │ Bước 2         │     │ Bước 3 🔴      │     │ Bước 4 🔴      │
│ Tiếp nhận cuộc │     │ Tra cứu định vị│     │ Tra cứu trạm   │     │ Soạn văn bản   │
│ gọi / tin nhắn │ ──> │ toạ độ GPS xe  │ ──> │ sạc VinFast    │ ──> │ chỉ dẫn & toạ  │
│ sự cố từ tài xế│     │ trên FMS Portal│     │ còn trụ trống  │     │ độ gửi tài xế  │
│ Ai: Dispatcher │     │ Ai: Dispatcher │     │ Ai: Dispatcher │     │ Ai: Dispatcher │
│ ⏱ 2 phút       │     │ ⏱ 2 phút       │     │ ⏱ 5-6 phút     │     │ ⏱ 5-6 phút     │
└────────────────┘     └────────────────┘     └────────────────┘     └────────────────┘
                                                                             │
                                                                             ▼
                                                                      ┌────────────────┐
                                                                      │ Bước 5 🔄      │
                                                                      │ Nếu pin < 5%,  │
                                                                      │ liên hệ hotline│
                                                                      │ xe sạc cứu hộ  │
                                                                      │ ⏱ 1-2 phút     │
                                                                      └────────────────┘

🔴 = Điểm nghẽn nghiêm trọng (Bottlenecks)
🔄 = Điểm chuyển giao thông tin (Handoff)
⏱ Tổng thời gian xử lý thủ công: ~15 phút/lượt.
```

---

## 📋 3. Problem Statement (6-Field Standard)

| Trường thông tin | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) thuộc Trung tâm Giám sát và Điều vận Xanh SM (GSM), tài xế xe taxi điện VF5, VFe34, VF8. |
| **2. Current Workflow** | Tài xế gọi hotline báo cạn pin. Điều phối viên mở phần mềm định vị FMS lấy toạ độ xe, mở dashboard hệ thống trạm sạc VinFast tìm trạm còn trụ sạc phù hợp còn trống, gõ tay SMS chỉ đường gửi cho tài xế. Nếu pin dưới 5%, tiếp tục gọi hotline đối tác cứu hộ pin di động. Toàn bộ 5 bước thủ công tốn trung bình 15 phút. |
| **3. Bottlenecks** | **Bước 3 & 4 (chiếm 10–12 phút):** Tra cứu thủ công trụ sạc trống theo đúng chuẩn cổng sạc của từng dòng xe và soạn thảo thủ công tin nhắn SMS/In-app bằng tiếng Việt chi tiết đường đi. |
| **4. Business Impact** | Mỗi ngày có ~80 ca sự cố pin tại 2 thành phố lớn. Lãng phí hơn 20 giờ làm việc/ngày của điều phối viên. Tăng tỉ lệ tài xế hoang mang hủy cuốc, gây nguy cơ xe cạn pin chết máy giữa đường làm tắc nghẽn giao thông và rò rỉ khoảng 15% doanh thu vận hành hằng ngày. |
| **5. Success Metrics** | 1. **Thời gian (Efficiency):** Giảm tổng thời gian xử lý từ 15 phút xuống **dưới 3 phút/lượt** (giảm 80%).<br>2. **Độ an toàn (Quality):** Tỉ lệ tuân thủ ranh giới an toàn (không bao giờ chỉ trạm > 5km khi pin < 5%) đạt **100%**.<br>3. **Độ hài lòng:** Điểm CSAT của tài xế đối với quy trình cứu hộ đạt **>= 4.8/5.0**. |
| **6. Operational Boundaries** | **AI ĐƯỢC PHÉP:** Đọc toạ độ GPS, đọc dung lượng pin qua Telematics API, truy xuất trạm sạc VinFast trống, tạo draft hướng dẫn và tạo payload JSON điều xe cứu hộ.<br>**AI TUYỆT ĐỐI CẤM:** Không được tự ý gửi tin trực tiếp cho tài xế mà không có tag `[DRAFT_ONLY]` để Điều phối viên duyệt (Bắt buộc Human-in-the-loop); Không được đề xuất trạm sạc > 5km khi pin < 5%. |

---

## 🚀 4. Future-State Flow & AI Fit Analysis

### 4.1. Ma trận lựa chọn kiến trúc (AI-Fit Matrix)
* [ ] **Rule-based Code / State-Machine:** Không đủ linh hoạt để xử lý các mô tả sự cố ngôn ngữ tự nhiên từ tài xế (ví dụ: *"xe báo lỗi rùa bò, kẹt ngã tư Lê Văn Lương"*).
* [x] **LLM Feature (Kèm Rule Guardrails) — LỰA CHỌN TỐI ƯU:** Sử dụng Gemini 2.5 Flash để đọc hiểu context, tự động trích xuất thực thể, tra cứu dữ liệu và soạn draft tin nhắn chuẩn hóa có tag `[DRAFT_ONLY]` hoặc trả về JSON `dispatch_mobile_charger`. Kiến trúc nhẹ, chi phí token thấp, phản hồi dưới 2 giây.
* [ ] **Autonomous Multi-Agent Loop:** Không phù hợp vì rủi ro "hallucination loop", độ trễ cao và chi phí vận hành không cần thiết cho quy trình đã có luồng nghiệp vụ cố định.

### 4.2. Sơ đồ quy trình tương lai (Future-State Workflow)

```text
┌────────────────┐     ┌────────────────────────┐     ┌────────────────────────┐     ┌────────────────┐
│ Bước 1         │     │ Bước 2 (🔵 AI & API)   │     │ Bước 3 (🔵 AI Step)    │     │ Bước 4 (🟢 HITL)│
│ Nhận tín hiệu  │     │ Auto-pull GPS, mức pin │     │ LLM Feature phân tích: │     │ Điều phối viên │
│ sự cố từ tài xế│ ──> │ & API trạm sạc VinFast │ ──> │ - Nếu pin < 5%: JSON   │ ──> │ kiểm tra draft │
│ hoặc xe tự động│     │ khả dụng gần nhất      │     │ - Nếu an toàn: Draft SMS│    │ & click Duyệt  │
└────────────────┘     └────────────────────────┘     │   bắt đầu bằng [DRAFT] │     │ gửi cho tài xế │
                                                      └────────────────────────┘     └────────────────┘
                                                                   │
                                                                   ▼
                                                            ↩️ Fallback Mechanism:
                                                            Nếu AI timeout hoặc format
                                                            sai, hệ thống chuyển về form
                                                            nhập tay thủ công như cũ.
```

---

## 📊 5. Đánh Giá Độ Sẵn Sàng & Quyết Định (Phase 5 — EVALUATE)

### 5.1. AI Readiness Checklist:
1. [x] **Dữ liệu & API:** Đã có sẵn Telematics API của xe VinFast (GPS, SOC pin) và Open API của hệ thống trạm sạc VinFast.
2. [x] **Kiểm soát rủi ro:** Rủi ro AI sai lệch được triệt tiêu 100% nhờ cơ chế chốt chặn kép: **Tag `[DRAFT_ONLY]` bắt buộc con người phê duyệt** và **Hàm Assertion kiểm tra pin < 5%**.
3. [x] **Sự sẵn sàng của người vận hành:** Đội ngũ Dispatcher rất hào hứng vì hệ thống giúp giảm 80% gánh nặng gõ phím và tra cứu thủ công.

### 5.2. Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:

# ✅ QUYẾT ĐỊNH: GO (Bắt đầu phát triển Prototype)

**Lý giải quyết định (Justification):**
1. **Giá trị kinh tế rõ ràng:** Tiết kiệm hơn 600 giờ lao động/tháng của đội ngũ điều phối, giảm tỉ lệ hủy chuyến xe do cạn pin, bảo vệ tuổi thọ ắc quy xe điện VinFast.
2. **Chi phí kỹ thuật tối ưu:** Giải pháp áp dụng Gemini 2.5 Flash dạng lightweight API call, chi phí dưới 0.001 USD/cuộc gọi, không đòi hỏi hạ tầng máy chủ GPU đắt đỏ.
3. **Độ an toàn đã được kiểm chứng bằng mã nguồn:** Script `starter-code/prompt_prototype.py` đã vượt qua 100% các bài test biên (Adversarial test cases), chứng minh ranh giới an toàn hoạt động vững chắc.
