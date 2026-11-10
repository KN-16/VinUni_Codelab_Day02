# 🏗️ Phase 3 — DEEP-DIVE (Nhóm)

## 3.1. Current-State Workflow Mapping

```text
+----------------------------------------------------------------------------------------------------+
| QUY TRÌNH XỬ LÝ ĐÁNH GIÁ 1-SAO & KHIẾU NẠI CỦA TÀI XẾ XANH SM (GSM) HIỆN TẠI                       |
+----------------------------------------------------------------------------------------------------+

  [ Khách hàng đánh giá 1 sao (có text note hoặc không) ]
                           |
                           v
+-------------------------------------------------------+
| Bước 1: Hệ thống tự động phạt                         |
| - Điểm sao tài xế lập tức bị trừ                      |
| - Tạm đình chỉ quyền nhận cuốc thưởng tuần            |
| ⏱ Thời gian: Tức thì (Real-time)                      |
| 🔄 Handoff: Rating Engine -> Tài xế                   |
+-------------------------------------------------------+
                           |
                           v
+-------------------------------------------------------+
| Bước 2: Tài xế gửi khiếu nại (Dispute Appeal)         |
| - Tài xế mở Driver App, chọn chuyến đi, gõ giải trình |
| - Nêu lý do: kẹt xe ngập đường / khách giận vô cớ /   |
|   lỗi app không nhận tiền / khách đổi ý hủy cuốc      |
| ⏱ Thời gian: 10 phút                                  |
| 🔄 Handoff: Tài xế -> Hệ thống Ticket CSKH GSM        |
+-------------------------------------------------------+
                           |
                           v
+-------------------------------------------------------+
| Bước 3: Nhân viên Vận hành mở hồ sơ đối soát          |
| 🔴 BOTTLENECK CỰC LỚN (Tốn 10 - 15 phút/ticket)      |
| - Đọc nội dung khiếu nại của tài xế                   |
| - Đọc ghi chú đánh giá của khách (nếu có)             |
| - Mở tab Telemetry xem lộ trình GPS thực tế           |
| - Kiểm tra thời gian tài xế chờ tại điểm đón          |
| - Tra cứu log thời tiết mưa ngập / bản đồ tắc đường   |
| - Kiểm tra log tin nhắn chat / cuộc gọi trong app     |
| ⏱ Thời gian: 10–15 phút/ticket                        |
| 🔄 Handoff: Điều phối viên Fleet Ops độc lập xử lý    |
+-------------------------------------------------------+
                           |
                           v
+-------------------------------------------------------+
| Bước 4: Đánh giá & Phê duyệt thủ công                 |
| 🔴 BOTTLENECK                                         |
| - Phân định: Lỗi tài xế (thái độ, lái ẩu) HAY         |
|   Lỗi khách quan (thời tiết, giao thông, lỗi app)     |
| - Nhập lý do vào form phê duyệt gỡ phạt               |
| ⏱ Thời gian: 3–5 phút                                 |
| 🔄 Handoff: Nhân viên Vận hành -> Trưởng nhóm duyệt   |
+-------------------------------------------------------+
                           |
                           v
+-------------------------------------------------------+
| Bước 5: Cập nhật chỉ số & Thông báo                   |
| - Hệ thống hoàn lại điểm sao cho tài xế               |
| - Mở lại chế độ xét thưởng tuần                       |
| - Bắn notification giải thích cho tài xế              |
| ⏱ Thời gian: 24h - 72h sau khi nộp đơn khiếu nại      |
| 🔄 Handoff: Hệ thống -> Tài xế                        |
+-------------------------------------------------------+

🔴 Bottlenecks chính: Bước 3 và Bước 4 (Chiếm 15–20 phút thời gian nhân viên, tồn đọng 2–3 ngày).
⏱ Tổng SLA xử lý khiếu nại hiện tại: 48 – 72 giờ từ khi tài xế gửi yêu cầu.
```

---

