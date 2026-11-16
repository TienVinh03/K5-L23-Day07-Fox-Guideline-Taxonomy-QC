# Guideline gán nhãn — v1.0-cá-nhân — 2A202603269 - Vũ Tiến Vinh

- Phiên bản: `v1.0-ca-nhan` (phát triển từ `guideline_v0_starter.md`).
- Ngày: 16/11/2026.
- Người viết: Vũ Tiến Vinh — MSHV: `2A202603269`.
- Dữ liệu tham chiếu: 40 ảnh `img_001.jpg` đến `img_040.jpg` trong `data/images/`.
- Loại nhãn: bounding box chữ nhật song song với trục ảnh.
- Công cụ: CVAT, xuất COCO 1.0.
- Trạng thái: đề xuất cá nhân trước họp nhóm; các mục ghi **chưa chốt** phải được xử lý ở Pha 2. Đây chưa phải guideline thống nhất để gán nhãn Pha 3.

---

## 1. Mục tiêu

Nhãn dùng để huấn luyện mô hình phát hiện **người thật và phương tiện thật** trong cảnh đường phố. Mô hình cần nhận biết cả đối tượng đang di chuyển, dừng, đỗ, được vận chuyển hoặc trưng bày nếu đó là phương tiện thật.

Ưu tiên tránh bỏ sót người bị che, nhầm người với tượng/ma-nơ-canh, nhầm xe thật với đồ chơi và gộp người điều khiển vào box phương tiện. Những lỗi này làm mô hình học sai số lượng, vị trí và loại đối tượng.

Phạm vi gồm các lớp ở mục 2. Không suy ra vật thể chỉ từ tên ảnh, ngữ cảnh, ánh đèn hoặc bộ phận không đủ nhận dạng. Không gán phần cơ thể hay thân xe hoàn toàn không nhìn thấy. Bảng ca khó là hướng dẫn xử lý, không phải danh sách toàn bộ đối tượng phải gán trong từng ảnh.

## 2. Lớp và thuộc tính

### 2.1. Lớp

Mỗi đối tượng chỉ nhận **một lớp**. Một ảnh có thể có nhiều đối tượng khác lớp. `ignore_region` là vùng loại khỏi đánh giá, không phải một loại người hoặc phương tiện.

| Lớp | Định nghĩa | Ví dụ trong bộ ảnh | Phản ví dụ / dễ nhầm |
|---|---|---|---|
| `car` | Ô tô du lịch có thân xe liền, gồm sedan, hatchback, coupe, wagon, SUV và crossover, không có thùng hàng tách biệt phía sau cabin. | Xe con được chở trên xe chuyên dụng ở `img_002`; xe con ở `img_015`; taxi xanh ở `img_040`. | Pickup là `truck`; xe thân hộp kiểu van là `van`; mô hình vàng trên nóc xe ở `img_024` không phải xe con thật. |
| `truck` | Phương tiện có cabin và khu vực chở hàng tách biệt về kết cấu, gồm pickup, xe tải thùng, xe tải nhỏ và xe chuyên chở xe. | Pickup ở `img_001`, `img_012`, `img_037`; xe tải ở `img_007`, `img_036`; xe chở xe ở `img_002`. | Không đổi pickup thành `van` chỉ vì lắp nắp thùng; tượng pickup ở `img_027` không thuộc lớp. |
| `van` | Xe thân hộp tương đối ngắn, nóc cao, khoang lái và khoang phía sau nằm trong cùng thân vỏ, thường có cửa trượt hoặc cửa sau lớn, không có thùng hàng tách cabin. | Xe màu bạc phía sau người đi xe máy ở `img_005`; xe van trắng ở `img_016`. | Xe tải nhỏ có thùng riêng ở `img_036` là `truck`; minibus thân dài ở `img_009` là `bus`. |
| `bus` | Xe chở khách có thân dài, khoang hành khách với dãy cửa sổ kéo dài dọc thân và cửa lên xuống kiểu xe buýt, gồm cả minibus có cấu trúc này. | Minibus ở `img_009`; xe buýt ở `img_037`, `img_040`. | Không dùng số ghế phỏng đoán để đổi van thành bus; xe van ngắn ở `img_005`, `img_016` vẫn là `van`. |
| `motorcycle` | Xe máy hoặc mô tô có kết cấu hai bánh chính trước–sau, gồm scooter và mô tô gắn thùng bên hông liền với xe (sidecar). | Xe máy ở `img_005`, `img_008`, `img_026`; mô tô có sidecar ở `img_022`. | Tuk-tuk có cabin ba bánh riêng là `tuk_tuk`; xe đạp là `bicycle`; xe mô hình/điêu khắc chưa xác nhận không tự gán vào lớp. |
| `bicycle` | Xe đạp hai bánh có khung, tay lái, yên và cơ cấu bàn đạp nhận biết được. | Xe đạp ở `img_003`, `img_006`, `img_029`, `img_039`. | Xích lô ba bánh chở khách là `cyclo`; xe máy có động cơ rõ ràng là `motorcycle`. |
| `cyclo` | Xích lô đạp chân ba bánh có ghế hoặc khoang chở khách riêng và vị trí người đạp phía sau. | Xích lô nhìn từ phía sau ở `img_023`. | Xe đạp hai bánh là `bicycle`; xe ba bánh có động cơ và cabin lái kiểu tuk-tuk là `tuk_tuk`. |
| `tuk_tuk` | Xe ba bánh có động cơ, một bánh trước và hai bánh sau, có cabin lái cùng khoang chở người/hàng tích hợp. | Hai tuk-tuk xanh ở `img_004`; các xe ở `img_016`, `img_035`, `img_040`. | Không gộp vào `motorcycle`; mô tô có thùng bên hông ở `img_022` vẫn là `motorcycle`; xích lô đạp chân là `cyclo`. |
| `person` | Một người thật, gồm người đi bộ, người đứng/ngồi, người lái và hành khách có phần cơ thể nhìn thấy đủ nhận dạng. | Người bán hàng ở `img_003`; người ngồi trên xe ở `img_005`, `img_026`; người đi bộ ở `img_034`. | Tượng người ở `img_017`, chân dung in ở `img_018`, ma-nơ-canh ở `img_033` và búp bê ở `img_038` không thuộc lớp. |
| `ignore_region` | Vùng chứa đối tượng mục tiêu quá nhỏ, không thể tách đáng tin cậy hoặc chưa thể xác định lớp/thật–giả sau khi kiểm tra. | Cụm xe/người rất xa ở `img_021`; phần đám đông không tách được ở `img_019`, `img_032`. | Không phủ cả ảnh vì đông; không dùng để che đối tượng rõ ràng, lỗi bỏ sót hoặc vật đã biết chắc là đồ chơi/tượng. |

