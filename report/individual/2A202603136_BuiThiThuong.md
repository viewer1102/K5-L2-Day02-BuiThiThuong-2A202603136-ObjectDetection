
## 1. Thông tin học viên

- Họ tên: Bùi Thị Thường
- MSSV: 2A20260316
- Email: 26ai.thuongbt@vinuni.edu.vn
- Lớp: L2A
- Link repo bài làm: https://github.com/viewer1102/K5-L2-Day02-BuiThiThuong-2A202603136-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1s4lLSRjFwINqf2KZGYqFspzvJNikV5Sb

## 2. Schema và mini guideline
[Mini guideline](../../GUIDELINE_MINI.md). Schema bài làm gồm 0=car, 1=truck, 2=bus, 3=van. Mỗi xe hợp lệ có một box ôm sát phần nhìn thấy. Xe đỗ vẫn gán nhãn khi nhận diện được lớp.

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_002` |Xe bạc sát mép phải | gán nhãn van | Chọn van dựa vào mui cao, thân dạng hộp và cửa sổ hông. Vẽ box đến mép ảnh, theo phần nhìn thấy. Gold gốc ghi ID 1, tương ứng với van khi đọc theo thứ tự lớp của gold. |
| `drive_003` |Xe trắng dạng hộp được model gợi ý là truck | gán nhãn van | Chọn van dựa vào thân liền, mui cao và cửa sổ dọc hông|
| `drive_005` |Xe trắng có cabin và phần khung sau tách biệt | Gán nhãn truck | Chọn truck dựa vào cấu trúc cabin và khung xe nhìn thấy. Box bao phần nhìn thấy của cùng chiếc xe|

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý:  Bản ban đầu đạt 50/50 file nhãn hợp lệ. Checker xác nhận cấu trúc file.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: Ảnh đã rà bằng mắt được ghi lại:drive_001.jpg, drive_002.jpg, drive_003.jpg, drive_005.jpg, drive_009.jpg. Cần bổ sung ít nhất 5 tên ảnh đã rà thực tế trước khi nộp.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Rà xe bị bỏ sót, các box trùng trên cùng một xe và các xe bị model gợi ý sai lớp. Tôi bổ sung box khi nhận diện được xe và ranh giới, xóa box trùng đã xác nhận và chỉnh lớp theo mini guideline.  
- Commit khóa nhãn ban đầu: 2d7b628. Bản sau rework đã được push lên repo.

## 4. Gold QC và rework
Gold gốc và annotation bài làm dùng thứ tự ID khác nhau. Tôi quy đổi một bản sao annotation để chấm với gold gốc theo mapping 0→0, 1→2, 2→3, 3→1. Nhãn trong repo dùng schema bài làm. Hai cột dưới đây dùng cùng phép quy đổi.

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 555 | 531|
| FP | 394 | 159|
| FN | 98 | 122|
| Precision | 0.5848| 0.7696|
| Recall | 0.8499| 0.8132|
| F1 | 0.6929| 0.7908|
| mean_iou_of_matches | 0.8473| 0.8527|
| passed | false| false|
[Kết quả evaluator](../../outputs/annotation_evaluation.json)
·	drive_001: Bản rework có 5 box và cả 5 đều khớp. Bản trước có 15 box và cũng khớp 5. Dãy xe bên trái là điểm khác biệt cần làm rõ về phạm vi gán nhãn. ·	drive_002: Đối chiếu lại lớp của xe bạc sát mép phải. Bản rework giảm từ 44 xuống 15 box và khớp 11 trên 13 box gold. Kết quả tổng hợp chưa xác định nguyên nhân của từng box chưa khớp. 
·	drive_009: Đối chiếu các box xe buýt và xe tải với ID gold. Bản rework có 21 box và khớp 14, giảm một cặp so với bản trước. Cần rà lại cặp bị mất sau chỉnh sửa. 
F1 tăng 0.0979 và mean IoU đạt ngưỡng 0.70. F1 vẫn dưới ngưỡng 0.80 nên CP5 chưa đạt. FP giảm 235, đồng thời FN tăng 24 sau rework.

## 5. Kết quả YOLO
[Notebook Colab đã chạy:](https://colab.research.google.com/drive/1s4lLSRjFwINqf2KZGYqFspzvJNikV5Sb) YOLO26n đã hoàn thành huấn luyện, validation và dự đoán trên 10 ảnh eval.
·	Runtime và thiết bị: Google Colab, CPU. Checkpoint ghi phiên bản Ultralytics 8.4.208. Phiên bản Torch cần đối chiếu log runtime.
·	Cấu hình thực tế: 50 epoch, imgsz=640, batch=-1, device=cpu, dự đoán với conf=0.25.
·	Best checkpoint trong phiên chạy:/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt


| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4679|
| Recall | 0.2464|
| mAP50 | 0.2806|
| mAP50–95 | 0.2068|

Kết quả đính kèm gồm [ảnh dự đoán trên 10 ảnh eval](assets/eval_predictions/visuals/) và [log train cùng biểu đồ YOLO26](assets/yolo26_runs/).

Trên [ảnh drive_051.jpg](assets/eval_predictions/visuals/drive_051.jpg), box quanh chiếc xe đỏ ở tiền cảnh bao khá sát phần xe nhìn thấy. Model dự đoán `van` với confidence 0.95, trong khi thân thấp và đuôi có dáng xe con. Đây là một ca nghi nhầm `car` thành `van` cần đối chiếu với nhãn eval.
1.	Lớp recall thấp nhất: [Ma trận nhầm lẫn chuẩn hóa](assets/yolo26_runs/vehicle_eval/confusion_matrix_normalized.png) cho thấy bus và van cùng có tỷ lệ nhận đúng bằng 0 tại thiết lập của ma trận. Đây là tỷ lệ đọc từ ma trận. Cần đối chiếu bảng validation theo lớp để ghi recall chính xác và ghép một box gold với prediction tương ứng để xác nhận ảnh minh họa.
2.	mAP50 cao hơn mAP50–95: Kiểm tra độ sát của các cạnh box, nhất là ở xe bị che hoặc cắt khung. Một box có thể khớp tại IoU 0.50 nhưng không khớp tại các ngưỡng cao hơn.
3.	Lỗi lớp có thể liên quan guideline hoặc schema: Ma trận cho thấy 0.92 ở ô nhãn tham chiếu van, dự đoán bus. Cần kiểm tra thứ tự ID trong nhãn eval và đối chiếu ảnh với quy tắc phân biệt van/minivan và bus. Tỷ lệ này chưa xác định được nguyên nhân.
4.	Vai trò của 10 ảnh eval: Tập này được giữ ngoài train để validation và chọn checkpoint tốt nhất. Vì đã dùng để chọn checkpoint, kết quả trên tập này là kết quả validation.


## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| CVAT | Box nháp có xe bị bỏ sót, box trùng và nhãn sai lớp
Thứ tự ID lớp của gold khác schema bài làm| Rà ảnh trong CVAT và dùng script IoU để khoanh cặp box nghi trùng
Quy đổi ID trên bản sao annotation rồi chấm với gold gốc| Đã rework và push bản v2|
| Gold QC | Thứ tự ID lớp của gold khác schema bài làm| Quy đổi ID trên bản sao annotation rồi chấm với gold gốc| F1 sau rework 0.7908, còn thiếu 0.0092|
| Colab | Phiên chạy dùng CPU hơi lâu| Đã hoàn thành huấn luyện, validation và xuất ảnh dự đoán| Đã có metric, checkpoint và ảnh prediction|

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): tôi đã có mini guideline, bộ 50 file nhãn train, kết quả checker của bản ban đầu, commit khóa nhãn và số liệu Gold QC trước/sau rework. Phần danh sách ảnh self-QC cần bổ sung đủ 10 tên ảnh đã rà. CP5 chưa đạt ngưỡng F1. Notebook CP6 đã chạy xong và có metric, checkpoint và ảnh prediction. Cần đối chiếu log Torch và bảng recall theo lớp để hoàn thiện mục 5.

## 8. Công cụ AI và tài liệu tham khảo

·	YOLO26n (Ultralytics): Tạo box nháp cho 46 ảnh. Tôi rà và sửa box, lớp và các xe bị bỏ sót trong CVAT.
·	ChatGPT: Hỗ trợ viết script tạo nhãn nháp, dò box nghi trùng, quy đổi ID trên bản sao phục vụ Gold QC và soạn báo cáo. Tôi tự kiểm tra nhãn trên ảnh, chạy checker và evaluator.
·	Tài liệu tham khảo:GUIDE.md, CVAT Ultralytics YOLO, Ultralytics YOLO26.