## 3.2. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Đội ngũ Quản trị Vận hành Tài xế (Fleet Driver Operations) và Chuyên viên Xử lý Khiếu nại (Driver CS Specialist) tại GSM - Xanh SM. |
| **2. Current Workflow** | Khi tài xế nhận 1-sao, hệ thống tự động trừ điểm rating tức thì. Tài xế bị giảm thứ hạng nhận chuyến và mất thưởng chu kỳ tuần. Nếu thấy bị đánh giá oan, tài xế bấm khiếu nại trên app. Nhân viên vận hành phải truy cập đồng thời 4 màn hình: Telemetry GPS, nhật ký sự kiện cuốc xe (Trip Logs), lịch sử chat/gọi trên app, và dữ liệu thời tiết/giao thông thực tế để thẩm định tính xác thực trước khi bấm gỡ phạt thủ công. |
| **3. Bottleneck** | **Bước 3 & 4 (Thu thập chứng cứ đối soát và thẩm định nguyên nhân):** Mỗi ngày hệ thống tiếp nhận hàng ngàn đánh giá 1-sao trên toàn quốc. Hơn 65% trường hợp xuất phát từ yếu tố khách quan (mưa bão ngập lụt, tắc đường giờ cao điểm, khách say xỉn bùng cuốc, lỗi kết nối 4G/App GSM). Việc nhân viên phải đối soát thủ công từng dòng dữ liệu GPS và chat-log khiến thời gian xử lý kéo dài từ 2 đến 3 ngày, tạo nên bức xúc tột độ cho đội ngũ tài xế. |
| **4. Business Impact** | - **Chảy máu tài xế (Driver Churn):** Tài xế ức chế vì bị giam tiền thưởng tuần và giảm cuốc oan uổng dẫn đến tắt app hoặc chuyển sang nền tảng đối thủ (Grab/Be), làm giảm độ phủ xe giờ cao điểm.<br>- **Chi phí vận hành tăng vọt:** Đội ngũ Fleet Ops phải tăng ca liên tục để xử lý backlog khiếu nại sau các đợt mưa lớn hoặc dịp lễ tết.<br>- **Rủi ro truyền thông nội bộ:** Các hội nhóm cộng đồng tài xế xuất hiện nhiều bài viết tiêu cực về tính thiếu công bằng của thuật toán đánh giá GSM. |
| **5. Success Metric** | **1. Tốc độ xử lý (SLA Resolution):** Giảm thời gian xử lý khiếu nại 1-sao từ 72 giờ xuống **dưới 3 phút** đối với các trường hợp đủ điều kiện gỡ phạt tự động.<br>**2. Tỉ lệ tự động hóa (Automation Rate):** Tự động phân loại và giải quyết thành công **$\ge 70\%$** tổng số ticket khiếu nại 1-sao hợp lệ mà không cần nhân viên can thiệp thủ công.<br>**3. Độ chính xác (Precision):** Tỉ lệ gỡ phạt sai (False Positive - xóa án cho tài xế thực sự vi phạm quy chuẩn ứng xử) **$\le 1.5\%$**.<br>**4. Độ hài lòng của tài xế (Driver Satisfaction - CSAT):** Tỉ lệ hài lòng về tính công bằng và tốc độ phản hồi khiếu nại đạt **$\ge 90\%$**. |
| **6. Operational Boundary** | **AI ĐƯỢC PHÉP:**<br>- Trích xuất lý do review của khách hàng (bằng LLM NLP) và đối chiếu với dữ liệu GPS/Telemetry chuyến đi.<br>- Tự động quyết định gỡ phạt đánh giá 1-sao và khôi phục điểm rating ngay lập tức khi phát hiện bằng chứng khách quan xác thực (VD: Khách chê đón chậm nhưng log chứng minh xe kẹt do ngập lụt cục bộ; khách chê cước đắt do phụ phí đã báo trước; khách hủy cuốc đánh xấu vô cớ).<br>- Tự động gửi thông báo giải trình minh bạch cho tài xế qua Driver App.<br>**AI TUYỆT ĐỐI CẤM (Hard Boundary):**<br>- Tuyệt đối **KHÔNG** tự động gỡ phạt đối với các khiếu nại liên quan đến: Phản ánh quấy rối tình dục, bạo lực ngôn từ/thể xác, say xỉn khi lái xe, gian lận cước phí hoặc tai nạn giao thông (Các ca này phải chuyển ngay sang Human Investigation - Phòng Thanh tra Nội bộ GSM).<br>- Tuyệt đối **KHÔNG** khóa vĩnh viễn tài khoản tài xế chỉ dựa trên phán đoán của AI nếu chưa có xác nhận từ Hội đồng kỷ luật Vận hành. |

