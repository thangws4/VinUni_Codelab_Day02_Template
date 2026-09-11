# 📋 Danh Sách Bài Toán Thực Tế Vận Hành Vingroup (Vin Smart Future)

**Kỹ sư thực hiện:** Trần Anh Quân  
**Đơn vị:** Khối Công nghệ Vin Smart Future (Vingroup)  
**Phạm vi áp dụng:** Xanh SM (GSM), VinFast, Vinhomes, Vinmec, Vinpearl  
**Mục tiêu:** Lưu trữ & chuẩn hóa danh mục các bài toán vận hành thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất để phục vụ quá trình Scoping, Prototyping và phát triển AI Products.

---

## 📊 1. Bảng Tổng Hợp Cơ Hội Qua 4 Lenses

| # | Đơn vị thành viên | Tên bài toán nghiệp vụ | Lens chính | Mức độ AI-Fit | Mức độ khả thi |
|---|---|---|---|---|---|
| **1** | **Xanh SM** | Xử lý sự cố pin xe điện thực địa & Điều phối xe cứu hộ | Tốn thời gian & Pain | LLM Feature + Guardrail | **Rất cao (Đã Prototype)** |
| **2** | **Xanh SM** | Đối soát & Xử lý khiếu nại cuốc xe lệch lộ trình / sai cước | Tốn thời gian | LLM Extraction & Rules | **Cao** |
| **3** | **Xanh SM** | Khai phá nguyên nhân hủy chuyến từ ghi âm cuộc gọi | AI-upgrade & Pain | STT + LLM Classification | **Cao** |
| **4** | **Xanh SM** | Kiểm tra ngoại quan xe & Chấm điểm vệ sinh 5 sao qua ảnh | Lặp lại & Tốn thời gian | VLM / Multimodal | **Trung bình** |
| **5** | **Xanh SM** | Dự báo nhu cầu & Điều xe đón đầu luồng khách sự kiện | AI-upgrade | Predictive ML + LLM | **Trung bình** |
| **6** | **VinFast** | Đối soát tự động hóa đơn trạm sạc đối tác nhượng quyền | Lặp lại & Tốn thời gian | OCR + Rule Reconciliation | **Rất cao** |
| **7** | **VinFast** | Chẩn đoán sơ bộ mã lỗi xe từ mô tả tiếng Việt của khách | AI-upgrade | LLM Search & RAG | **Cao** |
| **8** | **Vinhomes** | Phân loại & Định tuyến ticket phản ánh cư dân trên App | Lặp lại & Tốn thời gian | LLM Router & NER | **Rất cao** |
| **9** | **Vinmec** | Trích xuất & Soạn thảo tóm tắt hồ sơ xuất viện (Discharge Summary) | Tốn thời gian & Pain | EMR RAG + LLM Draft | **Cao (Cần HITL chặt)** |
| **10**| **Vinpearl**| Phân tích cảm xúc & Cảnh báo tức thời review đa kênh | AI-upgrade | Multilingual Sentiment LLM | **Cao** |

---

## 🚗 2. Phân Tích Chuyên Sâu 5 Bài Toán Vận Hành Xanh SM (GSM)

---

### Bài toán 1: Xử lý sự cố pin xe điện thực địa & Điều phối xe cứu hộ sạc pin

* **Actor / Operator:** Điều phối viên (Dispatcher) Trung tâm Điều vận Xanh SM & Tài xế Taxi điện (VF5, VFe34, VF8).
* **Quy trình thủ công 5 bước hiện tại:**
  1. Tài xế gọi hotline điều vận báo xe báo động mức pin nguy cấp (< 5%) hoặc nguy cơ chết máy giữa đường.
  2. Dispatcher mở phần mềm FMS tra cứu định vị toạ độ GPS của xe.
  3. Mở portal mạng lưới trạm sạc VinFast, rà soát thủ công danh sách trạm trong bán kính lân cận xem trạm nào còn trụ sạc trống đúng chuẩn cổng sạc của xe.
  4. Gõ tay địa chỉ, khoảng cách, soạn tin nhắn SMS/In-app chỉ dẫn gửi cho tài xế.
  5. Nếu xe báo pin dưới 5% và không có trạm sạc khả dụng dưới 5km, gọi hotline đối tác điều xe sạc pin di động (Mobile Charging Vehicle).
