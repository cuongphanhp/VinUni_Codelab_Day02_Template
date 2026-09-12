# 02 — Deep-Dive Report (Nhóm)

> Vin Smart Future — AI Product Scoping Lab
> **Bài toán chọn Deep-Dive:** Xanh SM — Phân luồng phản ánh khẩn cấp của tài xế (từ Quick Problem Card #3).

---

## 🏗️ Phase 3 — DEEP-DIVE

### 3.1. Current-State Workflow Mapping

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Tài xế gửi   │     │ Tin nhắn xếp │     │ Nhân viên đọc│     │ Nhân viên xác│
│ phản ánh vào │ ──→ │ vào hàng đợi │ ──→ │ tin theo thứ │ ──→ │ định mức độ  │
│ kênh chat    │ 🔄  │ chung (FIFO) │     │ tự đến       │     │ khẩn cấp     │
│ chung trên App│    │              │     │              │     │              │
│ Ai: Tài xế   │     │ Ai: Hệ thống │     │ Ai: Nhân viên│     │ Ai: Nhân viên│
│ ⏱ tức thời   │     │ ⏱ chờ TB     │     │ ⏱ 2 phút 🔴  │     │ ⏱ 1 phút 🔴  │
│              │     │ 3-5 phút     │     │ (đọc tin cũ  │     │ (đánh giá sau│
│              │     │              │     │ trước)       │     │ khi đọc xong)│
│ In: Nội dung │     │ In: Tin nhắn │     │ In: Nội dung │     │ In: Đánh giá │
│ phản ánh     │     │ thô          │     │ tin nhắn     │     │ chủ quan     │
│ Out: Tin nhắn│     │ Out: Vị trí  │     │ Out: Hiểu nội│     │ Out: Nhãn    │
│ vào hệ thống │     │ trong hàng đợi│    │ dung         │     │ khẩn cấp/thường│
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                       │
                                                                       ▼
                                                                ┌──────────────┐
                                                                │ Bước 5       │
                                                                │ 🔄 Escalate  │
                                                                │ cho đội an   │
                                                                │ toàn nếu     │
                                                                │ khẩn cấp     │
                                                                │ Ai: Nhân viên│
                                                                │ ⏱ 1 phút     │
                                                                └──────────────┘
🔴 = Bottleneck   🔄 = Handoff
⏱ Tổng thời gian trung bình để phát hiện + escalate 1 ca khẩn cấp: ~7 phút/lượt
```

---

### 3.2. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên trực tổng đài hỗ trợ tài xế (Driver Support) tại Trung tâm Điều hành Xanh SM. |
| **2. Current Workflow** | Tài xế gửi mọi loại phản ánh (từ hỏi đáp thông thường đến sự cố khẩn cấp như tai nạn, va chạm, mất an toàn) vào cùng một kênh chat hỗ trợ trên App. Nhân viên xử lý tin nhắn tuần tự theo thứ tự đến (FIFO); chỉ khi đọc xong nội dung mới biết đây có phải ca khẩn cấp hay không. Không có công cụ phân loại/ưu tiên tự động nào hỗ trợ. |
| **3. Bottleneck** | Bước đọc và đánh giá mức độ khẩn cấp (bước 3-4): vì xử lý tuần tự theo FIFO, một tin nhắn khẩn cấp gửi đến sau nhiều tin hỏi đáp thông thường sẽ bị "chôn" trong hàng đợi và chỉ được phát hiện khi đến lượt đọc. |
| **4. Business Impact** | Trung bình mỗi ca khẩn cấp mất 5-7 phút mới được phát hiện và escalate đúng đội xử lý, ảnh hưởng trực tiếp đến an toàn tài xế/hành khách và có nguy cơ vi phạm SLA an toàn nội bộ. Ước tính ~15-20 ca khẩn cấp/ngày trên toàn hệ thống Xanh SM khu vực Hà Nội. |
| **5. Success Metric** | 1. Giảm thời gian phát hiện + escalate ca khẩn cấp từ ~7 phút xuống dưới 30 giây (Speed).<br>2. Độ chính xác phân loại khẩn cấp đạt ≥ 95%, hạn chế tối đa bỏ sót ca thật (Recall ưu tiên hơn Precision). |
| **6. Operational Boundary** | AI được phép đọc, phân loại mức độ khẩn cấp của tin nhắn đến theo thời gian thực và tự động đẩy tin khẩn cấp lên đầu hàng đợi kèm cảnh báo. **CẤM:** AI không được tự động soạn và gửi phản hồi trực tiếp cho tài xế trong ca khẩn cấp; không được tự đóng/bỏ qua bất kỳ ca nào mà không có xác nhận của nhân viên trực (bắt buộc HITL cho mọi ca được gắn nhãn khẩn cấp). |

---

### 3.3. Future-State Flow & AI Fit

* **AI Fit:** Chọn **LLM Feature** (không cần Agentic Loop vì đây là tác vụ phân loại + định tuyến một bước, không cần chuỗi hành động phức tạp; cũng không dùng thuần Rule-based vì cách tài xế mô tả sự cố rất đa dạng về ngôn ngữ, rule từ khóa dễ bị bỏ sót hoặc báo động giả).
* **Quy trình tương lai (Future-State):**

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Tài xế gửi   │     │ 🔵 AI đọc &  │     │ 🔵 Nếu khẩn  │     │ 🟢 Nhân viên │
│ phản ánh vào │ ──→ │ phân loại mức│ ──→ │ cấp: đẩy lên │ ──→ │ (HITL) xác   │
│ kênh chat    │     │ độ khẩn cấp  │     │ đầu hàng đợi │     │ nhận & xử lý │
│              │     │ real-time    │     │ + cảnh báo đỏ│     │ ngay ca đó   │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                       │
                                                                       ▼
                                                                ↩️ Fallback:
                                                                Nếu AI không chắc
                                                                chắn (confidence
                                                                thấp) hoặc lỗi hệ
                                                                thống, tin nhắn về
                                                                lại hàng đợi FIFO
                                                                thông thường —
                                                                không mất dữ liệu.
```

---

## 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist:
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? — Có log lịch sử chat hỗ trợ tài xế từ hệ thống hiện tại để dùng làm test case và tinh chỉnh prompt.
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? — Có. AI chỉ đóng vai trò phân loại/định tuyến, không tự phản hồi hay tự đóng ca; mọi ca khẩn cấp đều bắt buộc nhân viên xác nhận.
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? — Cần đào tạo lại nhân viên trực tổng đài để chuyển từ thói quen xử lý theo thứ tự đến (FIFO) sang ưu tiên xử lý theo cảnh báo của AI.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp — chỉ phân loại và định tuyến, chưa tự động phản hồi.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):**
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):**

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> Bài toán có tác động an toàn rõ ràng và đo lường được (thời gian phát hiện ca khẩn cấp), kiến trúc kỹ thuật đơn giản (LLM classification một bước, không cần agent phức tạp), và rủi ro được kiểm soát chặt bằng thiết kế "AI chỉ định tuyến — con người quyết định" (HITL bắt buộc + fallback về FIFO khi AI không chắc chắn). Chi phí triển khai thấp vì tận dụng log chat có sẵn để test, phù hợp để bắt đầu với một prototype phạm vi hẹp trước khi mở rộng sang các tác vụ phức tạp hơn.
