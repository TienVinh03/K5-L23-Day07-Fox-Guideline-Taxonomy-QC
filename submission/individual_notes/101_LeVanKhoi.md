# Ghi chú cá nhân — 101 - Lê Văn Khởi

## 1. Pha 1: ba ca khó tôi tìm ra trong 40 ảnh

| Ảnh | v0 không trả lời được gì | Quyết định ban đầu của tôi |
|---|---|---|
| `img_002.jpg` | v0 không nói rõ xe chở xe (*car carrier*) thì các ô tô con cõng trên sàn có gán box `car` riêng không. | Tôi đề xuất chỉ gán 1 box `truck` cho xe chở xe; các ô tô con trên thùng coi là hàng hóa, không gán `car` riêng để tránh detector hiểu nhầm là xe đang độc lập lưu thông. |
| `img_003.jpg` | v0 không định nghĩa người gánh hàng rong thì đòn gánh có tính vào box `person` hay `other_vehicle` không. | Gán 1 box `person` ôm người, loại trừ đòn gánh và thúng hàng để giữ box khít; đòn gánh không có bánh nên không gán phương tiện. |
| `img_004.jpg` | v0 không có lớp cho xe ba bánh đặc thù (tuk-tuk). | Tôi đề xuất bổ sung lớp `three_wheeler` vào taxonomy; gán riêng từng xe và đánh thuộc tính `occluded_level`. |

## 2. Pha 2: chỗ tôi bất đồng với nhóm
- Nhóm ban đầu thảo luận về việc xe bán tải (pickup) thùng hở nên xếp vào `car` hay `truck`. Sau khi thảo luận trên ảnh `img_012` và `img_028`, cả nhóm đã đồng thuận chốt theo đề xuất của tôi: xếp pickup vào `truck` vì có sàn chở hàng riêng biệt.
- Cả nhóm đồng thuận nâng cấp taxonomy lên 10 lớp chính thức và bổ sung hệ thống quy tắc `R-01` đến `R-09` vào `guideline_v1.0_agreed.md`.

## 3. Pha 3: ảnh tôi đã gán
- Số ảnh được giao: 26 ảnh. Số box đã vẽ: 329 box. Số lần tôi tick `can_review`: 0 lần.
- Ca tôi lưỡng lự nhất khi gán: 
  - `img_028.jpg`: Xe bán tải cõng tượng ngựa lớn, phân vân box xe có ôm tượng không (sau đó áp dụng R-05, R-06 hạ box ôm khít nóc cabin).
  - Vách kính trạm xe buýt: Bóng phản chiếu ô tô màu hồng mờ ảo trên kính, phân vân xe thật hay ảnh ảo (sau đó áp dụng R-06 xóa bỏ để tránh phantom braking).

## 4. Pha 4–5: tôi lệch ở đâu
- Trong `calibration_candidates.csv`, các ca có tên tôi: tôi lệch chủ yếu ở việc đánh giá thuộc tính `occluded_level` (mức 1 vs mức 2) do cảm nhận chủ quan về phần trăm che khuất, và một số box người ở rất xa trong hậu cảnh (< 20px).
- Quyết định calibration tôi thấy quan trọng nhất: Thống nhất giữ box ôm sát hình học thật của xe theo `R-01` & `R-05` (xe tuk-tuk chở hàng `img_035` và xe bán tải `img_028` không bao trùm hàng cồng kềnh).

## 5. Dùng AI
- Không dùng AI cho gán nhãn thủ công.

## 6. Bài học
- Một điều tôi sẽ làm khác khi viết guideline lần sau: Khảo sát kỹ toàn bộ các góc chụp đặc thù (như ảnh chụp từ trong xe ra ngoài hoặc ảnh góc hẹp) ngay từ đầu để đặt mã quy tắc `R-xx` từ sớm, giúp quá trình so sánh và phân tích lỗi sau này diễn ra mạch lạc hơn.
