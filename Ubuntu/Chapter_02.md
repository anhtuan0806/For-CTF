# Chapter 2 – Quản Lý Gói và Cấu Hình Cơ Bản

## 2.1 Mục Đích

Tiếp nối Chapter 1, chương này giúp bạn:

- Hiểu và sử dụng quyền quản trị (sudo).
- Quản lý phần mềm bằng trình quản lý gói apt.
- Làm quen với trình soạn thảo văn bản nano trong Terminal.
- Hiểu và thay đổi quyền hạn của tệp tin và thư mục (chmod, chown).
- Biết cách quản lý người dùng và nhóm cơ bản.

---

## 2.2 Quyền Quản Trị (sudo)

Trong Linux, các tác vụ quan trọng (như cài đặt phần mềm, sửa file hệ thống) yêu cầu quyền quản trị viên, còn gọi là root.

Để thực hiện một lệnh với quyền root, bạn thêm `sudo` (Super User Do) vào trước lệnh.

```bash
$ apt update
E: Mở file khóa /var/lib/apt/lists/lock không thành công - open (13: Permission denied)
...
# Lệnh trên thất bại vì thiếu quyền.

$ sudo apt update
[sudo] password for anh: ********
# Nhập mật khẩu của bạn (sẽ không hiển thị khi gõ)
...
# Lệnh này thành công.
```

**sudo vs su:**

- `sudo` chỉ cấp quyền cho một lệnh và an toàn hơn.
- `su` (Switch User) chuyển bạn sang hẳn phiên làm việc của root, điều này nên hạn chế.

---

## 2.3 Quản Lý Gói Tin Với APT

Phần mềm trong Ubuntu được quản lý dưới dạng các gói (packages). **APT (Advanced Package Tool)** là công cụ dòng lệnh chính để quản lý các gói này.

APT lấy phần mềm từ các **kho chứa (repositories)** – là các máy chủ online chứa hàng ngàn gói đã được kiểm duyệt.

### Các lệnh apt cơ bản

> Bạn phải dùng `sudo` cho các lệnh thay đổi hệ thống.

| Lệnh                         | Công dụng                                                           |
| ---------------------------- | ------------------------------------------------------------------- |
| `sudo apt update`            | Cập nhật danh sách gói tin từ kho chứa. (Luôn chạy lệnh này trước). |
| `sudo apt upgrade`           | Nâng cấp tất cả các gói đã cài đặt lên phiên bản mới nhất.          |
| `sudo apt install <tên-gói>` | Cài đặt một gói mới. (ví dụ: `sudo apt install htop`)               |
| `sudo apt remove <tên-gói>`  | Gỡ bỏ một gói khỏi hệ thống (nhưng giữ file cấu hình).              |
| `sudo apt purge <tên-gói>`   | Gỡ bỏ một gói, bao gồm cả file cấu hình.                            |
| `sudo apt autoremove`        | Xóa các gói "mồ côi" (dependencies không cần dùng nữa).             |
| `apt search <từ-khóa>`       | Tìm kiếm gói tin. (Không cần sudo).                                 |
| `apt show <tên-gói>`         | Xem thông tin chi tiết về một gói. (Không cần sudo).                |

### Ví dụ cài đặt và gỡ bỏ

```bash
# 1. Tìm kiếm một công cụ:
$ apt search "system monitor"

# 2. Tìm thấy 'htop' và cài đặt nó:
$ sudo apt install htop

# 3. Chạy chương trình vừa cài:
$ htop
(Nhấn 'q' để thoát)

# 4. Gỡ bỏ htop:
$ sudo apt remove htop
```

---

## 2.4 Trình Soạn Thảo Văn Bản: nano

Để cấu hình hệ thống, bạn cần chỉnh sửa các file text. **nano** là trình soạn thảo văn bản đơn giản và thân thiện nhất cho người mới bắt đầu.

### Mở hoặc tạo file

```bash
$ nano ten_file.txt
```

### Các phím tắt cơ bản trong nano

| Tổ hợp phím  | Chức năng                               |
| ------------ | --------------------------------------- |
| **Ctrl + O** | Lưu file (WriteOut).                    |
| **Ctrl + X** | Thoát (Exit). Nếu chưa lưu, sẽ hỏi bạn. |
| **Ctrl + W** | Tìm kiếm (Where is).                    |
| **Ctrl + G** | Trợ giúp (Get Help).                    |

> Trong khi nano dễ dùng, **vim (hoặc vi)** là trình soạn thảo mạnh mẽ hơn, được cài đặt sẵn trên hầu hết hệ thống UNIX/Linux.

---

## 2.5 Hiểu Về Quyền Hạn Tệp Tin

Như đã thấy ở Chapter 1, lệnh `ls -l` hiển thị thông tin chi tiết:

```bash
$ ls -l
-rw-r--r-- 1 anh users 1024 Nov 9 10:30 config.txt
drwxr-xr-x 2 anh users 4096 Nov 9 10:31 Documents
```

Cột đầu tiên định nghĩa quyền hạn:

