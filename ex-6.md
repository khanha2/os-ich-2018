# Bài tập 6: Thao tác trên hệ thống tập tin FAT

## 1. Tổng quan

Thực hiện các thao tác xoá và phục hồi tập tin trên đĩa.

## 2. Bài tập

### 2.1. Bài tập 1

Viết chương trình xoá một tập tin trên hệ thống đĩa FAT.

**Hướng dẫn:**

Một tập tin khi bị xóa theo hình thức bình thường, hệ thống sẽ thay ký tự đầu của tên bằng ký tự có mã ASCII là 0xE5 (ký tự σ) đồng thời các ô trên bảng FAT cũng được đặt lại giá trị 0.

Ví dụ: Giả sử khi xóa tập tin có tên HDH.TXT ở thư mục gốc (chiếm các cluster là 2, 3, 6, 8), đầu tiên ta tiến hàng cập nhật phần tên trong entry bằng cách thay ký tự đầu (H) bằng ký tự σ (có mã ASCII là 0xE5) đồng thời các ô trên bảng FAT tại địa chỉ 2, 3, 6, 8 sẽ được đặt lại giá trị 0 (FREE – Cluster còn trống). 

### 2.2. Bài tập 2

Viết chương trình phục hồi lại một tập tin bin xoá.

**Hướng dẫn:**

Điều kiện tiên quyết để có thể phục hồi thành công tập tin đã xóa: 

- Tập tin không bị phân mảnh.
- Phần dữ liệu chưa bị ghi đè.

Trong trường hợp entry chứa thông tin của tập tin vẫn tồn tại trong RDET, chưa bị ghi đè, ta có thể phục hồi tập tin theo các bước sau:

- Đọc RDET, tìm entry của tập tin đã xóa (lưu ý lúc này tên tập tin được lưu trong entry có ký tự đầu là 0xE5).
- Đọc thông tin kích thước file, cluster bắt đầu.
- Tính số clusters mà tập tin đã chiếm giữ. (N)
- Ghi lên bảng FAT N phần tử liên tiếp tại vị trí cluster bắt đầu theo dạng danh sách liên kết.
- Thay đổi ký tự đầu của tên tập tin trong RDET entry. (bỏ đánh dấu xóa)
- Cập nhật bảng FAT1, bảng FAT2, RDET lên đĩa.

Trong trường hợp không còn tồn tại entry chứa tập tin, ta tiến hành các bước sau:

- Xác định header của tập tin: mỗi loại tập tin có một cấu trúc header tương ứng.
- Dựa vào header đã tìm, tiến hành duyệt toàn bộ đĩa để tìm vị trí mà header tồn tại.
- Thực hiện đọc dữ liệu từ vị trí tìm được header.
