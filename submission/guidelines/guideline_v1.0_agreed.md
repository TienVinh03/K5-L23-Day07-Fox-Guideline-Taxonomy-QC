# Guideline gán nhãn thống nhất — Phiên bản v1.0

- Phiên bản: v1.0 (Thống nhất toàn nhóm)
- Ngày hiệu lực: 16/11/2026
- Định dạng: Bounding Box 2D trên CVAT, xuất chuẩn COCO 1.0
- Nhóm: Fox
- Thành viên tham gia:
  - `101` - Lê Văn Khởi (Điều phối / Lead)
  - `102` - Vũ Hoàng Nam
  - `103` - Vũ Tiến Vinh

---

## 1. Mục tiêu

Bộ nhãn chuẩn này dùng để huấn luyện mô hình thị giác máy tính (Object Detection) phát hiện người và phương tiện tham gia giao thông trong môi trường đô thị châu Á.
- **Rủi ro bỏ sót (False Negative - FN):** Bỏ sót người đi bộ hoặc phương tiện lưu thông dẫn đến nguy cơ hệ thống xe tự lái hoặc giám sát hành trình không cảnh báo kịp thời, tiềm ẩn nguy cơ xảy ra va chạm mất an toàn giao thông.
- **Rủi ro nhận nhầm (False Positive - FP):** Nhận nhầm hình in quảng cáo, bóng phản chiếu qua gương, tượng đài, tranh ảnh hoặc đồ chơi thành người/xe thật dẫn đến hiện tượng phanh khẩn cấp không cần thiết (*phantom braking*), tiềm ẩn nguy cơ gây tai nạn dồn toa cho các phương tiện đi phía sau.

Do làm việc trên ảnh tĩnh 2D, người gán không thể dựa vào chuyển động để phán đoán. Toàn nhóm thống nhất ưu tiên hình dáng, đặc trưng thị giác và cấu trúc vật lý nhìn thấy được. Các ca mơ hồ hoặc nằm ở ranh giới hai lớp bắt buộc phải bật cờ `can_review = true` kèm lý do cụ thể để giải quyết trong phiên Calibration; tuyệt đối không tự ý đoán.

## 2. Lớp và thuộc tính

### 2.1. Lớp (Taxonomy)

Sau buổi họp thống nhất Pha 2, nhóm đồng thuận nâng cấp bộ nhãn từ 6 lớp ban đầu (v0) lên **10 lớp chính thức** để bao quát đặc thù giao thông đô thị (xích lô, tuk-tuk, xe buýt, xe kéo).

