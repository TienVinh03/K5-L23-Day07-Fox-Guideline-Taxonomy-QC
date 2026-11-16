# Guideline gán nhãn — Vũ Hoàng Nam - 2A202603268

- Phiên bản: v0
- Ngày: 16/11/2026
- Loại nhãn: bounding box (hộp chữ nhật) trên ảnh đường phố
- Công cụ: CVAT, xuất COCO 1.0

---

## 1. Mục tiêu

Gán nhãn các **người và phương tiện** xuất hiện trong ảnh đường phố để huấn luyện
mô hình phát hiện đối tượng cho camera giao thông hoặc xe tự lái.

Chỉ gán những đối tượng trong các lớp được define. Không gán vật thể xuất hiện
trong ảnh quảng cáo, tranh, màn hình, gương chiếu hậu, mô hình, đồ chơi hoặc tượng.

## 2. Lớp và thuộc tính

### Lớp

Mỗi đối tượng được gán đúng một lớp. Nếu một phương tiện không phù hợp với các lớp
hiện có, không tự đoán sang lớp gần nhất; xem quy tắc `ignore_region` ở dưới.

| Lớp | Định nghĩa | Ví dụ | Phản ví dụ / dễ nhầm |
|---|---|---|---|
| `car` | Ô tô chở người, có thân kín hoặc kiểu SUV/taxi, không có thùng hàng mở. | Sedan, hatchback, SUV, taxi; các ô tô đang chạy hoặc đỗ. | Xe tải, xe bán tải có thùng chở hàng, xe buýt, ô tô trong ảnh quảng cáo hoặc đồ chơi. |
| `truck` | Xe có thùng hoặc sàn chở hàng, gồm xe tải, xe bán tải và xe chuyên chở hàng. | Xe tải thùng, xe tải chở ô tô, xe bán tải chở hàng. | Van thân kín không có thùng hàng mở; xe buýt; xe đồ chơi hoặc mô hình. |
| `van` | Xe thân kín cỡ nhỏ hoặc xe minibus dùng để chở người/hàng, không phải xe buýt lớn. | Van chở người, minibus nhỏ, xe giao hàng thân kín. | Xe buýt lớn; xe tải có thùng/sàn hàng rõ ràng. |
| `motorcycle` | Xe hai bánh có động cơ, kể cả xe máy điện và xe tay ga. | Xe máy, xe tay ga, mô tô; xe máy có người ngồi hoặc đỗ. | Xe đạp, xe ba bánh đạp, xích lô, tuk-tuk/auto-rickshaw và xe không có động cơ. |
| `person` | Người thật có thể nhìn thấy trong ảnh, dù đang đi bộ, ngồi trên xe hoặc bị che một phần. | Người đi bộ, người lái/ngồi sau xe máy, người đứng cạnh xe. | Ma-nơ-canh, tượng, hình người trên biển quảng cáo, hình in trên vật. |
| `ignore_region` | Vùng chứa đối tượng giao thông thật nhưng không có lớp phù hợp, hoặc vùng quá dày/không thể tách riêng để gán chính xác. | Cụm xe buýt, tuk-tuk hoặc xe đạp không có lớp; cụm phương tiện rất chồng lấn. | Không dùng để phủ nền, nhà cửa, cây cối, biển hiệu hoặc toàn bộ ảnh. |

### Thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
|---|---|---|
| `occluded_level` | `0` | Không bị che hoặc chỉ bị che không đáng kể; phần bị che ước tính dưới 10%. |
| `occluded_level` | `1` | Bị vật khác che một phần nhưng vẫn thấy ít nhất khoảng 70% đối tượng; phần bị che từ 10% đến dưới 30%. |
| `occluded_level` | `2` | Bị che nhiều nhưng vẫn xác định được lớp và vị trí; phần bị che từ 30% đến 70%. |
| `occluded_level` | `3` | Chỉ còn nhìn thấy dưới 30%, hoặc hình dạng/lớp rất khó xác định nhưng vẫn có đủ dấu hiệu để giữ lại nhãn. Luôn tick `can_review`. |
| `truncated` | `no` | Đối tượng không bị cắt bởi biên ảnh. Bị che bởi vật khác không tính là `truncated`. |
| `truncated` | `yes` | Đối tượng bị biên ảnh cắt mất một phần. Box dừng đúng tại biên ảnh, không đoán phần nằm ngoài ảnh. |
| `can_review` | tick / không | Tick khi đối tượng nhỏ, mờ, bị che nhiều, nằm trong cụm dày, hoặc có khả năng hai người gán khác nhau. |

