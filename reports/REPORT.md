# Báo cáo Ngày 3 — Tracking Annotation

Họ tên / nhóm: Chu Đình Thắng / Solo
Ngày: 15/09/2026

---

## 1. Quá trình gán nhãn

| Mục | Giá trị |
| --- | --- |
| Công cụ | CVAT / khác: CVAT |
| Thời gian gán `clip_02` (warm-up) | 30 phút |
| Thời gian gán `clip_01` | 60 phút |
| Số track đã vẽ trong `clip_01` | 7 |
| Số keyframe trung bình mỗi track | 0 |

Ba tình huống khó nhất khi gán clip này, và bạn xử lý thế nào:

1. Chưa có ghi chép thực tế có frame/ID về tình huống khó trong các tệp hiện có.
2. Chưa có ghi chép thực tế có frame/ID về tình huống khó trong các tệp hiện có.
3. Chưa có ghi chép thực tế có frame/ID về tình huống khó trong các tệp hiện có.

## 2. Tự kiểm và kiểm chéo

Bài làm cá nhân (Solo), không thực hiện kiểm chéo với partner; vì vậy không tạo `reports/review_partner.md`.

Ba lượt tua bắt được gì (lượt 1 nhìn ID, lượt 2 frame đầu/cuối, lượt 3 frame giữa):

- Lượt 1: Chưa có checklist hoặc ghi chép tự kiểm trong repo.
- Lượt 2: Chưa có checklist hoặc ghi chép tự kiểm trong repo.
- Lượt 3: Chưa có checklist hoặc ghi chép tự kiểm trong repo.

Kiểm chéo với: Không áp dụng. Số lỗi từ partner: Không áp dụng.

Ca nào hai người quyết khác nhau, và luật nào còn thiếu trong `GUIDELINE_MINI.md`?

Không áp dụng vì bài làm cá nhân, không có ca hai người quyết khác nhau.

## 3. Pre-gold lock và chấm trước/sau rework

| Evidence | Giá trị |
| --- | --- |
| SHA-256 từ `evidence/pre-gold/clip_01/manifest.json` | `99f4cbda55cf2f86c344e505f202dbc6687822ff177d9c6da5de6cc2a1870cf5` |
| Thời điểm khóa | `2026-09-15T09:58:19.336641+00:00` |
| Số row / frame / track trước khi mở reference | `425 / 185 frame có annotation / 7 track` |

Kết quả `eval_pre_gold.json` và `eval_vs_gold.json` hiện giống nhau; chưa có bằng chứng đã rework giữa hai lần chấm.

| | HOTA | DetA | AssA | LocA | IDF1 | MOTA | MOTP | FP | FN | IDSW |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Bản pre-gold | 0.6121 | 0.4967 | 0.7549 | 0.8718 | 0.7315 | 0.5323 | 0.8572 | 60 | 208 | 0 |
| Sau rework | 0.6121 | 0.4967 | 0.7549 | 0.8718 | 0.7315 | 0.5323 | 0.8572 | 60 | 208 | 0 |

Qua cổng (`IDF1 >= 0.80`, `MOTA >= 0.75`, `MOTP >= 0.70`): **chưa**. MOTP đạt (`0.8572`), IDF1 và MOTA chưa đạt.

Sau khi đọc danh sách lỗi, bạn đã sửa cụ thể những gì? Ghi theo frame và ID:

| Loại lỗi | Frame | ID | Đã sửa thế nào |
| --- | --- | --- | --- |
| Chưa có change log | Chưa có | Chưa có | Chưa có bằng chứng đã sửa |
| Chưa có change log | Chưa có | Chưa có | Chưa có bằng chứng đã sửa |
| Chưa có change log | Chưa có | Chưa có | Chưa có bằng chứng đã sửa |

Các chẩn đoán từ lần chấm hiện tại: gold có 8 track nhưng annotation có 7; gold track 1 bị bỏ sót hoàn toàn; gold track 6 chỉ được phủ 42/56 frame; có 6 nhóm bbox treo/thừa và 7 frame bbox trôi được evaluator nêu ra.

## 4. Kết quả model: ByteTrack control vs ReID treatment

Cấu hình từ `outputs/model_run_config.json`:

| Mục | Giá trị |
| --- | --- |
| Python / ultralytics / torch / lap | 3.14.4 / 8.4.153 / 2.14.0+cpu / 0.5.13 |
| weights / hai tracker | `yolo26n.pt` / `bytetrack.yaml` và `configs/trackers/botsort-reid.yaml` |
| conf / IoU / imgsz / classes | 0.25 / 0.7 / 960 / `2,5,7` (car,bus,truck) |
| device | `cpu` |