| Lớp | Định nghĩa chuẩn | Ví dụ thực tế | Phản ví dụ / Dễ nhầm với |
|---|---|---|---|
| `car` | Ô tô con chở người dưới 9 chỗ ngồi (sedan, hatchback, coupe, SUV, MPV gia đình). | Toyota Vios, Mazda CX-5, Mitsubishi Xpander. | Xe bán tải (*pickup*) có thùng hàng hở (chốt xếp vào `truck`); xe thương mại 16 chỗ (xếp vào `van`). |
| `truck` | Xe tải, xe đầu kéo, xe chuyên dụng hoặc xe có sàn/thùng chở hàng riêng biệt. | Xe tải chở hàng, xe ben, xe chở xe (*car carrier*), xe bán tải (*pickup* thùng hở). | Không đổi lớp thành `car` kể cả khi xe tải đang chạy rỗng không có hàng. |
| `van` | Xe thương mại thân kín dạng hộp cỡ nhỏ đến trung (9–16 chỗ) hoặc xe tải van chở hàng liền khối. | Xe cứu thương, Ford Transit 16 chỗ, Toyota HiAce, xe bưu chính thùng kín. | Xe MPV/SUV gia đình dưới 9 chỗ (xếp `car`); xe buýt lớn trên 16 chỗ (xếp `bus`). |
| `bus` | Xe cỡ lớn thiết kế chuyên dụng để chở khối lượng lớn hành khách (trên 16 chỗ). | Xe buýt nội đô, xe khách tuyến 29–45 chỗ. | Xe van 16 chỗ dạng hộp (xếp vào `van`, không xếp `bus`). |
| `three_wheeler` | Phương tiện có cấu hình ba bánh (cơ giới hoặc sức người đạp). | Xe tuk-tuk, xích lô chở khách, xe lôi, mô tô có gắn sidecar 3 bánh. | Không gán cho xe máy 2 bánh thông thường; không gán cho xe đẩy hàng 4 bánh. |
| `motorcycle` | Xe cơ giới hai bánh có tay lái (ghi-đông). | Xe số (Honda Wave), xe tay ga (Vespa, SH), mô tô phân khối lớn. | Xe đạp điện, xe trợ lực (xếp `bicycle` nếu có bàn đạp); người điều khiển xe máy tách thành box `person` riêng. |
| `bicycle` | Xe hai bánh vận hành chủ yếu bằng sức đạp chân của con người. | Xe đạp thể thao, xe đạp mini, xe đạp trợ lực điện có bàn đạp. | Xe máy điện chạy hoàn toàn bằng tay ga không có bàn đạp (xếp vào `motorcycle`). |
| `other_vehicle` | Xe đẩy, xe kéo có bánh không thuộc các lớp phương tiện trên. | Xe đẩy hàng bằng tay, xe kéo rác, xe ba gác tự chế không động cơ. | Gánh hàng rong không có bánh không phải xe (không gán xe). |
| `person` | Con người đang sống nhìn thấy trong ảnh (đi bộ, lái xe, hành khách). | Người đi bộ qua đường, tài xế xe máy, người ngồi trên xích lô. | Tượng người, ma-nơ-canh, hình in người trên biển quảng cáo/áo quần (không gán nhãn). |
| `ignore_region` | Vùng chứa cụm đối tượng quá nhỏ hoặc quá dày đặc không thể tách rời nhất quán. | Đám đông người ở hậu cảnh rất xa, bãi đỗ xe dày đặc ở khoảng cách xa. | Không dùng một vùng ignore lớn để bao trùm các vật thể riêng lẻ đủ kích thước gán nhãn. |

*Xử lý vật thể ngoài Taxonomy:* Phương tiện có bánh xe nhưng cấu trúc tự chế lạ thì đưa vào `other_vehicle`. Đồ vật không có bánh (quang gánh, thùng hàng, kiệu) tuyệt đối không gán phương tiện.

### 2.2. Thuộc tính

Ngưỡng kích thước tối thiểu theo quy định là **chiều cao ≥ 20 px**. Đối tượng nhỏ hơn 20 px không gán box riêng.

| Thuộc tính | Giá trị | Ngưỡng đo lường & Quy tắc thống nhất |
|---|---|---|
| `occluded_level` | `0` | Che khuất từ 0% đến 10% (vật thể nhìn thấy gần như trọn vẹn). |
|  | `1` | Che khuất trên 10% đến 35% (bị che một phần nhỏ nhưng nhận diện rất rõ). |
|  | `2` | Che khuất trên 35% đến 70% (bị che đáng kể nhưng cấu trúc nhận diện lớp vẫn rõ). |
|  | `3` | Che khuất trên 70% đến ngưỡng nhận diện tối đa (~85%). Nếu bị che > 85% không còn đủ đặc trưng nhận diện thì KHÔNG gán; nếu còn phân vân thì gán và bật `can_review`. |
| `truncated` | `no` | Toàn bộ vật thể nằm trọn vẹn trong khung hình ảnh (kể cả khi nằm sát mép). |
|  | `yes` | Vật thể thực tế bị cắt cụt bởi ít nhất một mép ảnh (vật tiếp tục đi ra ngoài biên ảnh). |
| `can_review` | `false` | Nhãn lớp, viền box và thuộc tính đã rõ ràng theo đúng quy tắc. |
|  | `true` | Tình huống mơ hồ, có nhiều cách hiểu, hoặc chưa có tiền lệ trong guideline; bắt buộc ghi chú lý do để thảo luận ở Pha Calibration. |

