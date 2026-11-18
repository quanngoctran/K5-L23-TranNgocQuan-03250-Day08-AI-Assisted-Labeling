# Graph Report - K5-L23-DAY08-2A202603275-HoangMinhHai-AIPrelabel  (2026-11-17)

## Corpus Check
- 51 files · ~118,058 words
- Verdict: corpus is large enough that graph structure adds value.
- Unclassified: 12 file(s) not represented in the graph (top: (none) 6, .csv 5, .ipynb 1)

## Summary
- 455 nodes · 988 edges · 21 communities (19 shown, 2 thin omitted)
- Extraction: 98% EXTRACTED · 2% INFERRED · 0% AMBIGUOUS · INFERRED: 17 edges (avg confidence: 0.89)
- Token cost: 0 input · 0 output

## Graph Freshness
- Built from commit: `e2e9a5d3`
- Run `git rev-parse HEAD` and compare to check if the graph is stale.
- Run `graphify update .` after code changes (no API cost).

## Community Hubs (Navigation)
- common.py
- GUIDE.md
- prelabel.py
- json
- Day 08 Lab — AI hỗ trợ gán nhãn: Pre-label, Review & Calibration
- Day08RegressionAndWorkflowTest
- make_figures.py
- Dùng pre-label với CVAT
- Hướng dẫn làm lab (từng Part)
- cvat_function_yolo26.py
- Label contract — Phố châu Á (v1.0)
- Rubric — 100 điểm
- Báo cáo Day 08 — <<MSHV - Họ và tên>>
- Bài nâng cao (tuỳ chọn, ngoài 4 giờ)
- Tour model (Part 2): YOLO26 · SAM · YOLOE
- Day 08 Lab — AI hỗ trợ gán nhãn: Pre-label, Review, Calibration
- Quy trình review nhãn máy (Part 4)
- Cài đặt môi trường (Part 0: làm TRƯỚC buổi học)
- Quy tắc làm bài
- Label contract — Phố châu Á — BỔ SUNG v1.1 — <<MSHV - Họ tên>>
- configure_utf8_stdio

## God Nodes (most connected - your core abstractions)
1. `die()` - 33 edges
2. `rel()` - 33 edges
3. `load_taxonomy()` - 33 edges
4. `load_coco()` - 30 edges
5. `main()` - 19 edges
6. `Day08RegressionAndWorkflowTest` - 18 edges
7. `images_of()` - 17 edges
8. `main()` - 16 edges
9. `main()` - 15 edges
10. `make_ann()` - 14 edges

## Surprising Connections (you probably didn't know these)
- `8. Gỡ rối` --references--> `cvat_job_ui()`  [INFERRED]
  docs/CVAT_AI.md → docs/assets/make_figures.py
- `8. Gỡ rối` --references--> `cvat_upload_flow()`  [INFERRED]
  docs/CVAT_AI.md → docs/assets/make_figures.py
- `8. Gỡ rối` --references--> `two_paths()`  [INFERRED]
  docs/CVAT_AI.md → docs/assets/make_figures.py
- `main()` --calls--> `load_taxonomy()`  [EXTRACTED]
  scripts/check_submission.py → scripts/common.py
- `main()` --calls--> `die()`  [EXTRACTED]
  scripts/qc_report.py → scripts/common.py

## Import Cycles
- None detected.

## Communities (21 total, 2 thin omitted)

### Community 0 - "common.py"
Cohesion: 0.08
Nodes (48): main(), main(), area(), _bbox_of_polys(), box_iou(), build_coco(), CocoSet, covered_by() (+40 more)

### Community 1 - "GUIDE.md"
Cohesion: 0.06
Nodes (36): Checkpoints — Day 08 (240 phút trên lớp), Cần hiểu ở mỗi mốc, Dữ liệu, A. Quy ước số đo (luôn ghi mẫu số), B. Giới hạn thống kê của mẫu nhỏ, Bước 1. Calibration guideline (~20 phút), Bước 2–3. QC, quyết định và báo cáo (~25 phút), Calibration và QC (Part 6) (+28 more)

### Community 2 - "prelabel.py"
Cohesion: 0.07
Nodes (18): mask_iou(), mask_to_polys(), obj_mask(), pick_device(), polys_to_mask(), Taxonomy, Backend, GDinoBackend (+10 more)

### Community 3 - "json"
Cohesion: 0.08
Nodes (15): get(), main(), check(), Report, line(), main(), E(), exists() (+7 more)

