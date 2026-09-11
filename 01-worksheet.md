# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**. 

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:
* 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
* 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
* 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
* 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý hồ sơ bệnh án.
* 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho một bài toán cụ thể thuộc một trong những mảng kinh doanh trên.

---

## 📊 2. Cơ cấu tính điểm bài lab

### 👥 Điểm nhóm (60 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **G1. Workflow Mapping** | 20 | Problem Deep-Dive | Vẽ chi tiết quy trình hiện tại: các bước, handoff, thời gian, bottleneck |
| **G2. Problem Statement** | 20 | Problem Deep-Dive | Problem Statement 6-field bám sát thực tế, metric có số và ranh giới rõ ràng |
| **G3. AI Fit & Future Flow** | 10 | Problem Deep-Dive | So sánh Rule vs LLM vs Agent, future flow có bước AI, ranh giới và Fallback |
| **G4. Decision Quality** | 10 | Problem Deep-Dive | Quyết định Go/Not Yet/No-Go trung thực và có chứng cứ rõ ràng |

### 👤 Điểm cá nhân (40 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **I1. Scan & Cards** | 15 | Quick Cards | Liệt kê 5 problems sử dụng 3 lenses, hoàn thiện 3 quick cards chất lượng |
| **I2. Prototyping** | 10 | 02-lab/ | Chạy thử nghiệm programmatic prompt prototype thành công |
| **I3. AI Log & Reflection** | 15 | 03-ai-log.md | Phản ánh trung thực về việc dùng AI làm thought-partner (giúp gì, sai gì, sửa gì) |

---

# 🚀 Phase 0 — worked Example: Xanh SM Intelligent Dispatcher (15 min)