*Lưu ý kỹ thuật:*
- **Quy tắc độc lập giữa Che khuất và Cắt mép:** `truncated` chỉ tính phần bị cắt bởi biên mép ảnh; `occluded_level` chỉ tính phần bị che bởi các vật thể khác trong bối cảnh. Tuyệt đối không cộng dồn phần cắt mép vào độ che khuất.
- Phần trăm che khuất được ước lượng dựa trên tổng thể hình dáng cấu trúc vật lý của vật thể, không tính theo tỷ lệ diện tích bounding box.
- Với lớp `ignore_region`: Không áp dụng thuộc tính `occluded_level` và `truncated` (để giá trị mặc định).

## 3. Quy tắc vẽ Bounding Box

- **`R-01` [Ôm sát biên nhìn thấy - Tightness]:** Box phải ôm khít biên ngoài cùng của phần vật thể thực tế nhìn thấy được. Với vật thể bị che khuất một phần, tuyệt đối không tự tưởng tượng để vẽ phần bị khuất.
- **`R-02` [Loại trừ bóng đổ và chi tiết siêu mảnh]:** Không kéo dài box để bao trùm bóng đổ (*Drop shadow*) của người hoặc xe in trên mặt đường. Không cố kéo box để ôm các chi tiết phụ siêu mảnh không mang đặc trưng nhận dạng (dây ăng-ten radio, cán cờ).
- **`R-03` [Tách biệt người và phương tiện]:** Người lái và hành khách trên các phương tiện hở (xe máy, xe đạp, xích lô, tuk-tuk) phải được gán thành các box `person` riêng biệt, cho phép chồng lấn (*overlap*) với box của xe; không gộp chung người vào box xe.
- **`R-04` [Người ngồi trong cabin kín]:** Người ngồi bên trong cabin xe con (`car`), xe tải (`truck`), hoặc xe buýt (`bus`) kín cửa kính thì **không gán box `person`** (trừ trường hợp thò hẳn phần thân trên ra ngoài cửa sổ).
- **`R-05` [Phụ kiện cố định và hàng hóa rời]:** 
  - Tính các bộ phận cố định của xe (gương chiếu hậu, bánh xe dự phòng treo sau) vào box của phương tiện.
  - Hàng hóa cồng kềnh, rời rạc chở sau xe không tự động nhập vào box xe nếu làm phình to box; nếu hàng che khuất một phần xe thì tăng `occluded_level` của xe.
  - Với người gánh hàng rong, box `person` ôm cơ thể và phụ kiện gắn liền (quần áo, nón mũ, balo đeo); **không tính đòn gánh và thúng hàng** vào box người.
- **`R-06` [Vật thể không thật - False objects]:** Tuyệt đối không gán box cho người/xe xuất hiện trên biển quảng cáo (pano), màn hình điện tử, bóng phản chiếu qua gương/mặt kính, tượng đài, tượng trang trí, ma-nơ-canh hoặc mô hình đồ chơi.
- **`R-07` [Xe chủ - Ego-vehicle]:** Không gán box cho các bộ phận của xe chở camera (nắp capo, cần gạt nước, gương chiếu hậu bên trong) lọt vào góc dưới khung hình.
- **`R-08` [Ngưỡng kích thước & Ignore]:** Vật thể riêng lẻ có chiều cao từ 20 px trở lên mới vẽ box riêng. Cụm đối tượng dày đặc dưới 20 px hoặc ở quá xa thì dùng `ignore_region` bao quanh cả cụm.
- **`R-09` [Chất lượng ảnh kém & can_review]:** Trong điều kiện thiếu sáng, ảnh mờ hoặc nhiễu hạt, chỉ gán khi còn đủ đặc trưng nhận dạng lớp và ranh giới vật thể. Nếu không chắc chắn, bật `can_review = true`.

## 4. Ca khó và Quyết định đồng thuận của nhóm

