# 01 — Problem Scan & Quick Problem Cards
**Học viên:** Binh
**Dự án:** Vin Smart Future — AI Product Scoping

---

## 🔍 Phase 1 — SCAN (Cơ hội bài toán AI trong Vingroup)

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | Xanh SM & VinFast | AI có thể tốt hơn | Điều phối cứu hộ khẩn cấp khi xe taxi điện sắp cạn pin (< 5%) hoặc chết máy giữa đường |
| 2 | Xanh SM | Pain từ người khác | Phân tích lý do khách/tài xế hủy chuyến từ log và audio |
| 3 | VinFast | Lặp lại | Đối chiếu và so khớp dữ liệu log sạc xe điện với hóa đơn |
| 4 | Vinhomes | Tốn thời gian | Phân loại và điều phối phản ánh khiếu nại của cư dân |
| 5 | Vinmec | Tốn thời gian | Tự động dự thảo tóm tắt hồ sơ xuất viện cho bác sĩ duyệt |

---

## 🃏 Phase 2 — QUICK-ASSESS (3 Quick Problem Cards)

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Điều phối xe sạc lưu động cứu hộ khẩn cấp  │
│                   khi taxi điện báo pin < 5% hoặc chết máy. │
│ Công ty thành viên: [X] VinFast  [X] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Tài xế Xanh SM & Nhân viên tổng đài    │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Xe báo pin <5%/chết máy ──> 2. Tài xế gọi/nhắn tổng đài│
│   ──> 3. Tổng đài viên dò bản đồ tìm xe sạc/trạm gần nhất   │
│   ──> 4. Tổng đài gọi thủ công điều xe cứu hộ đến hỗ trợ    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 (dò tìm định vị xe   │
│ và kiểm tra khoảng cách trạm sạc khả dụng) (⏱ 10 - 15 phút) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3: Phân tích GPS │
│ & pin, nếu <5% và trạm >5km thì tự động kích hoạt điều xe   │
│ sạc lưu động (Mobile Charger).                              │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Giảm thời gian ra quyết định điều xe cứu hộ từ 15 phút   │
│    xuống dưới 60 giây; 0 trường hợp xe cạn pin bị chỉ định  │
│    chạy đến trạm sạc quá xa (>5km)."                        │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [X] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Tự động trích xuất và phân loại nguyên nhân│
│                   hủy chuyến từ ghi âm cuộc gọi và ghi chú. │
│ Công ty thành viên: [ ] VinFast  [X] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Chuyên viên Vận hành Xanh SM (Operations)│
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận báo cáo chuyến bị hủy ──> 2. Nhân viên nghe lại   │
│   ngẫu nhiên 50-100 cuộc ghi âm CSKH/tài xế hằng ngày       │
│   ──> 3. Ghi chép thủ công lý do vào file Excel             │
│   ──> 4. Họp tuần tổng hợp báo cáo tỷ lệ rò rỉ cuốc         │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (Nghe băng ghi  │
│ âm và gõ lại lý do thủ công) (⏱ 4 - 6 giờ/ngày)             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3: Dùng LLM  │
│ đọc transcript/ghi chú để phân loại 1 trong 10 nhóm lý do.  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Tăng tỷ lệ audit cuốc hủy từ 2% mẫu ngẫu nhiên lên 100%   │
│    toàn bộ cuốc hủy; giảm thời gian lập báo cáo tuần từ      │
│    2 ngày xuống 15 phút."                                   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [X] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Tự động phân loại và điều phối phản ánh    │
│                   của cư dân qua App Vinhomes Resident.     │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [X] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Ban Quản lý (BQL) & Cư dân Vinhomes    │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân gửi phản ánh lên App ──> 2. Lễ tân tòa nhà đọc  │
│   nội dung thủ công ──> 3. Đánh giá mức độ khẩn cấp         │
│   ──> 4. Chuyển tiếp ticket sang bộ phận Kỹ thuật/An ninh   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (Đọc và phân    │
│ loại thủ công, dễ bỏ sót sự cố khẩn cấp như kẹt thang máy,  │
│ ngập nước vào ban đêm) (⏱ 20 - 45 phút/ticket)             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3: LLM trích │
│ xuất từ khóa khẩn cấp, gắn tag phòng ban và kích hoạt SLA.  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "100% sự cố khẩn cấp (P1) được định tuyến đến đội trực kỹ │
│    thuật trong vòng dưới 30 giây; độ chính xác phân loại    │
│    phòng ban đạt trên 92%."                                 │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [X] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```
