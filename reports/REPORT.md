# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Văn Tiến   Nhóm: SOLO   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 339 / 123 / 31 |
| Thời gian trung bình mỗi ảnh | TBD |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear — 59%
2. right_ear — 41%
3. left_eye — 34% (đồng hạng với right_eye — 34%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.822 | 0.939 |
| OKS@0.50 | 0.931 | 1.000 |
| OKS@0.75 | 0.862 | 1.000 |
| Lỗi `dao_trai_phai` | 3 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 2 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- `train_06.jpg` — người #1 — sửa lỗi đảo trái/phải và sửa `left_wrist` bị trượt hẳn.
- `train_19.jpg` — người #2 — sửa lỗi đảo trái/phải và sửa `right_elbow`, `left_wrist`, `right_wrist` bị trượt hẳn.
- `train_13.jpg` — người #1 — sửa lỗi đảo trái/phải.
- `train_01.jpg` — người #1 — sửa `left_wrist` bị xóa khi bị che và bổ sung `left_hip`.
- `train_02.jpg` — người #1 — sửa `right_ankle` bị xóa khi bị che.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Lỗi đảo trái/phải của xảy ra ở `train_06.jpg`, `train_19.jpg` và `train_13.jpg`. Nguyên nhân là   lúc xác định trái/phải chưa nhất quán theo cơ thể người. Sau khi kiểm tra lại bằng visualization và Gold,sửa các trường hợp này.

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
| pose_mAP50 | TBD | TBD | TBD |
| pose_mAP50-95 | TBD | TBD | TBD |
| pose_precision | TBD | TBD | TBD |
| pose_recall | TBD | TBD | TBD |
| box_mAP50-95 | TBD | TBD | TBD |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   `pose_mAP50-95` tăng từ 0.6853 ở model gốc lên 0.6908 sau fine-tune,
   tăng 0.0055 (0.55 điểm phần trăm).
   Metric không giảm nên phần giả định "20 ảnh dạy model điều gì mà
   COCO chưa dạy và nó làm hỏng điều gì" không áp dụng trực tiếp. 
2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

   Sau fine-tune, `box_mAP50-95` = 0.8041 và `pose_mAP50-95` = 0.6908,
   chênh lệch 0.1133.
   Như vậy, trên tập test của bài này, metric của bounding box cao hơn
   metric của pose. Điều này cho thấy model xác định vùng người có kết quả
   metric cao hơn việc định vị chính xác các keypoint.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   
   Ảnh: `test_02.jpg`
   Loại lỗi: `nhầm người`
   Đối tượng: detection `person 0.31` ở phía bên trái ảnh.
   Quan sát visualization: Model phát hiện đúng một người ở phía bên phải
   với confidence 0.90, nhưng đồng thời tạo thêm một detection `person 0.31`
   ở phía bên trái, nơi không có một người hoàn chỉnh tương ứng. Đây là
   một false positive, cho thấy model bị nhầm một vùng/đối tượng trong ảnh
   thành người.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   Ảnh: `train_13.jpg`

   OKS giữa model và nhãn: `0.495`.
   Model và nhãn của tôi có sự khác biệt rõ ở một số keypoint, đối chiếu
   prediction của model với annotation của mình và Gold trên cùng ảnh để
   xác định vị trí keypoint nào đúng. Kết luận được dựa trên bằng chứng
   trực quan của ảnh và Gold, không chỉ dựa vào giá trị OKS.
5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
   
## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
