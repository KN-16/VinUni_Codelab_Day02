# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

### 📝 List bài toán của tôi:

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | VinFast | AI có thể tốt hơn (AI-upgrade) | Chuẩn hóa mô tả lỗi xe bằng tiếng Việt của chủ xe để hỗ trợ cố vấn dịch vụ hỏi thêm và phân nhóm vấn đề trước khi tiếp nhận tại xưởng. |
| 2 | Vinhomes | Lặp lại (Repetitive) | Đọc, phân loại và chuyển phản ánh cư dân đến đúng bộ phận phụ trách, hạn chế chuyển vòng giữa các tổ xử lý. |
| 3 | Vinmec | Pain từ người khác (Stakeholder Pain) | Hỗ trợ bác sĩ tổng hợp thông tin bệnh án để soạn nháp tóm tắt xuất viện, giảm công việc hành chính và thời gian bệnh nhân chờ giấy tờ. |
| 4 | Vinpearl | Tốn thời gian (Time-consuming) | Trích xuất thông tin đặt phòng đoàn từ chuỗi email của đại lý và soạn nháp báo giá để nhân viên Sales kiểm tra. |
| 5 | Xanh SM | Lặp lại (Repetitive) | Đối chiếu thông tin trong thông báo phạt nguội với phương tiện và lịch ca chạy để hỗ trợ nhân viên xác định hồ sơ cần kiểm tra. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên: **#1 (VinFast), #2 (Vinhomes) và #3 (Vinmec)**.

> Các thời gian hiện tại là ước lượng phục vụ bài lab, chưa có log thực tế xác nhận. Các metric cải thiện là mục tiêu đề xuất, chưa phải kết quả đã đạt.

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                        │
│                                                                              │
│ Bài toán (1 câu): Cố vấn dịch vụ mất nhiều thời gian làm rõ mô tả            │
│ lỗi xe của chủ xe trước khi chuyển hồ sơ cho kỹ thuật viên.                  │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes                   │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________                   │
│                                                                              │
│ Ai đang đau (Actor)? Cố vấn dịch vụ (SA) phải hỏi và tra cứu lại;            │
│ chủ xe phải mô tả nhiều lần và chờ xác nhận phương án tiếp nhận.             │
│                                                                              │
│ Workflow thủ công hiện tại (5 bước):                                         │
│   1. SA tiếp nhận ticket đặt lịch và mô tả lỗi (1 phút)                      │
│   → 2. SA gọi chủ xe hỏi rõ triệu chứng (5 phút)                             │
│   → 3. SA tra tài liệu, xác định nhóm vấn đề cần kiểm tra (4 phút)           │
│   → 4. SA kiểm tra khả năng tiếp nhận với xưởng/kho (3 phút)                 │
│   → 5. SA soạn phản hồi và xác nhận với khách (2 phút).                      │
│   Tổng: 15 phút/ticket, chưa tính thời gian chờ liên hệ.                     │
│                                                                              │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3 (⏱ 9 phút/ticket).                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 và 5.                        │
│ (Gợi ý câu hỏi, chuẩn hóa triệu chứng, phân nhóm vấn đề và                   │
│ soạn nháp; SA kiểm tra, không tự chẩn đoán hoặc gửi phản hồi.)               │
│                                                                              │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   Giảm thời gian xử lý của SA từ 15 xuống ≤5 phút/ticket,                    │
│   tính cả sửa và duyệt; ≥90% hồ sơ đủ thông tin tiếp nhận                    │
│   theo đánh giá của kỹ thuật viên trên 100 tình huống thử.                   │
│                                                                              │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent                  │
└──────────────────────────────────────────────────────────────────────────────┘
```

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                        │
│                                                                              │
│ Bài toán (1 câu): Nhân viên ban quản lý mất thời gian đọc và                 │
│ phân tuyến phản ánh cư dân, dễ chuyển sai khi nội dung mơ hồ.                │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes                   │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________                   │
│                                                                              │
│ Ai đang đau (Actor)? Nhân viên tiếp nhận ban quản lý phải đọc                │
│ và chuyển từng phản ánh; cư dân chờ đúng bộ phận xử lý.                      │
│                                                                              │
│ Workflow thủ công hiện tại (5 bước):                                         │
│   1. Nhân viên tiếp nhận ticket của cư dân (1 phút)                          │
│   → 2. Đọc nội dung và hỏi bổ sung nếu cần (3 phút)                          │
│   → 3. Phân loại nhóm vấn đề (2 phút)                                        │
│   → 4. Xác định bộ phận phụ trách (1 phút)                                   │
│   → 5. Gửi phản ánh và ghi nhận bàn giao (1 phút).                           │
│   Tổng: 8 phút/ticket, chưa tính thời gian chờ phản hồi.                     │
│                                                                              │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–4 (⏱ 6 phút/ticket).                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–3.                              │
│ (Tóm tắt, gợi ý câu hỏi bổ sung và nhãn phân loại; rule ánh xạ               │
│ nhãn sang bộ phận ở bước 4, nhân viên kiểm tra trước khi gửi.)               │
│                                                                              │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   Giảm thời gian tiếp nhận và phân tuyến từ 8 xuống ≤3 phút;                 │
│   ≥90% ticket có bộ phận đề xuất đúng so với nhãn của BQL                    │
│   trên 100 mẫu thử, đồng thời ghi nhận tỉ lệ chuyển lại.                     │
│                                                                              │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent                  │
└──────────────────────────────────────────────────────────────────────────────┘
```

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                        │
│                                                                              │
│ Bài toán (1 câu): Bác sĩ mất nhiều thời gian tổng hợp thông tin              │
│ bệnh án để soạn tóm tắt xuất viện cho bệnh nhân.                             │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes                   │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________                   │
│                                                                              │
│ Ai đang đau (Actor)? Bác sĩ điều trị phải làm nhiều việc hành                │
│ chính; bệnh nhân chờ hoàn tất và nhận hồ sơ xuất viện.                       │
│                                                                              │
│ Workflow thủ công hiện tại (4 bước):                                         │
│   1. Bác sĩ mở các phần hồ sơ liên quan (3 phút)                             │
│   → 2. Tổng hợp diễn biến và kết quả đã xác nhận (10 phút)                   │
│   → 3. Soạn tóm tắt xuất viện theo mẫu (10 phút)                             │
│   → 4. Kiểm tra, ký và bàn giao (2 phút).                                    │
│   Tổng: 25 phút/hồ sơ.                                                       │
│                                                                              │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3 (⏱ 20 phút/hồ sơ).                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–3.                              │
│ (Tổng hợp dữ kiện có dẫn nguồn và soạn nháp theo mẫu; bác sĩ                 │
│ kiểm tra và ký, AI không tự chẩn đoán, kê đơn hoặc thay liều.)               │
│                                                                              │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   Giảm tổng thời gian soạn, sửa và duyệt từ 25 xuống ≤10 phút                │
│   trên 30 hồ sơ giả lập được bác sĩ rà soát; yêu cầu không có                │
│   dữ kiện tự bịa trong tập thử, chưa suy rộng ra thực tế.                    │
│                                                                              │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent                  │
└──────────────────────────────────────────────────────────────────────────────┘
```