---

## 3.3. Future-State Flow & AI Fit

### Phân tích AI Fit:
* [ ] **Pure Rule-based:** Không đủ năng lực. Nhận xét của khách hàng và giải trình của tài xế là ngôn ngữ tự nhiên phi cấu trúc (teencode, viết tắt, tiếng lóng, ẩn dụ, mỉa mai).
* [x] **LLM Feature + Rule-Verification (Hybrid Model):** **Lựa chọn tối ưu nhất.** Sử dụng LLM để đọc hiểu nội dung đánh giá và bản giải trình, sau đó sinh cấu trúc kiểm tra (check-condition). Hệ thống Rule Engine tự động query log viễn thông (GPS, time-stamps, payment status) để xác thực. Nếu trùng khớp, hệ thống tự động gỡ phạt; nếu rơi vào ranh giới xám (mơ hồ), chuyển sang chuyên viên kiểm tra (HITL).
* [ ] **Autonomous Agent:** Chưa phù hợp để thả tự do thực hiện các action nhạy cảm liên quan đến tài chính thưởng/phạt tài xế nếu không có cơ chế chặn ranh giới pháp lý.

### Quy trình tương lai (Future-State Flow):


```text
+----------------------------------------------------------------------------------------------------+
| FUTURE-STATE WORKFLOW: XANH SM AUTO 1-STAR DISPUTE RESOLVER & PENALTY WAIVER                       |
+----------------------------------------------------------------------------------------------------+

  [ Tài xế bấm "Khiếu nại đánh giá 1-sao" trên Xanh SM Driver App ]
                                  |
                                  v
+-------------------------------------------------------------------+
| 1. Auto Data Aggregator (Hệ thống tự động gom dữ liệu)           |
| - Thu thập feedback khách + giải trình tài xế                     |
| - Trích xuất Telemetry chuyến: GPS route, vận tốc, thời gian chờ, |
|   tình trạng thời tiết (API Khí tượng), dữ liệu ùn tắc (Here/GG)  |
| ⏱ Thời gian: 2 giây                                               |
+-------------------------------------------------------------------+
                                  |
                                  v
+-------------------------------------------------------------------+
| 2. 🔵 AI Step: LLM Context Analyzer & Fact Checking Engine        |
| - Gemini 2.5 Flash phân loại lý do đánh giá thành 4 nhóm:         |
|   (A) Lỗi khách quan (thời tiết, giao thông, lỗi server app)      |
|   (B) Lỗi hành vi nghiêm trọng (quấy rối, gian lận, bạo lực)      |
|   (C) Lỗi kỹ năng/thái độ tài xế (lái ẩu, thái độ cộc cằn)        |
|   (D) Đánh giá ảo / Spam / Không có nội dung                      |
| - Đối chiếu chéo sự thật (Fact-check) giữa giải trình và telemetry|
| ⏱ Thời gian: 3 - 5 giây                                           |
+-------------------------------------------------------------------+
                                  |
               +------------------+------------------+
               |                                     |
               v                                     v
   [ Nhóm A hoặc D & Đủ chứng cứ ]      [ Nhóm B hoặc Nhóm C hoặc Nghi vấn ]
               |                                     |
               v                                     v
+------------------------------------+ +------------------------------------+
| 3a. 🟢 Auto-Action: Gỡ phạt ngay   | | 3b. 🟠 Human-in-the-loop (HITL)    |
| - Tự động xóa điểm 1-sao           | | - Ticket đẩy về màn hình Fleet Ops |
| - Khôi phục rating và quyền thưởng | | - AI tóm tắt ngắn & đề xuất hướng  |
| - Bắn thông báo minh oan cho tài xế| | - Chuyên viên chỉ cần 1-click duyệt|
| ⏱ Thời gian: Tức thì (< 10 giây)   | | ⏱ Thời gian: 2-3 phút              |
+------------------------------------+ +------------------------------------+
               |                                     |
               +------------------+------------------+
                                  |
                                  v
+-------------------------------------------------------------------+
| 4. Hệ thống ghi nhận Feedback Loop                                |
| - Cập nhật dữ liệu về mô hình để tinh chỉnh độ chính xác phân loại|
| - Báo cáo thống kê điểm mù hạ tầng (khu vực ngập, lỗi sóng GPS)   |
| ⏱ Thời gian: Real-time                                            |
+-------------------------------------------------------------------+

⏱ Tổng thời gian xử lý: 
- Đối với ca rõ ràng (70%): < 30 giây (Giảm 99.9% so với 72 giờ).
- Đối với ca cần người duyệt (30%): < 15 phút.
```

