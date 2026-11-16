# Tóm tắt IAA (taxonomy v1.0)

- Ảnh được so sánh (có >=2 người gán): **40**; số người gán: 3
- IoU trung bình và % cùng lớp **chỉ tính trên các cặp box đã ghép** (IoU >= 0.5, ghép 1-1 không phân biệt lớp). Box chưa ghép được thống kê riêng ở cột bỏ sót/thừa.
- Mẫu số: 321 cặp box ghép (tổng mọi cặp người gán, ảnh chung).

| Cặp | Ảnh | Cặp box ghép | IoU TB | Cùng lớp | Cùng occluded_level | Cùng truncated | Chưa ghép (A/B) | Bỏ qua do ignore | Đạt IAA |
|---|---|---|---|---|---|---|---|---|---|
| **TỔNG** | 40 | 321 | 0.857 | 97.5% | 48.6% | 91.6% | 91/100 | 0 | ĐẠT |
| 101|102 | 13 | 94 | 0.846 | 98.9% | 40.4% | 88.3% | 60/2 | 0 | CHƯA ĐẠT |
| 101|103 | 13 | 155 | 0.868 | 97.4% | 51.0% | 94.2% | 20/50 | 0 | ĐẠT |
| 102|103 | 14 | 72 | 0.846 | 95.8% | 54.2% | 90.3% | 11/48 | 0 | CHƯA ĐẠT |

Ngưỡng IAA: IoU TB >= 0.85, đồng thuận lớp >= 95%.

Số ca bất thường: 478 ({'iou_thap': 98, 'sai_thuoc_tinh': 176, 'bo_sot_thua': 191, 'co_can_review': 5, 'sai_lop': 8}), trên 35 ảnh cần calibration.
