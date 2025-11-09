# Chương 5 — Biến và Script Cơ Bản

Bạn đã học nhiều lệnh đơn lẻ — giờ là lúc kết hợp chúng thành một vũ khí tự động: **script**. Thay vì gõ 10 lệnh, chỉ cần chạy 1 file script là xong.

---

## 1. Biến (Variables) là gì?

Biến là một “hộp” chứa thông tin. Bạn đặt tên cho nó và dùng sau này.

**Tạo và dùng biến:**

```bash
ten_muc_tieu="192.168.1.100"
# KHÔNG có khoảng trắng quanh dấu =
# Dùng dấu " " để bao bọc giá trị nếu có khoảng trắng

# Lấy giá trị
echo $ten_muc_tieu
# Dùng trong lệnh
ping -c 4 $ten_muc_tieu
```

**Nháy đơn (') vs. Nháy kép (")**

- Nháy kép `"`: biến trong chuỗi sẽ được thay thế.
- Nháy đơn `'`: chuỗi nguyên xi, không thay biến.

```bash
ten="FLAG"
echo "Gia tri la: $ten"   # Gia tri la: FLAG
echo 'Gia tri la: $ten'     # Gia tri la: $ten
```

---

## 2. Biến Môi Trường (Environment Variables)

Là các biến hệ thống có sẵn, thường viết HOA.

- `$HOME`: thư mục home (ví dụ `/home/username`).
- `$USER`: tên người dùng.
- `$PWD`: thư mục hiện tại.
- `$PATH`: danh sách thư mục để shell tìm lệnh.

Xem tất cả biến môi trường: `env` hoặc `printenv`.

---

## 3. Viết Script Bash Đầu Tiên

Một script là file text chứa các lệnh.

**Bước 1 — Tạo file:**

```bash
touch scan.sh
```

**Bước 2 — Nội dung mẫu:**

```bash
#!/bin/bash

# Shebang: báo hệ thống dùng /bin/bash để chạy file

echo "Bat dau scan muc tieu..."
echo "Muc tieu cua toi la: google.com"

# Chay ping
ping -c 4 google.com

echo "Da ping xong. Ket thuc script."
```

**Bước 3 — Cấp quyền thực thi:**

```bash
chmod u+x scan.sh
```

**Bước 4 — Chạy script:**

```bash
./scan.sh
```

---

## 4. Tham số (Arguments)

Các biến đặc biệt trong script:

- `$0`: tên script.
- `$1`, `$2`, ...: tham số thứ 1, thứ 2, ...
- `$#`: số lượng tham số.
- `$@`: tất cả tham số.

**Ví dụ:** script nhận tham số mục tiêu:

```bash
#!/bin/bash

if [ $# -eq 0 ]; then
    echo "Loi: Ban phai nhap mot dia chi de scan!"
    echo "Vi du: ./scan.sh google.com"
    exit 1
fi

MUC_TIEU=$1

echo "Bat dau scan muc tieu: $MUC_TIEU"
ping -c 4 $MUC_TIEU

echo "Da ping xong $MUC_TIEU"
```

Chạy:

```bash
./scan.sh facebook.com
```

---

## 5. Bài tập thực hành

1. Viết `hello.sh` nhận `$1` là tên và in `Chao ban, [tên]`.

   - Ví dụ: `./hello.sh Binh` → `Chao ban, Binh`.

2. Viết `check_file.sh` nhận `$1` là đường dẫn file — script sẽ chạy `ls -l` trên file đó.

   - Ví dụ: `./check_file.sh /etc/passwd`.

3. Thử thách: viết `web_scan.sh` nhận `$1` là domain:

   - In `--- DANG PING ---` rồi `ping -c 4 $1`.
   - In `--- DANG TAI FILE ---` rồi dùng `wget` hoặc `curl` để lưu trang chủ vào `$1.html`.
   - Gợi ý: `curl $1 -o $1.html` hoặc `curl http://$1 -o $1.html`.

---

## Kết thúc Series Bash Cơ Bản

Bạn đã nắm được nền tảng:

- Chương 1: Di chuyển và xem file (`ls`, `cd`, `cat`).
- Chương 2: Tìm kiếm và quyền (`find`, `grep`, `chmod`).
- Chương 3: Pipes, redirection và tiến trình (`|`, `>`, `ps`, `kill`).
- Chương 4: Mạng và tải file (`ip`, `ping`, `wget`, `curl`).
- Chương 5: Tự động hóa bằng script (`#!/bin/bash`, biến, `$1`).

Bạn đã sẵn sàng học sâu hơn: nâng cao mạng và kỹ thuật leo thang đặc quyền trong CTF.

---
