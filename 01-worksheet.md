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
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **Xanh SM** | Lặp lại | Tự động xử lý sự cố sạc pin thực địa của tài xế bằng cách đề xuất trạm sạc gần nhất và draft tin nhắn chỉ đường. |
| 2 | **Vinhomes** | Tốn thời gian | Phân loại và điều hướng khiếu nại cư dân từ App Vinhomes Resident đến đúng bộ phận xử lý, giảm thời gian phản hồi thủ công. |
| 3 | **Vinmec** | AI có thể tốt hơn | Tóm tắt hồ sơ xuất viện (Discharge Summary) từ bệnh án, xét nghiệm và ghi chú bác sĩ để tiết kiệm thời gian viết báo cáo. |
| 4 | **Vinpearl** | Pain từ người khác | Tổng hợp và phân tích review khách sạn từ Booking, Agoda, Google Maps để phát hiện các vấn đề nghiêm trọng cần xử lý nhanh. |
| 5 | **VinFast** | AI có thể tốt hơn | Chẩn đoán lỗi xe từ mô tả tiếng Việt của khách hàng để hỗ trợ kỹ thuật viên xác định nguyên nhân ban đầu nhanh hơn. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                        │
│                                                             │
│ Bài toán: Tài xế Xanh SM báo sự cố sạc pin / hết pin giữa  │
│ đường cần được điều phối nhanh bằng trạm sạc gần nhất và     │
│ tin nhắn chỉ đường chính xác.                               │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Tài xế (chờ đợi), Điều phối viên       │
│ (quá tải), Trung tâm điều vận Xanh SM                       │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Tài xế gọi tổng đài báo sự cố pin                     │
│   → 2. Điều phối viên tra cứu vị trí xe trên hệ thống       │
│   → 3. Tra cứu trạm sạc VinFast còn trụ trống gần nhất     │
│   → 4. Soạn tin nhắn chỉ dẫn gửi cho tài xế                │
│   → 5. Gọi xe cứu hộ nếu pin quá thấp hoặc cần hỗ trợ thêm │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ 10-12 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4, tự động     │
│ tìm trạm sạc phù hợp và draft tin nhắn chỉ đường             │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian xử lý sự cố từ 15 phút xuống dưới 3 phút;    │
│ tăng độ chính xác hướng dẫn trạm sạc lên 98%.                │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                        │
│                                                             │
│ Bài toán: Hệ thống cần tự động phân loại các khiếu nại của   │
│ cư dân trên App Vinhomes Resident và chuyển đến bộ phận phù │
│ hợp.                                                        │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Cư dân, ban quản lý tòa nhà, nhân sự    │
│ CSKH Vinhomes                                                │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân gửi phản ánh qua App                            │
│   → 2. CSKH đọc từng tin nhắn và phân loại                  │
│   → 3. Chuyển sang bộ phận quản lý phù hợp                  │
│   → 4. Soạn phản hồi đầu tiên cho cư dân                    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-4 (⏱ 1-2 giờ/tin) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4, tự động     │
│ phân loại loại khiếu nại và draft phản hồi ban đầu           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian xử lý phản ánh từ 2 giờ xuống còn dưới 15    │
│ phút; tăng tỷ lệ chuyển đúng bộ phận lên 95%.               │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                        │
│                                                             │
│ Bài toán: Vinmec cần hỗ trợ bác sĩ tóm tắt hồ sơ xuất viện  │
│ từ bệnh án, xét nghiệm và ghi chú lâm sàng một cách nhanh   │
│ và rõ ràng.                                                 │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ, nhân sự bệnh viện, bộ phận hồ  │
│ sơ bệnh án                                                  │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Bác sĩ đọc toàn bộ hồ sơ bệnh án                       │
│   → 2. Trích xuất thông tin quan trọng từ xét nghiệm và ghi  │
│      chú lâm sàng                                           │
│   → 3. Viết bản tóm tắt xuất viện cho bệnh nhân             │
│   → 4. Kiểm tra và chỉnh sửa trước khi lưu hồ sơ            │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 1-3 (⏱ 20-30 phút/   │
│ bệnh nhân)                                                  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-3, tự động     │
│ tóm tắt và đề xuất đoạn văn bản cần review bởi bác sĩ        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian soạn tóm tắt từ 30 phút xuống dưới 5 phút;   │
│ đạt tỷ lệ bác sĩ đồng ý nội dung AI hỗ trợ trên 90%.        │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Bài toán chính được chọn:** Xanh SM — Trợ lý xử lý sự cố sạc pin thực địa

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ Tra cứu định │     │ Tra cứu trạm │     │ Soạn tin nhắn│
│ gọi sự cố    │ ──→ │ vị GPS xe    │ ──→ │ sạc trống    │ ──→ │ hướng dẫn    │
│ Ai: Tài xế   │     │ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │
│ ⏱ 2 phút     │     │ ⏱ 2 phút     │     │ ⏱ 5 phút 🔴  │     │ ⏱ 5 phút 🔴  │
│ In: Thông tin│     │ In: GPS xe   │     │ In: Vị trí,  │     │ In: Raw data │
│ Out: Log sự cố│     │ Out: Toạ độ  │     │ loại xe, trụ │     │ Out: SMS hoăc│
└──────────────┘     └──────────────┘     │ sạc trống    │     │ hướng dẫn    │
                                          └──────────────┘     └──────────────┘
                                                                     │
                                                                     ▼
                                                              ┌──────────────┐
                                                              │ Bước 5       │
                                                              │ Gọi xe cứu hộ│
                                                              │ nếu cần       │
                                                              │ Ai: Dispatch │
                                                              │ ⏱ 1 phút     │
                                                              └──────────────┘
