# Phase 6 — AI Log & Reflection

**Hình thức:** Bài cá nhân — branch `long`.  
**Học viên:** Nguyễn Hải Long — longhello2003@gmail.com.  
**Mã HV:** 2A202602471.  
**Công cụ trong phiên chỉnh sửa:** Codex.  
**Bài toán:** Soạn nháp tóm tắt xuất viện Vinmec; bối cảnh giả lập Lab 02.

> Nhật ký ghi nhận phiên rà soát hiện tại, không tái tạo hội thoại không có trong thư mục. Bản nháp cũ nhắc Gemini/Antigravity và một số phát ngôn AI nhưng không có transcript/API log xác nhận. Nội dung dưới đây phân biệt phát hiện từ file, sửa tài liệu và kiểm thử chưa thực hiện.

## 1. AI hỗ trợ gì?

Yêu cầu thực tế của học viên là đọc toàn bộ folder và slide đang mở, sửa đúng bốn file theo README. Codex đọc hướng dẫn, worksheet, worked example, inspiration kit, starter Python, autograder và bốn file bài làm; đồng thời đọc bộ slide 21 trang trên VLearn.

AI đối chiếu từng đầu ra: 5 cơ hội theo 4 lenses, 3 Quick Cards, workflow có handoff/thời gian/bottleneck, Problem Statement 6-field, Rule/LLM/Agent, HITL, fallback và quyết định có căn cứ. Chủ đề Vinmec được giữ từ bài nháp; ví dụ code Xanh SM được phân biệt với đề xuất này.

Phần hữu ích nhất là kiểm tra tính nhất quán: bước 2–3 là 15 + 20 = 35 phút nhưng Problem Statement cũ ghi 20–25 phút. Sửa card, báo cáo và sơ đồ theo cùng phạm vi giúp biết rõ chỉ số nào đang được cải thiện.

## 2. Sai sót, thiếu chứng cứ và quá tự tin trong bản nháp

| Nội dung cũ | Vấn đề | Sửa trong bản mới |
|---|---|---|
| Tổng 70 phút nhưng bottleneck 20–25 phút; mục tiêu soạn 4 phút | Không khớp tổng các bước, thiếu phạm vi review | Bước 2–3: 35 → ≤10 phút; toàn luồng 70 → ≤45 phút nếu bước khác giữ nguyên; ghi là giả định/mục tiêu. |
| “Vinmec đã số hóa 100% … HL7/FHIR”, có dữ liệu sạch, bác sĩ sẵn sàng | Không có chứng cứ trong repo | Bỏ khẳng định; đưa vào checklist cần xác minh. |
| HITL kiểm soát rủi ro “tuyệt đối”, GO thực địa | Thiết kế trên giấy chưa chứng minh hiệu quả/an toàn | NOT YET, nêu điều kiện chuyển GO cho prototype hẹp. |
| Confidence <0.85 là điểm dừng | Chưa định nghĩa/hiệu chuẩn | Dùng kiểm tra nguồn, trường thiếu, mâu thuẫn, schema và người duyệt. |
| Nhật ký kể AI từng tự gửi hồ sơ hoặc thêm chẩn đoán và người học đã sửa | Không có transcript xác nhận | Không kể như trải nghiệm thật; chuyển thành ca test dự kiến. |

Có thể kiểm tra số học, phạm vi và nguồn của khẳng định ngay khi chưa có dữ liệu vận hành. Nhưng rà soát tài liệu không thay thế khảo sát hoặc thử mô hình.

## 3. Điều chỉnh yêu cầu và ranh giới

Yêu cầu ban đầu là làm theo README/slide. Qua rà soát, ràng buộc được cụ thể hóa: giữ chủ đề; phân biệt giả định và dữ liệu thật; chỉ ghi kết quả có chứng cứ; chọn giải pháp theo công việc; có người duyệt và đường quay lại thủ công.

Prompt đề xuất để tiếp tục rà soát (không phải transcript gọi Gemini):

```text
Đọc card và báo cáo. Chỉ ra số liệu có nguồn và số liệu giả định.
Đối chiếu tổng thời gian với từng bước và phạm vi metric.
So sánh form tự điền, Rule và LLM sau khi tính thời gian review.
Không tự xác nhận hạ tầng, dữ liệu hoặc đồng ý của stakeholder.
Chỉ đề xuất GO khi có bằng chứng đáp ứng điều kiện đã nêu.
```

Ranh giới được sửa thành nháp có nguồn, không thêm chẩn đoán/thuốc/liều, báo thiếu/mâu thuẫn và không tự phát hành. Tag `[DRAFT_FOR_PHYSICIAN_REVIEW]` phải đi cùng quyền ứng dụng: LLM không có công cụ gửi/ký; người có thẩm quyền duyệt phiên bản cụ thể. Đây là thiết kế đề xuất, chưa phải hệ thống đã xây dựng.

## 4. Kiểm chứng và giới hạn

- README và slide 19 cùng yêu cầu bốn deliverables; slide 5 yêu cầu tên nhóm, họ tên/email từng thành viên. Theo xác nhận của học viên, bản này ghi Nguyễn Hải Long — longhello2003@gmail.com và nộp trên branch cá nhân `long`.
- Kiểm tra số học: 70 phút toàn luồng, 35 phút bottleneck, mục tiêu toàn luồng 45 phút; với 20 ca/ngày tiết kiệm lý thuyết 8,33 giờ công/ngày.
- T1–T5 là **kế hoạch kiểm thử**, chưa có output Gemini hoặc pass/fail thực tế.
- Starter Python còn TODO và `NotImplementedError`. Ví dụ trong slide không chứng minh code cá nhân đã chạy; phiên này sửa bốn file báo cáo/sơ đồ.
- Autograder phần A chỉ kiểm tra file tồn tại. Qua kiểm tra này không đồng nghĩa đạt rubric nội dung, chạy thành công prototype hoặc sẵn sàng triển khai.

## 5. Reflection

AI hữu ích khi kiểm tra logic và buộc bài làm nêu rõ điều chưa biết. Metric có nhiều con số vẫn sai nếu điểm bắt đầu/kết thúc không thống nhất hoặc bỏ qua công người duyệt. NOT YET với điều kiện chuyển bước rõ ràng có căn cứ hơn GO dựa trên hạ tầng và nhu cầu tự giả định.

Người học cần kiểm tra nội dung phản ánh đúng trải nghiệm của mình, thu thập baseline được phép và chạy prototype riêng. Khi có kết quả, bổ sung prompt thực tế, output, phiên bản và lỗi quan sát; không đổi kế hoạch test thành test thành công chỉ để hoàn chỉnh bài nộp.

**Nguồn:** README, worksheet, worked example, inspiration kit, mã nguồn trong repo và [bộ slide lab](https://vlearn.dev/course/k4p1/reader?day=D03&part=day-slides-material_mtwoa1r2_fbvete&page=15).