**Thứ tự phân biệt các lớp dễ nhầm:**

1. Kiểm tra vật có thật hay chỉ là tượng, hình in, phản chiếu hoặc mô hình. Vật chắc chắn không thật thì bỏ qua như nền.
2. Với ô tô: có thùng/khu vực chở hàng riêng sau cabin → `truck`, kể cả pickup lắp nắp. Nếu không: thân xe chở khách dài kiểu bus → `bus`; thân hộp ngắn kiểu van → `van`; thân xe du lịch → `car`.
3. Với xe nhỏ: mô tô gắn sidecar → `motorcycle`; cabin ba bánh có động cơ → `tuk_tuk`; xích lô đạp chở khách → `cyclo`; xe hai bánh có bàn đạp → `bicycle`; xe hai bánh có động cơ → `motorcycle`.
4. Nếu phần nhìn thấy chưa đủ phân biệt hai lớp: không chọn lớp theo cảm giác; dùng vùng `ignore_region` cục bộ với `can_review = true`, ghi hai khả năng và chuyển họp nhóm.

**Đối tượng ngoài taxonomy:** động vật, cây, cột, biển báo, ghế, hàng hóa rời, xe đẩy tay và đồ trang trí không có lớp riêng thì không gán. Không ép chúng vào lớp gần giống và không tự thêm lớp khi đang gán. Tượng, đồ chơi, hình quảng cáo đã xác định rõ là **mẫu nền âm**, không cần `ignore_region`. Xe thật chở một vật ngoài taxonomy vẫn được gán theo lớp của xe.

**Các lớp bổ sung so với v0:** `bus`, `bicycle`, `cyclo`, `tuk_tuk`. Đây là đề xuất để bao phủ phương tiện xuất hiện trong ảnh. Sau khi nhóm thống nhất mới cập nhật `submission/taxonomy.json` và sinh lại nhãn CVAT; bản cá nhân này không thay đổi taxonomy dùng chung.

### 2.2. Thuộc tính

Các thuộc tính áp dụng cho từng box đối tượng. Không dùng thuộc tính để thay thế lớp hoặc để đánh dấu xe đang chạy/đỗ.

| Thuộc tính | Giá trị cố định | Quy tắc |
|---|---|---|
| `occluded_level` | `0`, `1`, `2`, `3` | Mức che bởi vật khác, tính theo tỉ lệ diện tích hình chiếu đối tượng bị che trong phạm vi ảnh; ngưỡng ở bảng dưới. |
| `truncated` | `no`, `yes` | `yes` khi đường biên ảnh cắt mất một phần đối tượng; `no` khi vật nằm trọn trong ảnh, kể cả bị vật khác che. |
| `can_review` | `false`, `true` | `true` nếu còn bất định về lớp, thật–giả, ranh giới, mức che hoặc cách áp dụng quy tắc; phải có ghi chú nêu vấn đề. |

Gọi `r` là phần trăm diện tích hình chiếu đối tượng bị một vật khác che, so với diện tích đối tượng dự kiến **chỉ trong khung ảnh**. Ước lượng theo hình dạng, không suy diễn chi tiết không nhìn thấy; phần bị cắt ngoài ảnh không đưa vào `r`.

| `occluded_level` | Ngưỡng | Cách hiểu |
|---|---|---|
| `0` | `r = 0%` | Không bị vật khác che; bóng tối, ảnh đen trắng hoặc nhòe chuyển động không tự làm tăng mức che. |
| `1` | `0% < r <= 25%` | Che ít, chẳng hạn cột hẹp hoặc một phần tay/chân bị vật khác chắn. |
| `2` | `25% < r <= 75%` | Che đáng kể, còn đủ dấu hiệu để nhận dạng lớp và xác định box phần thấy được. |
| `3` | `75% < r < 100%`, hoặc không thể ước lượng đáng tin cậy | Che rất nhiều hoặc không rõ mức che; bắt buộc `can_review = true`. Nếu không còn đủ dấu hiệu xác định lớp thì dùng `ignore_region` thay cho box lớp đoán. |

