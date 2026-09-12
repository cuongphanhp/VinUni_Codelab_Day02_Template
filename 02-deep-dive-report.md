# 02 — Deep Dive Report: Trợ lý Điều vận & Cứu hộ Pin Khẩn cấp (Xanh SM & VinFast)

**Đơn vị:** Vin Smart Future (Vingroup)  
**Tác giả:** Binh & Nhóm AI Engineer  
**Bài toán lựa chọn:** Card #1 — Điều phối xe sạc pin lưu động cứu hộ khẩn cấp khi xe taxi điện báo pin < 5% hoặc chết máy giữa đường.  

---

## 3.1. Current-State Workflow Mapping (Quy trình hiện tại)

Sơ đồ chi tiết được minh họa trực quan tại file [`04-workflow-diagram.png`](04-workflow-diagram.png).

### Các bước vận hành thủ công:
1. **Bước 1: Tiếp nhận cuộc gọi sự cố (⏱ 2 phút)**  
   - Actor: Tài xế Xanh SM ──> Tổng đài viên (Dispatcher).
   - In: Cuộc gọi hotline, biển số xe, mô tả hiện trường.
   - Out: Phiếu ghi nhận sự cố ban đầu.
2. **Bước 2: Tra cứu định vị GPS xe & trạng thái pin (⏱ 2 phút)**  
   - 🔄 **Handoff:** Chuyển tiếp tra cứu trên phần mềm Telematics xe VinFast.
   - Actor: Tổng đài viên.
   - In: Biển số xe.
   - Out: Tọa độ GPS, mức SoC pin hiện tại.
3. **Bước 3: Dò tìm trạm sạc khả dụng & khoảng cách (⏱ 5 phút)**  
   - 🔴 **BOTTLENECK 1:** Tổng đài viên mở bản đồ trạm VinFast, rà soát thủ công từng trạm trong bán kính xem còn trụ sạc trống không và tính toán khoảng cách đường đi. Rất dễ sai sót và mất thời gian.
   - Actor: Tổng đài viên.
   - In: Tọa độ GPS xe, bản đồ trạm sạc.
   - Out: Danh sách trạm sạc khả dụng.
4. **Bước 4: Soạn tin nhắn hướng dẫn hoặc ra quyết định cứu hộ (⏱ 5 phút)**  
   - 🔴 **BOTTLENECK 2:** Tổng đài viên gõ tin nhắn SMS/App thủ công. Nếu xe cạn pin (<5%) mà vẫn chỉ trạm sạc xa (>5km) sẽ dẫn đến xe chết máy giữa đường.
   - Actor: Tổng đài viên.
   - In: Dữ liệu trạm sạc, tình trạng xe.
   - Out: Tin nhắn hướng dẫn gửi tài xế hoặc lệnh cứu hộ.
5. **Bước 5: Điều phối xe cứu hộ sạc pin lưu động (⏱ 3 phút)**  
   - 🔄 **Handoff:** Liên hệ đội cứu hộ cơ động (Mobile Charger).
   - Actor: Đội cứu hộ thực địa.
   - In: Tọa độ xe chết máy, lệnh điều động.
   - Out: Xe cứu hộ xuất phát tiếp cận hiện trường.

> ⏱ **Tổng thời gian xử lý thủ công:** **17 phút/lượt**.

---

## 3.2. Problem Statement (6-field)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) thuộc Trung tâm Điều vận Xanh SM & Đội cứu hộ lưu động VinFast. |
| **2. Current Workflow** | Khi tài xế báo cạn pin hoặc xe chết máy, điều phối viên tra cứu thủ công GPS trên Telematics, dò tìm trạm sạc VinFast còn trụ trống trên bản đồ, tính toán cự ly, soạn tin nhắn chỉ đường hoặc gọi đội cứu hộ lưu động nếu pin < 5%. Toàn bộ 5 bước thủ công mất trung bình 17 phút/lượt. |
| **3. Bottleneck** | Bước 3 & Bước 4 (mất 10 phút): Tra cứu thủ công trụ sạc trống phù hợp cự ly và soạn thảo tin nhắn hướng dẫn/quyết định cứu hộ. Nguy cơ tài xế bị chỉ sai đến trạm quá xa dẫn đến xe nằm đường gây ách tắc giao thông. |
| **4. Business Impact** | Trung bình 60-80 sự cố pin/ngày tại các đô thị lớn (Hà Nội, TP.HCM). Gây lãng phí ~20 giờ nhân sự điều vận mỗi ngày, rò rỉ 12-15% doanh thu cuốc xe do tài xế gián đoạn hoạt động và ảnh hưởng uy tín thương hiệu dịch vụ taxi xanh chuẩn 5 sao. |
| **5. Success Metric** | 1. **Hiệu suất (Efficiency):** Giảm thời gian ra quyết định và soạn hướng dẫn từ 15 phút xuống dưới 60 giây.<br>2. **Độ an toàn (Safety/Quality):** 0 trường hợp xe cạn pin (< 5%) bị chỉ định chạy đến trạm xa (> 5km); 100% trường hợp cạn pin được kích hoạt xe sạc lưu động đúng chuẩn. |
| **6. Operational Boundary** | **Được phép:** AI tự động đọc GPS, SoC pin, tra cứu cự ly trạm sạc, tự động soạn thảo tin nhắn chỉ dẫn hoặc tạo lệnh điều động xe sạc lưu động dạng nháp (`[DRAFT_ONLY]`).<br>**CẤM:** Tuyệt đối không tự động gửi tin nhắn cho tài xế hoặc điều xe thực địa mà không có sự phê duyệt của Dispatcher (Bắt buộc Human-In-The-Loop); TUYỆT ĐỐI KHÔNG hướng dẫn xe pin < 5% di chuyển đến trạm > 5km. |

