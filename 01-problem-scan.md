# 01 — Problem Scan & Quick Cards

> **Vai trò:** AI Product Engineer tại **Vin Smart Future**
> **Mảng kinh doanh focus:** Xanh SM (GSM) — Vận hành đội xe taxi điện thông minh
> **Phương pháp:** Problem First, AI Second — quét qua các công ty thành viên Vingroup bằng 4 Lenses.

---

## 🔍 Phase 1 — SCAN: Bảng quét cơ hội

Dùng 4 Lenses (Lặp lại / Tốn thời gian / Pain từ người khác / AI-upgrade) quét qua các công ty thành viên.

| # | Subsidiary    | Lens               | Mô tả ngắn bài toán                                                                                                |
|---|---------------|--------------------|--------------------------------------------------------------------------------------------------------------------|
| 1 | **Xanh SM**   | Lặp lại            | Điều phối viên xử lý thủ công các cuộc gọi khẩn từ tài xế về sự cố sạc pin / hết pin dọc đường (15-20 phút/lượt). |
| 2 | **VinFast**   | Lặp lại            | Đối chiếu hóa đơn sạc điện và số liệu trụ sạc đối tác hằng tuần — thủ công, dễ sai lệch.                          |
| 3 | **Vinhomes**  | AI-upgrade         | CSKH phản hồi khiếu nại cư dân rập khuôn, thời gian phản hồi trung bình 12 tiếng trên App Vinhomes Resident.        |
| 4 | **Vinmec**    | Tốn thời gian      | Bác sĩ mất 20-30 phút viết tóm tắt hồ sơ xuất viện cho mỗi bệnh nhân — quá tải hành chính.                         |
| 5 | **Xanh SM**   | Tốn thời gian      | So khớp & phân bổ lại cuốc xe khi khách hàng đổi điểm đến giữa chừng — tốn nhiều cuộc gọi qua lại.                  |
| 6 | **Vinpearl**  | Lặp lại            | Tổng hợp đánh giá trải nghiệm khách sau mỗi chuyến nghỉ — thủ công từ email / survey rời rạc.                      |

---

## 🃏 Phase 2 — QUICK-ASSESS: Top 3 Problem Cards

Chọn top 3: **#1 (Xanh SM — sự cố sạc pin), #3 (Vinhomes — CSKH), #5 (Xanh SM — đổi điểm đến).**

### Quick Problem Card #1 — Xanh SM: Xử lý sự cố sạc pin thực địa

```text
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                            │
│                                                                  │
│ Bài toán: Tài xế Xanh SM báo cáo sự cố sạc pin / hết pin        │
│ giữa đường, cần điều phối cứu hộ hoặc trạm sạc gần nhất.         │
│ Công ty thành viên: [x] Xanh SM (GSM)                            │
│                                                                  │
│ Ai đang đau?                                                     │
│   • Tài xế: chờ đợi 15-20 phút mới được chỉ dẫn tiếp tục hành   │
│     trình, doanh thu/giờ sụt giảm.                               │
│   • Điều phối viên: quá tải giờ cao điểm (15-20 phút/lượt).      │
│   • Khách hàng: chuyến bị delay, hủy chuyến nếu không xử lý kịp. │
│                                                                  │
│ Workflow thủ công hiện tại (5 bước):                             │
│   1. Tài xế gọi tổng đài điều vận báo hết pin                    │
│   → 2. Điều phối viên tra cứu thủ công vị trí xe trên bản đồ    │
│   → 3. Tra cứu thủ công các trạm sạc VinFast còn trụ trống       │
│   → 4. Viết tin nhắn chỉ dẫn / đường đi gửi qua App tài xế       │
│   → 5. Liên hệ đội xe cứu hộ nếu xe đã cạn kiệt pin             │
│                                                                  │
│ Bước nào tốn nhất? Bước 2-3-4 (⏱ 12-15 phút/lượt)                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3-4                  │
│   (Tự động hóa: lấy GPS → tra trạm trống → draft tin nhắn /       │
│    quyết định dispatch_mobile_charger nếu pin < 5% & xa > 5 km)   │
│                                                                  │
│ Metric đo thành công:                                            │
│   • Thời gian xử lý/lượt giảm từ 15 phút → dưới 2 phút           │
│   • Tỉ lệ tài xế nhận được chỉ dẫn đúng trạm ≥ 98%                │
│   • Số vụ pin cạn giữa đường / tháng giảm ≥ 70%                   │
│                                                                  │
│ Vì sao quan trọng cho Vin Smart Future?                          │
│   Đây là "khoảnh khắc sự thật" của trải nghiệm EV — nếu xử lý    │
│   sai có thể gây mất an toàn + tổn thất tài sản + tẩy chay thương │
│   hiệu.                                                          │
└──────────────────────────────────────────────────────────────────┘
```

