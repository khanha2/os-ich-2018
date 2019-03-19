#  Các kỹ thuật lập trình liên quan đến tập tin và thư mục

## 1. Tổng quan

Bài tập này gồm các nội dung:

- Cách sử dụng các hàm liên quan đến xử lý tập tin và thư mục trong thư viện `direct.h` và `windows.h`.

## 2. Nội dung

### 2.1. Các hàm trong thư viện `direct.h`

#### 2.1.1. Tạo thư mục

```c
int mkdir(const char *dirname);
```

Trong đó:

- `dirname`: tên/đường dẫn của thư mục mới.

Kết quả trả về: trong trường hợp tạo được thư mục: 0, ngược lại: -1.

#### 2.1.2. Chuyển thư mục

```c
int chdir(const char *dirname);
```

Trong đó:

- `dirname`: tên/đường dẫn của thư mục sẽ chuyển.

Kết quả trả về: trong trường hợp chuyển được: 0, ngược lại: -1.

#### 2.1.3. Xoá thư mục

```c
int rmdir(const char *dirname);
```

Trong đó:

- `dirname`: tên/đường dẫn của thư mục sẽ xoá.

Kết quả trả về: trong trường hợp xoá được: 0, ngược lại: -1.

### 2.2. Các hàm trong thư viện `stdio.h`

#### 2.2.1. Xoá tập tin

```c
int remove(const char *filename);
```

Trong đó:

- `filename`: đường dẫn của thư mục sẽ xoá.

Kết quả trả về: trong trường hợp xoá được: 0, ngược lại: -1.

### 2.3. Các hàm trong thư viện `windows.h`

#### 2.3.1. `FindFirstFileA`

Tìm một thư mục cho một tập tin hoặc thư mục con có tên phù hợp với tên được cung cấp.

Khai báo:

```c
HANDLE FindFirstFileA(
    LPCSTR              lpFileName,
    LPWIN32_FIND_DATAA  lpFindFileData
);
```

Trong đó:

- `lpFileName`: tên/đường dẫn của tập tin.
- `lpFindFileData`: con trỏ tới cấu trúc `WIN32_FIND_DATA` nhận thông tin về một tập tin hoặc thư mục tìm được.

Kết quả trả về: một handle để sử dụng cho việc tìm tập tin hoặc thư mục.

#### 2.3.2. `FindNextFileA`

Tiếp tục quá trình tìm tập tin hoặc thư mục dựa trên handle trả về từ hàm `FindFirstFileA`.

```c
BOOL FindNextFileA(
    HANDLE              hFindFile,
    LPWIN32_FIND_DATAA  lpFindFileData
);
```

Trong đó:

- `hFindFile`: handle đã được trả về từ `FindFirstFileA`
- `lpFindFileData`: con trỏ tới cấu trúc `WIN32_FIND_DATA` nhận thông tin về một tập tin hoặc thư mục tìm được.

Kết quả trả về: `TRUE` nếu tìm được tập tin tiếp theo, ngược lại `FALSE`.

## 3. Bài tập

Viết chương trình giả lập các lệnh trong DOS như sau:

- Tạo thư mục:

```
MD <tên/đường dẫn thư mục>
```

- Chuyển thư mục:

```
CD <tên/đường dẫn thư mục>
```

- Xoá thư mục:

```
RD <tên/đường dẫn thư mục>
```

- Liệt kê nội dung thư mục:

```
DIR
```

- Xóa tập tin:

```
DEL <tên/đường dẫn tập tin>
```

- Chép tập tin:

```
COPY <tên/đường dẫn tập tin nguồn> <tên/đường dẫn tập tin đích>
```

