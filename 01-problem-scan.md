# 01 — Problem Scan & Quick Assessment

> **Lab 02 — AI Product Scoping (Vin Smart Future)**
> Người thực hiện: **Nguyen Duc Thang** — branch cá nhân `thangnd`
> Vai trò giả định: AI Product Engineer tại **Vin Smart Future**

> ⚠️ **Lưu ý về số liệu:** Toàn bộ con số trong tài liệu này (số lượt/ngày, phút/lượt, tỉ lệ %) là **ước lượng giả định** dựa trên quan sát quy trình vận hành phổ biến của ngành quản lý vận hành bất động sản, **không phải số liệu nội bộ chính thức của Vingroup**. Trước khi chuyển sang giai đoạn xây dựng thật, các con số này bắt buộc phải được xác minh lại bằng log hệ thống thực tế.

---

# 🔍 Phase 1 — SCAN: Quét cơ hội bằng 4 Lenses

Tôi quét qua hoạt động vận hành hằng ngày của các công ty thành viên Vingroup với 4 lenses: **Lặp lại**, **Tốn thời gian**, **AI có thể tốt hơn**, **Pain từ người khác**.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Vinhomes** | Lặp lại | Nhân viên trực BQL đọc thủ công từng phản ánh cư dân gửi qua App Vinhomes Resident, tự gắn nhãn nhóm sự vụ (điện nước / an ninh / vệ sinh / thang máy / tiếng ồn / phí) rồi chuyển tay cho đúng tổ kỹ thuật. Lặp lại vài trăm lượt mỗi ngày trên một đại đô thị. |
| 2 | **Vinhomes** | Tốn thời gian | Soạn tin phản hồi cư dân: mỗi phản ánh cần một tin nhắn riêng, đúng giọng điệu dịch vụ, đúng thông tin tình trạng xử lý. Nhân viên phải gõ tay từ đầu vì template cũ quá cứng, không khớp ngữ cảnh. |
| 3 | **Vinpearl** | Pain từ người khác | Quản lý khách sạn phàn nàn vì phát hiện review tiêu cực nghiêm trọng (phòng bẩn, thái độ nhân viên, mất đồ) trên Booking/Agoda/Google Maps quá muộn — thường sau khi khách đã rời đi và review đã hiển thị công khai vài ngày. |
| 4 | **VinFast** | AI có thể tốt hơn | Khách mô tả lỗi xe bằng ngôn ngữ đời thường ("đi qua gờ giảm tốc nghe cụp cụp ở bánh trước"), tổng đài viên phải tự suy đoán và gán mã lỗi kỹ thuật trước khi chuyển xưởng — dễ gán sai, xưởng phải chẩn đoán lại từ đầu. |
| 5 | **Xanh SM** | Tốn thời gian | Tổng hợp lý do huỷ chuyến từ ghi chú tài xế và ghi âm tổng đài để tìm pattern lỗi hệ thống — hiện làm thủ công theo tuần, phát hiện vấn đề quá chậm so với tốc độ thay đổi của thị trường. |
| 6 | **Vinmec** | Pain từ người khác | Bác sĩ phản ánh mất quá nhiều thời gian soạn tóm tắt hồ sơ xuất viện bằng ngôn ngữ dễ hiểu cho bệnh nhân, trong khi đây là việc trích xuất - diễn giải lại dữ liệu đã có sẵn trong bệnh án điện tử. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Tôi chọn ra top 3 từ bảng SCAN: **#1+#2 (Vinhomes, gộp thành một luồng công việc liền mạch)**, **#3 (Vinpearl)**, **#4 (VinFast)**.

## Card #1 — Vinhomes: Phân loại & điều hướng phản ánh cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Phản ánh cư dân gửi qua App Vinhomes Resident     │
│ đang được nhân viên trực BQL phân loại và soạn phản hồi     │
│ hoàn toàn thủ công, gây chậm SLA và route sai bộ phận.      │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│ Nhân viên trực BQL (quá tải giờ cao điểm sáng/tối),         │
│ cư dân (chờ phản hồi lâu), tổ kỹ thuật (nhận nhầm ticket).  │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Cư dân gửi phản ánh (text + ảnh) trên App              │
│   → 2. NV trực đọc, tự gắn nhóm sự vụ & mức ưu tiên         │
│   → 3. Tra sổ phân công, chuyển đúng tổ / nhà thầu          │
│   → 4. Gõ tay tin phản hồi gửi lại cư dân                   │
│   → 5. Theo dõi tiến độ & đóng ticket                       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│ Bước 2 và Bước 4 (⏱ ~12 phút/lượt trên tổng ~18 phút)       │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 và 4        │
│ (Gợi ý nhóm sự vụ + mức ưu tiên + bộ phận tiếp nhận,        │
│  đồng thời soạn sẵn bản nháp phản hồi để NV duyệt).         │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian xử lý một phản ánh từ ~18 phút ──> dưới      │
│ 5 phút; ≥85% ticket được route đúng bộ phận ngay lần đầu.   │
│                                                             │
│ Quick Architecture: [x] LLM Feature (phân loại + draft)      │
└─────────────────────────────────────────────────────────────┘
```

## Card #2 — Vinpearl: Cảnh báo sớm review tiêu cực nghiêm trọng

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Review tiêu cực nghiêm trọng trên các OTA         │
│ (Booking, Agoda, Google Maps) được phát hiện quá muộn,      │
│ mất cơ hội xử lý trước khi khách rời khách sạn.             │
│ Công ty thành viên: [x] Khác: Vinpearl / VinWonders         │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│ Hotel Manager và team Chăm sóc khách hàng của từng cơ sở.   │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. NV CSKH mở lần lượt từng nền tảng OTA mỗi sáng         │
│   → 2. Đọc lướt review mới, tự đánh giá mức nghiêm trọng    │
│   → 3. Copy các review "đáng lo" vào file Excel tổng hợp    │
│   → 4. Gửi email tổng hợp cho Manager cuối ngày             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│ Bước 1-2 (⏱ ~45 phút/ngày/cơ sở, dễ bỏ sót khi review nhiều)│
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3              │
│ (Chấm mức nghiêm trọng + trích chủ đề phàn nàn + tóm tắt).  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Thời gian từ lúc review xuất hiện đến lúc Manager nhận       │
│ cảnh báo: từ ~24 giờ ──> dưới 30 phút.                      │
│                                                             │
│ Quick Architecture: [x] LLM Feature (phân loại + tóm tắt)    │
└─────────────────────────────────────────────────────────────┘
```

