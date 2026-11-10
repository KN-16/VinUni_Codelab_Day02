# NHẬT KÝ TƯƠNG TÁC THỰC TẾ VỚI AI

* **Trợ lý AI sử dụng:** Gemini (trực tiếp trong phiên làm việc)
* **Chủ đề phiên làm việc:** Deep-dive bài toán: *Tự động phân loại & gỡ phạt đánh giá 1-sao cho tài xế Xanh SM (GSM)*.

---

## 1. AI đã hỗ trợ tôi những gì?

* **Định vị điểm đau đặc thù của Xanh SM:** AI hỗ trợ phân tích sâu mô hình vận hành tài xế taxi công nghệ/xe điện của GSM, nhận diện rõ điểm nóng giữa việc đảm bảo tiêu chuẩn dịch vụ 5-sao khắt khe với tình trạng tài xế bị phạt oan do các yếu tố bất khả kháng (kẹt xe mưa bão, khách say xỉn bùng cuốc, lỗi mạng).
* **Chuẩn hóa khung 6-Field Problem Statement:** Điền thông tin cụ thể vào 6 trường cốt lõi, lượng hóa các chỉ số thành công (giảm SLA từ 72h xuống < 30 giây cho ca tự động, độ chính xác $\ge 98.5\%$).
* **Xây dựng sơ đồ quy trình trực quan:** Chuyển đổi toàn bộ logic phức tạp từ quy trình đa màn hình hiện tại (GPS log, Chat log, Weather API) sang sơ đồ text/ASCII rõ ràng, so sánh trực diện Current-State vs Future-State.
* **Thiết kế cơ chế Hybrid AI (LLM + Telemetry Rules):** AI giúp định hình cấu trúc kiến trúc kết hợp: dùng LLM xử lý ngôn ngữ tự nhiên từ feedback của khách hàng và giải trình của tài xế, sau đó dùng Rule-verification đối chiếu chéo với dữ liệu viễn thông xe điện VinFast.

---

## 2. AI trả lời sai, hallucination và lệch hướng ở đâu?

* **Rủi ro buông lỏng kiểm soát an toàn (Over-automation):** Ban đầu, AI có xu hướng đề xuất tự động hóa 100% tất cả các ca 1-sao để tối đa hóa tốc độ, thậm chí cho phép AI tự động hủy cả phản ánh của khách về tài xế thô lỗ hay chạy ẩu. Đây là rủi ro cực lớn làm sụp đổ uy tín dịch vụ cao cấp của Xanh SM.
* **Đề xuất kỹ thuật quá phức tạp:** AI từng gợi ý huấn luyện mô hình phân tích giọng nói real-time từ micro trong xe để nghe lén hội thoại giữa khách và tài xế. Ý tưởng này vừa tốn kém tài nguyên tính toán vừa vi phạm nghiêm trọng quyền riêng tư của hành khách.

---

## 3. Tôi đã can thiệp, nắn prompt và siết ranh giới ra sao?

* **Thiết lập Hard Operational Boundary:** Tôi yêu cầu bổ sung ngay ranh giới cấm nghiêm ngặt: Tuyệt đối không cho phép AI tự động gỡ phạt đối với các khiếu nại liên quan đến an toàn, quấy rối, bạo lực ngôn từ, say xỉn hoặc tai nạn giao thông; các ca này bắt buộc phải điều hướng về thanh tra con người (HITL).
* **Kéo giải pháp về tính khả thi thực tế:** Bác bỏ ý tưởng phân tích âm thanh trong xe; ép AI tập trung vào nguồn dữ liệu có sẵn và đáng tin cậy nhất của GSM: Dữ liệu viễn thông xe điện VinFast (Telemetry GPS, tốc độ, dừng đỗ), log chat trong app và API thời tiết/giao thông đô thị.
* **Yêu cầu tập trung văn phong nghiệp vụ:** Ra lệnh loại bỏ toàn bộ các câu chữ cảm thán, văn phong tiếp thị; chỉ giữ lại cấu trúc báo cáo chặt chẽ, số liệu đo lường cụ thể và logic phản hồi có tính thực thi cao.

---

## 4. Bài học đúc kết

* **Công nghệ phải đi cùng tính công bằng và an toàn thương hiệu:** Tự động hóa gỡ phạt cho tài xế không chỉ là bài toán giảm tải chi phí CSKH, mà là bài toán bảo vệ niềm tin của người lao động thực địa trong khi vẫn phải giữ vững chuẩn mực dịch vụ thương hiệu.
* **Sức mạnh của mô hình Hybrid:** LLM rất giỏi trong việc phân loại ngữ nghĩa và bóc tách lý do khiếu nại, nhưng cần phải có dữ liệu viễn thông khách quan (Ground Truth Data) làm chốt chặn xác thực để tránh việc tài xế lợi dụng AI nhằm gian lận trốn phạt.