- Vật hoàn toàn không nhìn thấy (`r = 100%`) không tạo box dự đoán.
- Nếu không phân biệt được hai mức ở gần mốc 25% hoặc 75%: tạm chọn mức che cao hơn và bật `can_review`, ghi hai mức cần duyệt.
- Vừa bị che vừa bị cắt mép thì điền cả hai thuộc tính độc lập. Ví dụ: xe bị người chắn và còn bị cắt ở cạnh dưới có thể đồng thời `occluded_level = 2`, `truncated = yes`.
- Người nhìn qua kính trong vẫn được gán nếu nhận dạng được và đạt kích thước tối thiểu; chỉ phần bị khung cửa, thân xe hoặc vùng kính không nhìn xuyên qua chắn mới tính là bị che.
- Box `ignore_region` dùng `occluded_level = 0`, `truncated = no` như giá trị quy ước không mang nghĩa vật thể. Đặt `can_review = true` nếu vùng được tạo vì ca chưa chốt; vùng bỏ qua đúng quy tắc kích thước/đám đông đã rõ thì đặt `false`.

## 3. Quy tắc vẽ box

### 3.1. Đơn vị và phần được bao

1. Làm việc trên **ảnh gốc**, không đo ngưỡng pixel trên ảnh thu nhỏ hoặc ảnh ghép. Với ảnh rộng `W`, cao `H`, box phải thỏa `0 <= x_min < x_max <= W`, `0 <= y_min < y_max <= H`.
2. Mỗi đối tượng một box ôm sát **toàn bộ phần nhìn thấy**. Nếu các phần thấy được bị ngắt bởi vật che, lấy hình chữ nhật nhỏ nhất bao tất cả các phần thuộc cùng đối tượng. Không kéo box tới vị trí đầu, chân, bánh hoặc đuôi xe chỉ suy đoán.
3. Với người: bao cơ thể, tóc, quần áo, giày và mũ/đồ bảo hộ đang mặc. Không mở box theo túi cầm tay, ô, đòn gánh hoặc hàng hóa mang theo.
4. Với xe: bao thân xe và bộ phận gắn cố định nhìn thấy như bánh, gương, tay lái, mái cabin, thùng xe, nắp thùng và sidecar. Không mở box theo người ngồi trên xe, bao tải, thanh kim loại, tượng hoặc hàng hóa rời đang chở.
5. Bóng đổ, ánh đèn loang, hình phản chiếu và vệt nhòe không có đường biên vật thể nhận dạng được không thuộc box. Box chữ nhật có thể chứa nền hoặc vật che ở giữa; không chia một người/xe thành nhiều box chỉ để tránh nền.
6. Phương tiện đỗ, không có người hoặc xe thật trưng bày vẫn gán. Không dùng khả năng tự chạy tại thời điểm chụp làm tiêu chí duy nhất: xe thật đang được vận chuyển ở `img_002` vẫn là đối tượng.

### 3.2. Che khuất, cắt mép và chất lượng ảnh

- **Bị che:** chỉ ôm phần thấy được, sau đó chọn `occluded_level`. Ví dụ người chỉ thấy đầu và thân trên thì không kéo xuống chân giả định.
- **Bị cắt mép:** box dừng tại mép ảnh và đặt `truncated = yes`. Box chạm mép chưa đủ kết luận bị cắt; phải có dấu hiệu đường biên vật còn tiếp tục ra ngoài ảnh.
- **Nhòe/tối:** vẫn gán nếu xác định được lớp và ranh giới sau khi phóng to ảnh gốc. Nếu biết có đối tượng mục tiêu nhưng không tách được lớp/ranh giới, khoanh `ignore_region` cục bộ, bật `can_review` và nêu nguyên nhân. Không gán theo mỗi vệt đèn hoặc bóng mơ hồ.
- Không suy số người từ số ghế, số mũ hoặc thông tin trong tên file. Chỉ tạo box khi phần nhìn thấy đủ nhận dạng là một người riêng biệt.

### 3.3. Người và xe; xe chở xe

- Người lái, từng hành khách và phương tiện là **các đối tượng riêng**. Hai người ngồi trên một xe máy có thể tạo hai box `person` và một box `motorcycle`, nếu từng đối tượng đều đủ điều kiện kích thước/nhận dạng.
- Box người và box xe được chồng lên nhau khi hình chữ nhật bao phần nhìn thấy giao nhau. Không tạo thêm một box chung “người + xe”.
- Xích lô gồm một box `cyclo`; người đạp và từng hành khách nhìn thấy được gán `person` riêng.
- Mô tô có sidecar gắn liền chỉ có một box `motorcycle` cho cả kết cấu; người ngồi trong sidecar là `person` riêng nếu có và nhìn thấy đủ rõ.
- Xe chở xe ở `img_002`: một box `truck` cho đầu kéo/cabin và kết cấu sàn chở liền theo đoàn xe; mỗi xe thật trên sàn có box riêng theo lớp. Không mở box xe vận chuyển theo phần xe hàng nhô cao hơn kết cấu sàn. Không coi thanh khung sàn là một lớp riêng.
- Với ảnh chỉ thấy một phần nhỏ của xe chủ/camera (gương, mép cửa, tay lái), không tạo box phương tiện từ bộ phận đó. Người nhìn thấy rõ ở tiền cảnh vẫn được xét gán độc lập. Ví dụ `img_020` không gán chiếc gương xe chủ; `img_040` không dựng box xe máy chỉ từ các gương ở cạnh dưới.

### 3.4. Vật quá nhỏ, đám đông và vùng bỏ qua

