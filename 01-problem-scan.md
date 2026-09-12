# 01-problem-scan.md

## Phase 1 — SCAN

### Bảng quét cơ hội (5 bài toán)

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|----------------------|
| 1 | Xanh SM | Lặp lại | Tự động xử lý sự cố sạc pin thực địa của tài xế bằng cách đề xuất trạm sạc gần nhất và draft tin nhắn chỉ đường. |
| 2 | Vinhomes | Tốn thời gian | Phân loại và điều hướng khiếu nại cư dân từ App Vinhomes Resident đến đúng bộ phận xử lý, giảm thời gian phản hồi thủ công. |
| 3 | Vinmec | AI có thể tốt hơn | Tóm tắt hồ sơ xuất viện (Discharge Summary) từ bệnh án, xét nghiệm và ghi chú bác sĩ để tiết kiệm thời gian viết báo cáo. |
| 4 | Vinpearl | Pain từ người khác | Tổng hợp và phân tích review khách sạn từ Booking, Agoda, Google Maps để phát hiện các vấn đề nghiêm trọng cần xử lý nhanh. |
| 5 | VinFast | AI có thể tốt hơn | Chẩn đoán lỗi xe từ mô tả tiếng Việt của khách hàng để hỗ trợ kỹ thuật viên xác định nguyên nhân ban đầu nhanh hơn. |

## Phase 2 — QUICK-ASSESS

### Quick Problem Card #1 — Xanh SM: Xử lý sự cố sạc pin thực địa

```text
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
│ Workflow thủ công hiện tại (5 bước):                      │
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

### Quick Problem Card #2 — Vinhomes: Phân loại và điều hướng khiếu nại cư dân

```text
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
│ Workflow thủ công hiện tại (5 bước):                        │
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

### Quick Problem Card #3 — Vinmec: Tóm tắt hồ sơ xuất viện

```text
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
│ Workflow thủ công hiện tại (5 bước):                        │
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

## Kết luận lựa chọn

Trong các bài toán trên, nhóm chọn bài toán chính là:
- Xanh SM — Trợ lý xử lý sự cố sạc pin thực địa

Lý do chọn: workflow rõ ràng, bottleneck dễ xác định, AI fit phù hợp, và có thể đo được ngay bằng thời gian xử lý và độ chính xác hướng dẫn.
