# Mini annotation guideline — Ngày 3 (tracking)

> Điền file này **trong lúc gán nhãn**, không phải sau khi xong. Mỗi lần bạn dừng
> lại nghĩ "cái này tính sao nhỉ?" thì đó là một dòng phải ghi vào đây.
>
> Đây là tài liệu mà người gán nhãn tiếp theo sẽ đọc để làm giống bạn. Nếu hai
> người trong nhóm gán khác nhau, gần như luôn là vì file này chưa nói rõ — chứ
> không phải vì ai kém.

Nhóm / tên: Chu Đình Thắng / Solo
Clip: `clip_01`, `clip_02`

---

## 1. Phạm vi: gán cái gì, không gán cái gì

Một lớp duy nhất: **`vehicle`** — xe bốn bánh (xe con, van, xe buýt, xe tải).

| Gán | Không gán |
| --- | --- |
| xe con, SUV, taxi, xe bán tải | người đi bộ |
| van, minivan | xe đạp |
| xe buýt, minibus | **xe máy / mô tô** |
| xe tải, xe đầu kéo | xe trong ảnh quảng cáo, trong gương, dưới bóng nước |

Bổ sung của nhóm (nếu có): chỉ gán phương tiện bốn bánh nhìn thấy được trong ảnh.

## 2. Luật ID — phần quan trọng nhất

| Tình huống | Luật của nhóm | Vì sao |
| --- | --- | --- |
| Xe bị che một phần rồi hiện lại | giữ nguyên ID nếu bị che **dưới 25 frame** (mặc định của lab: 25 frame = 2 giây @ 12.5 fps) | continuity hình học và chuyển động |
| Xe bị che lâu hơn ngưỡng trên | kết thúc track; nếu xuất hiện lại thì tạo track mới | tránh nối nhầm identity |
| Xe rời khung hình rồi quay lại | **track mới** | không đủ bằng chứng để nối identity cũ |
| Hai xe cắt nhau / chồng lên nhau | giữ ID theo chuyển động liên tục; kiểm tra frame trước và sau vùng chồng | ưu tiên continuity, không đổi ID chỉ vì bbox giao nhau |

## 3. Luật bbox

| Tình huống | Luật của nhóm |
| --- | --- |
| Xe bị cắt bởi rìa ảnh | bbox chạm đúng rìa, không đoán phần ngoài ảnh |
| Xe bị xe khác che một phần | bbox ôm phần **nhìn thấy được** |
| Xe vừa xuất hiện, còn rất nhỏ / rất mờ | bắt đầu từ frame đầu tiên xác định được là xe bốn bánh; không đoán bbox khi chưa phân biệt được |
| Xe đang đỗ, không di chuyển | vẫn giữ track nếu còn trong cảnh; chỉ kết thúc khi rời khung hoặc bị che quá ngưỡng |
| Keyframe đặt dày ở đâu | đặt thêm ở lúc vào/ra khung, trước-sau che khuất, giao cắt và khi bbox trôi |

## 4. Ít nhất ba ca mơ hồ đã gặp thật

Ghi **frame cụ thể** và **ID cụ thể**, không ghi chung chung.

### Ca 1
- Clip / frame / ID: clip_01 / 51-53 / ID 3
- Tình huống: evaluator phát hiện bbox xuất hiện trước track gold 4.
- Quyết định: kết thúc ID 3 trước frame 51; không để bbox treo trước khi xe xuất hiện.
- Lý do: tránh gán nhãn ngoài quãng đời của xe.

### Ca 2
- Clip / frame / ID: clip_01 / 79-100 / ID 5
- Tình huống: evaluator phát hiện bbox của ID 5 nằm trước khi track gold 6 xuất hiện.
- Quyết định: bấm outside ở frame xe rời khung; không kéo track sang xe xuất hiện sau.
- Lý do: mỗi lần xuất hiện lại sau khi rời khung là track mới.

### Ca 3
- Clip / frame / ID: clip_01 / 83-96 / ID 4
- Tình huống: bbox trôi, IoU thấp nhất được evaluator ghi nhận là 0.51 ở frame 83.
- Quyết định: thêm keyframe quanh vùng chuyển động và chỉnh bbox ôm phần nhìn thấy.
- Lý do: keyframe thưa làm bbox lệch khỏi xe.

## 5. Sửa gì sau khi chấm với gold và sau khi kiểm chéo

Luật nào trong file này hoá ra còn thiếu hoặc còn mơ hồ? Viết lại cho rõ:

- Luôn rà frame vào/ra của từng track bằng danh sách frame cụ thể trước khi khóa export.
- Khi evaluator báo bbox trôi hoặc bbox treo, sửa trong CVAT rồi export lại; không sửa trực tiếp file MOT.