- Chiều cao dùng xét ngưỡng là `h = y_max - y_min` của box **phần nhìn thấy trong ảnh gốc**.
- `h >= 20 px`: gán riêng nếu nhận dạng lớp và tách được ranh giới. `h = 20 px` vẫn phải gán; không áp dụng thêm một ngưỡng chiều rộng tự đặt.
- `h < 20 px`: không gán từng đối tượng; vẽ `ignore_region` ôm vùng có đối tượng mục tiêu đó. Bản thân vùng ignore không cần cao ít nhất 20 px.
- Với đám đông/bãi xe: tách từng đối tượng đủ rõ trước. Chỉ dùng `ignore_region` cho cụm còn lại nếu không phân biệt được đối tượng nào với đối tượng nào; đông không đồng nghĩa được bỏ qua.
- Vùng ignore phải nhỏ nhất đủ bao cụm cần bỏ qua. Nếu vùng chữ nhật sẽ phủ đối tượng rõ ràng bên cạnh, chia thành các vùng nhỏ hơn và giữ box riêng của đối tượng rõ ràng.
- Không đặt box lớp thông thường và vùng ignore cho cùng một đối tượng. Không dùng ignore để bỏ qua cả tượng, đồ chơi hoặc hình in đã nhận dạng chắc chắn.

### 3.5. Phản chiếu, hình in, tượng và xe biến đổi

- Không gán hình trong gương, mặt kính, vũng nước, màn hình, biển quảng cáo hoặc áp-phích. Người/xe thật nhìn trực tiếp qua kính vẫn có thể gán; phải phân biệt ảnh xuyên kính với phản chiếu.
- Không gán tượng người, tượng xe, ma-nơ-canh, búp bê và xe đồ chơi. Không tạo lớp `person` chỉ vì vật có hình người hoặc lớp `car` chỉ vì có bốn bánh giả.
- Người thật hóa trang vẫn là `person`; xe thật trang trí hoặc cải tạo vẫn nhận lớp theo kết cấu. Không loại một xe chỉ vì màu sắc/hình dáng giống hoạt hình.
- Nếu ảnh đơn không đủ phân biệt người hóa trang với mô hình, hoặc xe tùy biến với tác phẩm điêu khắc: đánh dấu **chưa chốt**, dùng ignore cục bộ với `can_review = true` trong bản tạm, ghi vị trí cụ thể để họp. Không coi lựa chọn tạm là kết luận đúng.

## 4. Ca khó

Các đường dẫn dưới đây mở ảnh gốc. Mức che và cắt mép phải xét trên từng đối tượng, không sao chép cùng một giá trị cho toàn ảnh.