* **Điểm nghẽn (Bottlenecks):** Bước 3 & 4 tốn **12 – 15 phút/lượt**. Dispatcher bị áp lực tâm lý lớn vào giờ cao điểm, dễ chỉ định nhầm trạm sạc đang bảo trì hoặc cổng sạc không tương thích dòng xe.
* **Thống kê tổn thất & Rò rỉ hiệu suất:**
  * Tiếp nhận trung bình **70 – 90 ca sự cố pin/ngày** tại Hà Nội và TP.HCM.
  * Lãng phí **~20 – 22 giờ công lao động/ngày** của đội ngũ điều phối viên.
  * Mỗi ca xe phải dừng hoạt động trung bình **35 – 50 phút**, làm thất thoát **15% doanh thu** tiềm năng trong khung giờ cao điểm và tạo nguy cơ xe chết máy gây ách tắc giao thông đô thị.
* **Giải pháp AI & Ranh giới an toàn:**
  * *Kiến trúc:* **LLM Feature (Gemini 2.5 Flash) kết hợp Rule Guardrails**.
  * *Ranh giới an toàn:* Bắt buộc xuất tiền tố `[DRAFT_ONLY]` cho mọi văn bản hướng dẫn gửi tài xế (bắt buộc Human-in-the-loop). Khi pin < 5%, tự động cấm đề xuất trạm > 5km và xuất cấu trúc JSON: `{"action": "dispatch_mobile_charger", "reason": "<lý_do>"}`.

---

### Bài toán 2: Đối soát & Xử lý khiếu nại cuốc xe lệch lộ trình / sai cước

* **Actor / Operator:** Nhân viên Chăm sóc Khách hàng (CSKH) & Đội Kiểm soát Tuân thủ Xanh SM.
* **Quy trình thủ công hiện tại:**
  1. Khách hàng gửi ticket khiếu nại qua App: *"Tài xế đi đường vòng tính thêm tiền"* hoặc *"Mất tín hiệu GPS làm cước tăng đột biến"*.
  2. CSKH mở ticket, đăng nhập hệ thống bản đồ tra lại từng toạ độ polyline chuyến đi.
  3. So sánh lộ trình tài xế thực tế với lộ trình gợi ý tối ưu của bản đồ giao thông tại thời điểm phát sinh cuốc xe.
  4. Nhận định nguyên nhân (do tắc đường, rào chắn công trình hay tài xế cố ý gian lận), tính toán lại số tiền chênh lệch, soạn email giải trình và gửi mã voucher bồi hoàn.
* **Điểm nghẽn (Bottlenecks):** Tra cứu, đối chiếu và so khớp bản đồ thủ công tốn **12 – 18 phút/ticket**.
* **Thống kê tổn thất & Rò rỉ hiệu suất:**
  * Tiếp nhận khoảng **1.200 – 1.500 khiếu nại cước/tuần**.
  * Tiêu tốn **~300 giờ làm việc/tuần** của bộ phận CSKH.
  * Chậm trễ SLA (mất 24 – 48 giờ để xử lý), dẫn đến **tỷ lệ rời bỏ dịch vụ của khách hàng VIP tăng 4%**, chi phí phát voucher đền bù trễ hạn ước tính **150 – 200 triệu VNĐ/tháng**.
* **Giải pháp AI:** LLM Feature trích xuất thông tin cuốc xe, phân tích lịch sử giao thông tự động, phân loại: *Hợp lý (kẹt xe)* vs *Bất thường (đi vòng)* và draft phương án bồi hoàn chuẩn hóa trong 30 giây.

---

### Bài toán 3: Khai phá nguyên nhân hủy chuyến từ ghi âm cuộc gọi & ghi chú tài xế

* **Actor / Operator:** Đội ngũ Quản lý Chất lượng Dịch vụ (QA/QC) & Trưởng nhóm Vận hành Đội xe.
* **Quy trình thủ công hiện tại:**
  1. Cuốc xe bị hủy (Cancel Trip) với lý do chọn sẵn trên ứng dụng (ví dụ: *"Đổi ý"*, *"Chờ lâu"*).
  2. Nhân viên QA chọn mẫu ngẫu nhiên (chỉ nghe lại được 2% – 3% tổng số ca hủy), mở file ghi âm cuộc gọi giữa tài xế và khách hàng.
  3. Nghe lại hội thoại, ghi chú lý do thực tế (tài xế ép khách hủy để chạy ngoài, xe sắp hết pin, khách giục gấp...).
  4. Nhập liệu thủ công vào bảng tính Excel để tổng hợp báo cáo hàng tuần.
