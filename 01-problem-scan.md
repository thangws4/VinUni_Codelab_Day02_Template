# Họ và tên : Ngô Tiến Dũng

# Mã học viên : 2A202602374

# Email :26ai.dungnt8@vinuni.edu.vn

# 🔍 Phase 1 — SCAN: VinFast, mảng ô tô

> **Phạm vi lựa chọn:** vận hành dịch vụ sau bán hàng và tiếp nhận bảo hành/sửa chữa xe VinFast. Các số liệu thời gian, chất lượng bên dưới là giả định để scoping; cần xác thực bằng log tổng đài, ticket dịch vụ và dữ liệu xưởng trước khi triển khai.

|   # | Subsidiary  | Lens               | Mô tả ngắn bài toán                                                                                                                                                                                                     |
| --: | ----------- | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   1 | **VinFast** | AI có thể tốt hơn  | **Chẩn đoán lỗi xe từ mô tả tiếng Việt của khách:** khách mô tả tự do như “xe đi qua gờ giảm tốc kêu cụp cụp ở bánh trước”; hệ thống đề xuất nhóm hệ thống liên quan và mã lỗi sơ bộ để CSKH/xưởng tiếp nhận nhanh hơn. |
|   2 | **VinFast** | Tốn thời gian      | Nhân viên CSKH đọc ticket, nghe ghi âm cuộc gọi và tra sổ tay kỹ thuật để xác định khách nên đặt lịch xưởng, hỗ trợ từ xa hay gọi cứu hộ.                                                                               |
|   3 | **VinFast** | Lặp lại            | Cố vấn dịch vụ phải nhập lại thông tin xe, lịch sử bảo dưỡng và triệu chứng từ nhiều kênh (app, hotline, đại lý) vào phiếu sửa chữa.                                                                                    |
|   4 | **VinFast** | Pain từ người khác | Khách hàng không biết tình trạng xe/lịch sửa chữa sau khi gửi xe vào xưởng; cố vấn dịch vụ trả lời lặp lại các câu hỏi về tiến độ và chi phí dự kiến.                                                                   |
|   5 | **VinFast** | Lặp lại            | Kỹ thuật viên ghi chú sửa chữa bằng ngôn ngữ tự do; bộ phận chất lượng phải đọc và tổng hợp thủ công các lỗi lặp lại theo dòng xe, khu vực và thời gian.                                                                |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

**Top 3 lựa chọn:** #1 (chẩn đoán sơ bộ từ mô tả khách), #2 (triage yêu cầu hỗ trợ), #5 (tổng hợp lỗi lặp lại từ ghi chú kỹ thuật).

## QUICK PROBLEM CARD #1 — Chẩn đoán sơ bộ lỗi xe từ mô tả tiếng Việt

| Hạng mục                        | Nội dung                                                                                                                                                                                                                                                                           |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Bài toán (1 câu)**            | Chuẩn hóa mô tả triệu chứng tiếng Việt của khách thành nhóm hệ thống và mã lỗi kỹ thuật **sơ bộ** để rút ngắn tiếp nhận tại CSKH/xưởng.                                                                                                                                            |
| **Công ty thành viên**          | **[x] VinFast**                                                                                                                                                                                                                                                                    |
| **Ai đang đau (Actor)?**        | Khách hàng mô tả khó chính xác; nhân viên CSKH/cố vấn dịch vụ mất thời gian hỏi lại; kỹ thuật viên nhận phiếu thiếu thông tin để chuẩn bị chẩn đoán.                                                                                                                               |
| **Workflow hiện tại**           | 1. Khách gọi hotline/tạo ticket trên app → 2. CSKH ghi mô tả tự do → 3. Cố vấn hỏi thêm về dòng xe, thời điểm xảy ra, đèn cảnh báo và mức độ an toàn → 4. Tra sổ tay/mã lỗi hoặc chuyển kỹ thuật viên → 5. Đặt lịch hoặc điều phối cứu hộ khi được phê duyệt.                      |
| **Bước tốn thời gian/lỗi nhất** | Bước 3–4, ước tính **8–12 phút/ticket**; cùng một lỗi có nhiều cách nói (“rung”, “giật”, “kêu cụp cụp”) nên dễ route sai nhóm kỹ thuật.                                                                                                                                            |
| **AI hỗ trợ ở đâu?**            | LLM trích xuất triệu chứng, bộ phận xe, điều kiện xuất hiện và mức khẩn cấp; RAG tra sổ tay kỹ thuật theo dòng xe/VIN; trả về 1–3 nhóm lỗi khả dĩ, câu hỏi làm rõ và bản nháp phiếu tiếp nhận.                                                                                     |
| **Metric thành công**           | Trong pilot, ≥ **85%** ticket được gán đúng _nhóm hệ thống_ trong top-3; giảm median thời gian tiếp nhận xuống **≤ 4 phút/ticket**; ≥ **95%** case có dấu hiệu an toàn cao được gắn cờ để chuyển người phụ trách ngay.                                                             |
| **Quick Architecture**          | **[x] LLM feature + RAG + rule an toàn.** Rule xử lý VIN/dòng xe và các từ khóa nguy hiểm; LLM chỉ hiểu ngôn ngữ, đặt câu hỏi, tạo nháp.                                                                                                                                           |
| **Ranh giới vận hành**          | AI **không** xác nhận nguyên nhân cuối cùng, không hướng dẫn sửa chữa, không xóa mã lỗi, không tự đặt lịch/cứu hộ. Với cảnh báo phanh, lái, pin cao áp, va chạm hoặc độ tin cậy thấp: hiển thị cảnh báo, chuyển CSKH/kỹ thuật viên; người có thẩm quyền quyết định bước tiếp theo. |

