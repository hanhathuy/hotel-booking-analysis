# NhomXX — Bài thực hành dữ liệu đặt phòng khách sạn

Làm sạch và phân tích 10.325 dòng đặt phòng của hai khách sạn (City Hotel – Lisbon, Resort Hotel – Algarve), giai đoạn ngày đến 07/2015 – 08/2017. Đơn vị phân tích: **1 đặt phòng = 1 `booking_id`**.

## 1. Thành viên và phân công

| Họ tên | MSSV | Phụ trách chính | Có thể giải thích thêm |
|---|---|---|---|
| Hà Nhật Huy | … | Thành viên A (TV-A) — Data Engineer & QA Lead: khảo sát A1–A3, làm sạch B1–B4, cột dẫn xuất C1, bảng tháng C2, xuất dữ liệu C3, Câu 1–5 (Hình 1–4), Báo cáo phần 1, 2, 4.1 | … |
| Lê Minh Quân | … | Thành viên B (TV-B) — Data Analyst & Storytelling Lead: khảo sát A4–A6, làm sạch B5–B8, kiểm tra C4 và rà soát chất lượng, Câu 6–10 (Hình 5–8), Báo cáo phần 3, 4.2, 5 | … |

Cả hai thành viên cùng chọn 6 insight, rà soát chéo mã và số liệu, và chuẩn bị trả lời vấn đáp toàn bộ quy trình.

## 2. Cách chạy lại

1. Giải nén `NhomXX_KhachSan.zip`, giữ nguyên cấu trúc thư mục (`data/`, `outputs/`, `tools/`).
2. Mở `hotel_analysis.ipynb` tại thư mục gốc; chọn **Restart & Run all**.
3. Kết quả được ghi vào `outputs/` (5 tệp dữ liệu, `tables/`, `figures/`).
4. Kiểm tra cấu trúc bài nộp: `python tools/kiem_tra_nop_bai.py .` (không còn ❌ mới nộp).

Thời gian chạy toàn bộ trên máy nhóm: khoảng … giây.

## 3. Môi trường và thư viện

| Thành phần | Phiên bản |
|---|---|
| Python | Yêu cầu ≥ 3.10; đã chạy lại không lỗi với 3.12.3 |
| pandas | Đã chạy và không lỗi với 2.2.3 |
| matplotlib | … |
| seaborn (nếu dùng) | … |
| Môi trường | Google Colab / Jupyter / VS Code (chọn một) |

## 4. Nguồn dữ liệu

Antonio, N., de Almeida, A., & Nunes, L. (2019). Hotel booking demand datasets. *Data in Brief*, 22, 41–49. https://doi.org/10.1016/j.dib.2018.11.126 (CC BY 4.0).
Bản biên soạn cho học tập `hotel_bookings_dirty.csv` (10.325 dòng × 24 cột).

## 5. Quy tắc riêng của nhóm (khác hoặc bổ sung so với đề)

| # | Tình huống | Quy tắc nhóm chọn | Lý do | Ảnh hưởng đến kết quả |
|---|---|---|---|---|
| 1 | Xung đột giá trị khi gộp cùng `booking_id` | Lấy giá trị của dòng đứng đầu nhóm (thứ tự `row_id` tăng dần); ô thiếu của dòng đó được điền từ dòng sau cùng nhóm | Đơn giản, xác định, chạy lại cho cùng kết quả | 144 `booking_id` có nhiều dòng sau B1; 15 `booking_id` có xung đột (`lead_time`: 7, `adr`: 8) |
| 2 | Dòng trùng tuyệt đối trên 23 cột nghiệp vụ | Loại ngay ở B1 (181 dòng, giữ dòng đầu) sau khi chuẩn hóa `booking_id`; trong `row_lineage` ghi `merged` (không dùng `dropped`) | Có bằng chứng trùng hoàn toàn; vẫn truy vết được từng `row_id` | 10.325 → 10.144 dòng; B8 gộp tiếp 144 dòng → 10.000 đặt phòng |
| 3 | Hai đặt phòng khác `booking_id` nhưng nội dung giống nhau | Không gộp | Có thể là nhiều phòng của cùng một đoàn | Giữ nguyên các dòng này (1.759 dòng giống hệt nội dung) |
| 4 | Nhãn `Undefined` | `meal`: đổi thành `SC`; `market_segment`, `distribution_channel`: chuyển thành thiếu | Theo Mục 5.2 của đề | `meal` không có ô thiếu do nhãn này |
| 5 | `is_canceled` mâu thuẫn `reservation_status` | Khi trạng thái hợp lệ, tính lại `is_canceled` từ trạng thái; trạng thái thiếu thì giữ `is_canceled` đã chuẩn hóa | Mục 5.3 của đề | 40 dòng được sửa (đếm sau B1) |
| 6 | Ngày nằm nhầm ở `reservation_status` | Chỉ chuyển khi `reservation_status` có dạng `YYYY-MM-DD` **và** `reservation_status_date` đang rỗng; để `reservation_status` thiếu | Không ghi đè dữ liệu có sẵn, không đoán trạng thái | 26 dòng được chuyển |
| 7 | Giá trị sai quy ước (adr âm hoặc > 1000, ngày ngoài miền, loại phòng lạ…) | Chuyển thành thiếu, không điền trung bình hay giá trị phổ biến | Không đoán dữ liệu | Tỷ lệ thiếu của một số cột tăng nhẹ (xem mục 7) |
| 8 | Giá trị hiếm nhưng hợp lệ | Giữ nguyên `adr = 0`, lưu trú 0 đêm, 0 khách | Nằm trong miền hợp lệ của đề | 132 đặt phòng `adr = 0`; 38 đặt phòng 0 đêm; 10 đặt phòng 0 khách |
| 9 | Mã quốc gia `CN` và `CHN` | Giữ nguyên, không gộp | Chưa có bằng chứng hai mã cùng nghĩa trong nguồn | Bảng sạch có 99 đặt phòng `CN` và 85 đặt phòng `CHN`; nêu trong phần giới hạn nếu ảnh hưởng Top 10 (Câu 5) |

