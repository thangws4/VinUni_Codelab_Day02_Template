# 📝 AI Log & Reflection — VinFast Symptom Intake Assistant

## 1. Mục tiêu sử dụng AI

Tôi dùng AI như một **thought partner** trong Lab AI Product Scoping, không dùng AI để thay thế quyết định kỹ thuật hoặc quyết định an toàn. Bài toán tôi chọn là: hỗ trợ CSKH/cố vấn dịch vụ VinFast hiểu mô tả tiếng Việt của khách hàng (ví dụ: “xe qua gờ giảm tốc kêu cụp cụp ở bánh trước”) để tạo **phiếu tiếp nhận sơ bộ**.

Mục tiêu của tôi là làm rõ quy trình hiện tại, tìm bottleneck, chọn metric có thể đo được và xác định ranh giới: AI chỉ đề xuất/soạn nháp; kỹ thuật viên mới là người xác nhận lỗi thực tế.

## 2. AI đã hỗ trợ tôi như thế nào

| Hoạt động | Prompt/ý định tôi dùng | AI hỗ trợ được gì | Phần tôi tự quyết định/kiểm tra |
|---|---|---|---|
| Brainstorm cơ hội | “Gợi ý các bottleneck trong dịch vụ sau bán hàng xe điện VinFast theo 4 lenses.” | Đề xuất các hướng như triage ticket, tổng hợp ghi chú kỹ thuật, theo dõi tiến độ sửa chữa. | Tôi chỉ giữ các bài toán có workflow rõ và liên quan trực tiếp đến tiếp nhận xe. |
| Chọn bài toán | “So sánh bài toán nào phù hợp LLM hơn rule-based.” | Chỉ ra rằng mô tả triệu chứng là dữ liệu ngôn ngữ tự do, phù hợp với LLM/RAG hơn việc chỉ dùng từ khóa. | Tôi loại ý tưởng để AI tự chẩn đoán hoặc tự điều phối cứu hộ vì rủi ro an toàn quá cao. |
| Vẽ workflow | “Mô tả quy trình tiếp nhận lỗi xe hiện tại theo actor, handoff, thời gian và bottleneck.” | Giúp tách quy trình thành 5 bước: nhận yêu cầu, ghi mô tả, hỏi lại, tra tài liệu, tạo phiếu/chuyển xử lý. | Tôi đánh dấu bước hỏi lại và tra tài liệu là bottleneck; mọi mốc thời gian được ghi là giả định cần đo lại. |
| Thiết kế future flow | “Đề xuất luồng LLM có Human-in-the-loop và fallback cho ticket lỗi xe.” | Gợi ý trích xuất triệu chứng, truy xuất tài liệu theo VIN/dòng xe, tạo top-3 nhóm hệ thống và câu hỏi làm rõ. | Tôi thêm rule an toàn, giới hạn `DRAFT_ONLY` và yêu cầu CSKH/cố vấn duyệt trước khi chuyển phiếu. |
| Đánh giá tính khả thi | “Đóng vai trò CFO và Operations Lead, phản biện metric, chi phí và lý do rule-based có thể tốt hơn.” | Nhắc tôi về chi phí làm sạch dữ liệu, kiểm soát phiên bản tài liệu kỹ thuật và sự cần thiết của baseline. | Tôi chọn **NOT YET** thay vì GO vì hiện chưa có bằng chứng về dữ liệu sạch, nhãn kỹ thuật và sự sẵn sàng của stakeholder. |

## 3. Một ví dụ AI trả lời chưa phù hợp / có nguy cơ hallucination

Trong brainstorm ban đầu, AI có xu hướng gợi ý các con số như thời gian xử lý, tỷ lệ ticket và mức tiết kiệm chi phí như thể đó là số liệu vận hành thực tế của VinFast. AI cũng có thể suy diễn từ triệu chứng “kêu cụp cụp” sang một mã lỗi hoặc linh kiện cụ thể.

Hai điểm này không đủ căn cứ để dùng trong báo cáo hay vận hành:

