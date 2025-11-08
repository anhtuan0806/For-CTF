# Chapter 1 – Giới Thiệu Tổng Quan Về Ubuntu

## 1.1 Mục Đích

Chương này giúp bạn:

- Hiểu được khái niệm cơ bản về hệ điều hành Ubuntu và Linux.
- Làm quen với môi trường dòng lệnh (Command Line Interface – CLI).
- Thực hành các lệnh cơ bản để thao tác với tệp, thư mục và tiến trình.
- Biết cách tìm tài liệu và trợ giúp khi cần.

## 1.2 Giới Thiệu Về Ubuntu Và Môi Trường Dòng Lệnh

Ubuntu là một bản phân phối phổ biến của **Linux**, được phát triển dựa trên nền tảng Debian. Nó thân thiện, dễ dùng, và được sử dụng rộng rãi cho cả người mới học lẫn chuyên gia.

Linux là hệ điều hành **UNIX-like**, tức là hoạt động tương tự UNIX. Điều này có nghĩa là:

> Tất cả các lệnh trong UNIX hoạt động gần như y hệt trong Linux.

Ubuntu có hai cách sử dụng chính:

- **Giao diện đồ họa (GUI)**: sử dụng chuột và biểu tượng.
- **Dòng lệnh (CLI)**: sử dụng cửa sổ Terminal để gõ lệnh.

Ví dụ: mở Terminal trong Ubuntu bằng một trong các cách sau:

- Nhấn **Ctrl + Alt + T**.
- Mở menu **Applications → Accessories → Terminal**.

Khi Terminal mở ra, bạn sẽ thấy một dấu nhắc lệnh (prompt):

```
user@ubuntu:~$
```

Trong đó:

- `user` là tên người dùng.
- `ubuntu` là tên máy.
- `~` là ký hiệu cho thư mục _home_ của bạn.

---

## 1.3 Đăng Nhập Và Bắt Đầu Phiên Làm Việc

Khi khởi động Ubuntu ở chế độ CLI, bạn cần đăng nhập bằng tài khoản:

```
Ubuntu 24.04 LTS
login: anh
Password: ********
```

Sau khi đăng nhập thành công, bạn sẽ thấy dấu nhắc shell. Từ đây bạn có thể nhập lệnh để thao tác.

Ví dụ:

```
$ date
Sat Nov 9 10:25:14 +07 2025
$ whoami
anh
```

Lệnh `date` hiển thị thời gian hiện tại và `whoami` cho biết bạn đang đăng nhập với người dùng nào.

---

## 1.4 Shell – Trình Thông Dịch Lệnh

**Shell** là chương trình nhận lệnh bạn nhập và thực thi chúng. Ubuntu thường sử dụng **bash** (Bourne Again Shell) làm shell mặc định.

### Một số shell phổ biến

| Tên Shell | Đặc điểm                                      |
| --------- | --------------------------------------------- |
| bash      | phổ biến nhất, mặc định trên Ubuntu           |
| zsh       | mạnh, có tính năng tự động hoàn thành tốt hơn |
| fish      | dễ dùng, có màu và gợi ý tự động              |

### Ví dụ sử dụng shell

```bash
$ echo "Xin chào Ubuntu!"
Xin chào Ubuntu!

$ cal 11 2025
    November 2025
Su Mo Tu We Th Fr Sa
                   1
 2  3  4  5  6  7  8
 9 10 11 12 13 14 15
16 17 18 19 20 21 22
23 24 25 26 27 28 29
30
```

Lệnh `echo` in ra thông báo, còn `cal` hiển thị lịch tháng.

---

## 1.5 Hệ Thống Thư Mục Và File

Linux tổ chức file theo cấu trúc cây, bắt đầu từ thư mục gốc `/`.

### Ví dụ cấu trúc:

```
/
├── bin        → chứa các chương trình cơ bản như ls, cp, mv
├── home       → thư mục của người dùng
│   ├── anh
│   └── admin
├── etc        → chứa file cấu hình hệ thống
├── var        → chứa dữ liệu thay đổi (log, cache, ...)
└── tmp        → thư mục tạm thời
```

### Một số lệnh cơ bản

