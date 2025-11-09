# Chương 1 — Những Lệnh Cơ Bản

Chào mừng bạn đến với series học **Bash**! Đây là kỹ năng nền tảng cực kỳ quan trọng trong **CTF**. Trong chương này, chúng ta sẽ làm quen với terminal và các lệnh "giao tiếp" cơ bản nhất.

---

## 1. Terminal và Shell (Bash) là gì?

- **Terminal (Thiết bị đầu cuối):** Là cửa sổ màu đen (hoặc màu khác) mà bạn thấy. Đây là giao diện để bạn gõ lệnh vào.
- **Shell (Trình bao):** Là chương trình chạy "bên trong" terminal. Nó đọc lệnh bạn gõ, thực thi, và trả về kết quả.
- **Bash (Bourne Again Shell):** Là một trong những shell phổ biến nhất. Khi mọi người nói "học bash", ý là học cách ra lệnh cho shell này.

---

## 2. Dấu nhắc lệnh (Prompt)

Khi mở terminal, bạn sẽ thấy một dòng chữ, thường kết thúc bằng dấu `$`, ví dụ:

```
username@hostname:~$
```

- `username`: Tên người dùng của bạn.
- `hostname`: Tên máy tính của bạn.
- `~`: Thư mục hiện tại (dấu `~` là ký hiệu cho thư mục **home**).
- `$`: Dấu hiệu cho biết shell đang chờ bạn gõ lệnh (nếu là `#` nghĩa là bạn đang ở quyền **root** — quản trị viên cao nhất).

---

## 3. Các Lệnh "Điều Hướng" Cơ Bản

Đây là 4 lệnh bạn sẽ dùng liên tục. Giống như việc bạn học cách "đi" và "nhìn" trong một căn phòng.

### `pwd` (Print Working Directory)

In ra đường dẫn đầy đủ của thư mục hiện tại.

**Ví dụ:**

```bash
$ pwd
/home/username
```

### `ls` (List)

Liệt kê tất cả file và thư mục con trong thư mục hiện tại.

**Ví dụ:**

```bash
$ ls
Desktop  Documents  Downloads  Music  Pictures  Videos
```

**Các tùy chọn (flags) hữu ích:**

- `ls -l`: Hiển thị ở dạng danh sách dài (long format) — quyền, chủ sở hữu, kích thước, ngày giờ.
- `ls -a`: Hiển thị tất cả (all), bao gồm file/thư mục ẩn (bắt đầu bằng `.`).

Kết hợp rất thường dùng: `ls -la`.

**Ví dụ:**

```bash
$ ls -la
total 40
drwxr-xr-x 5 user user 4096 Nov 09 12:00 .
drwxr-xr-x 3 root root 4096 Nov 01 10:00 ..
-rw-r--r-- 1 user user 220 Nov 01 10:00 .bash_logout
-rw-r--r-- 1 user user 3771 Nov 01 10:00 .bashrc
drwx------ 2 user user 4096 Nov 05 11:30 .cache
...
```

### `cd` (Change Directory)

Dùng để thay đổi thư mục hiện tại.

**Ví dụ:**

```bash
# Đi vào thư mục Downloads
$ cd Downloads

# Kiểm tra
$ pwd
/home/username/Downloads

# Đi lùi lại 1 cấp
$ cd ..

# Về thư mục home
$ cd ~
# Hoặc đơn giản chỉ gõ
$ cd
```

---

## 4. Các Lệnh "Tương Tác" Cơ Bản

### `echo`

In ra ("nói") văn bản bạn đưa vào.

```bash
$ echo "Hello CTF"
Hello CTF
```

### `cat` (Concatenate)

Đọc nội dung file và in ra màn hình — thường dùng để "đọc flag" trong CTF.

**Ví dụ:**

```bash
$ ls
flag.txt  another_file.dat

$ cat flag.txt
Congrats!{day_la_flag_cua_ban}
```

### `file`

Đoán loại file dựa vào nội dung (rất hữu ích khi file không có đuôi hoặc đuôi bị sửa).

**Ví dụ:**

```bash
$ ls
my_file  strange_binary

$ file my_file
my_file: ASCII text

$ file strange_binary
strange_binary: ELF 64-bit LSB executable
```

---

## 5. Bài tập thực hành

Mở terminal và thử gõ các lệnh sau theo thứ tự (hoặc copy-paste):

```bash
pwd
ls -la
mkdir hoc_bash   # tạo thư mục mới tên hoc_bash
cd hoc_bash
pwd
echo "flag{bash_la_de}" > flag.txt   # tạo file flag.txt với nội dung
ls
cat flag.txt
cd ..
```

Nếu mọi thứ đúng, `cat flag.txt` sẽ in ra `flag{bash_la_de}`.

---

### Kết thúc Chương 1

Bạn đã nắm được các lệnh tối cơ bản để di chuyển và xem xét file. Đây là nền tảng để khám phá hệ thống. Ở **Chương 2** chúng ta sẽ học về cách **tìm kiếm file** và **quản lý quyền** (permissions).

---
