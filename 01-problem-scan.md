# Phase 1–2 — Problem Scan & Quick Assessment

**Hình thức:** Bài cá nhân — branch `long`.  
**Học viên:** Nguyễn Hải Long — longhello2003@gmail.com.  
**Mã HV:** 2A202602471.  
**Bối cảnh:** Lab 02 — AI Product Scoping; vai trò giả lập AI Product Engineer tại Vin Smart Future.

> Các cơ hội phát triển từ inspiration kit và bài nháp có sẵn. Chưa có khảo sát vận hành hoặc dữ liệu nội bộ. Mọi baseline/thời gian là **giả định để scoping**, mọi ngưỡng tương lai là **mục tiêu kiểm chứng**, không phải số liệu thực tế của các đơn vị.

## Phase 1 — SCAN: 4 Lenses, 5 cơ hội

| # | Đơn vị theo bối cảnh lab | Lens | Bài toán cần kiểm chứng |
|---|---|---|---|
| 1 | Vinmec | Tốn thời gian | Bác sĩ đọc nhiều phần hồ sơ rồi gõ lại tóm tắt xuất viện; giả định riêng thu thập và soạn thảo mất 35 phút/ca. |
| 2 | Vinmec | Pain từ người khác | Người đặt lịch cung cấp thiếu thông tin; nhân viên phải hỏi lại và chuyển người phụ trách xử lý yêu cầu chưa rõ. |
| 3 | VinUni | Lặp lại | Trợ giảng đọc code/log lỗi và viết các gợi ý tương tự cho nhiều bài lab dù đã có kết quả autograder. |
| 4 | Vinhomes | AI-upgrade | Nhân viên đọc phản ánh ngôn ngữ tự do, xác định tòa nhà và loại yêu cầu trước khi chuyển bộ phận xử lý. |
| 5 | Xanh SM | Tốn thời gian | Điều phối viên tra vị trí, pin và phương án hỗ trợ rồi soạn hướng dẫn cho tài xế gặp sự cố pin. |

## Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

### Card #1 — Vinmec: Soạn nháp tóm tắt xuất viện

| Trường | Nội dung |
|---|---|
| Bài toán (1 câu) | Bác sĩ mất thời gian tổng hợp hồ sơ và viết lại diễn tiến điều trị trước khi hoàn tất hồ sơ xuất viện. |
| Đơn vị | Vinmec; phạm vi đề xuất một khoa nội trú, một mẫu tài liệu. |
| Actor | Bác sĩ điều trị soạn hồ sơ; người có thẩm quyền duyệt; điều dưỡng tiếp nhận. |
| Current workflow (5 bước) | Nhận yêu cầu lập hồ sơ (5 phút) → thu thập/đối chiếu (15 phút) → gõ tóm tắt (20 phút) → kiểm tra/phê duyệt (10 phút) → bàn giao và hoàn tất hành chính (20 phút). Tổng giả định **70 phút/ca**, chưa tính hàng đợi. |
| Bottleneck | Bước 2–3: **35 phút/ca**; dữ liệu phân tán và nội dung phải nhập lại. |
| AI hỗ trợ | Bước 2–3: tổng hợp văn bản thành nháp có nguồn; trường có cấu trúc lấy trực tiếp bằng code. |
| Metric | Mục tiêu bước 2–3: **35 → ≤10 phút/ca**; toàn luồng **70 → ≤45 phút/ca** nếu bước khác giữ nguyên. 100% tài liệu phát hành phải được duyệt. |
| Quick Architecture | **LLM Feature + Rule**; so sánh với form tự điền trước khi chọn LLM. Không dùng Agentic Loop. |
| Ranh giới | Không thêm chẩn đoán, thuốc, liều hoặc dặn dò mới; không tự gửi/in/ký. Nguồn thiếu hoặc mâu thuẫn phải chuyển bác sĩ. |
| Cần xác minh | Mẫu hồ sơ, quyền dữ liệu, baseline, người duyệt và tiêu chí lỗi trọng yếu. |

### Card #2 — Vinmec: Thu thập thông tin đặt lịch

