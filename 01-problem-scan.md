# Phase 1 — SCAN & QUICK-ASSESS: Quét Cơ Hội & Đánh Giá Nhanh

**Người thực hiện:** Lương Khánh Toàn  
**Vị trí:** AI Product Engineer — Vin Smart Future  
**Đơn vị phối hợp:** Khối Vận Hành Vingroup  

---

## 🏛️ Bối cảnh: Tôi là ai?

Tôi là kỹ sư AI tại **Vin Smart Future**. Nhiệm vụ của nhóm chúng tôi là khảo sát thực địa tại các công ty thành viên trong hệ sinh thái Vingroup, nhận diện các nút thắt cổ chai (bottlenecks) trong vận hành hàng ngày và đề xuất giải pháp ứng dụng AI/tự động hóa để giải phóng sức lao động, nâng cao trải nghiệm người dùng và an toàn vận hành.

---

# 🔍 Phase 1 — SCAN: Danh sách 5 bài toán quét qua 4 Lenses

Bằng cách áp dụng **4 Lenses** (*Lặp lại, Tốn thời gian, AI-upgrade, Stakeholder Pain*), tôi đã quét qua các đơn vị thành viên và xác định 5 bài toán thực tế sau:

| # | Subsidiary | Lens | Mô tả ngắn bài toán & Bottleneck |
|---|------------|------|----------------------------------|
| 1 | **Xanh SM (GSM)** | **Lặp lại** | Tài xế phải tự chụp ảnh đồng hồ ODO + nhập tay số km và % pin khi kết thúc ca để đối soát doanh thu; nhân viên vận hành mất nhiều giờ kiểm tra chéo thủ công hàng trăm ca/ngày, dễ nhầm lẫn số liệu. |
| 2 | **VinFast** | **Tốn thời gian** | Nhân viên trung tâm bảo hành/dịch vụ phải đọc thủ công log lỗi pin và hệ thống từ xe gửi về, phân loại mức độ nghiêm trọng trước khi xếp lịch sửa chữa (mất 15–20 phút/xe). |
| 3 | **Vinhomes** | **AI-upgrade** | Tổng đài cư dân tiếp nhận hàng trăm yêu cầu lặp lại mỗi ngày (báo rò rỉ nước, hỏng thang máy, hỏi phí quản lý) nhưng vẫn xử lý qua nhân viên trực tổng đài trả lời rập khuôn, thời gian phản hồi lâu (chờ 12–24h). |
| 4 | **Vinmec** | **Stakeholder Pain** | Bác sĩ phàn nàn phải tự đọc và tóm tắt lại hồ sơ bệnh án dài từ các lần khám trước để chuẩn bị cho ca khám tiếp theo, gây trễ lịch khám dây chuyền và tăng áp lực quá tải lâm sàng. |
| 5 | **Vinpearl / VinWonders** | **Lặp lại** | Nhân viên quầy vé phải trả lời lặp đi lặp lại các câu hỏi giống nhau (giờ mở cửa, giá vé combo, lịch trình show diễn) trong giờ cao điểm, gây ùn ứ kéo dài tại hàng chờ quầy vé. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn Top 3 bài toán tiêu biểu từ danh sách SCAN: **#1 (Xanh SM), #2 (VinFast), #4 (Vinmec)**.

---