* **Điểm nghẽn (Bottlenecks):** Nghe và tóm tắt thủ công tốn **8 – 10 phút/cuộc gọi**. Đánh giá chủ quan và bỏ sót 97% dữ liệu cuộc gọi còn lại.
* **Thống kê tổn thất & Rò rỉ hiệu suất:**
  * Tỷ lệ hủy chuyến giờ cao điểm dao động từ **8% – 12%** (tương đương **2.500 – 4.000 cuốc xe bị hủy/ngày**).
  * Do không phát hiện sớm các "điểm đen" đón khách hoặc tài xế cố tình gian lận, Xanh SM thất thoát ước tính **300 – 500 triệu VNĐ doanh thu/ngày**.
* **Giải pháp AI:** Kết hợp Speech-to-Text (VinBase / Whisper) chuyển âm thanh thành văn bản $\rightarrow$ LLM tự động trích xuất nguyên nhân gốc rễ (Root Cause) theo thời gian thực $\rightarrow$ gửi cảnh báo ngay cho quản lý đội xe.

---

### Bài toán 4: Kiểm tra ngoại quan xe & Chấm điểm vệ sinh 5 sao qua hình ảnh

* **Actor / Operator:** Quản lý Đội xe (Hub Supervisor) & Tài xế giao ca.
* **Quy trình thủ công hiện tại:**
  1. Đầu ca và cuối ca làm việc, tài xế chụp 6 góc ảnh (ngoại thất 4 góc, hàng ghế trước, hàng ghế sau) tải lên portal nội bộ.
  2. Quản lý Hub phải kiểm tra bằng mắt hàng trăm album ảnh mỗi ngày để tìm vết trầy xước, vết móp mới hoặc vết bẩn nội thất.
  3. Nhập tay điểm đánh giá vào phần mềm quản trị để tính thưởng/phạt KPI tài xế.
* **Điểm nghẽn (Bottlenecks):** Quản lý phải duyệt **300 – 500 lượt xe/hub/ngày**, dẫn đến tình trạng duyệt lướt, bỏ sót lỗi va quẹt mới phát sinh.
* **Thống kê tổn thất & Rò rỉ hiệu suất:**
  * Mỗi quản lý hub mất **3 – 4 giờ/ngày** chỉ để soi ảnh.
  * Khoảng **15% vết trầy xước nhẹ không được ghi nhận kịp thời**, dẫn đến Xanh SM không thể yêu cầu đơn vị bảo hiểm chi trả mà phải tự gánh chi phí sơn sửa dồn tích từ **60 – 80 triệu VNĐ/tháng cho mỗi Hub**.
* **Giải pháp AI:** Vision-Language Model (VLM Multimodal) tự động so sánh ảnh xe hiện tại với ảnh lưu kho của ngày hôm trước để phát hiện vết móp, vết xước và phân loại độ sạch nội thất trong vòng 3 giây.

---

### Bài toán 5: Dự báo nhu cầu & Điều xe đón đầu luồng khách tại sự kiện / điểm nóng

* **Actor / Operator:** Trưởng ca Điều phối Vận hành (Operations Lead).
* **Quy trình thủ công hiện tại:**
  1. Khi có sự kiện quy mô lớn (Concert ca nhạc tại Ocean Park, triển lãm, giờ tan tầm trời mưa bão), trưởng ca tự ước lượng nhu cầu theo cảm tính.
  2. Nhắn tin vào hàng chục nhóm chat Zalo/Telegram nội bộ kêu gọi tài xế di chuyển về khu vực sự kiện.
  3. Tài xế di chuyển tự phát, không có thông tin về số lượng xe đã có mặt tại hiện trường.
* **Điểm nghẽn (Bottlenecks):** Ra quyết định cảm tính, độ trễ truyền đạt qua nhóm chat mất **20 – 30 phút**, không nắm được lượng pin còn lại của các xe điều đến.
* **Thống kê tổn thất & Rò rỉ hiệu suất:**
  * Tỷ lệ xe chạy rỗng (Deadhead Miles) quanh khu vực sự kiện lên tới **28% – 35%**, hao tốn pin và giảm thu nhập tài xế.
  * Tỷ lệ bỏ lỡ nhu cầu khách gọi xe (Unmet Demand) đạt **30% – 40%**, rò rỉ khoảng **600 – 1.000 cuốc xe/sự kiện lớn**.
* **Giải pháp AI:** Predictive ML kết hợp LLM Dispatch Copilot: Tự động phân tích lịch sự kiện, thời tiết, vị trí GPS và dung lượng pin của đội xe để gửi thông báo điều xe thông minh theo từng cụm Geohash trực tiếp trên ứng dụng tài xế.

---

## 🏢 3. Danh Sách Bài Toán Mở Rộng Các Đơn Vị Thành Viên Khác

