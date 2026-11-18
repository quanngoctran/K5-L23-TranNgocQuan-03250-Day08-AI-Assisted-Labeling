# Báo cáo Day 08 — 2A202603250 - Trần Ngọc Quân

> Một file duy nhất cho cả bài (đã gộp recipe, QC và ghi chú cá nhân). Mọi con số kèm **mẫu số** và nguồn (file nào). Viết bằng lời của bạn.

## 1. Model và recipe (Part 2–3)

| Lớp | Model + cỡ | Ngưỡng | Vì sao (dẫn `run_id` trong `model_log.csv`) |
|---|---|---|---|
| person / bicycle / motorbike / car / bus / truck | YOLO26m | 0.3 (mặc định) | `threshold_sweep.md`: không ngưỡng nào ≤ 0.5 đạt recall 90% (person tối đa 72%/32 vật, bicycle 50%/6, motorbike 85%/13, car 88%/8). bus 1 vật, truck 0 vật trong gold nên không chọn riêng. `r02`/`r03`: YOLO26 bắt person và motorbike; conf 0.5 làm img_007 mất người và xe máy nhỏ. |
| three_wheeler | YOLOE-26s, prompt: tuk-tuk, cyclo, cargo tricycle | 0.15 | `r04`: YOLO26 không có lớp này (img_002 tuk-tuk xanh không có box xe). YOLOE bắt tuk-tuk img_002 ở 0.93 và xích lô img_032 ở 0.92. Sweep: recall 100%, precision 78% trên 7 vật gold. |

- Lệnh `merge_prelabels.py` đã dùng: `python scripts/merge_prelabels.py work/prelabel/yoloe_all/pre.coco.json work/prelabel/y26_all/pre.coco.json --thr "three_wheeler=0.15" --conf 0.3 --iou 0.6 --split "batch,gold" --out work/prelabel/merged_review/pre.coco.json`. Ra 606 box trên batch+gold: person 311, motorbike 154, car 65, three_wheeler 52, bicycle 13, truck 7, bus 4.
- **Phiên bản SAM bạn chọn** (SAM 1 / 2.1 / 3) và lý do: SAM 2.1, trọng số `sam2.1_b.pt` (`r05`). Máy Apple Silicon chạy được trên MPS (16 mask / 2 ảnh, 2.91 giây/ảnh). Không cần token Hugging Face như SAM 3. Mask bám người đứng ở img_004 (person 0.93).
- Lỗi nào **không sửa được bằng đổi ngưỡng** (ví dụ tuk-tuk bị gọi `car`)? Sửa thế nào? YOLO26 không có lớp `three_wheeler`. img_002 tuk-tuk xanh không được box xe ở cả conf 0.25 và 0.5 (`r02`, `r03`). img_013 và img_004 tuk-tuk bị gọi `car`, SAM vẫn tô mask theo box sai (`r05`). Sửa bằng YOLOE cho lớp này, rồi người review đổi nhãn; tăng conf chỉ bỏ box điểm thấp, không đổi tên lớp.
- Box **điểm cao mà sai** hoặc **điểm thấp mà đúng** (ảnh nào): Điểm cao mà sai: img_013 `car` 0.42 bao cả tuk-tuk và người lái (`r05`); img_002 bán tải trắng bị YOLO26 gọi `truck` 0.73 (`r02`). Điểm thấp mà đúng hướng: img_032 xích lô `three_wheeler` 0.27 vẫn là xích lô thật nhưng box rất lỏng (`r04`). img_002 xe máy bị YOLOE gọi `three_wheeler` 0.40 là điểm thấp mà sai.
- Giới hạn: chọn ngưỡng trên chính gold (4 ảnh) nên số đo máy trên gold là lạc quan: ngưỡng và precision/recall được chọn rồi chấm trên cùng 4 ảnh gold. truck có 0 vật, bus có 1 vật, bicycle có 6 vật, nên ngưỡng 0.3 của các lớp đó chỉ là mặc định, không phải ngưỡng đã đo chắc.

## 2. AI error profile (từ `qc/review_stats.md`)

| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---|---|---|
| accepted | 435 | 83.8% | 519 box máy |
| edited | 20 | 3.9% | 519 box máy |
| deleted | 58 | 11.2% | 519 box máy |
| added | 59 | 11.5% | 514 đối tượng sau duyệt |

Nguồn: `submission/qc/review_stats.md`. Script đã loại 23 đối tượng quanh bẫy khỏi bảng này. Trong deleted + added có 8 lần đổi lớp (box đúng chỗ, sai tên). Sót thật ≈ 59 − 8 = 51.

- Lớp nào máy tệ nhất? Vì sao (dẫn ảnh)? `three_wheeler`: 7 accepted, 8 edited, 24 deleted, 4 added. Trên 39 box máy của lớp này, giữ nguyên chỉ 7/39. Ngưỡng 0.15 để đủ recall trên gold nên để lại nhiều box thừa (đã thấy từ Part 2: img_002 gọi nhầm xe máy 0.40 và bán tải 0.37). `motorbike` sót nhiều nhất theo số vẽ thêm: added 20.
- So với gold: precision 69.6% (48/69), recall 71.6% (48/67). Mẫu số recall là 67 đối tượng gold. Khoảng tin cậy 95%: precision 58–79%, recall 60–81%. Số này lạc quan vì ngưỡng chọn trên chính 4 ảnh gold.

## 3. Thiên kiến tự động hóa và ảnh bẫy

- Bắt bẫy: 10/12 = 83.3% (KTC 95%: 55–95%). Đạt ngưỡng 80%. Lọt: `loose` trên img_023 (2/3) và `missing` trên img_009 (2/3). Bắt hết `extra` 3/3 và `wrong_class` 3/3. Hai bẫy lọt đúng hai bước dễ bỏ: không quét ảnh trước khi nhìn box (sót vật), và không kéo box đã gần đúng cho ôm sát (box lỏng).
- Accepted 83.8% và bắt bẫy 83.3% cùng cao. Không phải kiểu bấm nhận mà không nhìn: extra và sai lớp đều bị bắt. Điểm yếu là vật máy không vẽ và box chỉ lệch nhẹ. Khoảng tin cậy 55–95% rất rộng vì chỉ 12 bẫy, nên 10/12 chưa chứng minh lần sau vẫn trên 80%.

## 4. Máy có đáng dùng không?

- Thời gian (từ `time_log.csv`): gán tay 20 phút / 4 ảnh = 5.0 phút/ảnh; duyệt 52 phút / 24 ảnh = 2.2 phút/ảnh. Duyệt mất khoảng 44% thời gian gán tay, nhưng gold làm trước nên batch đã quen cảnh phố; một phần thời gian giảm là nhờ quen tay, không chỉ nhờ pre-label.
- Kết luận: đáng dùng cho `person` và `motorbike` (accepted 250/270 và 122/138 box máy của lớp đó), với người vẫn xóa và vẽ thêm. Không đáng tin một mình cho `three_wheeler` (xóa 24/39 box máy). Precision/recall trên gold khoảng 70%, dưới mức giao nhãn mà không duyệt.
- **Ba quy tắc giảm thiên kiến của bạn** (mỗi quy tắc kiểm tra được bằng số hoặc việc làm):
  1. Mỗi ảnh ẩn nhãn máy và đếm vật trước, rồi mới bật box. Kiểm bằng bẫy `missing`: lần này 2/3, lọt img_009.
  2. Box định giữ phải chạm mép vật, không để lệch rồi bấm nhận. Kiểm bằng bẫy `loose`: lần này 2/3, lọt img_023.
  3. Box `three_wheeler` dưới 0.4 không được accepted nếu chưa nhìn ảnh. Kiểm bằng số deleted của lớp này: lần này 24/39 box máy.

## 5. Mask SAM cho 3 vật (Part 5)

