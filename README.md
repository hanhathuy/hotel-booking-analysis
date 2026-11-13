# Nhom08 — Bài thực hành dữ liệu đặt phòng khách sạn

## 1. Thành viên và phân công

| Họ tên | MSSV | Phụ trách chính | Có thể giải thích thêm |
|---|---|---|---|
| Hà Nhật Huy  | 11253519 | A1-A5, B1-B4, C1-C3, D1-D5(Câu 1 - Câu 5) | Bonus 2 |
| Lê Minh Quân | 11257109 | A6, B5-B8, C4-C4+, D6-D10(Câu 6 - Câu 10) | Bonus 1 |
| | | | |

## 2. Cách chạy lại

1. Giải nén `Nhom10_KhachSan.zip`, giữ nguyên cấu trúc thư mục.
2. Mở `hotel_analysis.ipynb` tại thư mục gốc; chọn **Restart & Run all**.
3. Kết quả được ghi vào `outputs/` (5 tệp dữ liệu, `tables/`, `figures/`).

Thời gian chạy toàn bộ trên máy nhóm: khoảng … giây.

## 3. Môi trường và thư viện

| Thành phần | Phiên bản |
|---|---|
| Python | 3.14.2 |
| pandas | 3.0.1 |
| matplotlib | 3.11.1 |
| seaborn (nếu dùng) | 0.13.2  |
| Môi trường | Google Colab và VS Code |

## 4. Nguồn dữ liệu

Antonio, N., de Almeida, A., & Nunes, L. (2019). Hotel booking demand datasets. *Data in Brief*, 22, 41–49. https://doi.org/10.1016/j.dib.2018.11.126 (CC BY 4.0).
Bản biên soạn cho học tập `hotel_bookings_dirty.csv` (10.325 dòng × 24 cột).

## 5. Quy tắc riêng của nhóm (khác hoặc bổ sung so với đề)

| # | Tình huống | Quy tắc nhóm chọn | Lý do | Ảnh hưởng đến kết quả |
|---|---|---|---|---|
| 1 | Xung đột giá trị khi gộp cùng booking_id | Mỗi cột lấy giá trị không thiếu đầu tiên theo `row_id` tăng dần; khi hai giá trị khác nhau thì giữ giá trị của dòng `row_id` nhỏ nhất; ghi cột xung đột vào `row_lineage.note` | Không có bằng chứng bản trích xuất nào đáng tin hơn; quy tắc xác định, tái lập được, tốt hơn lấy trung bình | 15 booking_id xung đột (`lead_time` 7, `adr` 8; 0,15%); 22 ô thiếu được điền từ dòng trùng |
| 2 | Dòng trùng tuyệt đối bị loại ở B1 | Gán trạng thái `merged` trong `row_lineage` (không dùng `dropped`); cột `note` chỉ ghi xung đột | Dòng trùng là bản sao của dòng được giữ, không mất thông tin | `row_lineage` phủ đủ 10.325 `row_id`: 10.000 `kept` + 325 `merged` |
| 3 | Giá trị hiếm nhưng nằm trong miền hợp lệ | Giữ trong bảng sạch và báo cáo riêng: ADR = 0 (132 đặt phòng), 0 đêm (38), 0 khách (10), `lead_time` tối đa 709 ngày | Có thể là phòng tặng hoặc đặt phòng đặc biệt; xóa là suy đoán | Câu 8 loại ADR = 0 khỏi thống kê giá (120 đặt phòng không hủy) nhưng nêu số riêng; Câu 9 giữ 37 đặt phòng 0 đêm |
| 4 | Cách đếm trong `cleaning_log` | `affected_rows` = số dòng thỏa quy tắc phát hiện; `changed_cells` = số ô thực sự đổi giá trị; không cộng `affected_rows` giữa các quy tắc | Một dòng có thể thỏa nhiều quy tắc; một số ô đã trống từ trước nên không phải do quy tắc tạo ra | Ví dụ B5 `arrival_date`: 49 dòng thỏa quy tắc, trong đó 25 ô đã trống nên 24 ô đổi giá trị |

## 6. Danh sách tệp nộp

- [ ] `hotel_analysis.ipynb`
- [ ] `Bao_cao_Khach_san.pdf` hoặc `.docx`
- [ ] `data/hotel_bookings_dirty.csv`
- [ ] `outputs/` — 5 tệp dữ liệu
- [ ] `outputs/tables/` — 14 bảng
- [ ] `outputs/figures/` — 8 hình
