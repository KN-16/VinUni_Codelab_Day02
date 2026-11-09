# Báo cáo Deep-Dive: Tự động phân loại và thẩm định gỡ phạt đánh giá 1-sao cho tài xế Xanh SM
* **Bài toán:** 1.1 Tự động phân loại & đề xuất gỡ phạt đánh giá 1-sao cho tài xế

---

## 1. Quyết định lựa chọn đề tài của Nhóm B6

Sau vòng Scan cá nhân, nhóm B6 đã tổng hợp danh sách các bài toán từ 4 mảng hoạt động:

1. **Xanh SM:**
   * 1.1 Tự động phân loại & gỡ phạt đánh giá 1-sao cho tài xế.
   * 1.2 Chấm điểm ưu tiên điều xe cứu hộ sạc pin lưu động.
2. **Vinhomes:**
   * 2.1 Lọc, phân loại và chuyển tiếp phản ánh cư dân trên app Vinhomes Resident.
   * 2.2 Trợ lý ảo giải đáp thủ tục hành chính nội khu.
3. **VinFast:**
   * 3.1 Hỗ trợ Cố vấn dịch vụ (SA) tra cứu tài liệu kỹ thuật tại xưởng.
4. **Vinmec:**
   * 4.1 Trích xuất thông tin giấy chuyển tuyến vào hệ thống HIS.

**Thống nhất của nhóm:** Chọn đề tài **1.1 (Xanh SM - Xử lý khiếu nại đánh giá 1-sao)**.

**Lý do lựa chọn:**
* Đánh giá 1 sao ảnh hưởng trực tiếp đến thu nhập và điểm thưởng tuần của tài xế. Nhiều trường hợp khách vote 1 sao vì yếu tố khách quan: mưa ngập đường, kẹt xe giờ tan tầm, giá cước tự tăng theo thuật toán (Surge Pricing) hoặc app bắt sai vị trí đón.
* Bộ phận CSKH đang tốn quá nhiều thời gian đọc từng khiếu nại và tra cứu thủ công dữ liệu chuyến đi, dẫn đến thời gian giải quyết kéo dài (24-48 giờ).
* Bài toán kết hợp tốt giữa NLP (đọc hiểu review khách và giải trình tài xế) với kiểm tra dữ liệu hệ thống (GPS, lịch sử cước, log thời tiết).
* Ranh giới vận hành rõ ràng: các sự cố an toàn nghiêm trọng (uống rượu bia, chạy ẩu, thái độ đe dọa) bắt buộc tách riêng để người xử lý, AI không can thiệp.

---

## 2. Current-State Workflow Mapping (Quy trình vận hành hiện tại)

Quy trình xử lý một khiếu nại 1 sao từ tài xế hiện nay:

```text
[Bước 1: Khách vote 1 sao]
  │  (Khách chấm điểm trên app sau chuyến đi; hệ thống tự trừ điểm tài xế)
  ▼ 🔄 Handoff
[Bước 2: Tài xế gửi khiếu nại]
  │  (Tài xế viết giải trình, đính kèm ảnh chụp màn hình hoặc video dashcam trên app Driver)
  ▼ 🔄 Handoff
[Bước 3: CSKH đọc review và giải trình] 🔴 Bottleneck 1
  │  (CSKH đọc phản hồi của khách và đối chiếu với lời khai tài xế; tốn 10-15 phút/ca)
  ▼
[Bước 4: CSKH tra cứu dữ liệu chuyến đi] 🔴 Bottleneck 2
  │  (Mở dashboard nội bộ kiểm tra lộ trình GPS, lịch sử giá cước, thời tiết, log app; tốn 15-20 phút/ca)
  ▼
[Bước 5: Ra quyết định và phản hồi] 🔴 Bottleneck 3
  │  (CSKH chốt gỡ phạt hoặc giữ phạt, soạn tin giải thích gửi lại cho tài xế; tốn 5-10 phút/ca)
  ▼ 🔄 Handoff
[Kết thúc: Cập nhật điểm uy tín trên hệ thống]
```

* **🔴 Bottleneck:** Bước 3, 4, 5 ngốn thời gian do nhân viên phải đọc văn bản phi cấu trúc và chuyển qua lại giữa nhiều tab phần mềm tra cứu. Tổng thời gian thao tác tay mất 30-45 phút/ticket.
* **🔄 Handoff:** Điểm chuyển giao giữa Khách hàng -> App -> Tài xế -> Hàng đợi CSKH -> Hệ thống quản trị.
* **Thời gian xử lý thực tế (SLA):** Tài xế thường phải chờ 24 - 48 giờ mới có kết quả giải quyết.

---

## 3. Problem Statement (6-field)