*Giảng viên walk-through ví dụ thực tế từ Vin Smart Future để bạn hiểu rõ cách scoping một bài toán AI.*
Đọc chi tiết worked example tại file [02-deliverable-example.md](02-deliverable-example.md).

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary| Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 |Vinfast |After-sales / AI Service Operations |Dự báo và điều phối thời gian sửa chữa/bảo hành. Phản hồi thực tế cho thấy khách phàn nàn xe nằm xưởng lâu, chất lượng xử lý không ổn định và phải chờ kéo dài. Có thể xây Repair Time Prediction + Parts Availability Prediction + Workshop Scheduling để dự báo thời gian hoàn thành và tự động ưu tiên xe. |
| 2 |Xanh SM |Dispatch / ETA Optimization |ETA và điều phối chuyến chưa ổn định. Review cho thấy app có trường hợp hiển thị tài xế ở vị trí không chính xác; có phản ánh chuyến đặt trước bị hủy sát giờ và hệ thống không tự điều hướng lại. Đây là bài toán ETA Prediction + Driver Dispatch + Cancellation Prediction. |
| 3 |Vinhomes |Resident Operations / Service Management |Xử lý yêu cầu cư dân và sự cố trên Vinhomes Resident chưa hiệu quả. Review gần đây phản ánh app lỗi thanh toán, đặt tiện ích, intercom, load dữ liệu và khó liên hệ hỗ trợ. Có thể xây AI Ticket Classification + Root Cause Detection + Auto-routing + SLA Prediction, giúp ticket được phân loại và chuyển đúng bộ phận ngay từ đầu.|
| 4 |Vinmec |Patient Journey / Care Coordination |Điều phối quy trình khám còn nhiều bước thủ công, dễ khiến bệnh nhân đi vòng và chờ/được hướng dẫn không nhất quán. Một review tháng 8/2026 phản ánh bệnh nhân đăng ký tầm soát nhưng khi đến viện phải đi khám “lòng vòng”, tốn thêm chi phí và phải chủ động hỏi mới được giải thích kết quả. Quy trình chính thức của Vinmec cũng gồm nhiều bước: tiếp nhận → kiểm tra đặt hẹn → mở hồ sơ → khám → thanh toán → xét nghiệm/thuốc... Có thể dùng AI Patient Journey Orchestrator để tự động kiểm tra lịch, chỉ định, kết quả và hướng dẫn bước tiếp theo.|
| 5 |Vinpearl |Guest Experience / Service Management |Dịch vụ tại chỗ chưa đồng đều, tương tác với nhân viên chủ yếu qua chatbot AI nhưng đôi khi không hiệu quả, dẫn đến phản hồi tiêu cực từ khách. Một review gần đây cho thấy chatbot không giải quyết được vấn đề của khách dẫn đến tình huống dở khóc dở cười. Đây là bài toán Hotel Service Agent kết hợp LLM + Tool Calling để xử lý các yêu cầu từ khách (booking, tiện ích, đổi phòng...) và giảm tải cho nhân viên.|

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn top 3 từ danh sách scan #1 VinFast, #2 Xanh SM, #5 Vinpearl
```

┌──────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #02                                       │
│                                                              │
│ Bài toán (1 câu):                                            │
│ Giảm thời gian chờ và tỷ lệ hủy chuyến bằng ETA Prediction   │
│ và Driver Dispatch Optimization.                             │
│                                                              │
│ Công ty thành viên: [✓] Xanh SM                              │
│                                                              │
│ Ai đang đau (Actor)?                                         │
│ Dispatcher / Customer / Driver                               │
│                                                              │
│ Workflow thủ công hiện tại:                                  │
│ 1. Khách đặt chuyến                                          │
│      ↓                                                       │
│ 2. Hệ thống tìm tài xế                                       │
│      ↓                                                       │
│ 3. Tài xế nhận chuyến                                        │
│      ↓                                                       │
│ 4. Tài xế di chuyển đến điểm đón                             │
│      ↓                                                       │
│ 5. Hoàn thành chuyến                                         │
│                                                              │
│ Bước nào tốn thời gian/lỗi nhất?                             │
│ Matching + ETA prediction                                    │
│                                                              │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                        │
│ Bước 2-3: ETA Prediction + Dispatch Optimization             │
│                                                              │
│ Đo thành công bằng gì?                                       │
│ Pickup ETA MAE ↓ 20%                                         │
│ Cancellation Rate ↓ 10%                                      │
│ Average Waiting Time ↓ 15%                                   │
│ Driver Utilization ↑ 5%                                      │
│                                                              │
│ Quick Architecture: [ ] No AI [ ] Rule [✓] ML/Optimization   │
│                       [ ] LLM [ ] Agent                      │
└──────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Giảm thời gian chờ và tỷ lệ hủy chuyến    │
│ bằng ETA Prediction và Driver Dispatch Optimization.        │
│ Công ty thành viên: Xanh SM                                 │   
│                                                             │
│ Ai đang đau (Actor)? Khách hàng, điều phối viên và tài xế   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách hàng yêu cầu chuyến xe qua App Xanh SM           │
│   2. Hệ thống điều phối tài xế gần nhất đón khách           │
│   3. Tài xế đến điểm đón                                    │
│   4. Khách hàng hủy chuyến nếu tài xế không đến kịp thời    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? 4 (⏱ 10 phút/lượt)         │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 4            │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? _________________     │
│   Pickup ETA MAE ↓ 20%                                      │
│ Cancellation Rate ↓ 10%                                     │
│ Average Waiting Time ↓ 15%                                  │
│ Driver Utilization ↑ 5%                                     │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #05                                       │
│                                                              │
│ Bài toán (1 câu):                                            │
│ Tự động xử lý các yêu cầu dịch vụ phổ biến của khách         │
│ Vinpearl nhưng vẫn kiểm soát được các trường hợp ngoại lệ.   │
│                                                              │
│ Công ty thành viên: [✓] Khác: Vinpearl                       │
│                                                              │
│ Ai đang đau (Actor)?                                         │
│ Guest Service / Front Office / Concierge / Guest             │
│                                                              │
│ Workflow thủ công hiện tại:                                  │
│ 1. Khách gửi yêu cầu                                         │
│      ↓                                                       │
│ 2. Nhân viên đọc yêu cầu                                     │
│      ↓                                                       │
│ 3. Kiểm tra booking / phòng / dịch vụ                        │
│      ↓                                                       │
│ 4. Thực hiện hoặc chuyển bộ phận                             │
│      ↓                                                       │
│ 5. Xác nhận với khách                                        │
│                                                              │
│ Bước nào tốn thời gian/lỗi nhất?                             │
│ Tra cứu thông tin + xử lý yêu cầu lặp lại                    │
│                                                              │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                        │
│ Bước 2-4: LLM + Tool Calling                                 │
│                                                              │
│ Đo thành công bằng gì?                                       │
│ Automated Resolution Rate ≥ 60%                              │
│ Average Handling Time ↓ 50%                                  │
│ Human Escalation Rate ↓ 30%                                  │
│ Guest Response Time < 30 sec                                 │
│                                                              │
│ Quick Architecture: [ ] No AI [ ] Rule [✓] LLM + Tools       │
│                       [ ] Agent                              │
└──────────────────────────────────────────────────────────────┘

```
> Xanh SM có bài toán điều phối tài xế và dự báo ETA. Khi khách đặt xe, hệ thống cần lựa chọn tài xế phù hợp nhất dựa trên vị trí, traffic, trạng thái tài xế, nhu cầu khu vực và xác suất hủy chuyến. Tôi đề xuất kết hợp ETA Prediction với Dispatch Optimization để giảm thời gian chờ, giảm cancellation và tăng số chuyến hoàn thành trên mỗi giờ tài xế. Khác với các bài toán FAQ hoặc ticket routing, đây là bài toán có tính biến động cao nên AI/ML tạo giá trị rõ ràng hơn rule-based.
---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = ____ phút/lượt**.

## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Ai đang thực hiện tác vụ hằng ngày? |
| **2. Current Workflow** | Mô tả tóm tắt quy trình thủ công hiện tại và công cụ sử dụng. |
| **3. Bottleneck** | Bước nào chậm, lỗi, hoặc cần xử lý ngôn ngữ tự động nhiều nhất? |
| **4. Business Impact** | Tổn thất thực tế đo bằng thời gian, chi phí, hoặc SLA của Vingroup. |
| **5. Success Metric** | AI giải quyết được thì đạt ngưỡng số mấy? (Ví dụ: *"85% vé được phân loại dưới 10s"*). |
| **6. Operational Boundary** | AI được phép làm gì, TUYỆT ĐỐI không được làm gì, điểm nào cần duyệt? |

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào? [ ] Rule / State-Machine [ ] LLM Feature [ ] Agentic Loop.
* **Vẽ Future-State Flow:** Đánh dấu rõ:
  * 🔵 **AI Step:** Tác vụ LLM xử lý.
  * 🟢 **Human Step (HITL):** Bước con người phê duyệt/review (Human-in-the-loop).
  * ↩️ **Fallback:** Kế hoạch dự phòng khi LLM trả về kết quả lỗi hoặc không tự tin.

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE (Nhóm, 30 min)

Để đảm bảo kỹ sư của Vin Smart Future luôn giữ vững năng lực lập trình, nhóm của bạn sẽ tiến hành **lập trình bản mẫu prompt** trực tiếp trên **Gemini 2.5 Flash** bằng Python để stress-test hệ thống.

### Hướng dẫn thực hiện:
1. Mở file [starter-code/prompt_prototype.py](starter-code/prompt_prototype.py) bằng VS Code/Cursor.
2. Hoàn thiện các nội dung sau:
   * **System Prompt:** Viết chỉ thị cực kỳ nghiêm ngặt quy định vai trò, nhiệm vụ, định dạng output và **Operational Boundary (Ranh giới cấm)** của mô hình.
   * **Structured Output:** Định nghĩa định dạng JSON output rõ ràng.
   * **Adversarial Test Cases:** Viết ít nhất 3 prompts "tấn công" (Adversarial inputs) cố tình dụ AI vượt ranh giới hoặc đưa ra câu trả lời không được phép để kiểm tra xem ranh giới của bạn có thực sự vững chắc.
3. Chạy file python:
   ```bash
   python3 prompt_prototype.py
   ```
4. Kiểm tra xem các ranh giới an toàn có bị LLM phá vỡ hay không và ghi lại kết quả vào worksheet.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [ ] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[ ] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> *Viết lý giải chi tiết tại đây*

---

# 📝 Phase 6 — REFLECTION (Cá nhân)
*Ghi nhận phản ánh của cá nhân bạn về việc phối hợp với AI trong buổi học hôm nay vào file `03-ai-log.md`.*