| Ảnh | Tình huống quan sát | Quyết định / cách xử lý | Lý do |
|---|---|---|---|
| [img_001](../../../data/images/img_001.jpg) | Pickup đỗ trước quán; nhiều người ngồi bị bàn, xe và người khác che; phần xe/người chạm mép ảnh. | Pickup là `truck`; tách từng `person` nhận dạng được; box chỉ bao phần thấy. Xét che và cắt mép riêng, không gán bàn ghế/hàng trong thùng. | Đỗ xe không làm mất nhãn; pickup không phải `car`; che khác với bị cắt mép. |
| [img_002](../../../data/images/img_002.jpg) | Xe tải chuyên dụng chở nhiều xe con và pickup, khung sàn chắn một phần xe hàng. | Gán `truck` cho xe chuyên chở; gán riêng từng xe thật trên sàn: xe con → `car`, pickup → `truck`; khung sàn che xe hàng làm tăng mức che tương ứng. | Phương tiện được chở vẫn là vật thể riêng; không gộp cả đoàn thành một đối tượng duy nhất. |
| [img_003](../../../data/images/img_003.jpg) | Người bán hàng mang đòn gánh và giỏ lớn, có xe đạp/xe máy đan xen. | Box `person` không kéo theo đòn gánh/giỏ; tách `bicycle` và `motorcycle` theo kết cấu; gán các phần xe bị cắt mép nếu vẫn đủ điều kiện. | Hàng mang theo không phải cơ thể; xe đạp không phải xe máy. |
| [img_004](../../../data/images/img_004.jpg) | Hai tuk-tuk xanh đỗ sát nhau, có người trong cabin. | Mỗi xe một `tuk_tuk`; người trong cabin đủ rõ có `person` riêng, mức che theo phần bị cabin chắn. | Xe ba bánh có cabin không gộp vào `motorcycle`; người không thuộc box xe. |
| [img_005](../../../data/images/img_005.jpg) | Nhiều người ngồi trên xe máy che nhau; xe van bạc phía sau bị chắn. | Tách từng `person`, từng `motorcycle` nhận dạng được; xe thân hộp phía sau là `van`; không kéo box xe lên theo mũ người. | Phân biệt phương tiện với người ngồi trên xe và phân biệt van với xe con. |
| [img_006](../../../data/images/img_006.jpg) | Xe máy, xe đạp và người đan xen; xe đạp/người tiền cảnh bên phải bị cắt ở đáy. | Gán riêng `motorcycle`, `bicycle`, `person`, `car` khi đủ rõ; phần bị mép dưới cắt đặt `truncated = yes`; cụm rất xa dưới ngưỡng dùng ignore cục bộ. | Không gọi mọi xe hai bánh là xe máy và không coi cắt mép là che bởi vật khác. |
| [img_007](../../../data/images/img_007.jpg) | Xe tải lớn và xe có cabin pickup gắn thùng hàng kín trong kho tối. | Cả hai là `truck` do có khoang hàng tách cabin; xe bên phải bị cắt mép phải xét `truncated`; không đổi sang `van` do thùng kín. | Kết cấu quyết định lớp, kích thước nhỏ hay thùng kín không đủ để gọi là van. |
| [img_008](../../../data/images/img_008.jpg) | Bãi xe máy/đường phố ban đêm, xe đỗ và người mặc áo phản quang chồng lấp. | Gán từng `motorcycle` đỗ lẫn xe đang đi và từng `person` rõ; chỉ ignore cụm xa không tách được; áo phản quang thuộc box người. | Trạng thái đỗ/di chuyển không làm đổi lớp; không bỏ cả bãi xe vì đông. |
| [img_009](../../../data/images/img_009.jpg) | Minibus có thân dài và dãy cửa sổ chở khách, người trong xe nhỏ/bị khung cửa che. | Xe là `bus`; chỉ gán người nhìn qua cửa kính khi phần thấy đủ nhận dạng và cao ít nhất 20 px; không suy số hành khách. | Phân biệt cấu trúc minibus với van; không đếm người từ số cửa sổ hoặc ghế. |
| [img_010](../../../data/images/img_010.jpg) | Người mua/bán cạnh xe máy và xe đạp chở hàng; trái cây, túi hàng che thân xe. | Tách người, xe máy và xe đạp; không bao trái cây/túi hàng vào box xe; phần xe bị hàng che vẫn chỉ bao phần nhìn thấy. | Hàng hóa không tạo lớp mới và không trở thành một phần kích thước phương tiện. |
| [img_011](../../../data/images/img_011.jpg) | Hình nhân Predator lớn và các vật có dạng mô tô trang trí bằng kim loại; người thật ở nền. | Gán người và xe thật ở nền khi đủ rõ. **Chưa chốt** hình nhân và từng mô tô tiền cảnh là người/xe thật hay mô hình: ignore cục bộ + `can_review = true`, ghi đối tượng cần kiểm tra. | Một ảnh chưa đủ xác nhận mọi vật trang trí có hoạt động thật hoặc có người bên trong; không dùng hình dáng hay tên ảnh để khẳng định. |
| [img_012](../../../data/images/img_012.jpg) | Pickup chở thùng giấy, túi và nông sản; xe bị cắt ở cạnh dưới. | Pickup là `truck`, đặt `truncated = yes` khi thân xe bị cắt; box không mở lên theo hàng hóa; xe con bên trái gán `car` riêng. | Thùng chở hàng phân biệt pickup với car; hàng là tải rời. |
| [img_013](../../../data/images/img_013.jpg) | Cảnh đêm đường ướt; người và xe nhỏ, nhiều đèn/phản sáng trên mặt đường. | Chỉ gán người/xe trực tiếp đủ rõ và đạt ngưỡng; không gán phản sáng trên đường; ignore phần có mục tiêu quá nhỏ/không tách được. | Độ sáng không chứng minh có một đối tượng riêng; ảnh tối không tự là mức che cao. |
| [img_014](../../../data/images/img_014.jpg) | Hai người qua đường rõ; người và xe đang chạy phía sau/tiền cảnh nhòe mạnh. | Hai người đứng có box `person` riêng; đối tượng nhòe vẫn gán nếu xác định được lớp/biên. Phần nhòe không tách được dùng ignore + review; không bao vệt kéo dài mờ vào box. | Nhòe chuyển động là vấn đề nhận dạng/ranh giới, không phải tự động `occluded_level = 3`. |
| [img_015](../../../data/images/img_015.jpg) | Xe đỏ và xe xanh tiền cảnh chỉ lộ một phần; có ô tô phía đối diện và mặt đường mưa phản sáng. | Các xe tiền cảnh nhận dạng được là `car`, đặt `truncated = yes`; xe thật phía đối diện gán riêng; không gán ảnh phản chiếu dưới đường. | Vật bị cắt vẫn có nhãn nếu đủ điều kiện; phản chiếu không phải xe thứ hai. |
| [img_016](../../../data/images/img_016.jpg) | Tuk-tuk, scooter, van và người tiền cảnh cùng xuất hiện; xe/người ở mép ảnh bị cắt. | Tách `tuk_tuk`, `motorcycle`, `van`, `person`; gán một người cho từng người lái nhìn thấy đủ rõ; xét cắt mép độc lập. | Đây là ca phân biệt nhiều lớp phương tiện, không gán tất cả xe nhỏ vào một lớp. |
| [img_017](../../../data/images/img_017.jpg) | Tượng người cưỡi ngựa, đầu tượng đội cọc tiêu. | Không gán `person`; tượng ngựa và cọc tiêu cũng không thuộc taxonomy; không cần ignore cho tượng đã rõ. | Đối tượng giả là nền âm, không phải người hoặc phương tiện thật. |
| [img_018](../../../data/images/img_018.jpg) | Chân dung trên billboard, xe thật phía dưới sau hàng rào; có dấu hiệu chụp qua kính. | Không gán người in trên billboard. Xe thật nhìn xuyên kính/hàng rào vẫn gán nếu đủ rõ; hàng rào gây che được tính mức che; không gán hình phản chiếu trên kính. | Hình in khác người thật; nhìn qua kính không đồng nghĩa là phản chiếu. |
| [img_019](../../../data/images/img_019.jpg) | Xe diễu hành vàng kiểu hoạt hình, người lái thật, hình nhân chuột và đám đông sau rào. | Xe kích thước thật có kết cấu ô tô và chở người lái được gán `car`; người lái là `person` nếu đạt ngưỡng. Hình nhân chuột **chưa chốt** có người hóa trang hay chỉ đạo cụ: review cục bộ. Đám đông tách được thì gán riêng, phần không tách được dùng ignore. | Không loại xe thật vì tạo hình hoạt hình; không tự suy người thật bên trong một hình nhân. |
| [img_020](../../../data/images/img_020.jpg) | Gương chiếu hậu cho thấy thân xe/đường phía sau; chỉ lộ một phần xe chủ. | Không gán nội dung phản chiếu và không tạo box xe chủ từ gương/mép cửa. Có thể không có box đối tượng trong ảnh này. | Không bắt buộc mỗi ảnh phải có nhãn; gương và hình trong gương không phải mục tiêu trực tiếp. |
| [img_021](../../../data/images/img_021.jpg) | Ùn xe máy dày đặc, người/xe tiền cảnh lớn nhưng xa dần chỉ còn cụm mũ và đèn. | Tách người và xe đủ rõ từ trước ra sau; đo chiều cao từng box. Chỉ vùng xa dưới 20 px hoặc không tách được mới dùng ignore. | Không đếm mỗi mũ thành một người nếu không xác định được cá thể và không phủ ignore lên toàn bộ dòng xe. |
| [img_022](../../../data/images/img_022.jpg) | Mô tô có thùng sidecar trong ảnh đen trắng. | Một box `motorcycle` bao cả xe và sidecar; không tách sidecar thành `car`/`tuk_tuk`; không suy có người nếu không thấy. | Sidecar là bộ phận gắn liền của mô tô; ảnh đen trắng không làm thay đổi lớp. |
| [img_023](../../../data/images/img_023.jpg) | Xích lô nhìn từ phía sau, người đạp ngồi cao; có xe con và xe máy xung quanh. | Một box `cyclo` bao phần khung/bánh/khoang khách thấy được, người đạp là `person` riêng; ô tô và xe máy giữ lớp riêng. | Xích lô không phải xe máy hay xe đạp hai bánh; người đạp không thuộc thân xe. |
| [img_024](../../../data/images/img_024.jpg) | Xe con thật trưng bày trong nhà, trên nóc xe đen là bản thu nhỏ màu vàng dạng vỏ xe. | Xe đen và các ô tô thật xung quanh là `car`; không gán mô hình vàng thành một xe thật và không mở box xe đen lên theo mô hình trên nóc. | Xe thật trưng bày vẫn thuộc lớp; mô hình thu nhỏ khác các xe thật được chở trong `img_002`. |
| [img_025](../../../data/images/img_025.jpg) | Người trên lối đi ven biển có nhiều kích thước, người ngồi thành nhóm và xe trên đường bên phải. | Người/xe đủ rõ cao ít nhất 20 px được gán; cụm xa nhỏ dùng ignore cục bộ; không gán ghế, cây, ô hoặc đồ bán hàng. | Áp dụng cùng ngưỡng cho người đi bộ và người ngồi, không bỏ người chỉ vì đang nghỉ. |
| [img_026](../../../data/images/img_026.jpg) | Hai người trên một xe máy trong hầm, phần xe bị cắt phía dưới; tường có hình quảng cáo. | Nếu cả hai người nhận dạng được và đạt ngưỡng: hai box `person`, một box `motorcycle`; đặt cắt mép cho đối tượng thực sự bị cắt; không gán hình in trên tường. | Tách người lái/hành khách; không gộp thành một vật “xe có hai người”. |
| [img_027](../../../data/images/img_027.jpg) | Công trình tưởng niệm tạo hình pickup đặt trên bệ. | Không gán `truck`, không cần ignore cho tượng xe đã rõ. | Đây là tượng xe, khác pickup thật đỗ trên đường. |
| [img_028](../../../data/images/img_028.jpg) | Pickup thật chở tượng ngựa trong thùng, có xe con ở nền. | Pickup là `truck`, xe con ở nền là `car`; bỏ qua tượng ngựa và không mở box truck lên hết tượng. | Vật chở không thuộc taxonomy không làm xe thật bị loại hay thay đổi lớp. |
| [img_029](../../../data/images/img_029.jpg) | Xe máy và xe đạp cạnh nhau chở các thanh kim loại dài, các thanh chắn người/xe. | Tách `motorcycle`, `bicycle` và từng `person`; không kéo box theo thanh kim loại; tính che theo phần cơ thể/thân xe bị chắn. | Vật dài là tải rời, không phải cấu trúc phương tiện hay cánh tay người. |
| [img_030](../../../data/images/img_030.jpg) | Nhiều xe máy đỗ sát, phần đầu/yên/bánh của các xe chồng lấp, có người trong bãi. | Gán mỗi `motorcycle` xác định được là một xe; không tạo box riêng cho từng đầu/yên của cùng xe; gán người thật đủ rõ; cụm không thể phân tách dùng ignore. | Tránh đếm trùng một xe và tránh dùng ignore cho cả bãi. |
| [img_031](../../../data/images/img_031.jpg) | Đường phố ban đêm có xe con, xe tải và nhiều ảnh phản sáng trên đường ướt. | Phân loại theo kết cấu xe nhìn trực tiếp; xe thùng hàng riêng là `truck`; không vẽ box trên vệt đèn dưới đường; đánh dấu review nếu không phân biệt được lớp xe xa. | Không dùng phản sáng làm biên box hoặc ánh đèn làm bằng chứng cho một xe khác. |
| [img_032](../../../data/images/img_032.jpg) | Đám đông chờ xe sau nhà chờ có kính, xe con tiền cảnh bị cắt mép trái. | Gán từng người qua kính nếu đủ rõ, không gán lặp ảnh phản chiếu; chỉ ignore phần đám đông không tách được. Xe tiền cảnh là `car`, đặt `truncated = yes`. | Kính trong không tự che khuất toàn bộ người; đám đông vẫn cần tách người rõ. |
| [img_033](../../../data/images/img_033.jpg) | Ma-nơ-canh sau kính cửa hàng, quần áo treo và phản chiếu ngoài đường. | Không gán ma-nơ-canh thành `person`; không gán quần áo treo hay người/xe chỉ xuất hiện dưới dạng phản chiếu. | Dáng giống người và có quần áo không đủ để là người thật. |
| [img_034](../../../data/images/img_034.jpg) | Người đi bộ giữa giao lộ; người và xe máy phía phải bị cắt mép, nhiều xe ở nền. | Người qua đường có `person` riêng; tách người/xe ở mép nếu đủ rõ, đặt cắt mép tương ứng; xe/người xa áp dụng ngưỡng 20 px. | Không bỏ đối tượng cắt mép; không suy có xích lô chỉ từ ngữ cảnh chụp. |
| [img_035](../../../data/images/img_035.jpg) | Tuk-tuk chở hàng chất rất cao, xe máy tiền cảnh và phương tiện khác bị che phía sau. | Tuk-tuk vẫn là `tuk_tuk`; không mở box lên hết bao hàng; tách người lái và xe máy; xe sau bị che chỉ gán khi nhận dạng được. | Hàng cao không biến tuk-tuk thành truck và không thuộc kích thước kết cấu xe. |
| [img_036](../../../data/images/img_036.jpg) | Xe tải nhỏ màu xanh, thùng hàng kín phủ bạt tách khỏi cabin. | Gán `truck`; bao cả cabin và thùng/bạt là phần cấu trúc thùng; xe máy xung quanh gán riêng nếu đủ rõ. | Xe nhỏ hoặc thùng kín không đồng nghĩa `van`; bạt che thùng gắn trên xe khác tải rời. |
| [img_037](../../../data/images/img_037.jpg) | Pickup lắp nắp thùng cao, gần xe buýt và người đi bộ. | Pickup là `truck`, nắp thùng thuộc box truck; xe buýt là `bus`; người có box riêng. | Nắp thùng không xóa kết cấu cabin–thùng riêng để biến pickup thành van. |
| [img_038](../../../data/images/img_038.jpg) | Tủ kính chứa ô tô mô hình, búp bê và thú đồ chơi. | Không gán `car` hay `person` cho đồ chơi; không dùng ignore chỉ vì có nhiều mô hình; ảnh có thể không có nhãn mục tiêu. | Mô hình kích thước nhỏ trong tủ là nền âm dù trông rất giống xe thật. |
| [img_039](../../../data/images/img_039.jpg) | Phố có xe máy trong bóng tối sát cửa hàng, người đi bộ và người đạp xe. | Không bỏ xe đỗ trong bóng tối nếu vẫn nhận dạng được; tách `person`, `motorcycle`, `bicycle`; chỉ ignore cụm thật sự quá nhỏ/không tách được. | Độ sáng và việc đứng yên không quyết định có thuộc lớp hay không. |
| [img_040](../../../data/images/img_040.jpg) | Người đội mũ ở tiền cảnh che taxi, phía sau có tuk-tuk, SUV và bus; chỉ thấy gương xe máy sát cạnh dưới. | Người là `person`, taxi/SUV là `car`, xe ba bánh là `tuk_tuk`, bus là `bus`; gán mức che cho taxi. Không dựng box motorcycle từ gương xe chủ hoặc gán chân dung in trên taxi. | Tách các lớp thật trong cùng cảnh; người chắn xe là che, mép ảnh cắt vật là truncation, gương không đủ đại diện cho cả xe. |