```

**Chú thích:**
* 🔴 **Bottleneck:** Bước 3 và 4 (tra cứu trạm sạc phù hợp + soạn nội dung hướng dẫn) mất nhiều thời gian nhất.
* 🔄 **Handoff:** Từ tài xế qua tổng đài điều vận, rồi từ điều phối viên qua hệ thống trạm sạc và App tài xế.
* **Tổng thời gian xử lý thủ công:** khoảng 15 phút/lượt.

## 3.2. Problem Statement (6-field) & Metrics (15 min)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) và tài xế Xanh SM đang xử lý sự cố pin hoặc hết pin trên đường. |
| **2. Current Workflow** | Tài xế gọi tổng đài, điều phối viên tra cứu vị trí xe trên hệ thống, tìm trạm sạc VinFast còn trụ trống gần nhất, soạn tin nhắn chỉ đường gửi cho tài xế, và gọi xe cứu hộ nếu pin quá thấp. |
| **3. Bottleneck** | Bước tra cứu trạm sạc phù hợp với loại xe (VF5/VF8/VFe34) và bước soạn tin nhắn hướng dẫn chi tiết mất khoảng 10 phút/lượt, dễ sai ở tình huống khẩn cấp. |
| **4. Business Impact** | Mỗi ngày có khoảng 80 sự cố pin thực địa ở Hà Nội; đội điều vận mất 20 giờ/ngày để xử lý thủ công, ảnh hưởng đến thời gian chờ đợi của tài xế và làm giảm doanh thu do xe không kịp đón khách. |
| **5. Success Metric** | Giảm thời gian xử lý sự cố từ 15 phút xuống dưới 3 phút; đạt tỷ lệ đề xuất trạm sạc phù hợp hợp lệ trên 98%; giảm số lượng lỗi trong tin nhắn hướng dẫn xuống dưới 2%. |
| **6. Operational Boundary** | AI được phép truy xuất vị trí xe, đề xuất trạm sạc gần nhất, và soạn nháp tin nhắn chỉ đường. AI tuyệt đối không được tự động gửi tin mà không qua phê duyệt của điều phối viên. Nếu pin dưới 5% hoặc trạm sạc cách quá xa, AI phải đề xuất xe cứu hộ pin di động. |

## 3.3. Future-State Flow & AI Fit (25 min)
* **AI Fit:** Chọn **LLM Feature**. Đây là bài toán có cấu trúc rõ ràng, không cần Agentic Loop phức tạp, nhưng rất phù hợp với AI giúp draft nội dung và gợi ý tuyến đường nhanh hơn.
* **Future-State Flow:**

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │ ──→ │ 🔵 AI pull   │ ──→ │ 🔵 AI draft  │ ──→ │ 🟢 Human     │
│ gọi sự cố    │     │ vị trí + trạm│     │ tin nhắn     │     │ review & gửi │
│              │     │ sạc phù hợp  │     │ chỉ đường    │     │ cho tài xế    │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                              ↩️ Fallback:
                                                              Nếu AI không chắc chắn,
                                                              điều phối viên viết tay
                                                              hoặc gọi xe cứu hộ pin.
```

**AI Step:** kéo dữ liệu vị trí xe, trạm sạc còn trụ trống, và tự động draft hướng dẫn.  
**Human Step (HITL):** điều phối viên duyệt tin nhắn trước khi gửi cho tài xế.  
**Fallback:** nếu AI không tự tin hoặc pin dưới ngưỡng an toàn, hệ thống chuyển về quy trình cũ hoặc xử lý bằng xe cứu hộ pin di động.

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
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? (Vị trí xe, lịch sử sự cố pin, mẫu tin nhắn, dữ liệu trạm sạc sẵn có hoặc có thể thu thập nhanh.)
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? (Đúng, vì điều phối viên luôn phê duyệt trước khi gửi, và có fallback xe cứu hộ pin di động.)
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? (Có, vì quy trình mới giúp giảm tải cho đội điều vận và cải thiện trải nghiệm tài xế.)

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> Bài toán này có quy trình hiện tại rõ ràng, bottleneck dễ xác định, và AI có thể trực tiếp cải thiện hiệu suất ở bước tra cứu và soạn tin nhắn. Rủi ro kỹ thuật nằm trong tầm kiểm soát nhờ Human-in-the-loop, đồng thời có thể triển khai theo scope hẹp bằng LLM Feature thay vì Agentic Loop phức tạp. Vì mục tiêu mang lại lợi ích tức thì cho đội điều vận, mức đầu tư không quá lớn và có thể đo lường bằng thời gian xử lý, nên quyết định cuối cùng là GO.

---

# 📝 Phase 6 — REFLECTION (Cá nhân)
*Ghi nhận phản ánh của cá nhân bạn về việc phối hợp với AI trong buổi học hôm nay vào file `03-ai-log.md`.*
