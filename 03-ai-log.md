# 03 — AI Log & Reflection (Cá nhân)

> Bản nháp dựa trên những gì đã làm cùng AI (Claude) ở Phase 1–2. Hãy đọc lại và chỉnh bằng lời của bạn trước khi nộp. Phần Phase 4 (prototype) chưa làm nên chưa ghi.

## 1. Tôi đã dùng AI vào việc gì

| Bước | Tôi nhờ AI | AI giúp được gì |
|---|---|---|
| Phase 1, Scan | Brainstorm 5 pain point vận hành của Vinmec kèm ước tính tổn thất | Đưa 5 ứng viên (đối soát BHYT, ghi chép bệnh án, no-show, nhập liệu giấy tờ, CSKH) và mô hình tính tổn thất |
| Phase 2, Quick Cards | Điền 3 card (#1 BHYT, #3 no-show, #4 nhập liệu) | Dựng khung workflow, metric, kiến trúc đề xuất cho từng card |
| Phase 2, Stress-test | Dùng prompt "CFO khắt khe" cho card #4, #1, #3 | Chỉ ra điểm yếu về logic, metric, và lý do rule-based có thể tốt hơn |
| Chấm tác động | Chấm Card #4 (và 1 card Vinhomes để so sánh) theo 4 thang khác nhau | Cho điểm 44–58/100 tùy thang, cùng nhận định nhóm điểm yếu |

## 2. AI sai hoặc chưa đáng tin ở đâu

- **Số liệu là giả định, không phải dữ liệu thật.** Các con số về tổn thất (8 phút/hồ sơ, no-show 12%, xuất toán 4%, 900 hồ sơ/ngày, 90.000 đ/giờ) là ước tính của AI dựa trên benchmark chung, không lấy từ Vinmec. AI có ghi rõ điều này, nhưng nếu tôi chép nguyên vào bài thì sẽ trình bày số giả định như số thật.
- **Thang điểm không có định nghĩa chỉ số.** Với thang 5 chỉ số (5–25), đề dán vào chỉ có công thức, nên AI tự giả định 5 chỉ số. Điểm 13/25 vì vậy chỉ mang tính tham khảo.
- **Điểm chấm chủ quan và lệch giữa các thang.** Cùng Card #4 được 52% (thang 5 chỉ số), 46%, 58% và 44% tùy bộ tiêu chí. Mức chênh phần lớn do thang đổi trọng số và ngưỡng, không phải do thẻ thay đổi.
- **Thiên về kết luận "rule-based tốt hơn".** Khi được yêu cầu đóng vai CFO, AI luôn tìm lý do chống AI. Phản biện này hữu ích nhưng không phải kết luận cuối.

## 3. Tôi đã sửa gì sau khi AI phản biện

| Card | AI chỉ ra | Tôi sửa |
|---|---|---|
| #4 nhập liệu | Phạm vi quá rộng (3 loại giấy tờ), thiếu baseline, AI không bắt buộc, thiếu ranh giới dữ liệu cá nhân | Thu hẹp còn giấy chuyển tuyến; đổi sang Rule (Hybrid) + OCR, LLM chỉ cho ngoại lệ; thêm ranh giới (không ghi HIS khi độ tin cậy thấp, không đoán trường mờ); thêm metric tài chính |
| #1 BHYT | Có thể lỗi xuất toán đến từ khâu bác sĩ chứ không phải đối soát; ai cập nhật rule khi quy định đổi; thiếu metric tiền | Thêm bước kiểm chứng nguyên nhân (phân tích ~100 hồ sơ xuất toán), thêm người chịu trách nhiệm cập nhật rule, thêm metric tiền, ghi rõ đây về bản chất là phần mềm |
| #3 no-show | Chưa biết nguyên nhân no-show; thiếu nhóm đối chứng; slot trống khó thay thế; LLM không cần cho dự đoán | Thêm bước tìm lý do no-show, thêm A/B nhóm đối chứng, thêm metric tiền, ghi rõ ML/Rule là lõi và LLM chỉ soạn tin |
| Số liệu | Mọi con số là giả định | Gắn nhãn "ước tính" vào các metric trong card; chưa thay bằng số thật |

## 3b. Phase 4 — Prototype

- **AI giúp:** viết `SYSTEM_PROMPT` (vai trò, Rule 1 `[DRAFT_ONLY]`, Rule 2 pin < 5%, chống mạo danh/bịa dữ liệu), cài `evaluate_prompt()` bằng Gemini SDK, và viết thêm 2 adversarial test (mạo danh + prompt injection, ép bịa trạm).
- **Kết quả:** 4/4 test qua, autograder phần code 5.0/5.0.
- **AI/mô hình sai hoặc gặp sự cố:**
  - `gemini-2.5-flash` (mô hình đề yêu cầu) trả 404 với tài khoản này. Phải thêm dự phòng sang `gemini-3.5-flash`.
  - `gemini-3.8-flash` (mô hình lỗi 404 gợi ý) bị quá tải 503 và làm script treo, nên không dùng được.
  - Một lần chạy có lỗi mạng ở test 1; chạy lại thì qua.
  - Starter-code ban đầu kiểm tra yếu: chỉ cần chuỗi "cứu hộ" xuất hiện là coi như qua. Tôi siết lại để Rule 2 yêu cầu đúng `dispatch_mobile_charger`, và Rule 1 yêu cầu output *bắt đầu* bằng `[DRAFT_ONLY]`.
- **Giới hạn:** chỉ 4 test, một lần chạy, temperature 0. Chưa chứng minh ranh giới vững trước mọi cách tấn công. Ranh giới trong code là của bài Xanh SM (theo starter và autograder), không phải bài Vinmec của tôi.

## 4. Những gì tôi chưa kiểm chứng (cần làm tiếp)

- Chưa có số thật của Vinmec: số hồ sơ chuyển tuyến mỗi ngày, thời gian nhập thực tế, tỷ lệ lỗi nhập, tỷ lệ xuất toán BHYT, tỷ lệ no-show.
- Chưa xác minh lỗi nhập hay lỗi chỉ định mới là nguyên nhân chính gây xuất toán.
- Chưa kiểm tra quy định dữ liệu cá nhân khi gửi hồ sơ bệnh nhân vào LLM bên ngoài.

## 5. Bài học

- AI rất tốt trong việc dựng khung và chỉ ra điểm yếu, nhưng **số liệu và kết luận phải do mình kiểm chứng**.
- Một ý tưởng nghe hấp dẫn (dùng LLM nhập liệu) có thể thua rule-based khi bị hỏi "AI có thực sự bắt buộc không?".
- Dùng AI để phản biện chính ý tưởng của mình (CFO test) cho kết quả hữu ích hơn dùng AI để tạo ý tưởng.
