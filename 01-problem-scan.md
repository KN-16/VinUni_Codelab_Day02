# 01 — Problem Scan & Quick Cards (Cá nhân) — Vinmec

> Bài làm cá nhân Phase 1–2, mảng **Vinmec** (Vin Smart Future).
> **Lưu ý chung:** mọi con số bên dưới là **ước tính giả định**, chưa phải dữ liệu nội bộ Vinmec. Cần xác thực trước khi dùng làm baseline.

---

# 🔍 Phase 1 — SCAN (4 Lenses)

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | Vinmec | Lặp lại | Nhân viên đối soát BHYT kiểm tra thủ công mã ICD, thuốc và dịch vụ kỹ thuật của từng hồ sơ so với quy định thanh toán trước khi nộp. Hồ sơ bị xuất toán phải làm lại. |
| 2 | Vinmec | Tốn thời gian | Bác sĩ và điều dưỡng gõ tay tóm tắt bệnh án và tóm tắt ra viện, mất 20–30 phút/bệnh nhân (số lấy từ ví dụ lab, cần kiểm chứng). |
| 3 | Vinmec | AI-upgrade | Bệnh nhân hẹn khám nhưng không đến (no-show). Tổng đài chỉ nhắc lịch hàng loạt theo kịch bản cố định, không phân biệt nhóm nguy cơ cao, nên slot trống không được lấp kịp. |
| 4 | Vinmec | Pain từ người khác | Nhân viên tiếp nhận nhập tay giấy chuyển tuyến (PDF/ảnh/bản giấy) vào HIS; nhập lâu (~4 phút/hồ sơ, ước tính), dễ sai, ảnh hưởng thanh toán. Giai đoạn 1 chỉ giấy chuyển tuyến. |
| 5 | Vinmec | Lặp lại | Tổng đài/CSKH trả lời lặp lại các câu hỏi về giá, chuẩn bị trước xét nghiệm, bảo hiểm và kết quả qua điện thoại, Zalo, app. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Top 3 được chọn: **#1 (BHYT), #3 (no-show), #4 (nhập liệu giấy chuyển tuyến)**.

## Card #1 — Kiểm tra hồ sơ BHYT trước khi nộp

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Kiểm tra hồ sơ BHYT trước khi nộp để giảm hồ sơ   │
│ bị xuất toán/từ chối do sai mã ICD, thuốc, dịch vụ.         │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Nhân viên đối soát BHYT (quá tải, làm lại);    │
│ Kế toán viện phí (mất doanh thu do xuất toán)               │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Xuất bảng kê hồ sơ từ HIS                              │
│   → 2. Đối chiếu mã ICD với thuốc/dịch vụ đã chỉ định       │
│   → 3. Tra quy định thanh toán BHYT (thủ công)              │
│   → 4. Ghi chú lỗi, gửi lại bác sĩ/khoa chỉnh sửa           │
│   → 5. Nộp hồ sơ; chờ giám định, xử lý hồ sơ bị xuất toán   │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ ~8 phút/hồ sơ, ước tính)    │
│ Cần kiểm chứng: lỗi xuất toán đến từ khâu nào (bác sĩ chỉ   │
│ định, mã ICD, hay đối soát)? Phân tích ~100 hồ sơ bị xuất   │
│ toán gần nhất trước khi xây.                                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3: rule engine │
│ đối chiếu ICD - thuốc - dịch vụ, gắn cờ trước khi nộp. LLM  │
│ chỉ dùng để giải thích lỗi và đọc ghi chú tự do.            │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Xuất toán từ ~4% (ước tính) xuống dưới 1,5%               │
│ - Thời gian kiểm tra từ ~8 phút xuống dưới 2 phút/hồ sơ     │
│ - Tiền: giá trị xuất toán tránh được trừ chi phí vận hành   │
│   rule > 0 mỗi tháng (cần baseline từ phòng kế toán)        │
│ - Người cập nhật rule khi quy định BHYT đổi: Phòng BHYT/IT  │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
│ (Rule engine là lõi, về bản chất là phần mềm; LLM chỉ tùy   │
│ chọn để giải thích lỗi cho nhân viên)                       │
└─────────────────────────────────────────────────────────────┘
```

## Card #3 — Giảm no-show

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Giảm tỷ lệ bệnh nhân hẹn khám nhưng không đến     │
│ (no-show) bằng dự đoán nguy cơ và nhắc lịch cá nhân hóa.    │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Tổng đài/lễ tân (gọi nhắc thủ công);           │
│ Bác sĩ (slot trống); Vinmec (mất doanh thu)                 │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Bệnh nhân đặt lịch (app/tổng đài)                      │
│   → 2. Tổng đài gọi/nhắn nhắc lịch hàng loạt trước ngày khám│
│   → 3. Bệnh nhân không đến, slot bỏ trống                   │
│   → 4. Lễ tân gọi bù/đổi lịch thủ công (thường đã muộn)     │
│                                                             │
│ Bước nào tốn nhất? Bước 2 và 4 (⏱ ~3 phút/cuộc gọi, ước    │
│ tính; nhắc đồng loạt cả nhóm nguy cơ thấp)                  │
│ Cần kiểm chứng: lý do no-show (quên, đổi ý, tắc đường, khám │
│ nơi khác?). Nhắc lịch chỉ hiệu quả với nhóm "quên".         │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2: mô hình chấm  │
│ điểm nguy cơ (ML/rule); bước 4: gợi ý lấp slot từ danh sách │
│ chờ (không thay thế bệnh nhân hẹn đúng 1 bác sĩ).           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - No-show từ ~12% (ước tính) xuống dưới 8%, đo bằng nhóm    │
│   đối chứng (A/B): nhắc cá nhân hóa vs nhắc hàng loạt       │
│ - Lấp slot trống từ ~10% (ước tính) lên trên 40%            │
│ - Tiền: doanh thu cứu được trừ chi phí SMS/Zalo/tổng đài    │
│   nhóm nguy cơ cao > 0 mỗi tháng                            │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
│ (Rule/ML cổ điển dự đoán no-show; LLM chỉ soạn tin nhắn,    │
│ không quyết định lịch; cẩn trọng overbooking)               │
└─────────────────────────────────────────────────────────────┘
```

