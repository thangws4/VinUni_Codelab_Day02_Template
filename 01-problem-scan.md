# 01-problem-scan.md — Scan & Quick Cards (Vin Smart Future)

**Học viên thực hiện:** Trần Anh Quân  
**Vị trí:** AI Product Engineer — Vin Smart Future (Vingroup)  
**Đơn vị phối hợp:** Khối Vận Hành Xanh SM (GSM), Vinhomes, Vinmec, VinFast  
**Branch cá nhân:** `tran-anh-quan`  

---

## 🏛️ Bối cảnh thực tế: Vin Smart Future

Là kỹ sư AI thuộc **Vin Smart Future**, tôi tiến hành rà soát các quy trình vận hành cốt lõi xuyên suốt hệ sinh thái Vingroup nhằm phát hiện các "điểm nghẽn" (bottlenecks) có thể tối ưu hoá bằng trí tuệ nhân tạo, mang lại giá trị kinh tế trực tiếp và gia tăng trải nghiệm khách hàng.

---

# 🔍 Phase 1 — SCAN: Bảng Quét Cơ Hội Qua 4 Lenses

Áp dụng phương pháp luận **4 Lenses**:
1. **Lặp lại (Repetitive):** Tác vụ diễn ra tần suất cao, tốn công lặp đi lặp lại.
2. **Tốn thời gian (Time-consuming):** Thao tác thủ công ngốn thời gian xử lý của nhân viên.
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ hiện tại rập khuôn, thiếu linh hoạt, chậm trễ.
4. **Pain từ người khác (Stakeholder Pain):** Điểm nghẽn khiến khách hàng hoặc nhân sự tuyến đầu bức xúc, phàn nàn.

### 📝 Bảng tổng hợp 6 bài toán thực tế tại Vingroup:

| # | Subsidiary | Tên bài toán / Nghiệp vụ | Lens chính | Mô tả ngắn bài toán & Điểm nghẽn |
|---|---|---|---|---|
| **1** | **Xanh SM (GSM)** | **Xử lý sự cố pin nguy cấp & Điều phối trạm sạc thực địa** | **Tốn thời gian & Pain** | Tài xế báo xe sắp cạn pin (< 5%) trên đường đón khách; Điều phối viên mất 15 phút tra cứu toạ độ, tìm trụ sạc trống và soạn chỉ dẫn thủ công hoặc gọi cứu hộ, gây nguy cơ xe chết máy giữa đường. |
| **2** | **Xanh SM (GSM)** | **Tự động tái phân bổ & định tuyến lại chuyến xe thay đổi lộ trình** | **Lặp lại** | Khách hàng đột ngột đổi điểm đến hoặc yêu cầu dừng nhiều điểm giữa chừng; tổng đài viên phải tính toán lại cước và so khớp tài xế thủ công trên 5 màn hình. |
| **3** | **VinFast** | **Đối soát tự động hoá đơn sạc điện từ trạm sạc đối tác** | **Lặp lại & Tốn thời gian** | Cuối mỗi chu kỳ thanh toán, kế toán phải so khớp hàng chục nghìn log sạc từ các trạm sạc liên kết ngoài với hoá đơn thực tế gửi về, mất 4 ngày làm việc mỗi tháng. |
| **4** | **Vinhomes** | **Phân loại & Định tuyến thông minh phản ánh cư dân trên App** | **AI-upgrade & Tốn thời gian** | Hơn 600 phản ánh/ngày từ cư dân (mất nước, ồn ào, thủ tục sổ hồng...) bị CSKH phản hồi theo mẫu máy móc và mất 12-24 giờ để chuyển thủ công tới đúng Ban Quản trị từng tòa. |
| **5** | **Vinmec** | **Trích xuất & Soạn thảo tóm tắt hồ sơ xuất viện (Discharge Summary)** | **Tốn thời gian & Pain** | Bác sĩ mất 25–35 phút/bệnh nhân để lùng sục dữ liệu bệnh án điện tử (EMR), xét nghiệm, toa thuốc để gõ bản tóm tắt xuất viện, khiến bác sĩ quá tải và người bệnh chờ đợi lâu. |
| **6** | **Vinpearl** | **Phân tích cảm xúc & Cảnh báo tức thời review đa kênh khách sạn** | **AI-upgrade** | Khách phản ánh phòng bẩn hoặc dịch vụ kém trên TripAdvisor/Google Maps/Agoda nhưng bộ phận chăm sóc khách hàng chỉ đọc tổng hợp vào cuối tuần, bỏ lỡ thời điểm vàng xử lý sự cố. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn lọc Top 3 bài toán tiềm năng nhất từ danh sách trên để hoàn thiện thẻ phân tích nhanh:
* **Card #1:** Xanh SM — Xử lý sự cố pin xe điện thực địa & Điều phối cứu hộ di động.
* **Card #2:** Vinhomes — Tự động phân loại & định tuyến ticket phản ánh cư dân trên App Vinhomes Resident.
* **Card #3:** Vinmec — Tự động trích xuất & soạn thảo tóm tắt hồ sơ xuất viện (Discharge Summary).