| Ảnh | Tình huống cụ thể | Quyết định đồng thuận của nhóm | Quy tắc áp dụng | Căn cứ kỹ thuật / Lý do |
|---|---|---|---|---|
| `img_002.jpg` | Xe chở xe (*car carrier*) đang cõng các ô tô con trên sàn. | Gán 1 box `truck` cho toàn bộ xe chở xe. Không tạo box `car` riêng cho các ô tô con nằm trên thùng xe. | `R-05` | Các xe con trên sàn được định nghĩa là hàng hóa chuyên chở, không phải đối tượng đang độc lập tham gia giao thông. |
| `img_003.jpg` | Người bán hàng rong gánh hoa quả/đồ vật trên phố. | Gán 1 box `person` ôm sát người; không tính đòn gánh và thúng hàng vào box. Không gán nhãn phương tiện cho quang gánh. | `R-05` | Đồ gánh không có bánh xe nên không thuộc taxonomy phương tiện; loại đòn gánh ra khỏi box người để giữ IoU khít. |
| `img_004.jpg` | Hai xe tuk-tuk đỗ cạnh nhau trên vỉa hè ban đêm. | Gán lớp `three_wheeler` cho từng xe tuk-tuk; xe bị che lấp chọn `occluded_level = 1` hoặc `2`. | `R-01` | Tuk-tuk có cấu hình 3 bánh đặc thù của giao thông châu Á, không xếp lẫn vào xe con hay xe máy. |
| `img_011.jpg` | Nhân vật Predator đứng cạnh dàn xe máy độ ngoài trời. | Gán `person` nếu là người thật mặc trang phục cosplay/hóa trang. Gán box `motorcycle` riêng cho các xe máy nhìn thấy. | `R-03` | Trang phục hóa trang không làm thay đổi bản chất đối tượng là người sống đang di chuyển trong cảnh. |
| `img_017.jpg`, `img_027.jpg` | Tượng người cưỡi ngựa / Bia tưởng niệm chạm khắc hình xe bán tải. | Tuyệt đối KHÔNG gán nhãn cho tượng người hoặc tượng xe. | `R-06` | Đây là các tác phẩm điêu khắc nghệ thuật/công trình kiến trúc tĩnh, tránh dạy mô hình phát hiện dương tính giả (FP). |
| `img_018.jpg` | Biển quảng cáo pano bầu cử in hình người kích thước lớn. | Tuyệt đối KHÔNG gán box `person`. | `R-06` | Hình in 2D trên biển bảng không phải thực thể sống, không tham gia giao thông. |
| `img_020.jpg` | Hình ảnh cầu vồng và ngoại cảnh phản chiếu qua gương chiếu hậu ô tô. | KHÔNG gán box cho các vật thể chỉ xuất hiện qua bóng gương phản chiếu. | `R-06` | Tránh hiện tượng phát hiện bóng ma; gương là bộ phận của xe chủ hoặc xe khác, hình ảnh trong gương mang tính ảo thị giác. |
| `img_022.jpg` | Xe mô tô 3 bánh có gắn thùng phụ bên hông (*motorcycle with sidecar*). | Gán box `three_wheeler` ôm trọn cả xe máy và phần sidecar 3 bánh; gán box `person` riêng cho người ngồi trên xe. | `R-01`, `R-03` | Cấu hình xe 3 bánh chiếm diện tích làn đường tương đương xe tuk-tuk, thuộc lớp `three_wheeler`. |
| `img_024.jpg` | Xe ô tô con màu đen chở xe mô hình màu vàng thu nhỏ trên giá nóc trong phòng trưng bày. | Gán 1 box `car` cho xe ô tô con màu đen (box chỉ ôm tới nóc xe đen). Chiếc xe màu vàng trên nóc là xe mô hình đồ chơi, TUYỆT ĐỐI KHÔNG gán box. | `R-01`, `R-05`, `R-06` | Xe đen là xe wagon chở người (`car`); xe vàng là mô hình đồ chơi tĩnh theo `R-06` nên loại trừ hoàn toàn, tránh gây dương tính giả (FP). |
| `img_028.jpg` | Xe bán tải chở tượng lớn trên thùng hàng (*yorse*). | Gán 1 box `truck` cho xe bán tải. Tượng trên thùng xe coi là hàng hóa, KHÔNG gán box người hay động vật. | `R-05`, `R-06` | Pickup thùng hở thuộc `truck`; tượng điêu khắc không thuộc đối tượng người/phương tiện cần nhận dạng. |
| `img_029.jpg` | Xe máy chở khối hàng cồng kềnh quá khổ (*dangerous load*). | Box `motorcycle` chỉ ôm khung xe máy, không ôm khối hàng cồng kềnh; người lái bị che khuất thì tăng `occluded_level`. | `R-05` | Giữ bounding box của xe máy luôn khít với khung xe, tránh làm loãng dữ liệu huấn luyện detector. |
| `img_033.jpg`, `img_038.jpg` | Ma-nơ-canh mặc quần áo trong tủ kính / Tủ trưng bày mô hình xe đồ chơi. | Tuyệt đối KHÔNG gán box cho ma-nơ-canh hoặc mô hình xe thu nhỏ. | `R-06` | Tránh gây nhiễu cho mô hình thị giác trong môi trường thương mại/đô thị. |
| `img_035.jpg` | Xe lam/tuk-tuk chở khách chen chúc quá tải (*overloaded rickshaw*). | Gán 1 box `three_wheeler` cho xe; gán `person` cho từng hành khách nhìn thấy rõ; người bị che lấp quá sâu trong cabin thì bật `can_review`. | `R-03`, `R-09` | Tách nhãn người tối đa có thể để tăng độ phủ dữ liệu trong các tình huống kẹt xe/quá tải phức tạp. |

