# Ghi chú cá nhân — 102 - Vũ Hoàng Nam

## 1. Pha 1: ba ca khó tôi tìm ra trong 40 ảnh

| Ảnh | v0 không trả lời được gì | Quyết định ban đầu của tôi |
|---|---|---|
| `img_004.jpg` | v0 không có lớp tuk-tuk | Đề xuất gán lớp three_wheeler |
| `img_011.jpg` | Nhân vật Predator cosplay | Gán person cho người hóa trang |
| `img_022.jpg` | Mô tô 3 bánh có sidecar | Đề xuất xếp vào three_wheeler |

## 2. Pha 2: chỗ tôi bất đồng với nhóm
- Thảo luận về việc phân loại xe bán tải pickup thùng hở; cuối cùng đồng thuận theo đề xuất của nhóm xếp vào `truck`.
- Thống nhất mở rộng taxonomy lên 10 lớp chính thức.

## 3. Pha 3: ảnh tôi đã gán
- Số ảnh được giao: 27 ảnh. Số box đã vẽ: 176 box. Số lần tick `can_review`: 0 lần.
- Ca lưỡng lự nhất: các xe đỗ ở xa ban đêm trong bối cảnh thiếu sáng.

## 4. Pha 4–5: tôi lệch ở đâu
- Trong `calibration_candidates.csv`: lệch chủ yếu ở ước lượng phần trăm `occluded_level` và một số box nhỏ < 20px.
- Quyết định calibration quan trọng nhất: Thống nhất loại bỏ box < 20px theo R-08.

## 5. Dùng AI
- Không dùng AI cho gán nhãn thủ công.

## 6. Bài học
- Cần đo đạc kỹ kích thước pixel trên CVAT trước khi vẽ để tránh vẽ box quá nhỏ vi phạm quy tắc.
