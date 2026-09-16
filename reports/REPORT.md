# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Dang Duc Cuong   Nhóm: SOLO   Ngày: 09/16/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 342 / 62 / 89 |
| Thời gian trung bình mỗi ảnh: 4p | [điền tổng thời gian gán 80p / 20] |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear — 31%
2. right_ear — 28%
3. right_wrist — 21%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Ba khớp có `%v=1` cao nhất chủ yếu là những khớp hay bị che, nhưng không nhất thiết hoàn toàn trùng với những khớp khó xác định vị trí giải phẫu nhất. Tai trái và tai phải có tỷ lệ bị che cao nhất, trong khi cổ tay phải cũng thường bị che bởi vật thể hoặc tư thế của người. Việc khó xác định vị trí còn phụ thuộc vào mức độ nhìn thấy hình dạng và vị trí tương đối của khớp, nên cần dựa trên ảnh cụ thể thay vì chỉ dựa vào tỷ lệ `v=1`.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.893 | Chưa chạy lại |
| OKS@0.50 | 0.966 | Chưa chạy lại |
| OKS@0.75 | 0.931 | Chưa chạy lại |
| Lỗi `dao_trai_phai` | 1 | Chưa chạy lại |
| Lỗi `nham_nguoi` | 0 | Chưa chạy lại |
| Lỗi `xoa_khop_bi_che` | 9 | Chưa chạy lại |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- Chưa rework; lần chạy hiện tại là kết quả trước rework.
- `train_13.jpg` – người thứ 1 – sửa lỗi đảo trái/phải và bổ sung `right_ear`, `left_shoulder`, `right_shoulder`, `right_elbow`, `right_wrist` theo gold.
- `train_10.jpg` – người thứ 1 – sửa `nose` bị trượt hẳn và gán lại `left_hip`, `right_hip` với `v=1`.
- `train_06.jpg` – người thứ 1 – bổ sung `left_ear`, `right_ear`.
- `train_11.jpg` – người thứ 1 – gán lại `left_hip`, `right_hip` với `v=1`.
- `train_16.jpg` – người thứ 2 – gán lại `nose` với `v=1` và bổ sung `right_eye`.
- `train_12.jpg` – người thứ 1 – gán lại `left_knee`, `left_ankle` với `v=1`.
- `train_08.jpg` – người thứ 1 – gán lại `left_knee`, `left_ankle` với `v=1`.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?**  
Lỗi xảy ra ở `train_13.jpg`, người thứ 1. Đây là ảnh có nhiều người và tư thế/quan hệ giữa các cơ thể khiến việc xác định trái/phải dễ nhầm. Lỗi xảy ra khi gán nhãn, cho thấy tôi cần kiểm tra trái/phải theo **cơ thể người** thay vì theo vị trí trên ảnh, kể cả khi vị trí keypoint nhìn khá rõ.
## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

-

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.845 | 0.845 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |
### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu?

`pose_mAP50-95` tăng từ **0.6853** ở model `yolo26n-pose` gốc lên **0.6908** sau fine-tune.

Chênh lệch:

**0.6908 - 0.6853 = +0.0055**

Như vậy, sau fine-tune trên 20 ảnh, `pose_mAP50-95` tăng nhẹ **0.0055**. Điều này cho thấy dữ liệu 20 ảnh của tôi có đóng góp nhất định cho việc nhận dạng pose trên tập test, nhưng mức cải thiện nhỏ.

---
2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu?

Ở kết quả sau fine-tune:

- `box_mAP50-95` = **0.8041**
- `pose_mAP50-95` = **0.6908**

Chênh lệch:

**0.8041 - 0.6908 = 0.1133**

Như vậy, `box_mAP50-95` cao hơn `pose_mAP50-95` **0.1133**.

Điều này cho thấy trong tập test này, model xác định vị trí **người** bằng bounding box dễ hơn việc xác định chính xác từng **khớp cơ thể**. Bounding box chỉ cần xác định vùng chứa người, trong khi pose yêu cầu vị trí của nhiều keypoint phải đủ chính xác.

---

3. Một ảnh test model đoán sai

Cần đối chiếu ảnh test với kết quả dự đoán của model để xác định chính xác loại lỗi theo bốn loại:

- lệch nhẹ
- đảo trái/phải
- nhầm người
- trượt hẳn

**Chưa có đủ dữ liệu trong `eval_model.json` hiện tại để xác định chính xác ảnh và loại lỗi của model, nên không tự suy đoán.**

---
4. Ảnh nào có OKS thấp nhất giữa nhãn của tôi và model?

Kết quả hiện có chưa cung cấp bảng OKS theo từng ảnh giữa **nhãn của tôi và model**.

File `eval_vs_gold.json` là kết quả so sánh **nhãn của tôi với gold**, không phải kết quả so sánh **model với gold**.

Vì vậy cần kết quả per-image OKS của model để xác định ảnh có OKS thấp nhất và đối chiếu xem nhãn nào đúng hơn.

---

5. Ảnh tôi gán tệ nhất có cũng là ảnh model đoán tệ nhất không?

Kết quả `eval_vs_gold.json` cho thấy ảnh tôi có OKS thấp nhất là:

**`train_13.jpg`, người #1, OKS = 0.409.**

Tuy nhiên, chưa có bảng OKS theo từng ảnh của model nên chưa thể kết luận `train_13.jpg` có phải cũng là ảnh model đoán tệ nhất hay không.

Nếu sau khi đối chiếu kết quả model cho thấy cùng một ảnh có OKS thấp, điều đó sẽ cho thấy bản thân ảnh có những đặc điểm gây khó cho việc gán pose, chẳng hạn tư thế, che khuất hoặc các keypoint khó quan sát.

---

## 5. Một rule evidence tôi đã dùng

**Ảnh: `train_13.jpg` – người #1 – keypoint vùng vai phải (`right_shoulder`).**

Trong ảnh, phần thân trên của người được nhìn thấy khá rõ, đồng thời đường viền vai và phần tay áo giúp xác định vị trí tương đối của khớp vai. Mặc dù một số phần cơ thể bị che hoặc chồng lên nhau, vị trí của khớp vẫn có thể được ước lượng từ phần cơ thể liền kề. Vì vậy, khi khớp vẫn còn trong vùng ảnh và có đủ bằng chứng thị giác để ước lượng vị trí, tôi giữ keypoint thay vì coi nó là đã ra khỏi khung.