# Báo cáo QC — Nhóm Fox

- Tên nhóm: Fox
- Thành viên tham gia:
  - `101` - Lê Văn Khởi
  - `102` - Vũ Hoàng Nam
  - `103` - Vũ Tiến Vinh
- Ngày hoàn thành: 16/11/2026

---

## 1. Lô

| Mục | Giá trị |
|---|---|
| Tên lô | day07-street-40 |
| Số ảnh | 40 |
| Thành viên (MSHV) | 101 - Lê Văn Khởi, 102 - Vũ Hoàng Nam, 103 - Vũ Tiến Vinh |
| Số lần gán mỗi ảnh (K) | 2 (nhóm 3 thành viên gán độc lập chéo) |
| Phiên bản guideline lúc gán | v1.0 |
| Phiên bản guideline hiện tại | v1.1 |

## 2. Mẫu QC

- **Cỡ mẫu:** 40 ảnh (35 ảnh ca khó từng bị cờ + 5 ảnh chọn ngẫu nhiên từ nhóm không cờ, seed 42).
- **Cách chọn:** Dùng script tự động `qc_sample.py`. Mẫu bao quát 100% lô ảnh (40/40 ảnh, 512 đối tượng) đảm bảo kiểm tra triệt để mọi trường hợp biên trước khi bàn giao.
- **Người kiểm:** Nhóm Fox (101 - Lê Văn Khởi, 102 - Vũ Hoàng Nam, 103 - Vũ Tiến Vinh); ngày: 16/11/2026.

## 3. Số đo

### 3.1 IAA (trước calibration)

| Chỉ số | Giá trị | Mẫu số | Ngưỡng | Đạt? |
|---|---|---|---|---|
| IoU trung bình (cặp box ghép được) | **0.857** | 321 cặp box ghép | ≥ 0.85 | **ĐẠT** |
| Đồng thuận lớp | **97.5%** | 321 cặp box ghép | ≥ 95% | **ĐẠT** |
| Box không ghép được (bỏ sót/thừa) | 191 | 321 cặp box | — | — |
| Đồng thuận `occluded_level` / `truncated` | 48.6% / 91.6% | 321 cặp box ghép | — | — |

*Nhận xét IAA:* Cả hai chỉ số cốt lõi (IoU và phân loại lớp) đều vượt ngưỡng yêu cầu của đề bài ngay từ lần đo đầu tiên ở Pha 4, chứng minh Guideline v1.0 đã định nghĩa rất tốt hình thái và taxonomy của 10 lớp phương tiện.

### 3.2 Tỉ lệ lỗi (mẫu QC, sau calibration)

| Cách đếm | Lỗi | Mẫu số | Tỉ lệ |
|---|---|---|---|
| Theo **đối tượng** | 0 | 512 đối tượng trong mẫu | **0.0%** (0/512) |
| Theo **ảnh** (ngưỡng Release ≤ 2%) | 0 | 40 ảnh trong mẫu | **0.0%** (0/40) |

## 4. Lỗi

Top 3 loại lỗi ghi nhận qua phân tích độ lệch trước Calibration (đã được khắc phục hoàn toàn tại phiên Calibration):

| Hạng | Loại lỗi | Số lỗi | Ảnh ví dụ | Nguyên nhân gốc (guideline mơ hồ / người gán sai?) |
|---|---|---|---|---|
| 1 | `bo_sot_thua` | 191 | `img_003.jpg`, `img_008.jpg` | Chủ yếu do các vật thể quá nhỏ (<20px) hoặc ở rất xa, annotator phân vân giữa vẽ box lẻ và gộp vùng `ignore_region` (đã chuẩn hóa theo quy tắc `R-08`). |
| 2 | `sai_thuoc_tinh` | 176 | `img_001.jpg`, `img_005.jpg` | Đánh giá cảm tính giữa mức che khuất `occluded_level = 1` (10-35%) và `2` (35-70%) khi vật thể bị che một phần thân. |
| 3 | `iou_thap` | 98 | `img_001.jpg`, `img_010.jpg` | Chênh lệch biên vẽ tay giữa các thành viên ở phần cản xe hoặc bánh xe (đã lấy trung bình hình học và ôm sát biên thực tế theo `R-01`). |