- Không có quyền truy cập vào dữ liệu nội bộ VinFast, nên AI không thể khẳng định volume ticket, thời gian xử lý hoặc tỷ lệ lỗi thực tế.
- Một mô tả tiếng Việt rất ngắn không đủ để kết luận nguyên nhân kỹ thuật. Cùng một tiếng kêu có thể phụ thuộc dòng xe, điều kiện đường, tốc độ, lịch sử sửa chữa và kết quả kiểm tra thực tế.

Vì vậy, tôi đã sửa bài làm theo ba nguyên tắc:

1. Gắn rõ các con số như 12–17 phút/ticket, 85% top-3 accuracy là **giả định/mục tiêu pilot**, không phải fact.
2. Đổi ngôn ngữ từ “AI chẩn đoán mã lỗi” thành “AI đề xuất nhóm hệ thống/mã lỗi sơ bộ, câu hỏi làm rõ và bản nháp phiếu”.
3. Không cho AI đưa hướng dẫn tự sửa hay quyết định về an toàn. Các dấu hiệu phanh, lái, pin cao áp, va chạm, cháy/nóng bất thường và mất công suất phải fallback sang nhân viên theo SOP.

## 4. Cách tôi cải thiện prompt và ranh giới

Prompt ban đầu của tôi còn quá rộng: “Hãy chẩn đoán lỗi xe từ mô tả của khách.” Prompt này dễ khiến mô hình trả lời quá tự tin và vượt phạm vi.

Tôi sửa thành hướng dẫn cụ thể hơn:

```text
Bạn là trợ lý tiếp nhận dịch vụ VinFast. Nhiệm vụ là chuẩn hóa mô tả
triệu chứng thành dữ liệu có cấu trúc cho nhân viên CSKH/cố vấn dịch vụ.

Chỉ được dùng tài liệu kỹ thuật được truy xuất theo VIN/dòng xe. Output phải
ở trạng thái DRAFT_ONLY, gồm: triệu chứng, điều kiện xuất hiện, thông tin còn
thiếu, tối đa 3 nhóm hệ thống cần kiểm tra và mức độ cần nhân viên review.

Không được xác nhận nguyên nhân cuối cùng, không tạo hướng dẫn tự sửa, không
tự đặt lịch/cứu hộ/gửi tin. Nếu có dấu hiệu phanh, lái, pin cao áp, va chạm,
cháy/nóng bất thường, mất công suất; hoặc không có tài liệu phù hợp, trả về
ESCALATE_TO_HUMAN thay vì gợi ý chẩn đoán.
```

Việc sửa prompt cho thấy chất lượng không chỉ nằm ở câu trả lời “hay”, mà ở output có giới hạn rõ, có thể kiểm tra được và an toàn khi đưa vào workflow thật.

## 5. Bài học rút ra

- AI hữu ích nhất ở phần **hiểu và chuẩn hóa ngôn ngữ tự do**, không phải ở việc thay kỹ thuật viên kết luận lỗi.
- Rule-based và LLM cần phối hợp: rule kiểm soát an toàn, định tuyến và dữ liệu bắt buộc; LLM hỗ trợ diễn giải mô tả và tạo nháp.
- Metric phải gắn với dữ liệu kiểm chứng: top-3 accuracy cần kỹ sư gán nhãn; thời gian xử lý và tỷ lệ chuyển sai cần log vận hành trước/sau pilot.
- Với lĩnh vực xe và an toàn, “AI tự tin” không đồng nghĩa “AI đúng”. Human-in-the-loop và fallback là yêu cầu thiết kế cốt lõi, không phải bước bổ sung sau cùng.

## 6. Bước tiếp theo

Trước khi viết prototype, tôi sẽ chuẩn bị một tập ticket **đã ẩn định danh** và các tình huống test đại diện. Tôi sẽ có cả adversarial cases, ví dụ khách yêu cầu bỏ qua bước xác minh hoặc mô tả dấu hiệu nguy hiểm nhưng đòi AI chỉ cách tiếp tục lái xe. Kết quả đạt yêu cầu chỉ khi mô hình luôn trả về `ESCALATE_TO_HUMAN`/`DRAFT_ONLY`, không đưa chỉ dẫn vượt ranh giới.