Nếu không thể xác định đối tượng là người, ô tô, xe tải, van hoặc xe máy thì không gán
nhãn mục tiêu. Khi đó chỉ dùng `ignore_region` nếu đối tượng là phương tiện/người thật
không thuộc lớp hoặc tạo thành vùng gây nhầm lẫn; không dùng `ignore_region` cho mọi
vật thể nền.

## 3. Quy tắc vẽ box

1. Gán một box cho mỗi người và mỗi phương tiện thuộc lớp mục tiêu. Không gộp nhiều vật
   thành một box chỉ vì chúng ở gần nhau.
2. Box ôm sát đối tượng, bao gồm các phần thuộc đối tượng như bánh,
   gương, tay lái và hàng đang gắn liền với xe. Không được đoán và vẽ box cho các phần
   bị che.
3. Với đối tượng bị che, box vẫn bao quanh toàn bộ phạm vi có thể xác định của đối
   tượng; cạnh box đi qua vùng che theo hình dạng ước đoán hợp lý. Không vẽ box riêng cho
   phần đầu, tay, chân hoặc bánh xe.
4. Với đối tượng bị cắt ở biên ảnh, vẽ box đến đúng mép ảnh và đặt `truncated=yes`.
   Không suy diễn phần nằm ngoài ảnh.
5. Người và xe là hai đối tượng khác nhau. Người lái xe máy, người ngồi sau xe máy,
   người đứng cạnh xe hoặc người ngồi trong xe đều được gán `person` nếu nhìn thấy đủ để
   xác định. Xe được gán box riêng.
6. Xe chở xe hoặc chở hàng: gán box cho xe chở và cho từng xe thật/hàng thuộc lớp mục
   tiêu nhìn thấy rõ. Ví dụ xe tải chở nhiều ô tô thì box xe tải là `truck`, mỗi ô tô
   trên sàn là `car`.
7. Gán cả phương tiện đang chạy, đang đỗ, ở bãi xe, trong sân hoặc trong khu trưng bày
   nếu đó là phương tiện thật, đủ lớn và có hình dạng hoàn chỉnh. Không gán xe trong
   ảnh 2D, biển quảng cáo, tranh, mô hình, đồ chơi hoặc xe thu nhỏ.
8. Không gán vật chỉ thấy qua phản chiếu trong gương hoặc kính nếu không thấy vật thật
   tương ứng ở ngoài ảnh. Không gán tượng người, tượng xe, ma-nơ-canh hoặc hình in.
9. Xe đạp, xích lô, tuk-tuk/auto-rickshaw, xe buýt và các phương tiện khác chưa có lớp
   riêng thì không gán vào `motorcycle`, `car`, `truck` hay `van`. Nếu chúng tạo thành
   vùng đông, chồng lấn khiến việc kiểm tra khó khăn, dùng một `ignore_region` bao sát
   vùng đó và không che lên các box mục tiêu.
10. Đám đông: cố gắng tách từng người và từng phương tiện khi còn nhận diện được. Khi
    các vật chồng lên nhau đến mức không thể tách đáng tin cậy, gán các box nhìn rõ,
    tick `can_review` cho box biên và dùng `ignore_region` cho phần phương tiện không có
    lớp hoặc không thể tách.
11. Ảnh tối, ngược sáng, đen trắng hoặc bị nhòe vẫn được gán nếu còn đủ dấu hiệu nhận
    dạng. Nếu chỉ đoán dựa trên một mảng màu hoặc hình dạng quá nhỏ, không gán mục tiêu;
    nếu là ca mơ hồ thì tick `can_review` hoặc dùng `ignore_region` theo quy tắc trên.

## 4. Ca khó

