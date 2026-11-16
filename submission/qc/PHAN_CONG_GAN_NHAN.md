# Phân công gán nhãn 40 ảnh — Vinh, Khởi, Nam

- Thư mục ảnh: `data/images/`.
- Mỗi ảnh có đúng **2 người gán nhãn độc lập**.
- Tổng khối lượng: **40 ảnh × 2 = 80 lượt gán**.
- Phân bổ: **Vinh 27 ảnh, Khởi 27 ảnh, Nam 26 ảnh**. Vì 80 không chia hết cho 3, mức chênh lệch nhỏ nhất là 1 ảnh.
- Cách chia: lặp các cặp Vinh–Khởi, Khởi–Nam, Vinh–Nam theo thứ tự ảnh, giúp công việc được phân bố xuyên suốt bộ dữ liệu.
- Hai người được giao cùng ảnh làm riêng, không xem nhãn của nhau trước khi nộp; sau đó mới so sánh và calibration.

## Bảng phân công từng ảnh

Hai cột người gán có vai trò ngang nhau: mỗi người gán đầy đủ ảnh, không chia đối tượng trong cùng ảnh cho nhau.

| STT | Ảnh | Người gán 1 | Người gán 2 |
|---|---|---|---|
| 1 | [img_001.jpg](../../data/images/img_001.jpg) | Vinh | Khởi |
| 2 | [img_002.jpg](../../data/images/img_002.jpg) | Khởi | Nam |
| 3 | [img_003.jpg](../../data/images/img_003.jpg) | Vinh | Nam |
| 4 | [img_004.jpg](../../data/images/img_004.jpg) | Vinh | Khởi |
| 5 | [img_005.jpg](../../data/images/img_005.jpg) | Khởi | Nam |
| 6 | [img_006.jpg](../../data/images/img_006.jpg) | Vinh | Nam |
| 7 | [img_007.jpg](../../data/images/img_007.jpg) | Vinh | Khởi |
| 8 | [img_008.jpg](../../data/images/img_008.jpg) | Khởi | Nam |
| 9 | [img_009.jpg](../../data/images/img_009.jpg) | Vinh | Nam |
| 10 | [img_010.jpg](../../data/images/img_010.jpg) | Vinh | Khởi |
| 11 | [img_011.jpg](../../data/images/img_011.jpg) | Khởi | Nam |
| 12 | [img_012.jpg](../../data/images/img_012.jpg) | Vinh | Nam |
| 13 | [img_013.jpg](../../data/images/img_013.jpg) | Vinh | Khởi |
| 14 | [img_014.jpg](../../data/images/img_014.jpg) | Khởi | Nam |
| 15 | [img_015.jpg](../../data/images/img_015.jpg) | Vinh | Nam |
| 16 | [img_016.jpg](../../data/images/img_016.jpg) | Vinh | Khởi |
| 17 | [img_017.jpg](../../data/images/img_017.jpg) | Khởi | Nam |
| 18 | [img_018.jpg](../../data/images/img_018.jpg) | Vinh | Nam |
| 19 | [img_019.jpg](../../data/images/img_019.jpg) | Vinh | Khởi |
| 20 | [img_020.jpg](../../data/images/img_020.jpg) | Khởi | Nam |
| 21 | [img_021.jpg](../../data/images/img_021.jpg) | Vinh | Nam |
| 22 | [img_022.jpg](../../data/images/img_022.jpg) | Vinh | Khởi |
| 23 | [img_023.jpg](../../data/images/img_023.jpg) | Khởi | Nam |
| 24 | [img_024.jpg](../../data/images/img_024.jpg) | Vinh | Nam |
| 25 | [img_025.jpg](../../data/images/img_025.jpg) | Vinh | Khởi |
| 26 | [img_026.jpg](../../data/images/img_026.jpg) | Khởi | Nam |
| 27 | [img_027.jpg](../../data/images/img_027.jpg) | Vinh | Nam |
| 28 | [img_028.jpg](../../data/images/img_028.jpg) | Vinh | Khởi |
| 29 | [img_029.jpg](../../data/images/img_029.jpg) | Khởi | Nam |
| 30 | [img_030.jpg](../../data/images/img_030.jpg) | Vinh | Nam |
| 31 | [img_031.jpg](../../data/images/img_031.jpg) | Vinh | Khởi |
| 32 | [img_032.jpg](../../data/images/img_032.jpg) | Khởi | Nam |
| 33 | [img_033.jpg](../../data/images/img_033.jpg) | Vinh | Nam |
| 34 | [img_034.jpg](../../data/images/img_034.jpg) | Vinh | Khởi |
| 35 | [img_035.jpg](../../data/images/img_035.jpg) | Khởi | Nam |
| 36 | [img_036.jpg](../../data/images/img_036.jpg) | Vinh | Nam |
| 37 | [img_037.jpg](../../data/images/img_037.jpg) | Vinh | Khởi |
| 38 | [img_038.jpg](../../data/images/img_038.jpg) | Khởi | Nam |
| 39 | [img_039.jpg](../../data/images/img_039.jpg) | Vinh | Nam |
| 40 | [img_040.jpg](../../data/images/img_040.jpg) | Vinh | Khởi |

## Danh sách ảnh của từng người

### Vinh — 27 ảnh

```text
img_001.jpg
img_003.jpg
img_004.jpg
img_006.jpg
img_007.jpg
img_009.jpg
img_010.jpg
img_012.jpg
img_013.jpg
img_015.jpg
img_016.jpg
img_018.jpg
img_019.jpg
img_021.jpg
img_022.jpg
img_024.jpg
img_025.jpg
img_027.jpg
img_028.jpg
img_030.jpg
img_031.jpg
img_033.jpg
img_034.jpg
img_036.jpg
img_037.jpg
img_039.jpg
img_040.jpg
```

### Khởi — 27 ảnh

```text
img_001.jpg
img_002.jpg
img_004.jpg
img_005.jpg
img_007.jpg
img_008.jpg
img_010.jpg
img_011.jpg
img_013.jpg
img_014.jpg
img_016.jpg
img_017.jpg
img_019.jpg
img_020.jpg
img_022.jpg
img_023.jpg
img_025.jpg
img_026.jpg
img_028.jpg
img_029.jpg
img_031.jpg
img_032.jpg
img_034.jpg
img_035.jpg
img_037.jpg
img_038.jpg
img_040.jpg
```

### Nam — 26 ảnh

```text
img_002.jpg
img_003.jpg
img_005.jpg
img_006.jpg
img_008.jpg
img_009.jpg
img_011.jpg
img_012.jpg
img_014.jpg
img_015.jpg
img_017.jpg
img_018.jpg
img_020.jpg
img_021.jpg
img_023.jpg
img_024.jpg
img_026.jpg
img_027.jpg
img_029.jpg
img_030.jpg
img_032.jpg
img_033.jpg
img_035.jpg
img_036.jpg
img_038.jpg
img_039.jpg
```

## Kiểm đếm

| Nội dung | Số lượng |
|---|---|
| Ảnh duy nhất | 40 |
| Người gán mỗi ảnh | 2 |
| Tổng lượt gán | 80 |
| Vinh | 27 |
| Khởi | 27 |
| Nam | 26 |
| Ảnh chung Vinh–Khởi | 14 |
| Ảnh chung Vinh–Nam | 13 |
| Ảnh chung Khởi–Nam | 13 |