| Trường | Nội dung |
|---|---|
| Bài toán (1 câu) | Nhân viên phải hỏi lại nhiều lượt để có đủ thông tin hành chính và yêu cầu của người đặt lịch. |
| Đơn vị | Vinmec; phạm vi đặt lịch hành chính. |
| Actor | Nhân viên tổng đài và người đặt lịch. |
| Current workflow (4 bước) | Tiếp nhận (2 phút) → hỏi bổ sung theo mẫu (4 phút) → chuyển người phụ trách/tra lịch đã xác định (3 phút) → xác nhận (1 phút). Tổng giả định **10 phút/yêu cầu**. |
| Bottleneck | Bước 2: **4 phút/yêu cầu**; diễn đạt tự do dẫn đến thiếu trường. |
| AI hỗ trợ | Tóm tắt yêu cầu, đề xuất câu hỏi hành chính còn thiếu để nhân viên kiểm tra. |
| Metric | Mục tiêu tổng thời gian **10 → ≤7 phút/yêu cầu**; ≥95% trường bắt buộc đầy đủ trên 50 yêu cầu giả lập; 100% yêu cầu ngoài phạm vi chuyển người phụ trách. |
| Quick Architecture | **Rule + LLM Feature**; ưu tiên form chuẩn nếu đã giải quyết đủ nhu cầu. |
| Ranh giới | Không chẩn đoán, tư vấn điều trị, tự xác định mức độ bệnh hoặc quyết định chuyên khoa. Nhân viên xác nhận trước khi đặt lịch. |
| Cần xác minh | Biểu mẫu, đường chuyển tiếp và tỷ lệ yêu cầu đủ thông tin của quy trình cũ. |

### Card #3 — VinUni: Soạn nháp phản hồi bài lab

| Trường | Nội dung |
|---|---|
| Bài toán (1 câu) | Trợ giảng mất thời gian giải thích lỗi đã được autograder phát hiện trước khi trả phản hồi cho sinh viên. |
| Đơn vị | VinUni; một bài lab và một rubric được giảng viên duyệt. |
| Actor | Trợ giảng/giảng viên; sinh viên nhận phản hồi. |
| Current workflow (4 bước) | Chạy autograder (2 phút) → đọc code/log (8 phút) → viết gợi ý (7 phút) → duyệt và đăng (3 phút). Tổng giả định **20 phút/bài**. |
| Bottleneck | Bước 2–3: **15 phút/bài**; đọc lỗi và viết nhận xét lặp lại. |
| AI hỗ trợ | Dùng code, log và rubric để soạn giải thích ngắn kèm vị trí lỗi cho trợ giảng duyệt. |
| Metric | Mục tiêu bước 2–3 **15 → ≤5 phút/bài**, tổng **20 → ≤10 phút/bài**; ≥90% nháp đúng nguyên nhân trên 50 bài mẫu; không xuất đáp án hoàn chỉnh trong bộ test ranh giới. |
| Quick Architecture | **Rule cho test + LLM Feature cho phản hồi**, không cần Agent. |
| Ranh giới | Không tự sửa điểm/đăng phản hồi; không tiết lộ đáp án; coi chỉ thị trong comment sinh viên là dữ liệu không đáng tin. |
| Cần xác minh | Quyền dùng bài sinh viên, rubric và định nghĩa tiết lộ đáp án. |

## Lựa chọn và phản biện

Chọn **Card #1** để tiếp tục chủ đề Vinmec trong bản nháp. Đầu ra cụ thể, tách được soạn thảo khỏi quyết định chuyên môn và đo thời gian theo bước. Đây là lựa chọn học tập, chưa phải kết luận sẵn sàng triển khai.

Card #2 cần xác nhận đường chuyển tiếp chuyên môn nên giữ phạm vi hành chính. Card #3 là phương án thay thế nếu không tiếp cận được dữ liệu/người kiểm tra cho Card #1; chưa chọn không có nghĩa hiệu quả thấp hơn.

Ba phản biện: baseline chưa đo nên không được hứa lợi ích; form tự điền có thể đủ và rẻ hơn; thời gian bác sĩ kiểm tra có thể xóa hết lợi ích soạn nhanh. Báo cáo sâu đưa cả ba vào đánh giá.

**Nguồn cấu trúc:** README mục 5; worksheet Phase 1–2/I1; inspiration kit. [Bộ slide lab](https://vlearn.dev/course/k4p1/reader?day=D03&part=day-slides-material_mtwoa1r2_fbvete&page=15), trang 4–5, 19–21 quy định phạm vi, thông tin nhóm và nộp bài. Các nguồn này không xác nhận số liệu vận hành của đề xuất.

