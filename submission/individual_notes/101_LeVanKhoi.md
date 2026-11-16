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
- Số ảnh được giao: … Số box đã vẽ: … Số lần tôi tick `can_review`: …
- Ca nào tôi lưỡng lự nhất khi gán (ảnh, vì sao)?

## 4. Pha 4–5: tôi lệch ở đâu
- Trong `calibration_candidates.csv`, các ca có tên tôi: tôi lệch vì guideline mơ hồ hay vì tôi chưa nắm quy tắc?
- Quyết định calibration nào tôi thấy quan trọng nhất?

## 5. Dùng AI
- Công cụ: Antigravity AI Assistant.
- Mục đích: Hỗ trợ rà soát tính chặt chẽ của các định nghĩa, chuẩn hóa ngưỡng định lượng của thuộc tính che khuất (`occluded_level`), phát hiện các trường hợp biên (edge cases: bóng đổ, ăng-ten, cabin kín) và gợi ý phân tích rủi ro an toàn (False Positive / False Negative).
- Kiểm tra kết quả: Tự mình kiểm tra lại toàn bộ trên bộ ảnh thực tế và thảo luận trực tiếp với các thành viên trong nhóm để đưa ra quyết định thống nhất cuối cùng.

## 6. Bài học
- Một điều tôi sẽ làm khác khi viết guideline lần sau: Khảo sát kỹ toàn bộ các góc chụp đặc thù (như ảnh chụp từ trong xe ra ngoài hoặc ảnh góc hẹp) ngay từ đầu để đặt mã quy tắc `R-xx` từ sớm, giúp quá trình so sánh và phân tích lỗi sau này diễn ra mạch lạc hơn.