### 1. VinFast (Sản Xuất & Hạ Tầng Xe Điện)
* **Đối soát hóa đơn trạm sạc đối tác nhượng quyền:**
  * *Nghiệp vụ:* So khớp 50.000 logs sạc điện hàng tháng từ các cây xăng / trạm sạc tư nhân liên kết với hóa đơn gửi về.
  * *Tổn thất:* Kế toán mất 4 ngày làm việc mỗi tháng; rò rỉ 1.5% – 2% doanh thu do sai lệch số đo công tơ điện.
  * *AI Fit:* OCR hóa đơn + LLM Reconciliation Script.
* **Chẩn đoán ban đầu từ mô tả ngôn ngữ tự nhiên của khách hàng:**
  * *Nghiệp vụ:* Khách hàng mô tả hiện tượng xe (ví dụ: *"xe đi qua gờ giảm tốc kêu cụp cụp ở bánh trước"*). Cố vấn dịch vụ mất 15 phút mở tài liệu kỹ thuật tra mã lỗi.
  * *Tổn thất:* Trễ hẹn tiếp nhận xe tại xưởng dịch vụ, tăng 20% thời gian chẩn đoán ban đầu.
  * *AI Fit:* Semantic Search + RAG tra cứu cẩm nang kỹ thuật VinFast.

### 2. Vinhomes (Khu Đô Thị Thông Minh)
* **Phân loại & Định tuyến ticket phản ánh cư dân trên App:**
  * *Nghiệp vụ:* CSKH đọc hơn 600 phản ánh/ngày (hỏng đèn hành lang, tiếng ồn, đăng ký sửa chữa, thủ tục cấp thẻ xe) rồi gõ email chuyển tới từng Ban Quản lý tòa nhà.
  * *Tổn thất:* Tốn 8–10 phút/ticket; trễ hạn SLA phản hồi từ 12 đến 24 giờ.
  * *AI Fit:* LLM Classifier & NER (Named Entity Recognition) trích xuất mã căn hộ, hạng mục lỗi và route tự động trong 1 giây.

### 3. Vinmec (Hệ Thống Y Tế Hàn Lâm)
* **Tự động soạn thảo bản Tóm tắt hồ sơ xuất viện (Discharge Summary):**
  * *Nghiệp vụ:* Bác sĩ rà soát toàn bộ kết quả xét nghiệm máu, chẩn đoán hình ảnh, diễn biến lâm sàng trong EMR để gõ bản tóm tắt xuất viện bằng 2 thứ tiếng Anh - Việt.
  * *Tổn thất:* Bác sĩ mất 25–30 phút/bệnh nhân, gây quá tải giờ hành chính và bệnh nhân phải chờ xuất viện nửa ngày.
  * *AI Fit:* LLM Summarization kết hợp cơ sở dữ liệu EMR chuẩn HL7/FHIR, bắt buộc bác sĩ review & ký số (HITL).

### 4. Vinpearl (Khách Sạn & Nghỉ Dưỡng)
* **Phân tích cảm xúc & Cảnh báo tức thời đánh giá đa kênh:**
  * *Nghiệp vụ:* Khách hàng review trên TripAdvisor, Booking.com, Agoda, Google Maps. Nhân viên marketing tổng hợp thủ công vào bảng tính mỗi tuần.
  * *Tổn thất:* Bỏ lỡ "thời điểm vàng" 2 giờ đầu để xử lý khủng hoảng dịch vụ (phòng bẩn, thái độ nhân viên), làm giảm điểm xếp hạng của resort.
  * *AI Fit:* LLM Real-time Sentiment Analysis & Alerting System.

---

## 🎯 4. Tiêu Chí Đánh Giá Lựa Chọn Dự Án (Scoping Scorecard)

Khi lựa chọn 1 bài toán để phát triển AI Prototype tại Vin Smart Future, áp dụng bộ tiêu chí sàng lọc 4 yếu tố:

1. **Impact (Tác động kinh doanh):** Giảm thời gian xử lý thủ công ít nhất 70% hoặc ngăn chặn tổn thất tài chính trực tiếp.
2. **Feasibility (Độ khả thi kỹ thuật):** Có sẵn dữ liệu/API sạch (như GPS, Telematics, EMR, Logs) để mô hình tiêu thụ.
3. **Safety Boundaries (Ranh giới kiểm soát):** Có thể thiết lập cơ chế Human-in-the-loop (HITL) hoặc Fallback rõ ràng; sai số của AI không gây nguy hiểm tính mạng hoặc vi phạm pháp lý.
4. **Time-to-Prototype:** Có thể xây dựng bản mẫu kỹ thuật (Prompt Prototype / Streamlit) chứng minh năng lực trong vòng 1-2 tuần.