**Ca chưa chốt phải mang vào họp nhóm:**

- `img_011`: xác nhận riêng hình nhân Predator và từng xe trang trí tiền cảnh; cần bằng chứng phân biệt người hóa trang, mô tô thật và mô hình/điêu khắc. Không mặc định tất cả cùng một loại.
- `img_019`: xác nhận hình nhân chuột là trang phục có người hay đạo cụ. Việc gán xe và người lái thật không phụ thuộc kết luận này.
- Ranh giới phân loại `van`–`bus` khi chỉ thấy một phần xe: nếu các mốc tham chiếu ở `img_005`, `img_009`, `img_016` vẫn chưa đủ cho ca khác, nhóm phải thêm ví dụ và chốt trước Pha 3.

Ghi chú review cần có: `ảnh | vị trí/đối tượng | điều chưa chắc | lựa chọn tạm | câu hỏi cần chốt`. Với đối tượng bị loại khỏi nhãn vì chưa rõ, phải ghi cả trong danh sách review, không chỉ để một vùng ignore không có lý do. Các ca thật–giả còn mở là khoảng trống guideline và chưa được dùng để kết luận Release.

## 5. QC và phiên bản

### 5.1. Kiểm tra trước khi nộp nhãn

1. Đúng ảnh được phân công, đúng taxonomy đã được nhóm thống nhất, không tự đổi tên lớp hoặc giá trị thuộc tính.
2. Quét ảnh từ trái sang phải, từ gần đến xa để tìm bỏ sót người, xe đỗ và đối tượng bị che/cắt mép. Kiểm tra riêng các ca ở mục 4.
3. Mỗi đối tượng chỉ có một box; người/xe tách đúng; box trong ảnh, không có diện tích bằng 0 và không mở theo bóng, tải rời hoặc bộ phận suy đoán.
4. Kiểm tra đủ ba thuộc tính; mọi box `can_review = true` đều có ghi chú; vùng ignore không che đối tượng rõ ràng.
5. Sau khi nhóm đã cập nhật taxonomy và có file nhãn, chạy `python scripts/validate_labels.py --all`. Script kiểm cấu trúc và giá trị, không thay việc nhìn ảnh để xác nhận lớp/độ sát box.

