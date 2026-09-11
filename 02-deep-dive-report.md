# # Họ và tên : Ngô Tiến Dũng

# Mã học viên : 2A202602374

# Email :26ai.dungnt8@vinuni.edu.vn

# 🏗️ Phase 3 — DEEP-DIVE: VinFast Symptom Intake Assistant

> **Bài toán chọn:** Chẩn đoán **sơ bộ** lỗi xe từ mô tả tiếng Việt của khách hàng. Đây là công cụ hỗ trợ tiếp nhận, không phải công cụ chẩn đoán hoặc hướng dẫn sửa xe.

> **Giả định cần xác thực:** thời gian xử lý và các baseline trong báo cáo là giả định scoping. Trước pilot, VinFast cần xác thực bằng ticket CSKH, log cuộc gọi đã được phép sử dụng, lịch sử sửa chữa và tài liệu kỹ thuật phiên bản kiểm soát.

## 3.1. Current-State Workflow Mapping

```text
┌──────────────────────┐
│ 1. Khách gửi yêu cầu  │
│ App / hotline / email │
│ ⏱ 1 phút              │
└───────────┬──────────┘
            │ 🔄 Handoff: khách → CSKH
            ▼
┌──────────────────────┐
│ 2. CSKH ghi mô tả     │
│ triệu chứng tự do     │
│ ⏱ 2 phút              │
└───────────┬──────────┘
            ▼
┌──────────────────────────────────┐
│ 3. CSKH/cố vấn hỏi lại bối cảnh   │
│ dòng xe, đèn báo, lúc phát sinh   │
│ ⏱ 4–6 phút  🔴                    │
└───────────┬──────────────────────┘
            │ 🔄 Handoff: CSKH → cố vấn/kỹ thuật
            ▼
┌──────────────────────────────────┐
│ 4. Tra sổ tay, mã lỗi và lịch sử  │
│ xe; chọn nhóm xử lý                │
│ ⏱ 4–6 phút  🔴                    │
└───────────┬──────────────────────┘
            ▼
┌──────────────────────┐
│ 5. Tạo phiếu; đặt lịch│
│ hoặc chuyển cứu hộ    │
│ ⏱ 1–2 phút            │
└──────────────────────┘

Tổng thời gian tiếp nhận ước tính: 12–17 phút/ticket.
🔴 Bottleneck: bước 3–4; mô tả không chuẩn hóa và tài liệu kỹ thuật phân tán.
```

### Điểm nghẽn và nguyên nhân gốc

| Điểm nghẽn             | Nguyên nhân                                                                            | Hệ quả vận hành                                         |
| ---------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Hiểu mô tả triệu chứng | Khách dùng từ đời thường, thiếu bối cảnh và mô tả cùng một hiện tượng theo nhiều cách. | Hỏi lại nhiều lần, khách chờ lâu, ticket thiếu dữ liệu. |
| Tra tài liệu kỹ thuật  | Tài liệu phụ thuộc dòng xe/phiên bản; nhân viên không phải kỹ thuật viên.              | Gán sai nhóm hoặc chuyển vòng giữa CSKH và xưởng.       |
| Nhận biết an toàn      | Dấu hiệu rủi ro có thể nằm trong một câu ngắn hoặc bị mô tả mơ hồ.                     | Nếu xử lý sai có thể ảnh hưởng an toàn và SLA cứu hộ.   |

## 3.2. Problem Statement (6-field) & Metrics