### Community 4 - "Day 08 Lab — AI hỗ trợ gán nhãn: Pre-label, Review & Calibration"
Cohesion: 0.05
Nodes (39): Bản chất vấn đề: mỗi họ mô hình giải một bài toán nhận diện khác nhau, Bản chất vấn đề: vì sao box chữ nhật bất lực trước vật thể rỗng ruột và chen chúc?, Bản chất vấn đề: vì sao không thể kết thúc bài ngay sau khi bấm Save trên CVAT?, Bản chất vấn đề: vì sao một ngưỡng chung 0.3 làm hỏng chất lượng lô nhãn?, Bản chất vấn đề: vì sao phải gài bẫy nhân tạo vào pre-label trước khi review?, Bản chất vấn đề: vì sao phải khóa tập gold trước khi chạm vào model AI?, Checkpoint Pha 1 và bước tiếp theo, Checkpoint Pha 2 và bước tiếp theo (+31 more)

### Community 6 - "make_figures.py"
Cohesion: 0.27
Nodes (16): arrow(), cvat_job_ui(), cvat_labels_raw(), cvat_upload_flow(), decision_tree(), families(), fit(), font() (+8 more)

### Community 7 - "Dùng pre-label với CVAT"
Cohesion: 0.13
Nodes (15): 0. Cài CVAT bằng Docker (làm trước buổi học), 1. Project và nhãn (làm một lần), 2. Tạo Task, 3. Cách A — Nạp nhãn máy vào Task, 3a. Box (detection): COCO 1.0, 3b. (Nâng cao) Mask nhiều ảnh: XML thay vì COCO, 3c. Điều cần biết sau khi nạp, 4. Duyệt trong CVAT (phím tắt và mẹo) (+7 more)

### Community 8 - "Hướng dẫn làm lab (từng Part)"
Cohesion: 0.15
Nodes (13): Bức tranh tổng, Hướng dẫn làm lab (từng Part), Lỗi hay gặp, Máy yếu, Windows, Mac, Nếu bị trễ giờ (cắt theo thứ tự này), Part 0 — Cài đặt (TRƯỚC buổi học, ~45 phút), Part 1 — Gold: gán tay 4 ảnh, không AI (25 phút), Part 2 — Tour 3 model (30 phút) (+5 more)

### Community 9 - "cvat_function_yolo26.py"
Cohesion: 0.22
Nodes (3): _device(), _weights_path(), Yolo26Function

### Community 10 - "Label contract — Phố châu Á (v1.0)"
Cohesion: 0.18
Nodes (11): 1. Mục đích và phạm vi, 2. Lớp (mỗi vật thuộc đúng một lớp), 3. Thuộc tính, 4.1 Box (detection), 4.2 Mask / polygon (Part 5: 3 vật; nâng cao: 8 ảnh `seg`), 4.3 Chọn lớp khi nhìn không rõ, 4. Quy tắc hình học, 5. Ca khó đã chốt (bảng sống: thêm dòng ở v1.1) (+3 more)

### Community 11 - "Rubric — 100 điểm"
Cohesion: 0.20
Nodes (10): 1. Cài đặt, tái lập, git (8), 2. Gold và label contract (10), 3. Tour model và recipe (20), 4. Chọn ngưỡng (8), 5. Review và đo (28), 6. Mask SAM (8), 7. Calibration (10), 8. QC và báo cáo (8) (+2 more)

### Community 12 - "Báo cáo Day 08 — <<MSHV - Họ và tên>>"
Cohesion: 0.20
Nodes (9): 1. Model và recipe (Part 2–3), 2. AI error profile (từ `qc/review_stats.md`), 3. Thiên kiến tự động hóa và ảnh bẫy, 4. Máy có đáng dùng không?, 5. Mask SAM cho 3 vật (Part 5), 6. Calibration (Part 6), 7. QC và quyết định (Part 6), 8. Ghi chú cá nhân (+1 more)

### Community 13 - "Bài nâng cao (tuỳ chọn, ngoài 4 giờ)"
Cohesion: 0.22
Nodes (9): 1. Grounding DINO và OWLv2: open-vocab "nặng", 2. SAM 3: gõ chữ, ra mọi instance + mask, 3. Pilot riêng để chọn ngưỡng (đỡ phụ thuộc vào gold), 4. Mask cho cả 8 ảnh `seg` và review mask, 5. Dùng AI trực tiếp trong CVAT, 6. Gán lại mù để đo nhất quán của chính bạn, 7. Mở rộng cho GPU mạnh, 8. Phương án dự phòng: cache kết quả (+1 more)