| Ký tự | Ý nghĩa             |
| ----- | ------------------- |
| `-`   | File thông thường   |
| `d`   | Thư mục (directory) |

9 ký tự tiếp theo được chia làm 3 nhóm:

| Nhóm       | Mô tả                | Ví dụ |
| ---------- | -------------------- | ----- |
| **User**   | Quyền của chủ sở hữu | rw-   |
| **Group**  | Quyền của nhóm       | r--   |
| **Others** | Quyền của người khác | r--   |

### Các quyền cơ bản

| Ký hiệu | Quyền   | Ý nghĩa                                                        |
| ------- | ------- | -------------------------------------------------------------- |
| **r**   | Read    | Đọc                                                            |
| **w**   | Write   | Ghi (sửa/xóa)                                                  |
| **x**   | Execute | Thực thi (với chương trình/script) hoặc truy cập (với thư mục) |

Ví dụ: `-rw-r--r--`

- Chủ sở hữu (User): đọc, ghi (`rw-`)
- Nhóm (Group): chỉ đọc (`r--`)
- Người khác (Others): chỉ đọc (`r--`)

---

## 2.6 Thay Đổi Quyền Hạn (chmod và chown)

### 1. chmod (Change Mode)

Dùng để thay đổi quyền hạn.

#### Cách 1: Dùng ký hiệu (Symbolic)

| Ký hiệu | Ý nghĩa    |
| ------- | ---------- |
| **u**   | user       |
| **g**   | group      |
| **o**   | others     |
| **a**   | all        |
| **+**   | thêm quyền |
| **-**   | bỏ quyền   |
| **=**   | gán quyền  |

Ví dụ:

```bash
# Thêm quyền thực thi (x) cho chủ sở hữu (u)
$ chmod u+x script.sh

# Bỏ quyền ghi (w) của nhóm (g) và người khác (o)
$ chmod go-w data.log

# Cấp quyền đọc-ghi cho chủ sở hữu, chỉ đọc cho những người còn lại
$ chmod u=rw,go=r file.txt
```

#### Cách 2: Dùng số (Octal)

| Số  | Quyền (rwx) | Ý nghĩa                    |
| --- | ----------- | -------------------------- |
| 7   | rwx         | Đọc, ghi, thực thi (4+2+1) |
| 6   | rw-         | Đọc, ghi (4+2)             |
| 5   | r-x         | Đọc, thực thi (4+1)        |
| 4   | r--         | Chỉ đọc (4)                |
| 0   | ---         | Không có quyền (0)         |

Ba chữ số tương ứng cho User, Group, và Others.

```bash
# rwx r-x r-x (phổ biến cho script, thư mục)
$ chmod 755 my_script.sh

# rw- r-- r-- (phổ biến cho file dữ liệu)
$ chmod 644 config.txt

# rw- --- --- (chỉ chủ sở hữu được đọc/ghi)
$ chmod 600 private_key.pem
```

### 2. chown (Change Owner)

Dùng để thay đổi chủ sở hữu và nhóm của file (cần sudo).

```bash
# Thay đổi chủ sở hữu file 'data.txt' thành 'admin'
$ sudo chown admin data.txt

# Thay đổi cả chủ sở hữu (admin) và nhóm (staff)
$ sudo chown admin:staff data.txt
```

---

## 2.7 Quản Lý Người Dùng

Tạo và xóa người dùng:

```bash
# Tạo người dùng mới tên 'lisa'
$ sudo adduser lisa

# Thêm 'lisa' vào nhóm 'sudo'
$ sudo usermod -aG sudo lisa

# Xóa người dùng 'lisa'
$ sudo deluser lisa
```

> Lưu ý: `useradd` là lệnh cấp thấp hơn, còn `adduser` thân thiện hơn và tự động tạo thư mục `/home`, shell mặc định, v.v.

---

## 2.8 Bài Tập Thực Hành

```bash
# Cập nhật hệ thống
sudo apt update
sudo apt upgrade

# Tìm và cài đặt một gói tin thú vị
apt search cowsay
sudo apt install cowsay
cowsay "Hello Chapter 2!"

# Dùng nano để tạo file test.txt
nano lab1/test.txt

# Xem quyền hạn của file
ls -l lab1/test.txt

# Chỉ cho phép đọc
chmod 444 lab1/test.txt

# Thử sửa lại bằng nano (sẽ không lưu được)

# Trả lại quyền ban đầu
chmod 644 lab1/test.txt

# Gỡ bỏ gói cowsay
sudo apt remove cowsay
```

---

## 2.9 Kết Luận

Bạn đã học được cách quản lý hệ thống cơ bản: cài đặt phần mềm với `apt`, sử dụng quyền `sudo`, và chỉnh sửa file với `nano`.

Hiểu về quyền hạn `rwx` và lệnh `chmod` là kỹ năng cốt lõi khi làm việc với Linux.

> Ở chương tiếp theo, chúng ta sẽ tìm hiểu về **Shell Scripting cơ bản**, cách tự động hóa các tác vụ và viết các kịch bản (scripts) đầu tiên của bạn.