| Ảnh | SAM (phiên bản) | Số click | Mask gồm người lái / hàng hoá? | Lỗi quan sát | File trong `masks/` |
|---|---|---|---|---|---|
| img_013 | SAM 2.1 `sam2.1_b.pt` | 2 dương (mui, logo đuôi) + 1 âm trên người lái | Có. Cả người lái và người ngồi bàn nhìn xuyên khoang | 317255 px = 45.4% ảnh, 1 polygon. Click âm không loại được người lái. Mask nuốt người và ghế phía sau khoang hở | `tuktuk_img013.jpg` |
| img_032 | SAM 2.1 `sam2.1_b.pt` | 2 dương (thân và gầm xích lô) + 1 âm trên người đứng cạnh | Không gồm người đứng. Ghế hoa gắn trên xe thì có | 49286 px = 8.4% ảnh, 1 polygon. Click âm loại được người đứng. Bánh xe bị cắt, mask chưa ôm hết xe. Không lan sang xích lô bên cạnh | `cyclo_img032.jpg` |
| img_004 | SAM 2.1 `sam2.1_b.pt` | 2 dương trên tuk-tuk xanh + 1 âm lên người đứng giữa hai xe | Khoang và kính bị tô kín. Người đứng bị loại. Người áo đỏ trong xe có thể nằm trong mask | 41932 px = 6.0% ảnh, 1 polygon. Bám tuk-tuk xanh, không nuốt tuk-tuk xanh dương phía trước. Ca này xe khá tách, chưa phải vật bị che nặng | `tuktuk_img004.jpg` |

- Đối chiếu guideline mục 4.2: người là vật riêng (mục 4.1), nên mask xe không gồm người lái. img_032 một click âm loại được người đứng cạnh xe; img_013 một click âm trên người lái vẫn không loại được, và khoang hở (lỗ lớn hơn ~5% diện tích, mục 4.2.2) vẫn bị tô. Ghế hoa trên xích lô gắn liền xe nên nằm trong mask là hợp lý. Quy tắc còn thiếu: chưa nói bao nhiêu click âm thì đủ để tách người ngồi trong xe ba bánh, và kính xe có thuộc mask hay không (câu hỏi mở mục 6.5).
- *(Tuỳ chọn)* Interactor SAM trong CVAT: không làm. Mask làm offline bằng `sam2_points.py`, SAM 2.1 `sam2.1_b.pt`. Khác với Interactor: click ghi bằng tọa độ pixel, không bấm trực tiếp trên ảnh CVAT.

## 6. Calibration (Part 6)

- Số quyết định: 3 (`calibration_decisions.csv`). Quan trọng nhất là CAL-01: YOLO26 không có lớp `three_wheeler`, nên img_002 mất box xe và img_013 / img_004 bị gọi `car`. Sửa lớp, không sửa bằng tăng ngưỡng.
- Quy tắc mới đã vào `guideline_v1.1_calibrated.md`: xe ba bánh có mui vẫn là `three_wheeler` (CAL-01); người trong khoang còn thấy ≥ 20% là `person` riêng và mask xe không gồm người (CAL-02, câu hỏi mở mục 6.4 của v1.0); box lỏng và vật sót là lỗi duyệt, không đổi lớp (CAL-03, img_023 và img_009).
- Nguyên nhân gốc: CAL-01 là máy (danh mục COCO không có tuk-tuk). CAL-02 là guideline mơ hồ (ngưỡng 20% và mask người lái chưa nói số click). CAL-03 là người duyệt bỏ bước quét ảnh và bước kéo box.

## 7. QC và quyết định (Part 6)

- Mẫu: 5 ảnh (0 ảnh `can_review` + 5 ngẫu nhiên, seed 42: img_003, img_006, img_014, img_031, img_035), 100 đối tượng. Người kiểm: Hoàng Minh Hải (**hạn chế**: cùng người vừa duyệt 24 ảnh).
- Lỗi theo **đối tượng**: 3/100 = 3.0% · theo **ảnh**: 3/5 = 60% (ngưỡng Release ≤ 2%). Nguồn: `submission/qc/qc_report_auto.md`.
- Top lỗi: chỉ một loại, `sai_vi_tri_kich_thuoc`, 3/3 lỗi. img_003 object 19: box `three_wheeler` cắt mui và đáy bánh. img_014 object 153: box `motorbike` bỏ tay lái và gương. img_031 object 463: box `person` cắt bàn tay. img_006 và img_035 không ghi lỗi.
- **Giới hạn mẫu nhỏ** (cận trên 95% trong `qc_report_auto.md`): 3 ảnh lỗi trên 5 ảnh mẫu, cận trên Wilson của tỉ lệ ảnh lỗi là 88%. Ngưỡng 2% không chứng minh được bằng mẫu này.
- Script kiểm được số box, lớp, accepted/edited/deleted/added, bắt bẫy, và đếm phiếu QC. Script không kiểm box có ôm sát vật không, lớp có đúng ảnh không, hay vật mà cả người và máy cùng sót. Ba lỗi vị trí ở trên là do mắt người, không do script.