### Community 14 - "Tour model (Part 2): YOLO26 · SAM · YOLOE"
Cohesion: 0.22
Nodes (9): A. YOLO26 — detector đóng (closed-set), B0. Chọn phiên bản SAM (bắt buộc dùng ít nhất một), B1. Prompt bằng click (mô phỏng Interactor), B2. Box → mask (kiểu "detector rồi segmenter"; đổi `--sam` để chọn phiên bản), B. SAM — Segment Anything: từ prompt ra mask (**BẮT BUỘC**), Ba model bắt buộc, C. YOLOE-26 — open-vocabulary, gõ chữ ra box + mask, Sau khi thử (cuối Part 2) (+1 more)

### Community 15 - "Day 08 Lab — AI hỗ trợ gán nhãn: Pre-label, Review, Calibration"
Cohesion: 0.22
Nodes (9): Bạn học được gì, Bạn sẽ làm gì, Bắt đầu nhanh (5 bước), Bộ dữ liệu: phố châu Á, và những chiếc xe mà COCO không biết tên, CVAT: hai cách dùng AI, Day 08 Lab — AI hỗ trợ gán nhãn: Pre-label, Review, Calibration, Lộ trình, Quy trình review: 4 bước + quét ảnh trước (+1 more)

### Community 16 - "Quy trình review nhãn máy (Part 4)"
Cohesion: 0.25
Nodes (8): 1. Vì sao cần quy trình: thiên kiến tự động hóa, 2. Checklist bốn bước cho MỖI ảnh (thứ tự cố định), 3. Quy tắc làm việc (phải theo), 4. Ảnh bẫy: đo chính bạn, 5. Ghi giờ (time_log.csv), 6. Sau khi export: đo, Ba quy tắc giảm thiên kiến của riêng bạn (ghi vào REPORT.md), Quy trình review nhãn máy (Part 4)

### Community 17 - "Cài đặt môi trường (Part 0: làm TRƯỚC buổi học)"
Cohesion: 0.25
Nodes (8): 0. Bạn cần gì, 1. Tạo môi trường ảo và cài thư viện, 2. Tải trọng số, 3. Kiểm tra, 4. SAM 3 (nâng cao, không bắt buộc), 5. Máy không có GPU (CPU only) — hoặc dùng Google Colab, 6. Lỗi hay gặp, Cài đặt môi trường (Part 0: làm TRƯỚC buổi học)

### Community 18 - "Quy tắc làm bài"
Cohesion: 0.29
Nodes (7): Dữ liệu, model và giấy phép, Git và nộp bài, Gold phải sạch, Hình thức, Quy tắc làm bài, Sử dụng AI, Tính trung thực của số liệu

### Community 19 - "Label contract — Phố châu Á — BỔ SUNG v1.1 — <<MSHV - Họ tên>>"
Cohesion: 0.40
Nodes (4): 1. Câu hỏi mở đã chốt (từ mục 6 của v1.0), 2. Ca khó mới (từ lỗi của máy, bẫy lọt, box `can_review`), 3. Lịch sử thay đổi, Label contract — Phố châu Á — BỔ SUNG v1.1 — <<MSHV - Họ tên>>

## Knowledge Gaps
- **2 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `Day 08 Lab — AI hỗ trợ gán nhãn: Pre-label, Review & Calibration` connect `Day 08 Lab — AI hỗ trợ gán nhãn: Pre-label, Review & Calibration` to `GUIDE.md`?**
  _High betweenness centrality (0.154) - this node is a cross-community bridge._
- **Should `common.py` be split into smaller, more focused modules?**
  _Cohesion score 0.07719298245614035 - nodes in this community are weakly interconnected._
- **Why does `Dùng pre-label với CVAT` connect `Dùng pre-label với CVAT` to `GUIDE.md`, `make_figures.py`?**
  _High betweenness centrality (0.068) - this node is a cross-community bridge._
- **Should `GUIDE.md` be split into smaller, more focused modules?**
  _Cohesion score 0.05827067669172932 - nodes in this community are weakly interconnected._
- **Why does `Day08RegressionAndWorkflowTest` connect `Day08RegressionAndWorkflowTest` to `json`?**
  _High betweenness centrality (0.066) - this node is a cross-community bridge._
- **Should `prelabel.py` be split into smaller, more focused modules?**
  _Cohesion score 0.0726950354609929 - nodes in this community are weakly interconnected._
- **Should `json` be split into smaller, more focused modules?**
  _Cohesion score 0.08325624421831637 - nodes in this community are weakly interconnected._