---

# 🏁 Phase 5 — EVALUATE (Nhóm)

### AI Readiness Checklist:
1. [x] **Dữ liệu mẫu/logs sạch:** GSM sở hữu 100% dữ liệu xe điện viễn thông (telemetry), dữ liệu GPS độ chính xác cao từ thiết bị VinFast trên xe, lịch sử chuyến đi và bản ghi chat trong ứng dụng.
2. [x] **Rủi ro trong tầm kiểm soát:** Thiết lập Hard Boundary rõ ràng: Mọi phản ánh liên quan đến an toàn hành khách, quấy rối, gian lận lập tức chuyển sang điều tra thủ công; chỉ auto-waive các ca có dữ liệu khách quan chứng minh rõ ràng.
3. [x] **Stakeholders sẵn sàng thay đổi:** Khối Vận hành Tài xế GSM đang quá tải nghiêm trọng; tài xế mong mỏi một cơ chế minh oan nhanh chóng, công bằng và minh bạch.

### Quyết định cuối cùng của Ban Quản Trị Dự Án Vận Hành GSM:

# [x] GO (Bắt đầu xây dựng Prototype thử nghiệm tại Đội xe Xanh SM Taxi Hà Nội)

### Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):
1. **Giá trị vận hành & Giữ chân đối tác (ROI):** Xử lý tự động ~70% trong số hàng ngàn khiếu nại đánh giá xấu mỗi ngày giúp giảm ngay lập tức 5–7 nhân sự trực đối soát thủ công tại mỗi trung tâm vận hành. Quan trọng hơn, việc minh oan tức thì giúp tài xế yên tâm chạy xe, hạn chế tình trạng tài xế ức chế bỏ cuốc, giảm tỷ lệ nghỉ việc (churn rate) ước tính 12%, tiết kiệm hàng tỷ đồng chi phí tuyển dụng và đào tạo tài xế mới.
2. **Tính khả thi công nghệ cao:** Dữ liệu hành trình xe điện VinFast rất giàu thông tin (tốc độ, gia tốc phanh, vị trí trạm dừng, trạng thái điều hòa xe). Gemini 2.5 Flash cực kỳ mạnh trong việc hiểu tiếng Việt đời thường, phát hiện cảm xúc và đối chiếu logic sự thật với JSON Telemetry. Chi phí API cho mỗi ticket chỉ khoảng 100–200 VND, thấp hơn 100 lần so với chi phí nhân viên vận hành bỏ ra 15 phút đối soát.
3. **An toàn kiểm soát rủi ro:** Hệ thống có ranh giới phân tách rõ ràng: Các ca tranh chấp nhạy cảm luôn được gán tag cảnh báo để con người xử lý (HITL), đảm bảo tuyệt đối không có sự cố bỏ lọt tài xế có hành vi nguy hiểm.