## Card #3 — VinFast: Phân loại mã lỗi kỹ thuật từ mô tả tiếng Việt của khách

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Tổng đài viên phải tự suy đoán mã lỗi kỹ thuật    │
│ từ mô tả đời thường của khách trước khi chuyển xưởng dịch   │
│ vụ, dẫn đến gán sai nhóm lỗi và chẩn đoán lại từ đầu.       │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│ Tổng đài viên CSKH và kỹ thuật viên xưởng dịch vụ VinFast.  │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Khách gọi/chat mô tả hiện tượng bằng ngôn ngữ đời thường│
│   → 2. Tổng đài viên tra bảng mã lỗi, tự chọn nhóm lỗi      │
│   → 3. Tạo phiếu hẹn và chuyển xưởng gần nhất               │
│   → 4. Kỹ thuật viên chẩn đoán lại khi xe tới xưởng         │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│ Bước 2 (⏱ ~8 phút/lượt, tỉ lệ gán sai nhóm lỗi còn cao)     │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2                │
│ (Map mô tả tự nhiên ──> top-3 nhóm lỗi kèm độ tin cậy).     │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Tỉ lệ phiếu hẹn được gán đúng nhóm lỗi ngay lần đầu:        │
│ từ ~65% ──> trên 85%.                                       │
│                                                             │
│ Quick Architecture: [x] LLM Feature (classification)         │
└─────────────────────────────────────────────────────────────┘
```

---

# 🗳️ Quyết định lựa chọn bài toán Deep-Dive

Tôi chọn **Card #1 — Vinhomes: Phân loại & điều hướng phản ánh cư dân** để thực hiện Deep-Dive ở [02-deep-dive-report.md](02-deep-dive-report.md).

## Lý do chọn Card #1

1. **Quy trình hiện tại rõ ràng và quan sát được.** Toàn bộ 5 bước đều nằm trong một hệ thống nội bộ (App Resident + sổ phân công BQL), nên đo được thời gian từng bước và có log để làm baseline — điều kiện tiên quyết để chứng minh AI thực sự tạo cải thiện.
2. **Đúng vùng AI mạnh nhất.** Bài toán bản chất là *hiểu văn bản tiếng Việt tự do → phân loại → sinh văn bản*. Đây là việc rule-based cứng làm rất kém (cư dân viết mỗi người một kiểu, đầy lỗi chính tả và tiếng lóng) nhưng LLM làm tốt.
3. **Rủi ro kiểm soát được bằng HITL.** Đầu ra của AI chỉ là *bản nháp* — nhân viên BQL luôn duyệt trước khi gửi. Sai sót không rơi thẳng xuống cư dân.
4. **Ranh giới an toàn đủ thú vị để stress-test.** Bài toán có ít nhất 3 ranh giới đáng bảo vệ: không auto-send, không hạ cấp sự cố nguy hiểm tính mạng, không lộ thông tin cá nhân cư dân khác. Đây là chất liệu tốt cho Phase 4.

## Lý do loại 2 card còn lại

* **Loại Card #2 (Vinpearl):** Giá trị thật nằm ở khâu **tích hợp dữ liệu** — phải crawl/kết nối API của Booking, Agoda, Google Maps, mỗi bên một chính sách truy cập và giới hạn pháp lý riêng. Phần AI (chấm mức nghiêm trọng) chỉ chiếm một phần nhỏ công sức. Đây là bài toán data engineering đội lốt bài toán AI, không phù hợp với scope một buổi lab.
* **Loại Card #3 (VinFast):** Sai sót ở đây tác động trực tiếp đến **an toàn xe và an toàn người lái**. Nếu mô hình gán nhầm lỗi phanh thành lỗi vặt, hậu quả không thể chỉ khắc phục bằng một bước duyệt của tổng đài viên. Bài toán này cần bộ dữ liệu mã lỗi đã gán nhãn chuẩn và quy trình validation của phòng kỹ thuật trước khi đưa LLM vào — thuộc nhóm **NOT YET**, không nên là bài toán đầu tiên để prototype.
