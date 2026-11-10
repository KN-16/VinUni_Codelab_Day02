# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

### 📝 List bài toán của tôi:
| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | **Xanh SM (GSM)** | Stakeholder Pain | Tài xế bị khách đánh giá 1-sao oan (do kẹt xe giờ cao điểm, mưa ngập, lỗi định vị GPS hoặc lỗi cổng thanh toán App), dẫn đến bị hệ thống tự động trừ điểm thưởng tuần và giảm tỉ lệ nhận chuyến; khiếu nại gửi lên tốn 3–5 ngày để bộ phận Vận hành đối soát thủ công. |
| 2 | **Vinmec** | Tốn thời gian | Bác sĩ điều trị mất 25–35 phút/ca xuất viện để tổng hợp kết quả cận lâm sàng, diễn biến điều trị từ EMR và gõ tay bản Tóm tắt hồ sơ xuất viện (Discharge Summary). |
| 3 | **Vinhomes** | Lặp lại | Ban quản lý nhận hàng trăm phản ánh/khiếu nại mỗi ngày qua App Vinhomes Resident; nhân viên CSKH mất 3–5 giờ/ngày đọc, phân loại thủ công và gán việc (ticket dispatching) về từng ban chuyên trách. |
| 4 | **VinFast** | AI có thể tốt hơn | Khách hàng mô tả lỗi xe bằng tiếng Việt phi cấu trúc (*"xe thỉnh thoảng giật nhẹ khi đạp ga ở tốc độ 40km/h sau khi sạc đầy"*), chuyên viên tổng đài mất 15 phút tra cứu sổ tay kỹ thuật để xác định mã lỗi và đặt lịch bảo dưỡng. |
| 5 | **Vinpearl** | Lặp lại | Bộ phận đặt phòng (Reservations) phải đối chiếu thủ công các voucher khuyến mãi từ đại lý OTA và nhập mã phòng vào hệ thống PMS cho từng booking lẻ tẻ trong mùa cao điểm. |

---


# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân)

Chọn top 3 bài toán tiềm năng nhất để hoàn thiện **3 Quick Problem Cards**:

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tự động phân loại và gỡ phạt đánh giá 1-sao oan   │
│ cho tài xế taxi điện Xanh SM.                               │
│ Công ty thành viên: [x] GSM (Xanh SM)                       │
│                                                             │
│ Ai đang đau (Actor)? Tài xế Xanh SM (bị trừ sao, mất thưởng │
│ oan, ức chế), Đội ngũ Quản lý Tài xế / Fleet Ops (quá tải   │
│ hàng ngàn ticket khiếu nại đánh giá xấu mỗi ngày).          │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Khách đánh giá 1-sao -> Hệ thống trừ điểm rating       │
│   ──> 2. Tài xế nộp đơn giải trình khiếu nại                │
│   ──> 3. Nhân viên Vận hành mở log chuyến đi, tra cứu GPS,  │
│          lịch sử chat, thời tiết, log kẹt xe                │
│   ──> 4. Ra quyết định phê duyệt gỡ phạt / phục hồi điểm.   │
│                                                             │
│ Bước nào tốn nhất? Bước 3 & 4 (⏱ 12–15 phút/ticket)         │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 & 4         │
│ (Trích xuất lý do review, tổng hợp chéo dữ liệu telemetry   │
│ chuyến đi, phân loại lỗi thuộc về khách quan hay tài xế,    │
│ tự động auto-approve gỡ phạt cho các ca chứng cứ rõ ràng).  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ - Giảm thời gian xử lý khiếu nại 1-sao từ 72h -> < 5 phút   │
│ - Tự động hóa gỡ phạt cho >= 65% trường hợp 1-sao khách quan│
│ - Tỉ lệ sai sót (gỡ nhầm cho tài xế vi phạm thực tế) <= 2%  │
│                                                             │
│ Quick Architecture: [x] Rule Engine + LLM Classifier + HITL │
└─────────────────────────────────────────────────────────────┘
```

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Bác sĩ Vinmec tốn quá nhiều thời gian viết tay   │
│ bản Tóm tắt xuất viện và giải thích đơn thuốc cho bệnh nhân.│
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị (quá tải hành chính),  │
│ Bệnh nhân xuất viện (chờ đợi lâu, không hiểu y lệnh tại nhà)│
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Đọc EMR & lab results ──> 2. Gõ bản tóm tắt lâm sàng   │
│   ──> 3. Soạn hướng dẫn dùng thuốc ──> 4. In ấn & bàn giao │
│                                                             │
│ Bước nào tốn nhất? Bước 2 & 3 (⏱ 25 phút/ca xuất viện)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3            │
│ (Trích xuất EMR -> Tự động draft bản tóm tắt song ngữ y khoa│
│ chuẩn xác + phiên bản ngôn ngữ dễ hiểu cho người bệnh).     │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ - Giảm thời gian soạn thảo xuất viện từ 25 min -> under 5 min│
│ - Tỉ lệ bác sĩ chấp thuận bản draft (HITL) >= 85%           │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Structured Generation) │
└─────────────────────────────────────────────────────────────┘
```

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: CSKH Vinhomes phân loại và chuyển tiếp phản ánh   │
│ cư dân thủ công trên hệ thống Resident App chậm trễ.        │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)? Điều phối viên CSKH (quá tải vào giờ   │
│ cao điểm), Cư dân (bức xúc vì phản ánh chậm xử lý > 12h)    │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Đọc nội dung ticket ──> 2. Phân loại phòng ban xử lý   │
│   ──> 3. Đánh giá mức độ khẩn ──> 4. Giao việc kỹ thuật viên│
│                                                             │
│ Bước nào tốn nhất? Bước 1 & 2 (⏱ 6-8 phút/ticket)           │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1, 2, 3           │
│ (Trích xuất thực thể, gán nhãn sự cố và route ticket).      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ - Phân loại tự động 90% ticket dưới 15 giây                 │
│ - Độ chính xác định tuyến phòng ban đạt >= 95%              │
│                                                             │
│ Quick Architecture: [x] Rule + LLM Classifier               │
└─────────────────────────────────────────────────────────────┘
```