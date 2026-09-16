# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 13.93 khớp có v > 0 mỗi người
- Tổng: v=2 342 | v=1 62 | v=0 89

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 2 | 5 | 7% |
| 1 | left_eye | 18 | 5 | 6 | 17% |
| 2 | right_eye | 19 | 4 | 6 | 14% |
| 3 | left_ear | 14 | 9 | 6 | 31% |
| 4 | right_ear | 17 | 8 | 4 | 28% |
| 5 | left_shoulder | 27 | 1 | 1 | 3% |
| 6 | right_shoulder | 27 | 1 | 1 | 3% |
| 7 | left_elbow | 23 | 3 | 3 | 10% |
| 8 | right_elbow | 23 | 3 | 3 | 10% |
| 9 | left_wrist | 21 | 3 | 5 | 10% |
| 10 | right_wrist | 19 | 6 | 4 | 21% |
| 11 | left_hip | 21 | 5 | 3 | 17% |
| 12 | right_hip | 23 | 3 | 3 | 10% |
| 13 | left_knee | 19 | 1 | 9 | 3% |
| 14 | right_knee | 21 | 1 | 7 | 3% |
| 15 | left_ankle | 15 | 2 | 12 | 7% |
| 16 | right_ankle | 13 | 5 | 11 | 17% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
