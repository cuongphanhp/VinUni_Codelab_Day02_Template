# 02-deep-dive-report.md

## Deep Dive Report — Xanh SM: Trợ lý xử lý sự cố sạc pin thực địa

### 1. Current-State Workflow Mapping

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

- Bottleneck: Bước 3 và Bước 4.
- Handoff: Từ tài xế -> tổng đài/Xanh SM -> hệ thống trạm sạc -> App tài xế.
- Tổng thời gian xử lý thủ công: khoảng 15 phút/lượt.

### 2. Problem Statement (6-field)

| Field | Nội dung |
|---|---|
| 1. Actor / Operator | Điều phối viên (Dispatcher) và tài xế Xanh SM đang xử lý sự cố pin hoặc hết pin trên đường. |
| 2. Current Workflow | Tài xế gọi tổng đài, điều phối viên tra cứu vị trí xe, tìm trạm sạc VinFast còn trụ trống gần nhất, soạn tin nhắn chỉ đường gửi cho tài xế, và gọi xe cứu hộ nếu pin quá thấp. |
| 3. Bottleneck | Bước tra cứu trạm sạc phù hợp với loại xe và bước soạn tin nhắn hướng dẫn chi tiết mất khoảng 10 phút/lượt, dễ sai ở tình huống khẩn cấp. |
| 4. Business Impact | Mỗi ngày có khoảng 80 sự cố pin thực địa ở Hà Nội; đội điều vận mất 20 giờ/ngày để xử lý thủ công, dẫn đến thời gian chờ đáng kể của tài xế và ảnh hưởng đến doanh thu. |
| 5. Success Metric | Giảm thời gian xử lý sự cố từ 15 phút xuống dưới 3 phút; đạt tỷ lệ đề xuất trạm sạc phù hợp trên 98%; giảm lỗi trong tin nhắn hướng dẫn xuống dưới 2%. |
| 6. Operational Boundary | AI được phép truy xuất vị trí xe, đề xuất trạm sạc gần nhất, và soạn nháp tin nhắn chỉ đường. AI tuyệt đối không được tự động gửi tin mà không qua phê duyệt của điều phối viên. Nếu pin dưới 5% hoặc trạm sạc cách quá xa, AI phải đề xuất xe cứu hộ pin di động. |

### 3. Future-State Flow & AI Fit

- AI Fit: LLM Feature
- Vì đây là bài toán có cấu trúc rõ, cần tiết kiệm thời gian xử lý và có thể tạo nháp hướng dẫn nhanh, nhưng không cần agent tự trị.

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

### 4. Evaluate

#### AI Readiness Checklist
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? (Vị trí xe, lịch sử sự cố pin, mẫu tin nhắn, dữ liệu trạm sạc sẵn có hoặc có thể thu thập nhanh.)
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? (Có, điều phối viên luôn phê duyệt trước khi gửi, và có fallback bằng xe cứu hộ pin di động.)
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? (Có, vì quy trình mới giúp giảm tải cho đội điều vận và cải thiện trải nghiệm tài xế.)

#### Quyết định cuối cùng
- [x] GO (Bắt đầu xây dựng Prototype)

#### Justification
Bài toán này có quy trình hiện tại rõ ràng, bottleneck dễ xác định, và AI có thể cải thiện trực tiếp ở bước tra cứu và soạn tin nhắn. Rủi ro kỹ thuật nằm trong tầm kiểm soát nhờ Human-in-the-loop; mức đầu tư không quá lớn và có thể đo được bằng thời gian xử lý, nên nên đi tiếp với scope hẹp.
