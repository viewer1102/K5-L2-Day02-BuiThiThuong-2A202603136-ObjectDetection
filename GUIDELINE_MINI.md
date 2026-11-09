# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Ô tô con dùng để chở người, thường từ 4-7 chỗ ngồi| Car, Taxi, Sedan, Hatchback, SUV. Xe có thân cao vẫn là car nếu nhận ra dáng SUV|
| `truck` |Xe tải lớn chở hàng, có thùng hoặc rơ-moóc| Xe tải thùng, xe đầu kéo, pickup. Dấu hiệu dễ thấy là cabin và phần chứa hàng tách biệt|
| `bus` |Xe được thiết kế để chở nhiều hành khách, gồm xe buýt và xe khách| Xe buýt, xe khách, minibus có dáng xe khách rõ. Thường có thân dài, nhiều cửa sổ dọc hông hoặc cửa lên xuống lớn|
| `van` |Xe có thân liền khối dạng hộp, dùng để chở hàng hoặc chở người| Xe van giao hàng, van chở khách. Van chở khách có thể có cửa sổ hai bên, nên xét cả dáng thân xe khi phân biệt với bus|

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Vẽ một box cho mỗi xe. Căn bốn cạnh sát phần ngoài cùng nhìn thấy của xe, gồm cả bánh và gương nếu thấy rõ. Loại bóng đổ và phần đường khỏi box tối đa có thể.
- Vật thể bị che khuất: Nếu vẫn xác định được lớp, dùng một box bao hết các phần nhìn thấy của cùng chiếc xe. Ví dụ, đầu và đuôi xe lộ ra ở hai bên cột thì vẽ một box bao cả hai phần
- Vật thể bị cắt bởi rìa ảnh: Nếu vẫn xác định được lớp, vẽ box đến sát mép ảnh ở phía bị cắt. Giữ box trong phạm vi ảnh.
- Vật thể rất nhỏ/mơ hồ: Chỉ gán nhãn khi xác định được lớp và vẽ được box có ranh giới đủ rõ. Nếu chỉ nhận ra đó là xe nhưng chưa phân biệt được lớp, ghi lại tên ảnh hoặc số frame để xem lại khi self-QC.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_002` |59 | không gán nhãn | ảnh mờ không nhìn rõ là 1 xe hay 2 xe liền nhau |
| `drive_013` |67 | không gán nhãn | xe bị cắt ở rìa ảnh không phân biệt rõ lớp nào|
| `drive_014` |69 | Gán nhãn van | Phân vân xe van hay truck|