---

### 📇 QUICK PROBLEM CARD #1

```text
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                   │
│                                                                         │
│ Bài toán (1 câu): Tài xế Xanh SM báo sự cố xe cạn pin (< 5%) trên       │
│ đường cần điều phối viên tìm trạm sạc khả dụng hoặc điều xe sạc cứu hộ. │
│ Công ty thành viên: [x] Xanh SM (GSM)  [ ] VinFast  [ ] Vinhomes        │
│                     [ ] Vinmec         [ ] Khác: _____________________  │
│                                                                         │
│ Ai đang đau (Actor)? Tài xế Xanh SM (lo xe chết máy), Điều phối viên     │
│ (Dispatcher) tại TT Vận hành (quá tải vì phải tra cứu thủ công).        │
│                                                                         │
│ Workflow thủ công hiện tại (5 bước):                                    │
│   1. Tài xế gọi hotline điều vận báo dung lượng pin nguy cấp             │
│   ──> 2. Điều phối viên tra GPS xe trên phần mềm định vị FMS             │
│   ──> 3. Mở portal VinFast kiểm tra danh sách trạm sạc còn trụ trống    │
│   ──> 4. Nhập tay địa chỉ, gõ SMS chỉ đường gửi đến App tài xế          │
│   ──> 5. Nếu xe dưới 5% pin và không có trạm gần, gọi hotline cứu hộ    │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 10 - 12 phút/lượt)        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4                        │
│ (Tự động nhận diện mức pin -> Tra cứu khoảng cách -> Draft chỉ dẫn      │
│  hoặc tự động tạo payload trigger xe sạc pin di động cứu hộ).          │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)?                                   │
│ - Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút/lượt.           │
│ - Tỉ lệ chỉ dẫn an toàn (tránh trạm xa khi pin < 5%) đạt 100%.          │
│                                                                         │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent    │
└─────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 QUICK PROBLEM CARD #2

```text
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                   │
│                                                                         │
│ Bài toán (1 câu): Tự động phân loại, trích xuất thực thể và chuyển tiếp  │
│ phản ánh của cư dân trên App Vinhomes Resident đến đúng ban kỹ thuật.   │
│ Công ty thành viên: [ ] Xanh SM (GSM)  [ ] VinFast  [x] Vinhomes        │
│                     [ ] Vinmec         [ ] Khác: _____________________  │
│                                                                         │
│ Ai đang đau (Actor)? Cư dân Vinhomes (chờ phản hồi 12-24h), Nhân viên   │
│ CSKH Ban Quản lý Tòa nhà (quá tải đọc & gán nhãn 500+ ticket/ngày).     │
│                                                                         │
│ Workflow thủ công hiện tại (4 bước):                                    │
│   1. Cư dân gửi form văn bản/hình ảnh phản ánh trên App cư dân          │
│   ──> 2. Nhân viên CSKH đọc từng nội dung, xác định tòa/căn hộ/loại lỗi  │
│   ──> 3. Copy paste nội dung sang hệ thống ticket nội bộ gửi bộ phận    │
│   ──> 4. Gõ phản hồi thủ công xác nhận với cư dân                       │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 8 - 10 phút/ticket)       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3                        │
│ (Trích xuất: mã căn, hạng mục lỗi; Tự động gán nhãn & route đến kỹ thuật│
│  và draft tin nhắn phản hồi tiếp nhận chuyên nghiệp).                   │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)?                                   │
│ - Giảm thời gian tiếp nhận & phân bổ từ 12 giờ ──> dưới 1 phút.         │
│ - Tỉ lệ định tuyến chính xác đến đúng bộ phận phụ trách đạt >= 95%.     │
│                                                                         │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent    │
└─────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 QUICK PROBLEM CARD #3