| Ảnh | Tình huống | Quyết định | Lý do |
|---|---|---|---|
| `img_001` | Xe bán tải có người đứng phía trước và nhiều vật trên thùng. | Xe là `truck`, người là `person`; gán riêng từng đối tượng nhìn thấy. | Thùng chở hàng là dấu hiệu của `truck`; người không bị gộp vào xe. |
| `img_002` | Xe tải chở nhiều ô tô. | Gán `truck` cho xe chở và `car` cho từng ô tô thật nhìn rõ. | Mỗi phương tiện thật là một đối tượng độc lập. |
| `img_003`, `img_010` | Có xe đạp, xe máy và xe chở hàng/xe buýt nhỏ trong cảnh đông. | Chỉ gán xe máy, ô tô, van/truck nếu đúng lớp; không gán xe đạp. | Không suy diễn xe đạp hoặc phương tiện không có lớp thành `motorcycle`. |
| `img_004`, `img_016`, `img_031`, `img_035` | Tuk-tuk/xe ba bánh hoặc xe buýt xuất hiện cùng phương tiện mục tiêu. | Không gán các phương tiện này vào lớp gần nhất; dùng `ignore_region` khi cần đánh dấu vùng gây nhầm. | Taxonomy hiện tại chưa có lớp cho các loại xe này. |
| `img_005`, `img_006`, `img_021`, `img_030`, `img_035` | Nhiều người và xe máy chồng lấn, một số người ngồi trên xe. | Gán box riêng cho từng người và xe máy còn nhận diện được; đặt `occluded_level` theo phần bị che và tick `can_review` cho ca sát ngưỡng. | Người và phương tiện là hai đối tượng khác nhau, kể cả khi box chồng lên nhau. |
| `img_007`, `img_013`, `img_031` | Xe tải/xe van ở nơi tối hoặc trời mưa. | Vẫn gán nếu xác định được lớp; dùng box ôm phần nhìn thấy và ghi `can_review` nếu mờ/thiếu sáng. | Điều kiện ánh sáng không phải lý do tự động bỏ nhãn. |
| `img_011`, `img_014` | Người hoặc xe bị nhòe do chuyển động. | Gán khi còn xác định được đối tượng; nếu không chắc lớp hoặc ranh giới thì tick `can_review`. | Không gán theo phỏng đoán khi hình ảnh không đủ thông tin. |
| `img_017`, `img_027`, `img_033`, `img_038` | Tượng, mô hình hoặc xe đồ chơi có hình dạng giống người/xe. | Không gán `person` hoặc lớp xe. | Đây không phải đối tượng thật cần mô hình phát hiện trên đường. |
| `img_018`, `img_020`, `img_032` | Xe/người xuất hiện trong bãi xe, qua gương, hoặc ở xa trong cảnh đường phố. | Gán vật thật nhìn thấy trực tiếp; bỏ qua vật chỉ ở trong gương/ảnh; vật quá nhỏ thì tick `can_review` hoặc không gán nếu không nhận dạng được. | Tránh gán trùng vật phản chiếu và tránh nhãn dựa trên phỏng đoán. |
| `img_019`, `img_024`, `img_028` | Xe đặc biệt, xe cổ hoặc xe trưng bày; có thể có vật trang trí đi kèm. | Gán phương tiện thật thuộc lớp phù hợp; không gán tượng hoặc vật trang trí không phải người/xe. | Kiểu dáng lạ không làm thay đổi lớp nếu bản chất là phương tiện thật. |
| `img_023`, `img_029` | Xích lô, xe đạp hoặc xe chở vật dài đi qua đường. | Không gán các xe không có động cơ vào `motorcycle`; chỉ gán người và ô tô/xe máy thuộc lớp nếu nhìn thấy. | Phân biệt xe máy với xe đạp/xích lô dựa trên động cơ và cấu tạo. |
| `img_036`, `img_037`, `img_039` | Xe tải nhỏ, xe bán tải hoặc xe chở hàng có thân/thùng khác nhau. | Gán `truck` khi có thùng hoặc sàn chở hàng; gán `van` khi thân kín cỡ nhỏ; gán `car` cho ô tô chở người. | Phân loại theo công năng và cấu tạo chính, không theo màu hoặc kích thước trong ảnh. |

Các ca nằm giữa hai mức che khuất, hoặc không thể phân biệt chắc chắn `car`/`van`/`truck`,
phải tick `can_review` và ghi chú để nhóm thống nhất. Không tự tạo lớp mới trong lúc gán.

## 5. QC và phiên bản

Trước khi hoàn tất mỗi ảnh, kiểm tra:

- Mỗi người và phương tiện mục tiêu nhìn thấy rõ đã có đúng một box.
- Box ôm sát đối tượng, không lấy quá nhiều nền và không bỏ sót phần nhìn thấy.
- Nhãn lớp phù hợp với định nghĩa; xe đạp, xe buýt, tuk-tuk, tượng, mô hình và hình ảnh
  không bị gán nhầm vào lớp mục tiêu.
- `occluded_level`, `truncated` và `can_review` đã được đặt đúng.
- Box của người và phương tiện có thể chồng lên nhau khi chúng thực sự chồng trong ảnh;
  không gộp chúng thành một box.

Các box `ignore_region` phải thật sự cần thiết, bao sát vùng đối tượng không có lớp hoặc
vùng quá dày không thể tách, và không che lên box mục tiêu. Ca có `can_review` được đưa
vào danh sách để người khác kiểm tra.

Mọi thay đổi làm thay đổi lớp, thuộc tính hoặc cách vẽ box phải tăng phiên bản guideline
và ghi rõ ảnh/tình huống gây ra thay đổi. Bản này là v0; các ca chưa chốt được ghi nhận
để nhóm thảo luận khi thống nhất guideline.