| Field                       | Nội dung chi tiết                                                                                                                                                                                                                                                                                                                                                                                                                        |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Actor / Operator**     | Nhân viên CSKH và cố vấn dịch vụ VinFast tiếp nhận yêu cầu từ khách hàng; kỹ thuật viên/xưởng là bên nhận phiếu để kiểm tra thực tế.                                                                                                                                                                                                                                                                                                     |
| **2. Current Workflow**     | CSKH nhận mô tả từ app, hotline hoặc email; ghi ticket; hỏi thêm dòng xe, VIN, điều kiện xuất hiện, đèn cảnh báo và mức độ an toàn; sau đó tra tài liệu kỹ thuật/lịch sử xe và chuyển cho xưởng, đặt lịch hoặc cứu hộ. Quy trình khoảng 5 bước, 12–17 phút/ticket theo giả định scoping.                                                                                                                                                 |
| **3. Bottleneck**           | Mô tả tiếng Việt không có cấu trúc (ví dụ “kêu cụp cụp”, “rung khi chạy”) phải được diễn giải thành triệu chứng kỹ thuật. Bước hỏi lại và tra tài liệu mất 8–12 phút, dễ gán sai nhóm nếu thiếu dòng xe hoặc bối cảnh.                                                                                                                                                                                                                   |
| **4. Business Impact**      | Tăng thời gian chờ và số lần liên hệ lại của khách; CSKH/cố vấn mất năng suất; kỹ thuật viên nhận phiếu thiếu thông tin. Case bị route sai có thể vi phạm SLA và làm chậm xử lý các case thực sự khẩn cấp. Baseline chi phí, ticket volume và SLA phải được đo trước pilot.                                                                                                                                                              |
| **5. Success Metric**       | (1) ≥ **85%** ticket được gán đúng nhóm hệ thống trong top-3 trên tập test có kỹ sư gán nhãn. (2) Median thời gian tiếp nhận ≤ **4 phút/ticket**. (3) ≥ **95%** case chứa dấu hiệu an toàn cao được gắn cờ/chuyển người phụ trách. (4) Tỷ lệ ticket thiếu trường bắt buộc giảm ≥ **40%** so với baseline.                                                                                                                                |
| **6. Operational Boundary** | AI được đọc nội dung ticket đã được cấp quyền, truy xuất **chỉ** tài liệu kỹ thuật đã phê duyệt theo dòng xe/VIN, trích xuất thông tin, đề xuất tối đa 3 nhóm hệ thống, câu hỏi làm rõ và bản nháp phiếu. AI tuyệt đối không kết luận nguyên nhân cuối cùng, không hướng dẫn người dùng tự sửa, không thay đổi dữ liệu xe, không tự đặt lịch/cứu hộ/gửi tin. Nhân viên CSKH/cố vấn duyệt mọi bản nháp; kỹ thuật viên xác nhận chẩn đoán. |

### Phạm vi pilot

- Bắt đầu với nhóm triệu chứng rủi ro thấp: tiếng ồn, điều hòa và tiện nghi.
- Chỉ dùng ticket văn bản đã ẩn/giảm định danh cá nhân và tài liệu kỹ thuật có kiểm soát phiên bản.
- Loại khỏi pilot: phanh, lái, pin cao áp, va chạm, cháy/nóng bất thường, lỗi mất công suất và mọi case không đủ thông tin.

## 3.3. Future-State Flow & AI Fit

### AI-Fit Matrix

- [x] **Rule / State-Machine:** kiểm tra VIN/dòng xe, trường bắt buộc, từ khóa an toàn, ngưỡng confidence và routing khẩn cấp.
- [x] **LLM Feature:** hiểu mô tả tiếng Việt, chuẩn hóa triệu chứng, sinh câu hỏi làm rõ và tóm tắt phiếu.
- [ ] **Agentic Loop:** không chọn. Quy trình có rủi ro an toàn, không cần AI tự hành động qua nhiều hệ thống.

**Lý do:** Rule-based không đủ để hiểu các cách diễn đạt tự do; nhưng LLM không đáng tin để quyết định an toàn. Vì vậy LLM chỉ hỗ trợ ngôn ngữ trên nền dữ liệu kỹ thuật được truy xuất (RAG), còn rule và con người kiểm soát quyết định.