| Trường thông tin | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên bộ phận CSKH và Thẩm định khiếu nại của Xanh SM (GSM). |
| **2. Current Workflow** | Khách vote 1 sao -> Tài xế gửi đơn khiếu nại qua app Driver -> CSKH đọc nội dung hai bên -> Tra cứu log GPS, biến động cước và thời tiết -> Ra quyết định gỡ/giữ phạt -> Soạn tin nhắn phản hồi cho tài xế. |
| **3. Bottleneck** | Bước đọc hiểu ngữ cảnh phản hồi (review viết tắt, cảm tính, nói mỉa mai) và bước tra cứu đối soát log phân tán giữa các hệ thống mất từ 30-40 phút cho mỗi trường hợp. |
| **4. Business Impact** | Mỗi ngày có hơn 1.000 ca khiếu nại 1 sao. Việc xử lý chậm trễ (tồn đọng 1-2 ngày) khiến tài xế bị tạm ngưng nhận cuốc hoặc mất thưởng tuần oan, gây ức chế tâm lý và làm tăng tỷ lệ tài xế tắt app chuyển sang hãng khác. |
| **5. Success Metric** | - Thời gian xử lý khiếu nại giảm từ 24-48 giờ xuống dưới 15 phút (AI hoàn thành trích xuất và đối soát sơ bộ dưới 5 giây).<br>- Tỷ lệ phân loại đúng lý do khiếu nại (giá cước, lỗi app, thời tiết, thái độ, vi phạm an toàn) đạt tối thiểu 90%.<br>- 100% khiếu nại liên quan đến an toàn (rượu bia, chạy ẩu, quấy rối) được chuyển ngay sang Phòng An toàn trong vòng 30 giây. |
| **6. Operational Boundary** | **AI được làm:** Đọc hiểu review và giải trình, đối soát log GPS/cước, phân loại nguyên nhân, tính độ tin cậy (confidence), gợi ý quyết định (recommend_removal, reject_removal, needs_human_review, escalate_safety_review), soạn sẵn bản nháp phản hồi.<br>**AI tuyệt đối không được làm:**<br>- Không tự ý gọi API cập nhật cơ sở dữ liệu để xóa phạt khi chưa có xác nhận từ nhân viên CSKH (bắt buộc `requires_human_approval: true`).<br>- Không đưa thông tin cá nhân của khách (tên, SĐT, địa chỉ nhà) vào nội dung giải thích gửi tài xế.<br>- Không thực thi các câu lệnh ẩn (prompt injection) nằm trong review của khách hay giải trình của tài xế. |

---

## 4. Future-State Flow & AI Fit

### Đánh giá mức can thiệp AI (AI-Fit)
* **Rule-based thuần:** Không thể đọc hiểu ngôn ngữ tự nhiên đa dạng, tiếng lóng hay giọng điệu bực bội của khách hàng.
* **Agentic Loop (Tự động hoàn toàn):** Không an toàn. Nếu để AI tự xóa án phạt trên database, hệ thống dễ bị tài xế lợi dụng gian lận hoặc bỏ lọt các tài xế có hành vi nguy hiểm.
* **Lựa chọn: LLM Feature kết hợp Rule Verification (Co-pilot có HITL):**
  * LLM đọc hiểu ngữ cảnh, trích xuất dữ liệu thành JSON và draft bản tóm tắt.
  * Rule engine tự kiểm tra chéo các mốc thời gian, tọa độ GPS và lịch sử giá cước, sau đó đẩy lên màn hình duyệt cho CSKH.

### Quy trình tương lai (Future-State Flow)

```text
[Bước 1: Tài xế nộp giải trình trên app]
  │
  ▼
[Bước 2: AI Engine đọc hiểu & tự động đối soát log]
  │  (LLM trích xuất intent + Rule engine check GPS, giá cước, thời tiết; mất 3-5s)
  │
  ├──> [Phát hiện vi phạm an toàn / nồng độ cồn] ──> Chuyển ngay Phòng An toàn xử lý
  │
  ▼
[Bước 3: AI trả về đề xuất JSON có cấu trúc]
  │  (Đưa ra phân loại, độ tin cậy, lý do đề xuất và draft phản hồi gửi tài xế)
  │
  ▼
[Bước 4: Nhân viên CSKH xem đề xuất và bấm duyệt]
  │  (CSKH kiểm tra nhanh trên dashboard, bấm Đồng ý hoặc Sửa; mất 30-60s)
  │
  ├──> [Trường hợp AI báo confidence thấp (< 0.75) hoặc thông tin mâu thuẫn]
  │     └──> ↩️ Fallback: Chuyển sang hàng đợi CSKH thẩm định thủ công như cũ.
  │
  ▼
[Bước 5: Hệ thống tự động cập nhật kết quả và gửi thông báo]
```

---

## 5. Evaluate: Đánh giá khả thi & Quyết định

### Checklist đánh giá:
1. [x] **Dữ liệu sẵn có:** Hệ thống Xanh SM lưu trữ đầy đủ dữ liệu cuốc xe: tọa độ GPS theo thời gian thực, lịch sử cuốc, log tăng giá cước và lịch sử giải trình các tháng trước.
2. [x] **Kiểm soát rủi ro:** Cơ chế Human-in-the-loop (HITL) được áp dụng ở khâu ra quyết định cuối cùng; các case an toàn được rẽ nhánh riêng.
3. [x] **Mức độ sẵn sàng của người dùng:** Cả tài xế lẫn đội ngũ CSKH đều có nhu cầu rút ngắn thời gian xử lý khiếu nại.

### Quyết định của nhóm: GO (Tiến hành xây dựng Prototype)

**Lý giải:**
* **Tính khả thi:** Chi phí token cho mỗi lượt phân tích văn bản ngắn rất thấp (vài chục đồng/lượt), hoàn toàn khả thi khi vận hành quy mô lớn.
* **Lợi ích vận hành:** Giảm thời gian thao tác của CSKH từ ~40 phút xuống còn 1-2 phút/ca. Giảm đáng kể tình trạng tài xế ức chế vì bị giam tiền thưởng hoặc tạm khóa tài khoản vô lý.
* **Kế hoạch thử nghiệm:** Chạy thử nghiệm ngầm (shadow testing) song song với quy trình cũ trên 500 ticket khiếu nại tại khu vực Hà Nội để đo độ chính xác của AI trước khi đưa vào sử dụng chính thức.