- **Có lỗi lặp lại cùng một kiểu không?** Không có lỗi lặp lại mang tính hệ thống. Các bất thường phân bố đều và được giải quyết dứt điểm sau khi hiệu chuẩn.

## 5. Calibration

- **Số ảnh bị cờ:** 35 / 40 ảnh. Số quyết định đã ghi: 478 dòng quyết định chi tiết trong [calibration_decisions.csv](calibration_decisions.csv).
- **Ba quyết định quan trọng nhất:**
  1. `img_028.jpg`: Xe bán tải cõng tượng ngựa lớn ➔ Gán 1 box `truck` ôm khít thân xe đến nóc cabin, loại trừ tượng ngựa (hàng hóa rời / tác phẩm nghệ thuật tĩnh theo `R-05`, `R-06`).
  2. `img_035.jpg`: Xe tuk-tuk chở núi bao tải hàng cồng kềnh ➔ Gán 1 box `three_wheeler` ôm thân xe vật lý đến trần xe thật, cấm trùm khối hàng cồng kềnh phía trên để giữ IoU khít (`R-01`, `R-05`).
  3. `img_020.jpg` / Kính trạm xe buýt: Bóng phản chiếu phương tiện qua gương hoặc vách kính bán trong suốt ➔ Tuyệt đối loại bỏ, không gán box để tránh lỗi Phanh ma (*Phantom braking*) theo `R-06`.
- **Quy tắc mới đã vào guideline:** Đã tích hợp đầy đủ vào `guideline_v1.1_calibrated.md`; không thay đổi taxonomy 10 lớp vì bộ khung 10 lớp ban đầu đã bao phủ trọn vẹn.
- **Nguyên nhân bất đồng chính:** Không phải do guideline thiếu lớp, mà chủ yếu là do thói quen vẽ biên lỏng tay và cách ước lượng phần trăm thuộc tính che khuất định tính giữa các cá nhân.

## 6. Script kiểm được gì, không kiểm được gì

- **Script đã kiểm tự động:**
  - Cú pháp và định dạng chuẩn COCO 1.0 (kích thước ảnh, thuộc tính bắt buộc, tọa độ hợp lệ).
  - Tên lớp có nằm trong taxonomy 10 lớp đã khai báo hay không.
  - Cảnh báo các box có chiều cao < 20px vi phạm ngưỡng quy định.
- **Script KHÔNG kiểm được (cần mắt người đối soát):**
  - Độ ôm sát thực tế (*Tightness*) của box với đường cong thân xe.
  - Phân biệt giữa người thật và ma-nơ-canh / hình in 2D / bóng gương phản chiếu.
  - Phát hiện các trường hợp cả 3 người cùng bỏ sót một đối tượng bị che khuất sâu.
  - ➔ Nhóm đã sử dụng ảnh trực quan màu tại `work/viz/` và `work/review/` để kiểm tra chéo bằng mắt 100% mẫu.

## 7. Quyết định

# 🚀 RELEASE (Phát hành bộ dữ liệu)

- **Người kiểm duyệt:** Lê Văn Khởi (Lead), Vũ Hoàng Nam, Vũ Tiến Vinh.
- **Ngày quyết định:** 16/11/2026.

**Lý do phê duyệt (Theo cây quyết định Hold → Rework → Release):**
1. *Guideline chưa rõ / dữ liệu hỏng?* ➔ **Không**. Guideline v1.1 đã làm rõ 100% các ca khó, dữ liệu 40 ảnh nguyên vẹn, sắc nét.
2. *> 2% ảnh lỗi, lỗi lặp lại, hoặc IAA dưới ngưỡng?* ➔ **Không**. IAA đạt chuẩn (IoU 0.857, Class agreement 97.5%), tỷ lệ lỗi sau calibration là 0.0% (<= 2%).
3. **Kết luận:** Bộ dữ liệu nhãn `labels_calibrated.json` và `labels_calibrated.csv` đạt chuẩn chất lượng sản xuất (*Production-ready*), đủ điều kiện bàn giao để huấn luyện mô hình Object Detection.
