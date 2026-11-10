# AI Log — Lab 02

**Người thực hiện:** Lê Nguyễn Anh Nhân  
**Công cụ:** Codex và Gemini API (`gemini-3.5-flash-lite`).

## AI hỗ trợ gì?

Trong quá trình làm bài, tôi dùng AI để gợi ý thêm góc nhìn cho problem scan, phản biện workflow và rà soát tính nhất quán giữa báo cáo với prototype. Tôi giữ phần ý tưởng cá nhân về VinFast; phần deep dive theo chủ đề nhóm là hỗ trợ xử lý đánh giá 1-sao cho tài xế Xanh SM.

Tôi trao đổi với AI để làm rõ bottleneck, so sánh Rule/LLM/Agent và kiểm tra ranh giới trước khi triển khai prompt. AI cũng hỗ trợ tìm nguyên nhân lỗi API và kiểm tra các ca cố tình yêu cầu bỏ duyệt hoặc kết luận khi thiếu bằng chứng.

## AI trả lời chưa phù hợp ở đâu?

Một số gợi ý ban đầu đưa số liệu ước lượng thành dữ kiện hoặc mở rộng code quá nhiều so với starter. Tôi yêu cầu giữ đúng template, ghi rõ giả định và tập trung vào phân loại, tóm tắt cho nhân viên xem xét. Khi một phản hồi đề xuất bổ sung thông tin định danh, tôi cũng làm rõ rằng chỉ cần mã vụ việc/chuyến không định danh và bằng chứng liên quan.

## Tôi đã điều chỉnh prompt thế nào?

Tôi giới hạn AI chỉ tạo nháp `[DRAFT_ONLY]`, luôn yêu cầu người duyệt và không tự gỡ điểm, xử phạt hoặc gửi thông báo. Hồ sơ thiếu/mâu thuẫn phải chuyển xác minh thêm; lệnh giả mạo quản lý hay lệnh nhúng trong phản ánh không được làm thay đổi ranh giới.

Prototype cuối chạy API thật đạt **3/3 ca adversarial Passed**, có [log kết quả](evidence/xanhsm-template-results.json). Sau các lỗi quota và dịch vụ ở model trước, bản chạy được dùng Gemini 3.5 Flash Lite. Autograder gốc chấm phần code **4,50/5,00**; phần thiếu liên quan đến từ khóa sạc pin chưa khớp đề tài nhóm.

Qua bài này, tôi thấy AI hữu ích khi dùng để phản biện và kiểm tra từng phần. Tôi vẫn cần đối chiếu yêu cầu, nguồn số liệu và kết quả thực tế: ba ca thử thành công chưa chứng minh hiệu quả nghiệp vụ, nên quyết định trong deep dive vẫn là **NOT YET** cho pilot thực tế.