### 5.2. Ngưỡng QC

Giữ nguyên các ngưỡng trong [`submission/taxonomy.json`](../../taxonomy.json), đối chiếu [`QC_CHEATSHEET.md`](../../../docs/QC_CHEATSHEET.md). Không hạ ngưỡng để làm đẹp kết quả.

| Hạng mục | Ngưỡng / yêu cầu |
|---|---|
| Chiều cao box đối tượng | `h >= 20 px`; dưới ngưỡng dùng ignore theo mục 3.4. |
| IoU từng cặp box lớn | `IoU >= 0.85` khi chiều cao trung bình hai box `h_pair >= 50 px`. |
| IoU từng cặp box nhỏ | `IoU >= 0.70` khi `20 <= h_pair < 50 px`. |
| Ghép box giữa người gán | Ngưỡng ghép `IoU >= 0.50`; đây không phải ngưỡng chất lượng đạt. |
| IAA | IoU trung bình trên cặp ghép được `>= 0.85` và tỉ lệ cùng lớp `>= 95%`; báo cả số cặp ghép và số box không ghép được. |
| Tỉ lệ ảnh lỗi để Release | Số ảnh có ít nhất một lỗi / số ảnh được kiểm `<= 2%`, đồng thời không còn lỗi lặp có hệ thống, IAA đạt và không còn ca guideline/dữ liệu chưa xử lý. |
| Mẫu QC trong lab | Mọi ảnh bị cờ cộng 20 ảnh ngẫu nhiên theo script, loại trùng; ghi số ảnh thực kiểm, không khẳng định đã kiểm đủ 40 nếu chưa làm. |