```bash
$ pwd              # Hiển thị thư mục hiện tại
/home/anh

$ ls               # Liệt kê nội dung thư mục
Documents  Downloads  Pictures

$ cd Downloads     # Chuyển vào thư mục Downloads
$ mkdir test       # Tạo thư mục mới tên test
$ rmdir test       # Xóa thư mục rỗng tên test
```

> Lưu ý: trong Linux, đường dẫn phân biệt chữ hoa và chữ thường (`File.txt` khác `file.txt`).

---

## 1.6 Quản Lý Tệp Tin Và Thư Mục

| Lệnh    | Công dụng                 | Ví dụ                          |
| ------- | ------------------------- | ------------------------------ |
| `cp`    | Sao chép file             | `cp a.txt b.txt`               |
| `touch` | Tạo file                  | `touch a.txt`                  |
| `mv`    | Di chuyển hoặc đổi tên    | `mv b.txt /home/anh/Documents` |
| `rm`    | Xóa file                  | `rm b.txt`                     |
| `diff`  | So sánh nội dung hai file | `diff file1.txt file2.txt`     |
| `grep`  | Tìm chuỗi trong file      | `grep "Ubuntu" notes.txt`      |

Ví dụ:

```bash
$ cp hello.txt ~/Documents
$ ls ~/Documents
hello.txt

$ grep "linux" hello.txt
Ubuntu là một hệ điều hành Linux phổ biến.
```

> ⚠️ **Cẩn thận khi dùng `rm`** – file bị xóa sẽ không có trong thùng rác.

---

## 1.7 Quản Lý Tiến Trình (Processes)

Trong Linux, mỗi chương trình đang chạy được gọi là một **process** (tiến trình).

### Một số lệnh thường dùng

```bash
$ ps          # Hiển thị các tiến trình đang chạy
$ top         # Theo dõi tiến trình theo thời gian thực
$ kill <PID>  # Dừng tiến trình có mã PID cụ thể
```

Ví dụ:

```bash
$ ps
PID TTY          TIME CMD
1021 pts/0    00:00:00 bash
1203 pts/0    00:00:00 vim
1210 pts/0    00:00:00 ps
```

Muốn kết thúc tiến trình `vim`, ta dùng:

```bash
$ kill 1203
```

Hoặc dùng tổ hợp **Ctrl + C** nếu tiến trình đang chạy foreground.

---

## 1.8 Trợ Giúp Và Tài Liệu

Linux có hệ thống tài liệu rất mạnh mẽ.

### 1. Dùng lệnh `man`

```bash
$ man ls
```

Hiển thị hướng dẫn chi tiết về lệnh `ls`.

- Nhấn **Space** để cuộn xuống.
- Nhấn **q** để thoát.

### 2. Dùng lệnh `info`

```bash
$ info ls
```

Phiên bản mở rộng hơn của `man`, có thể điều hướng bằng phím mũi tên.

### 3. Đọc tài liệu hệ thống

Các hướng dẫn (HOWTOs) nằm trong:

```
/usr/share/doc/howto/en
```

hoặc bạn có thể xem online tại [Ubuntu Manpages](https://manpages.ubuntu.com/).

---

## 1.9 Bài Tập Thực Hành

1. Mở Terminal và chạy các lệnh sau. Ghi lại kết quả:

   ```bash
   whoami
   pwd
   ls -l
   date
   cal
   ```

2. Tạo một thư mục tên `lab1` trong thư mục home, tạo file `note.txt` và chép sang `Documents`.
3. Dùng `grep` để tìm từ "Ubuntu" trong file `note.txt`.
4. Mở hai terminal song song, chạy `top` trong một và `ps` trong một để quan sát tiến trình.
5. Thực hành `man` và `info` với các lệnh `ls`, `cd`, `rm`.

---

## 1.10 Kết Luận

- Ubuntu là nền tảng mạnh mẽ và linh hoạt cho cả học tập và phát triển phần mềm.
- Biết cách thao tác với dòng lệnh giúp bạn kiểm soát hệ thống hiệu quả hơn.
- Ở chương tiếp theo, bạn sẽ học về **cài đặt phần mềm, quản lý gói và cấu hình hệ thống cơ bản**.
