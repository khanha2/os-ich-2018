# Kỹ thuật lập trình đọc/ghi sử dụng Windows API 

## 1. Tổng quan

Bài tập này hướng dẫn sử dụng một số hàm trong Windows API (sử dụng thư viện `windows.h`) để thực hiện thao tác đọc/ghi trên tập tin/thiết bị hỗ trợ nhập xuất.

## 2. Nội dung



### 2.1. CreateFileA

Tạo hoặc truy cập một tập tin/thiết bị.

```c++
HANDLE CreateFileA(
    LPCSTR                  lpFileName,
    DWORD                   dwDesiredAccess,
    DWORD                   dwShareMode,
    LPSECURITY_ATTRIBUTES   lpSecurityAttributes,
    DWORD                   dwCreationDisposition,
    DWORD                   dwFlagsAndAttributes,
    HANDLE                  hTemplateFile
);
```

Trong đó:

- `lpFileName`: tên tập tin/thiết bị muốn tạo hoặc truy cập.
- `dwDesiredAccess`: chế độ truy cập vào tập tin/thiết bị. Có các chế độ sau:
  - `GENERIC_READ`: đọc.
  - `GENERIC_WRITE`: ghi.
  - `GENERIC_READ | GENERIC_WRITE`: đọc/ghi.
- `dwShareMode`: chế độ chia sẻ khi thao tác trên tập tin/thiết bị. Có các chế độ chia sẻ sau:
  - `FILE_SHARE_READ`.
  - `FILE_SHARE_WRITE`.
  - `FILE_SHARE_READ | FILE_SHARE_WRITE`.
- `lpSecurityAttributes`: con trỏ tới cấu trúc `SECURITY_ATTRIBUTES`.
- `dwCreationDisposition`: phương thức thức tạo tập tin khi đã tồn tại hoặc chưa tồn tại.
- `dwFlagsAndAttributes`: các cờ và thuộc tính của tập tin/thiết bị.
- `hTemplateFile`: handle có hiệu lực đang truy cập tập tin/thiết bị tạm với chế độ truy cập GENERIC_READ.

Kết quả trả về: một handle xử lý việc đọc/ghi tập tin/thiết bị.

### 2.2. SetFilePointerEx

Di chuyển con trỏ đọc/ghi của một tập tin (hoặc thiết bị).

```c++
BOOL SetFilePointerEx(
    HANDLE            hFile,
    LARGE_INTEGER   lDistanceToMove,
    PLARGE_INTEGER  lpNewFilePointer,
    DWORD           dwMoveMethod
);
```

Trong đó:

- `hFile`: handle của tập tin/thiết bị được phát sinh từ mục 2.1.
- `lDistanceToMove`: số byte cần dịch chuyển.
- `lpNewFilePointer`: con trỏ đến một vùng nhớ nhận giá trị là vị trí mới của con trỏ đọc/ghi.
- `dwMoveMethod`: Điểm xuất phát để di chuyển con trỏ đọc/ghi. Các vị trí đặt biệt:
  - `FILE_BEGIN`: vị trí bắt đầu.
  - `FILE_CURRENT`: vị trí hiện tại.
  - `FILE_END`: vị trí kết thúc.

Kết quả trả về: trả về TRUE nếu thực hiện di chuyển thành công, ngược lại trả về FALSE.

Lưu ý: Đối với việc đọc/ghi trên một thiết bị lưu trữ, `lDistanceToMove` và `dwMoveMethod` phải có gía trị là bội số của **kích thước cluster nhỏ nhất** (thường là 512 byte) mà thiết bị đó hỗ trợ.

### 2.3. ReadFile

```c++
BOOL ReadFile(
    HANDLE          hFile,
    LPVOID          lpBuffer,
    DWORD           nNumberOfBytesToRead,
    LPDWORD         lpNumberOfBytesRead,
    LPOVERLAPPED    lpOverlapped
);
```

Trong đó:

- `hFile`: handle của tập tin/thiết bị được phát sinh từ mục 2.1, sử dụng chế độ `GENERIC_READ`, `GENERIC_READ | GENERIC_WRITE`.
- `lpBuffer`: con trỏ tới vùng đệm chứa dữ liệu cần đọc.
- `nNumberOfBytesToWrite`: số byte cần đọc.
- `lpNumberOfBytesRead`: con trỏ đến một vùng nhớ nhận giá trị là số byte thực sự đọc được.
- `lpOverlapped`: con trỏ tới cấu trúc `LPOVERLAPPED`.

Kết quả trả về: trả về `TRUE` nếu thực hiện đọc thành công, ngược lại trả về `FALSE`.

### 2.4. WriteFile

```c++
BOOL WriteFile(
    HANDLE          hFile,
    LPCVOID         lpBuffer,
    DWORD           nNumberOfBytesToWrite,
    LPDWORD         lpNumberOfBytesWritten,
    LPOVERLAPPED    lpOverlapped
);
```

Trong đó:

- `hFile`: handle của tập tin/thiết bị được phát sinh từ mục 2.1, sử dụng chế độ `GENERIC_WRITE`, `GENERIC_READ | GENERIC_WRITE`.
- `lpBuffer`: con trỏ tới vùng đệm chứa dữ liệu cần ghi.
- `nNumberOfBytesToWrite`: số byte cần ghi.
- `lpNumberOfBytesWritten`: con trỏ đến một vùng nhớ nhận giá trị là số byte thực sự ghi được.
- `lpOverlapped`: con trỏ tới cấu trúc `LPOVERLAPPED`.

Kết quả trả về: trả về `TRUE` nếu thực hiện ghi thành công, ngược lại trả về `FALSE`.

## 3. Bài tập

**Bài tập 4.1:** Sử dụng các hàm trong mục 2 để thực hiện các yêu cầu trong **Bài tập 2**.

**Bài tập 4.2:** Đọc cấu trúc FAT trên Đĩa mềm ảo (phương thức kết nối đã được hướng dẫn trong **Bài tập 1**).


## 4. Tài liệu tham khảo

- https://docs.microsoft.com/en-us/windows/desktop/api/fileapi/nf-fileapi-createfilea
- https://docs.microsoft.com/en-us/windows/desktop/api/fileapi/nf-fileapi-setfilepointerex
- https://docs.microsoft.com/en-us/windows/desktop/api/fileapi/nf-fileapi-readfile
- https://docs.microsoft.com/en-us/windows/desktop/api/fileapi/nf-fileapi-writefile
