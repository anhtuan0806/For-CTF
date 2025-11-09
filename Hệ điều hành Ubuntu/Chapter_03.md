# Chapter 3 – Giới Thiệu Về Shell Scripting

## 3.1 Mục Đích

Tiếp nối các kiến thức cơ bản từ hai chương trước, chương này giúp bạn:

- Hiểu khái niệm về Shell Script và tại sao nó hữu ích.
- Viết, cấp quyền, và thực thi script "Hello, World!" đầu tiên.
- Sử dụng biến (variables) để lưu trữ và truy xuất dữ liệu.
- Nhận dữ liệu đầu vào từ người dùng (`read`) và từ tham số dòng lệnh (`$1`, `$2`).
- Sử dụng các cấu trúc điều kiện cơ bản (`if-else`) để ra quyết định.

---

## 3.2 Shell Script Là Gì?

Một **Shell Script** (kịch bản shell) là một tệp tin văn bản chứa một chuỗi các lệnh của shell (giống như các lệnh bạn gõ trong Terminal).

Thay vì gõ lặp đi lặp lại nhiều lệnh, bạn có thể nhóm chúng vào một tệp tin (một script) và chạy tệp tin đó để thực thi tất cả các lệnh cùng lúc. Đây là cách cơ bản để tự động hóa các tác vụ.

---

## 3.3 Script "Hello, World!" Đầu Tiên

Chúng ta sẽ tạo một script đơn giản in ra dòng chữ **"Hello, World!"**.

### Bước 1: Tạo tệp tin script

Dùng `nano` để tạo một tệp tin tên `hello.sh`:

```bash
$ nano hello.sh
```

### Bước 2: Viết mã nguồn

Trong nano, gõ nội dung sau:

```bash
#!/bin/bash

# Đây là một dòng bình luận (comment)
# Dòng #!/bin/bash ở trên được gọi là 'shebang',
# nó chỉ cho hệ thống biết cần dùng shell 'bash' để chạy tệp này.

echo "Hello, World!"
echo "Hôm nay là: $(date)"
```

`echo`: Lệnh in ra một dòng.

`$(date)`: Thực thi lệnh `date` và chèn kết quả của nó vào bên trong lệnh `echo`.

Lưu tệp và thoát nano (`Ctrl + O`, rồi `Ctrl + X`).

### Bước 3: Cấp quyền thực thi

Khi mới tạo, tệp `hello.sh` chưa có quyền thực thi. Cấp quyền `x` (execute) cho nó:

```bash
# Xem quyền hiện tại (sẽ là -rw-r--r--)
$ ls -l hello.sh

# Thêm quyền thực thi cho chủ sở hữu (u+x)
$ chmod u+x hello.sh

# Xem lại quyền (sẽ là -rwxr--r--)
$ ls -l hello.sh
```

### Bước 4: Chạy script

Để chạy một script trong thư mục hiện tại:

```bash
$ ./hello.sh
Hello, World!
Hôm nay là: Sun Nov 9 10:45:17 +07 2025
```

---

## 3.4 Biến (Variables)

Biến dùng để lưu trữ dữ liệu. Shell script có hai loại biến chính.

### 1. Biến tự định nghĩa

Cách khai báo và sử dụng:

```bash
#!/bin/bash

# KHAI BÁO: Tên biến, dấu bằng, và giá trị (không có khoảng trắng)
TEN="Anh"
THONG_BAO="Chào mừng bạn"

# SỬ DỤNG: Dùng $ để gọi giá trị của biến
echo "$THONG_BAO $TEN."
```

- Dùng dấu **ngoặc kép ("")** cho phép nội suy biến (hiểu `$TEN`).
- Dùng dấu **ngoặc đơn ('')** sẽ in ra chuỗi nguyên gốc (`'$THONG_BAO $TEN'`).

### 2. Biến hệ thống

Shell cung cấp sẵn một số biến môi trường hữu ích:

| Biến        | Ý nghĩa                       |
| ----------- | ----------------------------- |
| `$USER`     | Tên người dùng đang đăng nhập |
| `$HOME`     | Đường dẫn đến thư mục home    |
| `$HOSTNAME` | Tên máy (hostname)            |
| `$PWD`      | Thư mục làm việc hiện tại     |

Ví dụ:

```bash
echo "Bạn đang đăng nhập với $USER trong thư mục $PWD."
```

---

## 3.5 Đọc Dữ Liệu Đầu Vào (Lệnh read)

Bạn có thể yêu cầu người dùng nhập dữ liệu bằng lệnh `read`.

Tạo file `welcome.sh`:

