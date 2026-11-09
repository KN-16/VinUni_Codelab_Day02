
Tự động phân loại và gỡ đánh giá 1-sao cho tài xế Xanh SM
# Problem Statement (6-field) — Vin Smart Future Standard
- Actor / Operator: Nhân viên tiếp nhận vi phạm và nhân viên xử lý vi phạm
- Current Workflow: Khi khách hàng đánh giá 1-sao, nhân viên tiếp nhận vi phạm ghi nhận thông tin, dựa trên thông tin sẽ phân loại phiếu tố cáo và gửi cho bên xử lí vi phạm. Nhân viên xử lí vi phạm tiếp nhận thông tin, dựa trên đó điều tra và ghi nhận xem có sai phạm không, nếu không sẽ gỡ đánh giá 1-sao cho tài xế.
- Bottleneck: ở bước 3 và 6, việc phân loại phiếu tố cáo thủ công và việc điều tra vi phạm thủ công sẽ gây mất nhiều thời gian (bước 3: 3-5 phút và bước 6: 15-45 phút)
- Bussines impact: Xanh SM có khoảng 70000 tài xế với 1 triệu chuyến 1 ngày(toàn bộ hệ sinh thái), lượng đánh giá xấu 1 ngày rơi vào khoảng 0.5% = 5000 lượt. Mỗi lượt xử lý 1-sao trung bình sẽ 45 phút. Trong 45 phút đó tài xế có thể thực hiện được 1 chuyến xe. Ngoài ra còn làm mất tổng cộng khoảng 3750 giờ của bên Tiếp nhận và xử lý vi phạm.
- Success Metric:
       + Giảm thời gian đánh giá và xử lí vi phạm xuống dưới 15 phút
       + Tỉ lệ đánh giá chính xác đạt 98%
- Operational Boundary: AI chỉ được phép truy cập vào thông tin khách hàng và tài xế phản ánh, không truy cập vào thông tin cá nhân định danh của cả tài xế và khách hàng. AI chỉ được phân loại và đánh giá vi phạm tài xế không được quyền tự ý gỡ điểm đánh giá cho tài xế mà cần xác nhận của nhân viên


# AI Fit

* **AI Fit:** AI Agent vì AI Agent tiếp nhận thông tin phản ánh, tự phân loại và tự đánh giá vi phạm

# AI Readiness Checklist:
[ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
[x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
[x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?
# Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[ ] GO (Bắt đầu xây dựng Prototype): Bắt đầu phát triển với scope hẹp. [x] NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline): Trì hoãn để chuẩn bị thêm. [ ] NO-GO (Không khả thi / Rule-based tốt hơn): Hủy bỏ dự án AI này.