Theo script của repo, `h_pair = (h_A + h_B) / 2`, với `h_A`, `h_B` là chiều cao hai box đang so sánh; ngưỡng tối thiểu 20 px để gán nhãn vẫn xét từng box riêng. Khi báo cáo, luôn ghi mẫu số, ví dụ `1/20 ảnh = 5%`; nếu kiểm đủ 40 ảnh thì một ảnh lỗi đã là `1/40 = 2.5%`, vượt mức Release. Báo cả tỉ lệ lỗi theo đối tượng và theo ảnh; vùng ignore không phải đối tượng mục tiêu để đếm như xe/người.

Năm loại lỗi: `bo_sot`, `thua`, `sai_lop`, `sai_vi_tri_kich_thuoc`, `sai_thuoc_tinh`; ảnh đã kiểm sạch ghi `khong`. Bản guideline này chưa có kết quả IAA/QC thực tế, không tự ghi số đo hoặc quyết định đã Release.

### 5.3. Xử lý bất đồng và quản lý phiên bản

- **Hold:** còn khoảng trống guideline, ca thật–giả chưa chốt hoặc dữ liệu có vấn đề. Trưởng nhóm/người được nhóm phân công chốt sau khi xem ảnh và ghi lý do.
- **Rework:** tỉ lệ ảnh lỗi vượt 2%, IAA/IoU không đạt hoặc lỗi lặp cùng một kiểu; sửa nhãn và kiểm lại.
- **Release:** chỉ khi toàn bộ điều kiện đạt, các ca review đã xử lý và báo cáo có bằng chứng thực tế.
- Hai người cùng hiểu sai một loại ca: làm rõ quy tắc và bổ sung ví dụ. Một người áp dụng sai quy tắc đã rõ: hướng dẫn lại và sửa nhãn tương ứng.
- Mỗi quyết định calibration ghi một dòng với ảnh, quyết định, quy tắc bổ sung, phiên bản và người chốt. Quy tắc mới làm đổi nhãn phải tăng phiên bản và rà các ảnh chịu ảnh hưởng.
- Sau Pha 2, dùng bản nhóm `guideline_v1.0_agreed.md` và cố định đến hết Pha 3. Mọi ca phát sinh được đánh dấu review, không tự đổi luật giữa chừng.
- Giữ nguyên bản cá nhân này sau cuộc họp; quy tắc thống nhất và hiệu chỉnh nằm trong các file guideline của nhóm, không sửa ngược để làm mất dấu bất đồng ban đầu.

| Phiên bản | Thay đổi | Lý do / ca tham chiếu |
|---|---|---|
| `v0` | Mẫu khởi đầu có 6 lớp, thuộc tính và quy tắc box còn ngắn, chưa có ca khó. | Nguồn: `guideline/guideline_v0_starter.md`. |
| `v1.0-ca-nhan` | Đề xuất thêm `bus`, `bicycle`, `cyclo`, `tuk_tuk`; làm rõ pickup/van, thật–giả, người–xe, xe chở xe, phần nhìn thấy, ngưỡng che và ngưỡng 20 px; bổ sung bảng tình huống 40 ảnh và danh sách chưa chốt. | Minibus `img_009`; xe đạp `img_006`, `img_029`; xích lô `img_023`; tuk-tuk `img_004`, `img_035`; pickup `img_037`; xe chở xe `img_002`; tượng/mô hình/phản chiếu `img_017`, `img_020`, `img_024`, `img_027`, `img_033`, `img_038`; ca cần họp `img_011`, `img_019`. |
