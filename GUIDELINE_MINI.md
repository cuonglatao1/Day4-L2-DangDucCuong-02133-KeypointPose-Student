# Mini guideline - nhóm: SOLO | người gán: dang-duc-cuong | ngày: 16/09/2026

> Điền file này trong lúc gán nhãn. Các quy tắc dưới đây được rút ra từ
> những trường hợp thực tế gặp trong quá trình gán 20 ảnh.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không đặt chấm**.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Nếu vị trí hông bị quần áo che nhưng vẫn xác định được vị trí giải phẫu thì đặt điểm tại vị trí ước lượng và dùng `v=1`. Nếu hông nhìn thấy rõ thì `v=2`. | Quần áo che bề mặt khớp không có nghĩa là khớp nằm ngoài ảnh; vẫn có thể ước lượng vị trí từ tư thế cơ thể. Ảnh mẫu: `train_06`. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu tai vẫn nằm trong ảnh nhưng bị tóc/mũ che, vẫn đặt điểm tại vị trí tai ước lượng và dùng `v=1`. Nếu không thể xác định vị trí tai thì xem xét `v=0` chỉ khi tai thực sự nằm ngoài khung ảnh. | Các ảnh người đi xe máy/xe đạp cho thấy tai thường bị mũ bảo hiểm che, nhưng vị trí vẫn có thể suy ra từ vùng đầu. Ảnh mẫu: `train_04`. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp nằm ngoài khung ảnh dùng `v=0` và không đặt tọa độ. Các khớp vẫn nằm trong ảnh vẫn phải gán bình thường. | Không được đặt điểm ra ngoài ảnh. Ảnh mẫu: `train_13`. |
| Cổ tay nằm sau tay lái / sau thân mình | Nếu cổ tay vẫn nằm trong ảnh nhưng bị tay lái hoặc thân người che thì vẫn đặt tại vị trí ước lượng và dùng `v=1`. | Cổ tay có thể bị vật thể phía trước che nhưng vị trí giải phẫu vẫn có thể suy ra từ cẳng tay và tư thế. Ảnh mẫu: `train_02`. |
| Hai người chồng lên nhau | Mỗi người vẫn tạo một skeleton riêng. Điểm thuộc người nào phải gán cho đúng người đó; nếu một khớp bị người khác che nhưng vẫn nằm trong ảnh thì dùng `v=1`. | Không gộp hai người thành một skeleton. Ảnh mẫu: `train_03`. |
| Người nhỏ đến mức nào thì không gán nữa | Không bỏ người chỉ vì người đó nhỏ. Nếu người đó vẫn đủ nhận diện là người và có thể xác định tư thế, vẫn tạo skeleton và gán đủ 17 điểm; các điểm không quan sát được xử lý bằng `v=1` hoặc `v=0` theo guideline. | Ảnh `train_13` có người ở xa/nhỏ nhưng vẫn là đối tượng người cần xem xét khi gán nhãn. |

### Ảnh mẫu cho từng luật

**1. Hông bị quần áo che — `train_06`**

![train_06 - hông bị quần áo che](images/train_06.png)

**2. Tai bị mũ bảo hiểm che — `train_04`**

![train_04 - tai bị mũ bảo hiểm che](images/train_04.png)

**3. Người bị cắt ở mép ảnh — `train_13`**

![train_13 - người ở mép ảnh](images/train_13.png)

**4. Cổ tay bị che bởi tay lái/thân người — `train_02`**

![train_02 - cổ tay bị che](images/train_02.png)

**5. Hai người chồng lên nhau — `train_03`**

![train_03 - hai người chồng lên nhau](images/train_03.png)

**6. Người nhỏ trong ảnh — `train_13`**

![train_13 - người nhỏ/ở xa](images/train_13.png)

---

## 3. Ba ca mơ hồ đã gặp (bắt buộc)

### Ca 1 - ảnh `train_04`, người thứ 1, khớp `left_eye/right_eye`

- **Mơ hồ ở chỗ nào:** Người đội mũ bảo hiểm, vùng mặt bị che nhiều nên vị trí hai mắt khó quan sát trực tiếp.
- **Bạn quyết thế nào:** Vẫn đặt hai keypoint mắt tại vị trí ước lượng và đánh dấu `v=1` nếu mắt nằm trong ảnh nhưng bị che.
- **Vì sao:** Mắt không nằm ngoài khung ảnh; vị trí có thể ước lượng từ vùng mặt và cấu trúc đầu.
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Model có thể học rằng các mắt bị che phải bị bỏ qua/đặt `v=0`, làm giảm khả năng học keypoint khi người đội mũ hoặc bị che mặt.

![Ca 1 - train_04](images/train_04.png)

### Ca 2 - ảnh `train_02`, người thứ 1, khớp `right_wrist`

- **Mơ hồ ở chỗ nào:** Cổ tay bị các bộ phận của xe đạp và tư thế người che khuất một phần nên không nhìn thấy rõ tâm khớp.
- **Bạn quyết thế nào:** Đặt điểm tại vị trí cổ tay ước lượng và dùng `v=1`.
- **Vì sao:** Cổ tay vẫn nằm trong khung ảnh và có thể suy ra vị trí dựa trên cẳng tay/bàn tay và tư thế đang điều khiển xe.
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Model có thể học sai vị trí cổ tay trong các tư thế điều khiển xe, đặc biệt khi cổ tay bị tay lái che.

![Ca 2 - train_02](images/train_02.png)

### Ca 3 - ảnh `train_13`, người thứ 1, khớp `left_ankle/right_ankle`

- **Mơ hồ ở chỗ nào:** Người ở xa và một số phần cơ thể bị vật thể/người khác che, khiến việc xác định chính xác khớp chân khó khăn.
- **Bạn quyết thế nào:** Nếu khớp còn trong ảnh nhưng bị che thì đặt vị trí ước lượng và dùng `v=1`; nếu khớp đã nằm ngoài mép ảnh thì dùng `v=0` và không đặt điểm.
- **Vì sao:** Phân biệt rõ hai trường hợp "không nhìn thấy vì bị che" và "không tồn tại trong vùng ảnh".
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Model có thể học lẫn giữa keypoint bị occluded và keypoint nằm ngoài ảnh, gây nhiễu khi huấn luyện pose estimation.

![Ca 3 - train_13](images/train_13.png)

---

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `______` (bạn `___%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:
- Luật mới bổ sung vào mục 2 sau khi thống nhất:
