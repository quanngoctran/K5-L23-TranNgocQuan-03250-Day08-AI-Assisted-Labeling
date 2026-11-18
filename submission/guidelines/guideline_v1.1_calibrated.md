# Label contract — BỔ SUNG v1.1 — 2A202603250 - Trần Ngọc Quân

> Chỉ phần thay đổi so với v1.0. Mỗi ca khớp một dòng trong `qc/calibration_decisions.csv`.

## 1. Câu hỏi mở đã chốt (từ mục 6 của v1.0)

| # | Câu hỏi | Quyết định | Ảnh ví dụ | `case_id` |
|---|---|---|---|---|
| 4 | Người ngồi trong xe ba bánh có mui, chỉ thấy một phần: ngưỡng "còn thấy ≥ 20%" có giữ không? | Giữ 20%. Còn thấy đầu hoặc thân thì gán một `person` riêng. Mask của xe không gồm người đó. Một click âm không đủ thì thêm click âm đến khi người ra khỏi mask. | img_013 | CAL-02 |

Các câu 1, 2, 3, 5, 6, 7 của mục 6 v1.0 chưa chốt: trong lô này không có ca xe đạp điện rõ, chưa đủ ảnh để đặt ngưỡng cụm xe máy gần camera, hàng nhô, kính xe, box đêm, hay mức `conf` được accept không cần nhìn.

## 2. Ca khó mới (từ lỗi của máy, bẫy lọt, box `can_review`)

| Ảnh | Tình huống | Máy làm gì (model nào) | Quyết định của bạn | Quy tắc mới (vào mục nào của v1.0) |
|---|---|---|---|---|
| img_002, img_013, img_004 | Tuk-tuk bị bỏ hoặc bị gọi là ô tô | YOLO26 (`r02`, `r05`): img_002 không có box xe; img_013 `car` 0.42 bao cả xe và người lái; img_004 `car` 0.37 | Sửa thành `three_wheeler`. Nếu YOLOE đã có box chồng thì xóa box `car`/`truck` của YOLO26 | mục 2: tuk-tuk, xích lô, ba gác là `three_wheeler` dù model COCO gọi `car` hoặc `truck` |
| img_023, img_009 | Bẫy lọt khi duyệt | Không phải lỗi định nghĩa lớp. `loose` img_023 và `missing` img_009 không bị sửa | Không đổi lớp. Ẩn nhãn, đếm vật trước; box giữ phải ôm sát (lệch thì kéo) | mục 4.1.1, bước duyệt, không thêm lớp |

## 3. Lịch sử thay đổi

| Phiên bản | Thay đổi | Lý do (ca nào, `case_id`) |
|---|---|---|
| v1.0 | Bản giảng viên | — |
| v1.1 | Chốt người trong khoang xe ba bánh vẫn là `person` nếu còn thấy ≥ 20%, và mask xe không gồm người. Nhắc tuk-tuk không được để nhãn `car`/`truck`. Thêm bước duyệt cho box lỏng và vật sót. | CAL-02, CAL-01, CAL-03 |
