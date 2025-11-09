# Chương 3 — Pipe, Chuyển Hướng & Tiến Trình

Trong chương này bạn sẽ học cách **kết hợp lệnh** (pipes), **chuyển hướng I/O**, và **quản lý tiến trình** — những kỹ năng quan trọng để xây dựng các “dây chuyền” xử lý trong CTF.

---

## 1. Chuyển hướng (Redirection)

Linux có 3 luồng chính:

- **stdin (0)** — nhập (thường là bàn phím)
- **stdout (1)** — đầu ra chuẩn (kết quả thành công)
- **stderr (2)** — đầu ra lỗi (thông báo lỗi)

**Ghi đè (>)** — Ghi stdout vào file (xóa nội dung cũ):

```bash
ls -la /etc > etc_listing.txt
```

**Ghi nối (>>) — Append**:

```bash
echo "Dòng đầu" >> output.txt
```

**Chuyển hướng lỗi (2>)** — Ghi stderr vào file:

```bash
find / -name "*flag*" 2> errors.log
```

**2>/dev/null** — Vứt bỏ lỗi (lỗ đen):

```bash
find / -name "*flag*" 2>/dev/null
```

**&>** — Gộp cả stdout và stderr vào cùng file:

```bash
find / -name "*flag*" &> all_results.txt
```

---

## 2. Ống (Pipe `|`)

Pipe truyền stdout của lệnh trái thành stdin của lệnh phải — cách để ghép lệnh lại thành chuỗi xử lý.

Ví dụ:

```bash
history | grep "find"
ps aux | grep "apache"
cat /var/log/syslog | grep "error"
ls /bin | wc -l   # đếm số file trong /bin
```

**Tip CTF:** Dùng pipe để lọc danh sách file/tiến trình/logs rồi xử lý tiếp (ví dụ sort, uniq, head).

---

## 3. Quản lý tiến trình (Process Management)

**ps aux** — xem tiến trình đang chạy:

```bash
ps aux
```

Các cột quan trọng: `USER`, `PID`, `%CPU`, `%MEM`, `COMMAND`.

**kill** — gửi tín hiệu dừng tới PID:

```bash
kill 12345       # gửi SIGTERM (cho tiến trình dọn dẹp rồi chết)
kill -9 12345    # gửi SIGKILL (ép buộc, không dọn dẹp)
```

**Chạy nền (&)** — chạy tiến trình trong background để tiếp tục dùng terminal:

```bash
./heavy_scan.sh &
# Shell trả về: [job_num] PID
```

**Ví dụ thực tế CTF:** chạy listener (nc -lvp 4444) hoặc reverse-shell handler trong nền, còn bạn làm việc khác trên terminal.

---

## 4. Bài tập thực hành

1. Gõ `ls -la /` và chuyển hướng stdout vào `root_fs.txt`.
2. Dùng `echo` để nối dòng `--- GHI CHU CUA TOI ---` vào cuối `root_fs.txt`.
3. Dùng `ls /etc | grep conf` để liệt kê các file/thư mục chứa "conf".
4. Đếm số file trong `/etc`:

```bash
find /etc -type f 2>/dev/null | wc -l
```

5. Mở terminal, chạy `sleep 1000`.
6. Ở terminal khác, tìm PID của `sleep` bằng `ps aux | grep "sleep"`.
7. Dùng `kill` để dừng tiến trình `sleep` và quan sát terminal đầu tiên (sẽ in `Terminated`).

---

### Kết thúc Chương 3

Bạn đã nắm được cách **điều hướng dữ liệu** qua file và qua các lệnh, cùng cách kiểm soát tiến trình đang chạy. Chương 4 sẽ đưa bạn ra mạng: `ping`, `ifconfig`/`ip`, `curl`, `wget` và các kỹ thuật tải/tương tác mạng cơ bản trong CTF.

---