## QUICK PROBLEM CARD #2 — Triage yêu cầu hỗ trợ sau bán hàng

| Hạng mục                        | Nội dung                                                                                                                                                                                                |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Bài toán (1 câu)**            | Phân loại và ưu tiên ticket dịch vụ để khách có tình huống khẩn cấp đến đúng nhóm hỗ trợ trong thời gian ngắn.                                                                                          |
| **Công ty thành viên**          | **[x] VinFast**                                                                                                                                                                                         |
| **Ai đang đau (Actor)?**        | CSKH phải xử lý lượng ticket không đồng đều; khách chờ lâu khi case bị chuyển nhầm; nhóm cứu hộ/xưởng nhận thiếu bối cảnh.                                                                              |
| **Workflow hiện tại**           | 1. Nhận yêu cầu từ hotline/app/email → 2. CSKH đọc nội dung và tra loại xe → 3. Gán thủ công vào bảo hành, lịch xưởng, cứu hộ hoặc hỗ trợ từ xa → 4. Nhập ghi chú → 5. Theo dõi SLA và nhắc nhóm xử lý. |
| **Bước tốn thời gian/lỗi nhất** | Bước 2–3, ước tính **3–6 phút/ticket**; tin nhắn ngắn, viết tắt hoặc có nhiều vấn đề khiến ưu tiên/đội nhận bị sai.                                                                                     |
| **AI hỗ trợ ở đâu?**            | Mô hình phân loại ý định và mức ưu tiên, tóm tắt ticket theo cấu trúc, đề xuất queue và SLA; agent CSKH duyệt trước khi chuyển.                                                                         |
| **Metric thành công**           | ≥ **90%** ticket được đề xuất đúng queue; ≥ **95%** ticket ưu tiên cao được review trong **≤ 2 phút**; giảm tỷ lệ chuyển lại queue **≥ 30%** so với baseline.                                           |
| **Quick Architecture**          | **[x] Rule + LLM feature.** Rule ưu tiên an toàn/SLA; LLM chuẩn hóa nội dung. Không dùng agent tự động đóng ticket hoặc cam kết chi phí với khách.                                                      |

## QUICK PROBLEM CARD #3 — Phát hiện lỗi lặp lại từ ghi chú kỹ thuật

| Hạng mục                        | Nội dung                                                                                                                                                                                                                  |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Bài toán (1 câu)**            | Tổng hợp ghi chú sửa chữa tự do để phát hiện sớm xu hướng lỗi theo dòng xe, cụm linh kiện, khu vực và thời gian.                                                                                                          |
| **Công ty thành viên**          | **[x] VinFast**                                                                                                                                                                                                           |
| **Ai đang đau (Actor)?**        | Kỹ thuật viên viết ghi chú; bộ phận chất lượng/bảo hành phải tự đọc và lập báo cáo; quản lý nhận tín hiệu muộn về lỗi có thể lặp lại.                                                                                     |
| **Workflow hiện tại**           | 1. Kỹ thuật viên hoàn tất phiếu → 2. Nhập mô tả triệu chứng, chẩn đoán và thao tác sửa chữa → 3. Analyst xuất dữ liệu định kỳ → 4. Đọc, chuẩn hóa mã/từ khóa trong Excel → 5. Tổng hợp báo cáo và gửi bộ phận chất lượng. |
| **Bước tốn thời gian/lỗi nhất** | Bước 3–4, ước tính **1–2 phút/phiếu**; cách ghi không đồng nhất làm bỏ sót các cụm lỗi có tần suất tăng.                                                                                                                  |
| **AI hỗ trợ ở đâu?**            | LLM chuẩn hóa triệu chứng–nguyên nhân–khắc phục thành taxonomy; dashboard dùng rule thống kê để cảnh báo xu hướng bất thường; analyst kiểm tra các cụm có tác động cao.                                                   |
| **Metric thành công**           | ≥ **90%** ghi chú được chuẩn hóa trong **≤ 15 giây**; macro-F1 **≥ 0,85** trên tập phiếu đã gán nhãn; báo cáo xu hướng ngày có trước **09:00** thay vì tổng hợp thủ công theo tuần.                                       |
| **Quick Architecture**          | **[x] LLM feature + analytics rules.** LLM không được tự kết luận lỗi an toàn/ra quyết định triệu hồi; mọi cảnh báo phải được kỹ sư chất lượng xác nhận.                                                                  |

---

## Quyết định scoping ban đầu

**Ưu tiên Deep-Dive: Card #1 — Chẩn đoán sơ bộ lỗi xe từ mô tả tiếng Việt của khách.** Bài toán có vấn đề ngôn ngữ rõ ràng, có thể giới hạn output theo tài liệu kỹ thuật đã phê duyệt và mang lại giá trị cho cả khách hàng, CSKH lẫn xưởng.

Pilot nên bắt đầu với một nhóm lỗi ít rủi ro (ví dụ: tiếng ồn, điều hòa, tiện nghi) và luôn có người duyệt. Các trường hợp liên quan đến phanh, lái, pin cao áp, va chạm, cháy/nóng bất thường hoặc AI không đủ tự tin phải **fallback** sang quy trình tiếp nhận khẩn cấp thủ công.