```text
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                   │
│                                                                         │
│ Bài toán (1 câu): Tự động trích xuất thông tin bệnh án điện tử và soạn   │
│ thảo bản Tóm tắt xuất viện (Discharge Summary) chuẩn y khoa cho bác sĩ. │
│ Công ty thành viên: [ ] Xanh SM (GSM)  [ ] VinFast  [ ] Vinhomes        │
│                     [x] Vinmec         [ ] Khác: _____________________  │
│                                                                         │
│ Ai đang đau (Actor)? Bác sĩ điều trị (kiệt sức vì thủ tục giấy tờ),     │
│ Bệnh nhân & người nhà (chờ nửa ngày để nhận giấy tờ xuất viện).         │
│                                                                         │
│ Workflow thủ công hiện tại (4 bước):                                    │
│   1. Bác sĩ rà soát toàn bộ kết quả xét nghiệm, chẩn đoán, toa thuốc EMR│
│   ──> 2. Bác sĩ gõ tay bản tóm tắt diễn biến lâm sàng và hướng dẫn về nhà│
│   ──> 3. Kiểm tra thủ công tương tác thuốc và chống chỉ định            │
│   ──> 4. In ấn, ký tươi và bàn giao cho điều dưỡng giao bệnh nhân       │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? Bước 1 & 2 (⏱ 25 - 30 phút/ca bệnh)    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1, 2 & 3                     │
│ (Trích xuất các chỉ số quan trọng, tổng hợp lịch sử điều trị, draft văn │
│  bản tóm tắt xuất viện song ngữ Anh - Việt để bác sĩ review).           │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)?                                   │
│ - Giảm thời gian hoàn thành giấy xuất viện từ 30 phút ──> dưới 5 phút.  │
│ - Tỉ lệ bác sĩ chỉ cần chỉnh sửa nhẹ (giữ nguyên > 90% draft) đạt 85%.  │
│                                                                         │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent    │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Quyết định Lựa chọn Bài toán Thực hiện Deep-Dive

Sau khi cân nhắc kỹ lưỡng giữa 3 bài toán trên, tôi chọn **Card #1 — Xanh SM: Xử lý sự cố pin xe điện thực địa & Điều phối cứu hộ di động** làm trọng tâm nghiên cứu và phát triển Prototype.

### Lý do lựa chọn Card #1:
1. **Tính cấp bách và rủi ro thực địa cao:** Sự cố pin xe điện ảnh hưởng trực tiếp đến an toàn giao thông và trải nghiệm người dùng trong thời gian thực (real-time). Khi pin dưới 5%, nếu tài xế tiếp tục cố di chuyển đến trạm sạc xa quá 5km, xe sẽ chết máy giữa lòng đường, gây ách tắc giao thông và làm hỏng ắc quy cao áp.
2. **Độ sẵn sàng công nghệ & Ranh giới an toàn rõ ràng:** Bài toán có thể giải quyết dứt điểm bằng mô hình **LLM Feature kết hợp Rule-based Guardrails**. Có thể thiết lập các ranh giới vận hành cứng: bắt buộc tag `[DRAFT_ONLY]` để kiểm soát bởi con người (HITL) và định dạng có cấu trúc JSON kích hoạt điều xe cứu hộ khi pin < 5%.
3. **Hiệu quả kinh tế tức thì (ROI cao):** Rút ngắn thời gian xử lý từ 15 phút xuống dưới 3 phút/lượt giúp đội ngũ điều phối viên phục vụ được số lượt sự cố gấp 5 lần, giải phóng công suất tài xế quay lại đón khách nhanh chóng.

### Lý do tạm hoãn các Card còn lại:
* **Card #2 (Vinhomes CSKH):** Mặc dù khối lượng lớn nhưng tính chất không khẩn cấp thời gian thực (SLA là giờ/ngày). Rủi ro pháp lý về thông tin sở hữu căn hộ cần thêm thời gian chuẩn hóa cơ sở dữ liệu phân quyền trước khi triển khai AI rộng rãi.
* **Card #3 (Vinmec Discharge Summary):** Lĩnh vực y tế đòi hỏi kiểm định lâm sàng cực kỳ khắt khe theo tiêu chuẩn JCI. Việc triển khai cần tích hợp sâu vào hệ thống EMR độc quyền và có hội đồng chuyên môn y khoa thẩm định phê duyệt trong nhiều tháng.
