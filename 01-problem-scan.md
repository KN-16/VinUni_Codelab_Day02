# Phase 1 - SCAN: Tìm kiếm bài toán vận hành

Sử dụng 4 lenses (Lặp lại, Tốn thời gian, AI-upgrade, Pain người khác) để quét các vấn đề thực tế tại các đơn vị Vingroup.

| # | Đơn vị | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | Vinhomes | AI-upgrade | Lọc và phân loại ticket khiếu nại của cư dân trên app Resident sang cho đúng BQL tòa nhà, ưu tiên tin nhắn khẩn cấp thay vì CSKH phải đọc tay từng tin. |
| 2 | Xanh SM | Lặp lại | Khách đổi điểm đến giữa chuyến, hệ thống tự tính lại cước và cập nhật lộ trình cho tài xế thay vì tổng đài can thiệp thủ công. |
| 3 | Vinmec | Pain từ người khác | Bác sĩ tốn 20-30 phút/ca chỉ để ngồi đọc lại EMR, tổng hợp xét nghiệm và đơn thuốc thành giấy tóm tắt xuất viện. |
| 4 | VinFast | Tốn thời gian | Kỹ thuật viên xưởng đọc mã lỗi DTC xong phải lật từng file PDF service manual dài hàng trăm trang để tìm sơ đồ mạch và hướng xử lý. |
| 5 | Vinhomes | Lặp lại | Đối soát phí dịch vụ, phí gửi xe hàng tháng giữa công nợ trên hệ thống và sao kê ngân hàng để tìm giao dịch lệch. |

---

# Phase 2 - QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3 bài toán từ danh sách: #1 (Vinhomes), #3 (Vinmec), #4 (VinFast).

---

## Thẻ bài toán #1: Vinhomes - Phân loại và điều hướng ticket cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Tự động phân loại và chuyển ticket khiếu  │
│ nại của cư dân trên app Vinhomes đến đúng BQL tòa nhà, kèm  │
│ bản nháp phản hồi tiếp nhận.                                │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? CSKH và BQL tòa nhà (quá tải vì đọc    │
│ và route hàng trăm ticket mỗi ngày, cư dân kêu phản hồi chậm)│
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi phản ánh + ảnh trên app                     │
│   --> 2. CSKH đọc nội dung, phân loại thủ công (điện/nước...)│
│   --> 3. Chuyển ticket sang BQL tòa nhà phụ trách           │
│   --> 4. Gõ tin nhắn báo đã tiếp nhận gửi lại cư dân        │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (10-15 phút/ca, │
│ dễ bỏ sót tin khẩn như kẹt thang máy hoặc vỡ đường nước)    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 và 4        │
│ (Đọc text, gắn tag, cảnh báo độ khẩn cấp, draft sẵn tin     │
│ để CSKH chỉ cần bấm duyệt)                                  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Giảm thời gian xử lý ticket ban đầu: 15 phút -> dưới 2p │
│   - Tỉ lệ phân loại đúng phòng ban: đạt trên 88%            │
│   - Cư dân nhận phản hồi xác nhận: dưới 5 phút              │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

## Thẻ bài toán #3: Vinmec - Soạn tóm tắt bệnh án xuất viện

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Trích xuất dữ liệu từ EMR để draft sẵn    │
│ bản tóm tắt xuất viện và hướng dẫn dùng thuốc cho bệnh nhân.│
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị (mất quá nhiều thời    │
│ gian làm giấy tờ hành chính, giảm giờ khám bệnh thực tế)    │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Bác sĩ mở EMR đọc lại diễn biến bệnh, xét nghiệm, đơn  │
│   --> 2. Tổng hợp lại các can thiệp và thuốc đã dùng        │
│   --> 3. Gõ tay bản tóm tắt xuất viện và dặn dò uống thuốc  │
│   --> 4. Ký xác nhận rồi chuyển in giao cho bệnh nhân       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (20-30 phút/ca) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3 (Đọc EMR,  │
│ trích xuất đúng mốc điều trị, draft bản tóm tắt theo form   │
│ sẵn để bác sĩ review và ký, không phải gõ tay từ đầu)       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Thời gian làm giấy xuất viện: 25 phút -> dưới 5 phút/ca │
│   - Tỉ lệ bác sĩ phải sửa lại thông tin cơ bản: dưới 10%    │
│   - 100% bản draft bắt buộc qua bác sĩ duyệt (HITL)         │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

## Thẻ bài toán #4: VinFast - Tra cứu chẩn đoán sự cố xe tại xưởng

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                       │
│                                                             │
│ Bài toán (1 câu): Trợ lý tra cứu nhanh tài liệu kỹ thuật    │
│ cho thợ dựa trên mã DTC và hiện tượng khách mô tả.          │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Kỹ thuật viên xưởng sửa chữa VinFast   │
│ (xe nằm chờ chẩn đoán lâu, tốn công lật tài liệu PDF dày)   │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Nhận xe và ghi nhận mô tả lỗi từ khách hàng            │
│   --> 2. Cắm máy chẩn đoán quét mã lỗi DTC từ ECU           │
│   --> 3. Mở Service Manual/TSB tra sơ đồ mạch để tìm hướng  │
│   --> 4. Lên danh sách đo kiểm linh kiện và báo giá sửa     │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 (30-45 phút/xe vì   │
│ mã DTC chung chung, phải đối chiếu nhiều sơ đồ điện)        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 (Nhận DTC +    │
│ mô tả lỗi -> search tài liệu xưởng -> gợi ý luôn linh kiện  │
│ hay hỏng và bước đo điện ưu tiên kèm số trang tài liệu)     │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Thời gian tra tài liệu kỹ thuật: 40 phút -> dưới 8 phút │
│   - Tỉ lệ bắt đúng bệnh ngay lần đầu: tăng lên tầm 85-90%   │
│   - Giảm thời gian xe nằm chờ kiểm tra tại xưởng: ~30%      │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```
