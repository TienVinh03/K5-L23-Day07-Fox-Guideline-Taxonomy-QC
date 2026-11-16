# Báo cáo QC tự động (nháp)

> Đây là **gợi ý** do script tính - người kiểm phải xác nhận. Script chỉ đếm theo phiếu kiểm bằng mắt người; không tự biết box có ôm sát vật hay không.

## Mẫu
- Cỡ mẫu: 40 ảnh (35 ca khó + 5 ngẫu nhiên), 512 đối tượng; tổng lô: 40 ảnh.
- Phiên bản taxonomy: v1.0

## Số đo
| Chỉ số | Giá trị | Mẫu số |
|---|---|---|
| Tỉ lệ lỗi **theo đối tượng** | 0/512 = 0.0% | 512 đối tượng trong mẫu (lỗi bỏ sót tính như 1 đối tượng lỗi) |
| Tỉ lệ **ảnh có lỗi** (dùng cho ngưỡng) | 0/40 = 0.0% | 40 ảnh trong mẫu |
| IoU trung bình (IAA, trước calibration) | 0.857 (ngưỡng 0.85) | 321 cặp box ghép |
| Đồng thuận lớp (IAA) | 97.5% (ngưỡng 95%) | 321 cặp box ghép |

Ngưỡng Release: ảnh có lỗi <= 2% (đếm theo ảnh).

## Top 3 loại lỗi
(không ghi nhận lỗi nào)

## Gợi ý quyết định (người kiểm xác nhận)
**RELEASE**

- tỉ lệ ảnh lỗi 0.0% <= 2%, không lỗi hệ thống, IAA đạt

Cây quyết định: Hold (guideline chưa rõ/dữ liệu hỏng) -> Rework (>ngưỡng, lỗi lặp, IAA thấp) -> Release.