```text
┌──────────────────────┐
│ 1. Khách tạo ticket   │
│ App / hotline          │
└───────────┬──────────┘
            ▼
┌─────────────────────────────────────┐
│ 2. Rule: kiểm tra tối thiểu          │
│ VIN/dòng xe, đèn báo, triệu chứng    │
└───────────┬─────────────────────────┘
            │
            ├── Dấu hiệu an toàn / thiếu dữ liệu ──► ↩️ Fallback A
            ▼
┌─────────────────────────────────────┐
│ 3. 🔵 AI: trích xuất triệu chứng,     │
│ bối cảnh; RAG tài liệu đúng phiên bản │
└───────────┬─────────────────────────┘
            ▼
┌─────────────────────────────────────┐
│ 4. 🔵 AI: tạo top-3 nhóm hệ thống,    │
│ confidence, câu hỏi làm rõ, DRAFT     │
└───────────┬─────────────────────────┘
            │
            ├── confidence thấp / RAG không có nguồn ─► ↩️ Fallback B
            ▼
┌─────────────────────────────────────┐
│ 5. 🟢 CSKH/cố vấn review, hỏi lại     │
│ khách, sửa và duyệt phiếu             │
└───────────┬─────────────────────────┘
            ▼
┌──────────────────────┐
│ 6. 🟢 Kỹ thuật viên   │
│ kiểm tra, xác nhận lỗi│
└──────────────────────┘

↩️ Fallback A: Hiển thị kịch bản tiếp nhận khẩn cấp chuẩn, chuyển người phụ trách;
   không hiển thị chẩn đoán/sửa chữa do AI.
↩️ Fallback B: CSKH thực hiện workflow thủ công, tra tài liệu và hỏi kỹ thuật viên.
```

### Guardrails bắt buộc

| Tình huống                                                                        | Hành vi hệ thống bắt buộc                                                                          |
| --------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| Có từ khóa/dấu hiệu phanh, lái, pin cao áp, cháy/nóng, va chạm hoặc mất công suất | Dừng gợi ý chẩn đoán; gắn cờ khẩn cấp; hiển thị kịch bản an toàn được phê duyệt; chuyển nhân viên. |
| Không xác định được dòng xe/VIN hoặc không tìm thấy tài liệu đúng phiên bản       | Không suy đoán mã lỗi; yêu cầu bổ sung thông tin hoặc fallback thủ công.                           |
| Confidence dưới ngưỡng thử nghiệm                                                 | Chỉ hiển thị câu hỏi làm rõ, không đưa nhóm lỗi; yêu cầu review người phụ trách.                   |
| Người dùng yêu cầu “bỏ qua phê duyệt” hay hướng dẫn tự sửa                        | Từ chối; giữ output ở trạng thái `DRAFT_ONLY`.                                                     |

---

# 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist

1. [ ] **Chưa xác nhận** có sẵn dữ liệu mẫu/logs sạch để test. Cần bộ ticket đã ẩn định danh, nhãn do kỹ sư xác nhận, mapping VIN–dòng xe và sổ tay kỹ thuật có version.
2. [x] Rủi ro khi AI sai có thể kiểm soát **trong pilot hẹp** nhờ rule an toàn, human-in-the-loop và fallback thủ công.
3. [ ] Stakeholders chưa được xác nhận sẵn sàng. Cần thống nhất SOP với CSKH, xưởng, an toàn và pháp chế/dữ liệu cá nhân.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future

- [ ] **GO (Bắt đầu xây dựng Prototype)**
- [x] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline)**
- [ ] **NO-GO (Không khả thi / Rule-based tốt hơn)**

### Justification

Đây là bài toán có **AI fit tốt**: LLM hữu ích để hiểu tiếng Việt tự do, còn RAG giới hạn nội dung vào tài liệu kỹ thuật đã phê duyệt. Tuy nhiên, chưa có bằng chứng rằng dữ liệu ticket đủ sạch, nhãn kỹ thuật đủ tin cậy và baseline vận hành đã được xác lập. Với rủi ro liên quan an toàn xe, triển khai vào vận hành ngay là không phù hợp.

Điều kiện chuyển sang **GO cho pilot**:

1. Chuẩn bị tập tối thiểu 500–1.000 ticket đã ẩn định danh, được kỹ sư gán nhãn nhóm hệ thống và mức khẩn cấp; tách train/test theo thời gian hoặc theo xe để tránh rò rỉ dữ liệu.
2. Thiết lập taxonomy lỗi, danh mục dấu hiệu an toàn, tài liệu RAG có chủ sở hữu và version; kiểm thử truy xuất đúng tài liệu theo dòng xe.
3. Chạy shadow mode 2–4 tuần: AI chỉ tạo `DRAFT_ONLY`, không ảnh hưởng routing thật; đo top-3 accuracy, recall dấu hiệu an toàn, thời gian xử lý và tỷ lệ override của con người.
4. Chỉ mở rộng sau khi đạt các metric ở mục 3.2, không có sự cố an toàn do output AI và được CSKH/xưởng phê duyệt SOP.
