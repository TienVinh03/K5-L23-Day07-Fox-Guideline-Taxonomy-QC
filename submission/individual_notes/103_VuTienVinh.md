# Ghi chú cá nhân — 103 - Vũ Tiến Vinh

## 1. Pha 1: ba ca khó tôi tìm ra trong 40 ảnh

| Ảnh | v0 không trả lời được gì | Quyết định ban đầu của tôi |
|---|---|---|
| `img_002.jpg` | Xe chở ô tô con trên sàn | Gán truck cho xe chở xe, ô tô con là hàng hóa |
| `img_007.jpg` | Xe giao hàng đóng thùng kín | Gán 1 box truck trọn vẹn cả đầu và thùng |
| `img_018.jpg` | Biển quảng cáo in hình người | Không gán person cho pano quảng cáo 2D |

## 2. Pha 2: chỗ tôi bất đồng với nhóm
- Thảo luận về việc tách biệt đòn gánh và người bán hàng rong; thống nhất gán person ôm người, loại trừ quang gánh.
- Đồng thuận hệ quy tắc R-01 đến R-09.

## 3. Pha 3: ảnh tôi đã gán
- Số ảnh được giao: 27 ảnh. Số box đã vẽ: 325 box. Số lần tick `can_review`: 0 lần.
- Ca lưỡng lự nhất: các đối tượng xe máy trong khu chợ đông đúc bị che lấp nhiều tầng.

## 4. Pha 4–5: tôi lệch ở đâu
- Trong `calibration_candidates.csv`: lệch ở việc đánh giá độ che khuất mức 2 vs 3.
- Quyết định calibration quan trọng nhất: Thống nhất áp dụng bảng định lượng phần trăm cho occluded_level.

## 5. Dùng AI
- Không dùng AI cho gán nhãn thủ công.

## 6. Bài học
- Cần phóng to ảnh (*zoom-in*) ở các khu vực đông đúc để kiểm tra kỹ ranh giới vật thể trước khi chốt box.