```bash
#!/bin/bash

# -p (prompt): Hiển thị lời nhắc trước khi chờ nhập
read -p "Vui lòng nhập tên của bạn: " ten_nguoi_dung

echo "Xin chào, $ten_nguoi_dung! Chúc một ngày tốt lành."
```

Chạy script:

```bash
$ chmod u+x welcome.sh
$ ./welcome.sh
Vui lòng nhập tên của bạn: Linh
Xin chào, Linh! Chúc một ngày tốt lành.
```

---

## 3.6 Tham Số Dòng Lệnh

Bạn có thể truyền dữ liệu vào script ngay khi gọi nó. Chúng được gọi là **tham số dòng lệnh (positional parameters)**.

| Biến | Ý nghĩa                          |
| ---- | -------------------------------- |
| `$1` | Tham số thứ nhất                 |
| `$2` | Tham số thứ hai                  |
| `$0` | Tên của chính script             |
| `$#` | Số lượng tham số được truyền vào |
| `$@` | Danh sách tất cả tham số         |

Ví dụ: `backup.sh`

```bash
#!/bin/bash

# Script này mong đợi 2 tham số: file nguồn và thư mục đích
echo "Đang sao chép $1 ..."
cp $1 $2
echo "Hoàn tất! Đã sao chép $# tệp."
```

Chạy script:

```bash
$ chmod u+x backup.sh
$ ./backup.sh note.txt ~/Documents
Đang sao chép note.txt ...
Hoàn tất! Đã sao chép 2 tệp.
```

---

## 3.7 Cấu Trúc Điều Kiện (If-Else)

`if` cho phép script của bạn ra quyết định.

### Cú pháp cơ bản

```bash
if [ <điều_kiện> ]; then
    # nếu điều kiện ĐÚNG
else
    # nếu điều kiện SAI
fi
```

> **Lưu ý:** Phải có khoảng trắng trong ngoặc vuông, ví dụ `[ "$A" = "$B" ]`.

### Các phép so sánh phổ biến

#### So sánh chuỗi

| Biểu thức          | Ý nghĩa            |
| ------------------ | ------------------ |
| `[ "$A" = "$B" ]`  | A bằng B           |
| `[ "$A" != "$B" ]` | A khác B           |
| `[ -z "$A" ]`      | Chuỗi A rỗng       |
| `[ -n "$A" ]`      | Chuỗi A không rỗng |

#### So sánh số

| Biểu thức       | Ý nghĩa               |
| --------------- | --------------------- |
| `[ $N -eq $M ]` | N bằng M              |
| `[ $N -ne $M ]` | N khác M              |
| `[ $N -gt $M ]` | N lớn hơn M           |
| `[ $N -lt $M ]` | N nhỏ hơn M           |
| `[ $N -ge $M ]` | N lớn hơn hoặc bằng M |

#### Kiểm tra tệp

| Biểu thức        | Ý nghĩa               |
| ---------------- | --------------------- |
| `[ -f "$FILE" ]` | Tồn tại và là tệp     |
| `[ -d "$DIR" ]`  | Tồn tại và là thư mục |

### Ví dụ if-else

Tạo file `check_age.sh`:

```bash
#!/bin/bash

read -p "Nhập tuổi của bạn: " TUOI

if [ "$TUOI" -ge 18 ]; then
    echo "Bạn đã đủ tuổi."
else
    echo "Bạn chưa đủ tuổi."
fi
```

---

## 3.8 Bài Tập Thực Hành

1. **Script chào hỏi:** Viết script `greeting.sh` in ra `"Chào buổi sáng, [Tên User]!"`, sử dụng biến `$USER`.
2. **Script kiểm tra file:** Viết script `check_file.sh` nhận một tham số `$1`. Kiểm tra xem file đó có tồn tại không (`[ -f "$1" ]`). In ra "File tồn tại" hoặc "File không tìm thấy".
3. **Script sao lưu nâng cao:** Cải tiến `backup.sh` – kiểm tra xem `$1` (file nguồn) có tồn tại không trước khi sao chép.

---

## 3.9 Kết Luận

Shell script là một công cụ mạnh mẽ để tự động hóa các lệnh bạn thường xuyên sử dụng.

Bạn đã học được các khối xây dựng cơ bản: **biến**, **read**, **tham số `$1`**, và **if-else**.

> Nắm vững cú pháp (khoảng trắng, dấu `$`, dấu `[]`) là rất quan trọng.

Ở chương tiếp theo, chúng ta sẽ khám phá **vòng lặp (for, while)** và cách làm việc với nhiều dịch vụ, tiến trình hệ thống.