---

## 3.3. Future-State Flow & AI Fit

### Xác định mức độ phù hợp AI (AI Fit):
- **Phân loại kiến trúc:** **LLM Feature kết hợp Rule Validation (Hybrid)**.
- **Lý do lựa chọn:** Không sử dụng Agent tự trị hoàn toàn (Autonomous Agent) vì tính chất an toàn giao thông và chi phí vận hành xe cứu hộ cao. Cần ranh giới deterministic rõ ràng (Rule kiểm tra SoC pin và cự ly) kết hợp với khả năng xử lý ngôn ngữ tự nhiên của LLM để soạn thảo hướng dẫn linh hoạt, thân thiện và trấn an tâm lý tài xế.

### Quy trình tương lai (Future-State Flow):
```text
┌──────────────┐     ┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│ Bước 1       │     │ Bước 2              │     │ Bước 3              │     │ Bước 4              │
│ Nhận tín hiệu│ ──> │ 🔵 Telematics API    │ ──> │ 🔵 LLM Feature       │ ──> │ 🟢 Human Approval   │
│ SOS từ xe/app│     │ Tự động pull GPS,   │     │ Soạn Draft chỉ dẫn  │     │ Dispatcher click    │
│              │     │ pin SoC & trạm trống│     │ hoặc lệnh cứu hộ    │     │ duyệt gửi tin/điều xe│
└──────────────┘     └─────────────────────┘     └─────────────────────┘     └─────────────────────┘
                                                            │
                                                            ▼
                                                     ↩️ Fallback:
                                                     Nếu LLM timeout/lỗi,
                                                     hệ thống chuyển sang
                                                     Rule-based template cứng
                                                     và báo Dispatcher xử lý tay.
```

- 🔵 **AI Step:** Tự động tổng hợp dữ liệu GPS, pin, trạm sạc và sinh prompt chỉ dẫn / đề xuất cứu hộ gắn thẻ `[DRAFT_ONLY]`.
- 🟢 **Human Step (HITL):** Dispatcher kiểm tra nhanh trong 10-15 giây và nhấn "Approve" để phát lệnh.
- ↩️ **Fallback:** Nếu API Gemini gặp sự cố, hệ thống tự động fallback về mã thông báo khẩn cấp định sẵn (Rule-based static SMS).

---

## 3.4. AI Readiness & Final Evaluation (Đánh giá độ sẵn sàng & Quyết định)

### AI Readiness Checklist:
1. **Dữ liệu mẫu/logs sạch:** [X] SẴN SÀNG — Hệ thống Telematics của VinFast và ứng dụng Xanh SM đã lưu trữ đầy đủ log định vị GPS, mức pin từng giây và vị trí các trạm sạc V-GREEN.
2. **Kiểm soát rủi ro (HITL & Fallback):** [X] SẴN SÀNG — Ranh giới an toàn nghiêm ngặt với thẻ `[DRAFT_ONLY]` bắt buộc duyệt thủ công, quy tắc chặn cự ly khi pin < 5% đã được kiểm thử qua 3 Adversarial Test Cases trong `starter-code/prompt_prototype.py`.
3. **Mức độ sẵn sàng thay đổi quy trình:** [X] SẴN SÀNG — Đội ngũ Dispatcher rất hào hứng vì giải pháp giúp giảm 80% thời gian thao tác thủ công giờ cao điểm.

### Quyết định của Hội đồng Kỹ thuật Vin Smart Future:
- **Quyết định:** **GO (Bắt đầu xây dựng Prototype & PoC)**
- **Lý giải (Justification):**
  - Bài toán có phạm vi (scope) hẹp, rõ ràng, tác động trực tiếp vào pain point vận hành cốt lõi của Xanh SM.
  - Kiến trúc kỹ thuật gọn nhẹ (LLM Feature with Guardrails), chi phí token thấp (~300 tokens/lượt xử lý).
  - Rủi ro được triệt tiêu nhờ cơ chế Human-in-the-loop và Fallback an toàn.