## 6. Danh sách tệp nộp

- [ ] `hotel_analysis.ipynb` (đã Restart & Run all, hết chữ TODO)
- [ ] `Bao_cao_Khach_san.pdf` hoặc `.docx`
- [ ] `data/hotel_bookings_dirty.csv`
- [ ] `outputs/` — 5 tệp dữ liệu (`bookings_clean.csv`, `monthly_summary.csv`, `cleaning_log.csv`, `row_lineage.csv`, `quality_report.csv`)
- [ ] `outputs/tables/` — 14 bảng (hiện có 2/14: `a_missing_survey`, `a_issue_catalog`)
- [ ] `outputs/figures/` — 8 hình (hiện có 0/8)

## 7. Kết quả kiểm tra chất lượng sau làm sạch

**Kiểm tra tự động (ô C4 và `tools/kiem_tra_nop_bai.py`):** 51/51 kiểm tra dữ liệu đạt (khóa, nhãn, miền giá trị, ngày `YYYY-MM-DD`, cột dẫn xuất, bảng tháng khớp khi tính lại, truy vết 10.325 `row_id`, nhật ký ≥ 10 quy tắc).

**Rà soát bổ sung (mục C4+ trong notebook):** kiểu dữ liệu (`Int64` cho cột số nguyên, `float64` cho `adr`), định dạng chữ trong CSV xuất ra (không có đuôi `.0`, ngày đúng `YYYY-MM-DD`), khóa `booking_id` duy nhất và đối soát độc lập số ô thiếu (22/22 cột khớp).

| Chỉ tiêu | Trước (mẫu số 10.325 dòng) | Sau (mẫu số 10.000 đặt phòng) |
|---|---|---|
| Số dòng | 10.325 | 10.000 |
| `booking_id` khác nhau (sau strip + upper) | 10.000 | 10.000 |
| Dòng trùng trên 23 cột (sau chuẩn hóa `booking_id`) | 181 | 0 |
| `booking_id` lặp (sau strip + upper) | 325 | 0 |
| Tỷ lệ thiếu `adr` | 0,24% (25 ô) | 0,36% (36 ô) |
| Tỷ lệ hủy | 36,50% (3.769/10.325, đếm `is_canceled = "1"` thô) | 37,41% (3.741/10.000) |

Tỷ lệ thiếu `adr` tăng vì 11 ô có giá trị sai quy ước (âm hoặc > 1000) được chuyển thành thiếu thay vì đoán giá. Tỷ lệ thiếu của `meal`, `country`, `children` giảm vì các ô thiếu nằm ở dòng trùng `booking_id`: một phần được điền từ dòng còn lại khi gộp, phần còn lại bị loại cùng dòng trùng. Chi tiết từng cột: bảng `quality_report.csv` và mục C4+ của notebook.

## 8. Giới hạn cần nhớ khi đọc kết quả

- Chỉ có hai khách sạn ở Bồ Đào Nha (2015–2017); không suy rộng cho ngành khách sạn nói chung.
- Chỉ năm 2016 đủ 12 tháng; không so sánh tổng năm 2015, 2017 với 2016.
- `adr` là giá phòng bình quân mỗi đêm, `revenue_est` chỉ là ước tính tiền phòng; không phải doanh thu thực.
- Dữ liệu không cho biết lý do hủy phòng; các kết quả chỉ mô tả mối liên hệ quan sát, không phải nhân quả.
