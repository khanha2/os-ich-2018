# Các kỹ thuật lập trình liên quan đến tập tin

## 1. Tổng quan

Bài tập này gồm các nội dung:

- Đọc/ghi tập tin theo dạng nhị phân.
- Hiển thị thông tin dựa trên kiểu dữ liệu đã quy ước.

## 2. Đọc/ghi tập tin

### 2.1. Các hàm liên quan đến tập tin

#### 2.1.1. Mở tập tin

```c
FILE* fopen(const char *filename, const char *mode)
```

Trong đó:

- `filename`: tên tập tin
- `mode`: chế độ đọc ghi, gồm có:
    - `r`: đọc tập tin (nếu tập tin tồn tại).
    - `w`: ghi mới tập tin.
    - `a`: ghi thêm vào tập tin có sẵn.
    - `r+`: đọc hoặc ghi tập tin, trong đó không xoá nội dung cũ của tập tin và không tạo tập tin mới nếu tập tin không tồn tại.
    - `w+`: đọc hoặc ghi tập tin, trong đó xoá nội dung cũ của tập tin và tạo tập tin mới nếu tập tin không tồn tại.
    - `a+`: đọc hoặc ghi thêm vào tập tin, trong đó tạo tập tin mới nếu tập tin không tồn tại.
    - `t`: đọc dưới dạng văn bản (text).
    - `b`: đọc dưới dạng nhị phân (binary).

#### 2.1.2. Đọc dữ liệu

```c
size_t fread(void *ptr, size_t size, size_t n, FILE* stream) 
```

#### 2.1.3. Ghi dữ liệu

```c
size_t fwrite(const void *ptr, size_t size, size_t n, FILE* stream)
```

Trong đó:

- `ptr`: vùng đệm để đọc/ghi.
- `size`: kích thước (theo byte) 1 item cần đọc/ghi.
- `n`: số phần tử cần đọc/ghi.
- `stream`: con trỏ đến tập tin sẽ đọc/ghi.

Kết quả trả về: số phần tử đã được đọc/ghi.

### 2.2. Chỉ thị nhập/xuất cho các hàm scanf, printf

- Ký tự (`char`): `%c`.
- Chuỗi (`char[]`) có ký tự kết thúc chuỗi: `%s`.
- Số nguyên (`int`):
    - Dạng bát phân: `%o`.
    - Dạng thập phân: `%d`.
    - Dạng thập lục phân: `%x` (chữ thường a, b…), `%X` (chữ hoa A, B…).
    - Số thực (`float`): `%f`.
    - Số nguyên dài (`long`): `%ld` (long int)
    - Số thực dài (`double`): `%lf` (long float)

### 2.3. Định dạng số khoảng trống xuất

#### 2.3.1. Đối với số nguyên

Cấu trúc:

```c
%nd
```
Trong đó

- `n`: khoảng trống dành biểu diễn số nguyên (n âm: canh trái, n dương: canh phải). Nếu n = 0 thì không canh lề.

Ví dụ:

```c
int a = 123, b = 15;
printf("%-4d%5d", a, b);
```

Kết quả: dành 4 khoảng trống và canh trái cho biến a; dành 5 khoảng trống và canh phải cho biến b

```
123    15
```

#### 2.3.2. Đối với số thực

Cấu trúc:

```c
%n.md
```

Trong đó:

- `n`: khoảng trống dành biểu diễn số thực tính luôn dấu chấm “.” (n âm: canh trái, n dương: canh phải).
- `m`: số chữ số lẻ của số thực đó. Lưu ý, nếu dùng cách này để biểu diễn số nguyên thì số nguyên đó tự động thêm số 0 ở đầu để bảo đảm tổng số chữ số của số nguyên bằng `m`.

Ví dụ:

```c
int a = 4; float b = 1.2, c = 2.126;
printf("%-5.3d%6.2f%7.2f", a, b, c);
```

Kết quả: dành 5 khoảng trống, canh trái, 3 số để biểu diễn biến a; dành 6 khoảng trống, canh phải, 2 số lẻ để biểu diễn biến b; dành 7 khoảng trống, canh phải, 2 số lẻ để biểu diễn biến c.

```
004    1.20   2.13
```

### 2.4. Bài tập

**Bài tập 1:** Viết chương trình mã hóa nội dung của một tập tin từ dạng bảng tính thô sang dạng tập tin nhị phân và chương trình giải mã tương ứng. Bảng tính thô là một tập tin *csv* với mỗi ô ngăn cách nhau bởi khoảng trắng và có các cột được mô tả như sau:

Tên cột | Kiểu dữ liệu
-|-
C1| int
C2| double
C3| char

Ví dụ nội dung tập tin:

```
1 0.5 a
2 0.245 b
```

Tập tin nhị phân có đuôi *.dat* với mỗi dòng dữ liệu chứa trong tập tin là một vùng đệm (buffer, hay khối dữ liệu) chứa 3 thông tin lần lượt là C1, C2, C3.

**Bài tập 2:** Viết chương trình đọc và hiển thị nội dung một tập tin ra màn hình dưới dạng HEX.

Trường hợp tập tin thuộc dạng văn bản thô (ví dụ: *txt*, *csv*): dữ liệu trả về dưới dạng văn bản.

Trường hợp tập tin thuộc dạng tập tin có cấu trúc (tập tin nhị phân) (ví dụ: *exe*, *zip*, *doc*): dữ liệu bên trong được ghi dưới dạng các buffer liên tiếp nhau.
