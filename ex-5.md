# Bài tập 5: Thao tác trên hệ thống tập tin FAT

## 1. Tổng quan

Giới thiệu cấu trúc và một số thao tác trên hệ thống tập tin FAT (cụ thể là FAT12).

## 2. Cấu trúc đĩa mềm

### 2.1. Tổ chức dữ liệu

Đĩa mềm 1.44MB có 2 head (side), mỗi head có 80 track, mỗi track có 18 sector, mỗi sector có kích thức 512 byte.

Do đó, tổng số sector của đĩa mềm là:

$$
2 * 80 * 18 = 2880\ (sector).
$$

2880 sector này được phân bổ theo thứ tự sau:

Vị trí | Kích thước (sector) | Tên
-|-|-
0 | 1 | Boot Sector
1 | 9 | Bảng cấp phát tập tin 1 - FAT1 (FAT - File Allocation Table)
10 | 9 | Bảng FAT2
19 | 14 | Bảng thư mục gốc (RDET - Root Directory Entry Table)
33 | 2847 | Vùng chứa dữ liệu.

### 2.2. Bảng cấp phát tập tin

Một hệ thống tập tin sử dụng bảng FATX sẽ sử dụng X bit để biểu diễn địa chỉ của một ô (cluster) trên vùng dữ liệu và biểu diễn được tối đa 2X địa chỉ.

Ví dụ:
- FAT8 sẽ sử dụng 8 bit để biểu diễn tối đa 28 = 256 địa chỉ.
- FAT12 sử dụng 12 bit để biểu diễn tối đa 212 = 4096 địa chỉ.
- FAT16 sử dụng 16 bit để biểu diễn tối đa 216 = 65536 địa chỉ.

Do vùng dữ liệu của đĩa mềm gồm 2847 sector và 1 cluster bằng 1 sector, do đó ta sẽ sử dụng FAT12 cho hệ thống tập tin trên đĩa này là hiệu quả nhất (FAT8 không đủ đánh địa chỉ cho 2847 cluster, FAT16 quá dư…).

Sử dụng FAT12 tức là sử dụng 12 bit cho mỗi địa chỉ. Vùng dữ liệu có tất cả 2847 cluster (= 2847 sector), do đó cần:

$$
2847 \times 12 = 34164\ (bit) = 4270.5\ byte ≈ 8.34\ sector
$$

Vậy muốn đánh hết địa chỉ cho vùng dữ liệu bằng FAT12 thì phải sử dụng 9 sector cho bảng FAT.

Mỗi phần tử trong bảng FAT chứa lần lượt các thông tin sau:

Offset | Mô tả
-|-
0 | Cluster còn trống.
1 | Giá trị dành riêng, không sử dụng.
2 - 4079 | Các cluster sử dụng, giá trị là chỉ số của cluster kế tiếp trong dãy dũ liệu chứa tập tin.
4080 - 4086 | Các giá trị dành riêng, không dùng.
4087 | Sector hỏng trong cluster hoặc cluster dự trữ.
4088 - 4095 | Cluster cuối cùng của tập tin.

### 2.3. Bảng thư mục gốc

Là 1 dãy phần tử (entry), mỗi phần tử chứa tên và các thuộc tính của 1 tập tin trên thư mục gốc của volume (hoặc là phần tử trống – chưa thuộc về 1 tập tin nào hết). Mỗi phần tử có kích thước 32 byte, chứa các thông tin được tổ chức theo trật tự như sau:

Offset | Độ dài (byte) | Nội dung
-|-|-
0 | 8 | Tên chính của tập tin
8 | 3 | Tên mở rộng
11 | 1 | Thuộc tính (0-0-A-D-V-S-H-R)
12 | 10 | Không dùng
22 | 2 | Giờ cập nhật tập tin
24 | 2 | Ngày cập nhật tập tin
26 | 2 | Cluster bắt đầu
28 | 4 | Kích thước tập tin

## 3. Bài tập

### 3.1. Bài tập 1

Viết chương trình đọc bảng thư mục gốc và hiển thị tên của các phần tử đó.

**Hướng dẫn:**

- Tạo cấu trúc sau cho một ENTRY:

```c
typedef unsigned char BYTE;

typedef struct {
    BYTE fileName[8];
    BYTE fileExt[3];
    BYTE attrib;
    BYTE reserved[10];
    WORD time;
    WORD date;
    WORD firstCluster;
    DWORD fileSize;
} ENTRY;
```

- Cấp phát (hoặc tạo mảng) có kích thước 224 lần ENTRY trên (đúng bằng 14 sector).
- Dùng vùng đệm trên để đọc toàn bộ nội dung bảng thư mục gốc (14 sector bắt đầu từ sector 19).

### 3.2. Bài tập 2

Viết chương trình đọc bảng FAT và lưu vào một tập tin.

**Hướng dẫn:**

- Đọc toàn bộ 9 sector của bảng FAT1 (bắt đầu tại sector 1)
- Xác định giá trị của ô trên FAT bằng các phép & (and), | (or), << (shift left), >> (shift right)

Tham khảo hàm sau:

```c
int GetFatValue(int k, unsigned char* Fat) {
    int i = (k * 3) / 2;
    int nHi = Fat[i + 1];
    int nLo = Fat[i];

    if (k%2 == 0) {
        return (nLo | ((nHi & 0xF) << 8));
    }

    return ((nHi << 4) | (nLo >> 4));
}
```

### 3.3. Bài tập 3

Viết chương trình định dạnh lại đĩa (format).

**Hướng dẫn:**

- Quick Format: Xóa nội dung 2 bảng FAT và bảng thư mục gốc.
- Full Format: Quick Format + Xóa nội dung vùng dữ liệu.

## 4. Tài liệu sử dụng

- http://fileadmin.cs.lth.se/cs/Education/EDA385/HT09/student_doc/FinalReports/FAT12_overview.pdf