| So sánh | HOTA | DetA | AssA | LocA | IDF1 | MOTA | MOTP | FP | FN | IDSW |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| bạn vs gold | 0.6121 | 0.4967 | 0.7549 | 0.8718 | 0.7315 | 0.5323 | 0.8572 | 60 | 208 | 0 |
| ByteTrack control vs gold | 0.7085 | 0.6487 | 0.7761 | 0.8463 | 0.8746 | 0.7487 | 0.8226 | 88 | 54 | 2 |
| BoT-SORT + ReID vs gold | 0.7635 | 0.7110 | 0.8204 | 0.8721 | 0.9001 | 0.7923 | 0.8595 | 91 | 26 | 2 |
| ReID vs bạn | 0.5750 | 0.4363 | 0.7583 | 0.8872 | 0.6548 | 0.1412 | 0.8765 | 288 | 75 | 2 |

## 5. Phân tích — năm câu hỏi

**1. MOTA của bạn cao hơn hay thấp hơn IDF1? Nếu MOTA cao mà IDF1 thấp thì điều đó nói gì, và vì sao MOTA không phạt nặng lỗi ID?**

Trong kết quả hiện có, MOTA `0.5323` thấp hơn IDF1 `0.7315`. Điều này phản ánh lỗi coverage/detection còn lớn: `FN=208`, `FP=60`, trong khi `IDSW=0`. MOTA phụ thuộc mạnh vào FP, FN và IDSW; vì vậy lỗi bỏ sót bbox làm MOTA giảm rõ rệt, còn việc không có ID switch giúp IDF1 vẫn cao hơn MOTA. Chưa có kết quả model để phân tích trường hợp MOTA cao nhưng IDF1 thấp.

**2. ByteTrack control và BoT-SORT + ReID treatment khác nhau thế nào ở IDF1, AssA và IDSW? Dẫn một frame sequence để giải thích treatment tốt hơn, tệ hơn hoặc không đổi đáng kể. Nhắc rõ đây không cô lập causal effect của ReID vì hai tracker implementation khác.**

ReID cao hơn ByteTrack ở IDF1 (0.9001 so với 0.8746) và AssA (0.8204 so với 0.7761), trong khi IDSW giữ nguyên ở 2. ReID cũng cải thiện rõ ở sequence quanh frame 104-113 của track 6: FN giảm và coverage tăng từ 42/56 lên 44/56 frame. Đây là so sánh hai implementation tracker khác nhau, không cô lập causal effect của ReID.

**3. DetA, FP và FN đổi thế nào? Lỗi còn lại là detector hay association?**

So với ByteTrack, ReID tăng DetA từ 0.6487 lên 0.7110 và giảm FN từ 54 xuống 26, nhưng FP tăng nhẹ từ 88 lên 91. AssA cũng tăng 0.7761 lên 0.8204, còn IDSW không đổi ở 2. Vì cả coverage và association đều cải thiện, treatment cho thấy lỗi còn lại là hỗn hợp detector/coverage và association, không chỉ một phía.

**4. Một chỗ bạn đúng và ReID sai (frame, ID, vì sao):**

Ở frame 104 quanh track gold 6, ReID vẫn giữ được một detection khớp (IoU 0.522) trong khi annotation của bạn bị thiếu đoạn track 6; đây là điểm model bắt được vật thể mà nhãn tay chưa phủ đủ.

**5. Một chỗ ReID làm bạn xem lại annotation (frame, ID, vì sao), hoặc lý do evidence cho thấy model sai:**

Ở frame 113, ReID có ID switch từ 24 sang 31 trên track gold 6. Vì model vẫn giữ được coverage nhưng đổi ID, đây là bằng chứng tracker sai association chứ không phải lý do để đổi annotation thủ công.

## 6. Nếu phải gán thêm 10 clip nữa

Giữ ngưỡng che khuất và quy tắc track mới khi xe quay lại, nhưng thêm checklist frame vào/ra cho từng ID. Sau mỗi lượt tua, ghi ngay các frame có bbox treo, bbox trôi và vùng giao cắt; trước khi khóa pre-gold phải xử lý hoặc đánh dấu từng finding.

## 7. Tệp đã nộp

- [x] `annotations/clip_01/gt.txt`
- [x] `annotations/clip_02/gt.txt`
- [x] `evidence/pre-gold/clip_01/gt.txt` và `manifest.json`
- [x] `GUIDELINE_MINI.md` đã điền đầy đủ
- [x] `outputs/eval_vs_gold.json`
- [x] `outputs/model_bytetrack_clip_01.txt`
- [x] `outputs/model_reid_clip_01.txt`
- [x] `outputs/model_run_config.json`
- [x] `outputs/eval_bytetrack_vs_gold.json`, `outputs/eval_reid_vs_gold.json`, `outputs/eval_reid_vs_me.json`
- [x] `reports/review_partner.md` không áp dụng vì làm Solo
- [x] `reports/REPORT.md` (file này)