## Card #4 — Trích xuất dữ liệu giấy chuyển tuyến vào HIS

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                       │
│                                                             │
│ Bài toán: Tự động trích xuất dữ liệu từ GIẤY CHUYỂN TUYẾN   │
│ (phạm vi hẹp, giai đoạn 1) vào HIS. Xét nghiệm ngoài và bảo │
│ hiểm tư nhân để giai đoạn sau.                              │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Nhân viên tiếp nhận/hành chính (nhập tay,      │
│ dễ sai); Bệnh nhân (chờ lâu); Kế toán (sai mã gây vướng TT) │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Nhận giấy chuyển tuyến (PDF/ảnh/bản giấy)              │
│   → 2. Đọc và nhập tay các trường vào HIS                   │
│   → 3. Đối chiếu, gắn đúng hồ sơ bệnh nhân                  │
│   → 4. Sửa lỗi khi phát hiện sai (thường ở khâu thanh toán) │
│                                                             │
│ Bước nào tốn nhất? Bước 2 (⏱ ~4 phút/hồ sơ, ước tính CHƯA  │
│ ĐO; lỗi nhập ~2-3%, chưa xác minh có gây xuất toán không)   │
│ → Việc đầu tiên: đo baseline thật trên ~100 hồ sơ.          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (OCR + rule    │
│ cho trường chuẩn; LLM chỉ xử lý ngoại lệ). Bước 3 chỉ GỢI Ý │
│ hồ sơ khớp, người xác nhận (tránh gắn nhầm bệnh nhân).      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ 1. Thời gian xử lý gồm CẢ bước duyệt: 4 phút → dưới 1,5     │
│    phút/hồ sơ (đo trên mẫu 100 hồ sơ).                      │
│ 2. Lỗi sau duyệt dưới 0,5% (đo trên mẫu ≥ 500 hồ sơ, do     │
│    người thứ hai chấm); trường quan trọng: bắt buộc duyệt.  │
│ 3. Tài chính: giờ công tiết kiệm trừ chi phí OCR/API mỗi    │
│    tháng > 0 (tính thời gian hoàn vốn).                     │
│                                                             │
│ Ranh giới: KHÔNG tự ghi vào HIS khi độ tin cậy thấp; thiếu/ │
│ mờ thì để trống và gắn cờ, KHÔNG đoán; không gửi ảnh hồ sơ  │
│ ra LLM ngoài nếu chưa duyệt chính sách dữ liệu cá nhân.     │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
│ (Hybrid: Rule + OCR theo template là lõi; LLM chỉ dùng cho  │
│ phần ngoại lệ không khớp mẫu, nhân viên duyệt kết quả)      │
└─────────────────────────────────────────────────────────────┘
```

---

# 📝 Ghi chú (Notes)

## 1. Stress-test "CFO khắt khe"

| Card | Điểm yếu chính AI chỉ ra | Đã xử lý |
|---|---|---|
| #1 BHYT | Lỗi xuất toán có thể do khâu bác sĩ chứ không phải đối soát; ai cập nhật rule khi quy định đổi; thiếu metric tiền; về bản chất là phần mềm rule, không phải AI | Thêm bước kiểm chứng nguyên nhân, người cập nhật rule, metric tiền; ghi rõ rule là lõi |
| #3 No-show | Chưa biết nguyên nhân no-show; thiếu nhóm đối chứng; slot trống khó thay thế; LLM không cần cho dự đoán | Thêm bước tìm lý do, A/B, metric tiền; ghi rõ ML/Rule là lõi, LLM chỉ soạn tin |
| #4 Nhập liệu | Phạm vi quá rộng (3 loại giấy tờ); thiếu baseline; AI không bắt buộc; thiếu ranh giới dữ liệu cá nhân | Thu hẹp còn giấy chuyển tuyến; đổi sang Rule (Hybrid); thêm ranh giới và metric tài chính |

## 2. Điểm tác động (tham khảo, chủ quan)

Card #4 được chấm 44–58% tùy bộ tiêu chí (13/25 theo thang 5 chỉ số; 44/100 theo bộ 10 tiêu chí). Nhóm điểm yếu nhất là Kỹ thuật (AI không bắt buộc, dữ liệu chưa sẵn sàng). Card #1 và #3 **chưa được chấm**. Các điểm này do AI chấm dựa trên giả định, chỉ để tham khảo và không thay thế dữ liệu thật.

## 3. Việc cần xác thực trước khi chọn bài toán cho nhóm

- Số hồ sơ chuyển tuyến mỗi ngày, thời gian nhập thực tế, tỷ lệ lỗi nhập.
- Tỷ lệ xuất toán BHYT và nguyên nhân gốc.
- Tỷ lệ no-show thực tế và lý do bỏ hẹn.
- Chính sách dữ liệu cá nhân khi dùng LLM bên ngoài với hồ sơ bệnh nhân.

## 4. Chưa chốt

Bài toán cho Deep-Dive nhóm **chưa được chọn**. Nếu nhóm chọn bài khác (ví dụ Xanh SM theo starter-code, hoặc Vinpearl), cần làm lại Scan và Cards cho phù hợp.