### Quick Problem Card #2 — Vinhomes: CSKH phản hồi khiếu nại cư dân

```text
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                            │
│                                                                  │
│ Bài toán: Hệ thống phản hồi khiếu nại cư dân trên App            │
│ Vinhomes Resident chậm (SLA 12 tiếng), nội dung rập khuôn.       │
│ Công ty thành viên: [x] Vinhomes                                 │
│                                                                  │
│ Ai đang đau?                                                     │
│   • Cư dân: phải chờ phản hồi, cảm giác bị bỏ rơi.               │
│   • Nhân viên CSKH: phản hồi thủ công, lặp lại nhiều case.       │
│                                                                  │
│ Workflow thủ công:                                               │
│   1. Cư dân gửi khiếu nại qua App                                │
│   → 2. Nhân viên CSKH phân loại thủ công (sự cố / yêu cầu /     │
│        phản ánh)                                                  │
│   → 3. Soạn phản hồi mẫu + cá nhân hoá                           │
│   → 4. Chuyển ban quản lý tòa nhà duyệt                           │
│   → 5. Gửi phản hồi qua App                                      │
│                                                                  │
│ Bước nào tốn nhất? Bước 2-3 (phân loại + soạn thảo)              │
│ AI có thể nhảy vào ở bước nào? Bước 2-3                          │
│   (LLM phân loại + soạn draft có [DRAFT_ONLY] để manager duyệt)  │
│                                                                  │
│ Metric:                                                          │
│   • Thời gian phản hồi trung bình: 12 tiếng → 1 tiếng             │
│   • Tỉ lệ phản hồi đúng phân loại: ≥ 95%                          │
│   • CSKH có thêm 30% thời gian xử lý case phức tạp                │
└──────────────────────────────────────────────────────────────────┘
```

### Quick Problem Card #3 — Xanh SM: Đổi điểm đến giữa chừng

```text
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                            │
│                                                                  │
│ Bài toán: Khách hàng yêu cầu đổi điểm đến giữa chừng — tài xế   │
│ & điều phối phải gọi qua lại nhiều lần để xác nhận.              │
│ Công ty thành viên: [x] Xanh SM (GSM)                            │
│                                                                  │
│ Ai đang đau?                                                     │
│   • Khách hàng: bực bội vì phải gọi nhiều lần.                   │
│   • Tài xế: mất tập trung, phải gọi điều phối xin cập nhật.      │
│   • Điều phối: giải quyết cuộc gọi reroute thủ công.              │
│                                                                  │
│ Workflow thủ công:                                               │
│   1. Khách gọi tổng đài yêu cầu đổi điểm đến                     │
│   → 2. Điều phối viên xác nhận điểm đến mới                        │
│   → 3. Tra cứu tài xế + thông báo qua điện thoại / radio          │
│   → 4. Tài xế xác nhận → cập nhật hệ thống thủ công               │
│   → 5. Tính lại cước phí theo quãng đường mới                     │
│                                                                  │
│ Bước nào tốn nhất? Bước 3-4 (gọi qua lại, dễ sai sót)           │
│ AI có thể nhảy vào ở bước nào? Bước 3-4-5                         │
│   (Tự động reroute + draft tin nhắn xác nhận cho tài xế)          │
│                                                                  │
│ Metric:                                                          │
│   • Thời gian reroute: 8 phút → 1 phút                             │
│   • Tỉ lệ tài xế nhận đúng điểm mới: ≥ 99%                        │
│   • Số cuộc gọi reroute/ngày giảm 60%                              │
└──────────────────────────────────────────────────────────────────┘
```

---

## 🏆 Bài toán chọn cho Deep-Dive

**Card #1 — Xanh SM: Xử lý sự cố sạc pin thực địa** được chọn vì:
1. **Tính an toàn cao:** Pin cạn giữa đường có thể gây mất an toàn cho tài xế & hành khách.
2. **Rõ ranh giới AI:** Có thể thiết lập Rule rõ ràng (pin < 5% → không chỉ trạm > 5 km).
3. **Đo lường được:** Có metric thời gian & tỉ lệ xử lý rõ ràng.
4. **Có sẵn dữ liệu:** GPS xe + trạng thái trụ sạc VinFast là dữ liệu nội bộ đã số hoá.
