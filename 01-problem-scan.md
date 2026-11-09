

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
| 1 | Vinhomes| Tốn thời gian      | Tiếp nhận và phân tuyến phản ánh cư dân           |
| 2 | Xanh SM | Pain từ người khác | Phân tích lí do hủy chuyển của khách hàng         |
| 3 | Xanh SM | Lặp lại            | Tự động phân loại & gỡ phạt đánh giá 1-sao cho    |
|                                  | tài xế                                            |
| 4 | Vinfast | Pain từ người khác | Chuản đoán lỗi xe từ mô tả Tiếng Việt của khách   |
| 5 | Vinhomes| AI upgrade         | Tỷ lệ báo động giả an ninh/PCCC                   |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                  │
│                                                             │
│ Bài toán (1 câu): Chuản đoán lỗi xe từ mô tả Tiếng Việt của khách│
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên kĩ thuật và CHSK             │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│    1. Khách hàng phản ánh lỗi xe và mô tả                    |  
|    2. Nhân viên CSKH tiếp nhận phản ánh                      |
|    3. Nhân viên CSKH đặt câu hỏi để khoanh vùng lỗi          |
|    4. Nhân viên CSKH đề xuất kiểm tra                        |
|    5. Xác minh lỗi và chuyển cho nhân viên kĩ thuật sửa chữa |
│ Bước nào tốn thời gian/lỗi nhất? Bước 5 (⏱ 30-60 phút/lượt) |
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3                │
│ (Khoanh vùng và dự đoán lỗi để hỗ trợ cho nhân viên kĩ thuật)│
│ Đo thành công bằng gì (Metric có số)?                        |
|  Giảm thời gian xử lý ở bước 3 từ 10 phút về dưới 5 phút     |
|  Phản ánh đúng giúp thời gian xác minh lỗi giảm còn 10-15 phút│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘



┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                  │
│                                                             │
│ Bài toán (1 câu): Tiếp nhận và phân tuyến phản ánh cư dân   │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên tiệp nhận                    │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân phản ánh ──> 2. Nhân viên tiếp nhận phản ánh    |
|  ──> 3. Đọc và phân loại phản ánh  ──> 4. Xác định bộ phận  |
|  phụ trách ──> 5. Gửi phản ánh đến bộ phận xử lý            │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 (⏱ 5-7 phút/lượt)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4-             │
│ (Tự đông phân loại phản ánh và xác định bộ phận phụ trách ) │
│ Đo thành công bằng gì (Metric có số)? 
|  Giảm thời gian xử lý ở bước 3-4 từ 7-10 phút về dưới 3 phút│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                  │
│                                                             │
│ Bài toán (1 câu): Tự động phân loại & gỡ phạt đánh giá 1-sao|
| cho tài xế (ý tưởng nhóm)                                   │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Bộ phận tiếp nhận và xử lý tố cáo      │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Tiếp nhận tố cáo                                        |
|    2. Kiểm tra thông tin tài xế và khách hàng                |
|    3. Phân loại phiếu tố cáo                                 |
|    4. Gửi cho bộ phận xử lý tố cáo                           |
|    5. Bộ phận xử lý tố cáo tiếp nhận                         |
|    6. Bộ phânh xử lý điều tra và xác minh vi phạm            |
|    7. Xử lý gỡ đánh giá 1-sao nếu không vi phạm              |
|                                                              |
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 6 (⏱ 15-45 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 3 bước 6 bước 7  |      
│ (Tự động phân loại phiếu tố cáo và tự động điều tra xác     |
|minh vi phạm )                                               │
│ Đo thành công bằng gì (Metric có số)? 
|  Giảm thời gian xử lý ở bước 3  từ 5 phút về dưới 2 phút    │
│  Giảm thời gian xác minh vi phạm từ 15-45 phút về dưới 15 phút
│                                                             |
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [X] Agent │
└─────────────────────────────────────────────────────────────┘