### 📇 QUICK PROBLEM CARD #1: Xanh SM — Tự động hóa đối soát ODO & Pin cuối ca

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                       │
│                                                                             │
│ Bài toán: Tự động trích xuất & đối soát dữ liệu ODO và % Pin cuối ca của    │
│ tài xế taxi điện Xanh SM qua ảnh chụp taplo.                                │
│ Công ty thành viên: [x] Xanh SM (GSM)                                       │
│                                                                             │
│ Ai đang đau? Tài xế (chờ giao ca), Nhân viên vận hành (đối soát thủ công)   │
│                                                                             │
│ Workflow thủ công hiện tại (5 bước):                                        │
│   1. Tài xế dừng xe, chụp ảnh màn hình taplo (ODO + % pin)                  │
│   → 2. Tài xế gõ tay số km và pin vào form giao ca trên App tài xế          │
│   → 3. Dữ liệu gửi về bảng tính Excel của điều phối viên ca trực            │
│   → 4. Nhân viên vận hành mở từng ảnh kiểm tra chéo số nhập vs số trên ảnh  │
│   → 5. Ký duyệt hoàn thành ca và tính toán doanh thu/điện năng tiêu thụ     │
│                                                                             │
│ Bước nào tốn nhất? Bước 4 (⏱ 5–7 phút/xe x hàng trăm xe/ngày)               │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 4                                │
│ (Vision-Language Model tự đọc ODO, pin từ ảnh và đối chiếu tự động)         │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                       │
│ - Giảm thời gian nghiệm thu ca từ 7 phút/xe xuống dưới 30 giây/xe.          │
│ - Giảm tỉ lệ sai sót nhập liệu do con người từ 4.5% xuống < 0.2%.           │
│                                                                             │
│ Quick Architecture: [x] LLM / VLM Feature                                   │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 QUICK PROBLEM CARD #2: VinFast — Phân loại & cảnh báo log lỗi xe điện khẩn cấp

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                       │
│                                                                             │
│ Bài toán: Đọc hiểu và phân loại mức độ khẩn cấp log lỗi pin/hệ thống xe     │
│ điện gửi về xưởng dịch vụ VinFast để điều phối sửa chữa.                    │
│ Công ty thành viên: [x] VinFast                                             │
│                                                                             │
│ Ai đang đau? Kỹ sư chẩn đoán (quá tải), Khách hàng (xe nằm xưởng lâu)       │
│                                                                             │
│ Workflow thủ công hiện tại (5 bước):                                        │
│   1. Xe phát sinh lỗi, ECU ghi nhận mã DTC và truyền telematics về server  │
│   → 2. Kỹ sư xưởng mở file log mã lỗi chuyên dụng (hàng trăm dòng code)     │
│   → 3. Tra cứu thủ công tài liệu kỹ thuật của từng dòng xe (VF5, VF8, VF9)  │
│   → 4. Đánh giá mức độ nguy hiểm (Pin nguy kịch vs Lỗi cảm biến phụ)        │
│   → 5. Soạn phiếu yêu cầu kỹ thuật và phân bổ cầu nâng sửa chữa             │
│                                                                             │
│ Bước nào tốn nhất? Bước 3 & 4 (⏱ 15–20 phút/xe)                            │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4                            │
│ (AI trích xuất mã lỗi, giải mã ý nghĩa kỹ thuật và draft phân loại mức độ)  │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                       │
│ - Rút ngắn thời gian chẩn đoán ban đầu từ 20 phút xuống dưới 2 phút/xe.     │
│ - Phát hiện 100% lỗi pin nguy kịch (< 5% hoặc quá nhiệt) ngay lập tức.      │
│                                                                             │
│ Quick Architecture: [x] LLM Feature + Rule-based Boundary                   │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 QUICK PROBLEM CARD #4: Vinmec — Trợ lý tóm tắt bệnh án cho ca khám tiếp theo

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                                       │
│                                                                             │
│ Bài toán: Trích xuất & tóm tắt tiền sử bệnh án, dị ứng thuốc và diễn biến    │
│ các lần khám trước cho bác sĩ lâm sàng Vinmec trước giờ khám bệnh.          │
│ Công ty thành viên: [x] Vinmec                                              │
│                                                                             │
│ Ai đang đau? Bác sĩ điều trị (quá tải đọc bệnh án), Bệnh nhân (chờ đợi)     │
│                                                                             │
│ Workflow thủ công hiện tại (5 bước):                                        │
│   1. Bệnh nhân vào phòng khám, bác sĩ mở hồ sơ bệnh án điện tử (EHR)        │
│   → 2. Bác sĩ cuộn qua 5-10 trang ghi chú khám bệnh, xét nghiệm cũ          │
│   → 3. Đọc tìm tiền sử dị ứng và các loại thuốc đang dùng tại nhà           │
│   → 4. Tổng hợp thông tin trong đầu để định hướng câu hỏi khám mới          │
│   → 5. Tiến hành khám và chỉ định cận lâm sàng                              │
│                                                                             │
│ Bước nào tốn nhất? Bước 2 & 3 (⏱ 7–10 phút/bệnh nhân)                       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3                            │
│ (LLM đọc EHR đã khử định danh, tóm tắt 5 gạch đầu dòng tiền sử lâm sàng)    │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                       │
│ - Giảm thời gian chuẩn bị trước ca khám từ 8 phút xuống dưới 1.5 phút.       │
│ - Không bỏ sót 100% các cảnh báo dị ứng thuốc có trong hồ sơ cũ.            │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (Human-in-the-loop: Bác sĩ duyệt)      │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# 🗳️ Quyết định lựa chọn của nhóm

Nhóm quyết định chọn bài toán **"Xanh SM — Tối ưu hóa điều vận & xử lý sự cố pin xe điện khẩn cấp (Dispatcher Co-pilot for EV Fleet)"** (gắn liền với Card #1 & Card #2) để thực hiện Deep-Dive trong `02-deep-dive-report.md`.

### Lý do lựa chọn và loại bỏ các thẻ khác:
1. **Lý do chọn Xanh SM & Pin xe điện:** 
   - Đây là bài toán có tác động trực tiếp và tức thì đến sự sống còn trong vận hành xe taxi điện (tài xế cạn pin giữa đường gây ách tắc giao thông và nguy cơ tai nạn).
   - Có ranh giới vận hành kỹ thuật rất sắc nét (ngưỡng pin `< 5%` bắt buộc điều xe cứu hộ lưu động, cấm điều hướng trạm xa `> 5km`).
   - Đã được nhóm kiểm chứng tính khả thi và ranh giới an toàn thông qua bản mẫu kỹ thuật `starter-code/prompt_prototype.py`.
2. **Lý do loại bài toán Vinmec (#4):**
   - Mặc dù mang lại giá trị lớn, bài toán y tế đòi hỏi tuân thủ nghiêm ngặt chuẩn HIPAA/bảo mật dữ liệu bệnh nhân và quy trình thẩm định lâm sàng phức tạp, chu kỳ triển khai kéo dài.
3. **Lý do loại bài toán Vinhomes (#3) & Vinpearl (#5):**
   - Chủ yếu là hỏi đáp thông tin (FAQ/CSKH), độ phức tạp thấp và có thể giải quyết hiệu quả bằng Rule-based FAQ bot hoặc RAG cơ bản, chưa phát huy tối đa vai trò của AI Dispatcher Co-pilot.