**Quyết định: Hold**, ngày 2026-11-17. Lý do (đi theo Hold → Rework → Release): dừng ở Hold vì guideline v1.1 còn 6 câu mở (xe đạp điện, cụm xe máy gần camera, hàng nhô, kính xe, box đêm, mức `conf` được nhận mà không nhìn). Nếu bỏ qua Hold, mẫu vẫn không được Release: 3/5 ảnh = 60% > 2%, và `sai_vi_tri_kich_thuoc` là 3/3 lỗi nên là lỗi lặp. Bắt bẫy 10/12 = 83.3% thì đạt ngưỡng 80%.
Nếu Rework / Hold, việc cần làm tiếp: chốt hoặc ghi rõ từng câu mở còn lại không áp dụng cho lô này; sửa box lỏng trên img_003, img_014, img_031 và rà lại các box accepted khác theo mục 4.1.1 (ôm sát, sai số khoảng 2 px), rồi chạy lại QC.

## 8. Ghi chú cá nhân

- **Dùng AI để làm bài** (công cụ nào, việc gì, kiểm tra thế nào; "không dùng" nếu không): Cursor soạn `model_log.csv`, mục 1–7 báo cáo này, `calibration_decisions.csv` và `guideline_v1.1_calibrated.md`. Số đo lấy từ `work/runs.csv`, `threshold_sweep.md`, `review_stats.md`, `qc_report_auto.md` và `time_log.csv`, rồi đối chiếu lại các file đó. Phiếu `qc_review.csv` (3 lỗi vị trí, 2 ảnh không lỗi) là mắt người điền. Quyết định Hold theo cây Hold → Rework → Release và gợi ý của `qc_report.py --guideline-gap 6`.
- Gold có bị nhiễm không (bạn có nhìn pre-label hay hỏi AI trước khi gán xong)? không. Gán tay 4 ảnh gold (20 phút) và ghi `time_log` trước khi chạy `prelabel.py`. Ảnh gold là img_001, img_012, img_020, img_036. Ảnh đã mở khi xem model là pilot và batch (img_002, img_004, img_007, img_011, img_013, img_032), không phải gold.
- Máy / GPU / CVAT dùng; vướng mắc cài đặt: macOS Apple Silicon, thiết bị MPS, Python 3.12. YOLOE lần đầu tự cài CLIP từ GitHub rồi tải bộ mã hoá chữ (~242 MB). CVAT chạy Docker tại localhost. Python mặc định trên máy là 3.15, không dùng được; venv tạo bằng Python 3.12.
- Dùng cache `data/cache/` thay vì tự chạy? không. YOLO26 và YOLOE chạy local, file ở `work/prelabel/y26_all/` và `work/prelabel/yoloe_all/`.
- Lỗi / bug script đã gặp (nếu có): không có lỗi dừng lệnh. Cảnh báo `torch.jit.load` is deprecated khi nạp YOLOE, lệnh vẫn chạy xong. `check_submission.py` ban đầu báo còn `<<>>` trong báo cáo và `STUDENT.md`; đã điền hết các ô đó.
- Một điều bạn làm khác lần sau: trước khi accept, kiểm mép box có cắt tay lái, bánh xe hoặc bàn tay không. Lần này 3/3 lỗi QC là `sai_vi_tri_kich_thuoc`, và bẫy loose trên img_023 đã lọt. Thời gian ghi trong báo cáo lấy từ `time_log.csv` sau khi sửa: gán tay 20 phút / 4 ảnh, duyệt 52 phút / 24 ảnh.