## 5. QC và Lịch sử phiên bản

### 5.1. Quy trình QC nội bộ của nhóm
- **Gán độc lập ở Pha 3:** Từng thành viên gán ảnh được phân công độc lập theo đúng `submission/qc/assignment.csv`. Tuyệt đối không nhìn nhãn của nhau trong quá trình gán để đảm bảo tính khách quan của phép đo IAA.
- **Ngưỡng chỉ số mục tiêu:** Mean IoU ≥ 0.85 (cho box lớn ≥ 50px) và độ đồng thuận phân loại lớp ≥ 95%.
- **Quy tắc giải quyết bất đồng:** Mọi ca lệch nhãn giữa hai annotator sẽ được trích xuất tự động qua script `qc_compare.py` và giải quyết dứt điểm tại buổi họp Calibration ở Pha 5; quyết định cuối cùng được ghi vào `calibration_decisions.csv` (dẫn mã `rule_id` từ `R-01` đến `R-09`).

### 5.2. Bảng lịch sử thay đổi (Changelog: v0 ➔ v1.0)

| Phiên bản | Nội dung thay đổi | Căn cứ kỹ thuật & Ca ảnh giải quyết |
|---|---|---|
| **v0** | Bản sơ sài do giảng viên cung cấp ban đầu: chỉ có 6 lớp, thuộc tính định tính chung chung, chưa có quy tắc cho ca khó. | Baseline ban đầu. |
| **v1.0** | **1. Mở rộng Taxonomy:** Thêm 4 lớp chính thức: `bus`, `three_wheeler`, `bicycle`, `other_vehicle`. | Xử lý triệt để xe tuk-tuk (`img_004`), xích lô (`img_006`), mô tô sidecar (`img_022`), xe buýt (`img_032`). |
| **v1.0** | **2. Định lượng thuộc tính:** Chuẩn hóa `occluded_level` thành các khoảng phần trăm rõ ràng (0–10%, 10–35%, 35–70%, 70–85%); tách độc lập `truncated` và `occluded_level`. | Tránh lỗi phạt trùng lặp hai lần và loại bỏ đánh giá cảm tính. |
| **v1.0** | **3. Bổ sung hệ thống quy tắc Bounding Box (`R-01` → `R-09`):** Thêm quy tắc loại trừ bóng đổ, ăng-ten mảnh, người trong cabin kín, xe chở hàng cồng kềnh và xe chủ. | Tránh làm phình to box (bloated box), đảm bảo Mean IoU đạt ngưỡng ≥ 0.85; giải quyết ca `img_002`, `img_003`, `img_029`. |
| **v1.0** | **4. Loại trừ vật thể ảo/không thật:** Cấm gán nhãn cho biển quảng cáo, ma-nơ-canh, bóng gương phản chiếu, tượng nghệ thuật (`R-06`). | Tránh lỗi nhận nhầm (False Positive); giải quyết triệt để ca `img_011`, `img_017`, `img_018`, `img_020`, `img_027`, `img_033`, `img_038`. |
