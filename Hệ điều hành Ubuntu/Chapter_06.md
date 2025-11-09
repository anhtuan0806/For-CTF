# Chapter 6 – An Ninh, Tường Lửa và Các Bước Tiếp Theo

## 6.1 Mục Đích

Chào mừng bạn đến với chương cuối cùng trong loạt bài hướng dẫn về Ubuntu. Chương này sẽ giới thiệu các khái niệm cơ bản nhưng cực kỳ quan trọng về bảo mật:

- Hiểu các nguyên tắc an ninh cơ bản.
- Làm quen và sử dụng **ufw (Uncomplicated Firewall)** để quản lý tường lửa.
- Biết cách đọc các tệp nhật ký (log) hệ thống cơ bản bằng **journalctl**.
- Tìm hiểu về các tài nguyên và định hướng cho hành trình Linux tiếp theo của bạn.

---

## 6.2 Các Nguyên Tắc An Ninh Cơ Bản

Bảo mật không phải là một hành động đơn lẻ, mà là một quá trình liên tục. Dưới đây là những điều cơ bản nhất bạn nên làm.

### 1. Luôn Cập Nhật Hệ Thống

Đây là điều quan trọng nhất. Các bản cập nhật không chỉ mang lại tính năng mới mà còn vá các lỗ hổng bảo mật đã biết.

```bash
# Luôn chạy hai lệnh này định kỳ (ví dụ: hàng tuần)
sudo apt update
sudo apt upgrade
```

### 2. Quản Lý Dịch Vụ SSH

SSH (Secure Shell) là dịch vụ cho phép bạn đăng nhập vào Terminal của máy từ xa (như đã đề cập ở Bài tập 4.6). Hầu hết các máy chủ Linux đều bật dịch vụ này.

- **Kiểm tra trạng thái:** `sudo systemctl status ssh` (hoặc `sshd`).
- **Bảo mật:** Trong môi trường sản xuất, người ta thường **tắt đăng nhập bằng mật khẩu** và chỉ cho phép đăng nhập bằng **SSH Keys** để tăng cường bảo mật.

### 3. Nguyên Tắc Đặc Quyền Tối Thiểu

Không bao giờ chạy các lệnh thông thường với `sudo` nếu không cần thiết. Chỉ sử dụng `sudo` khi thực sự cần quyền quản trị. Điều này giảm thiểu thiệt hại nếu bạn gõ nhầm một lệnh.

---

## 6.3 Quản Lý Tường Lửa Với ufw

Tường lửa (Firewall) là lớp bảo vệ kiểm soát lưu lượng mạng (traffic) đi vào và ra khỏi máy của bạn. **ufw** (Uncomplicated Firewall) là công cụ mặc định, thân thiện của Ubuntu.

ufw hoạt động dựa trên các quy tắc (rules). Theo mặc định, nó ở trạng thái **tắt**.

### 1. Kiểm Tra Trạng Thái

```bash
sudo ufw status
```

_Output:_ `Status: inactive`

### 2. Bật ufw và Đặt Quy Tắc Mặc Định

Một chính sách tốt là: **Chặn tất cả kết nối đến**, **Cho phép tất cả kết nối đi**.

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### 3. Cho Phép Các Kết Nối Cần Thiết

Trước khi bật ufw, bạn **phải** cho phép các dịch vụ quan trọng, nếu không có thể bị khóa ngoài (đặc biệt là SSH).

```bash
sudo ufw allow ssh         # Cho phép SSH (cổng 22)
sudo ufw allow http        # Cho phép HTTP (cổng 80)
sudo ufw allow https       # Cho phép HTTPS (cổng 443)
sudo ufw allow 8080        # Cho phép cổng cụ thể 8080
```

### 4. Kích Hoạt ufw

```bash
sudo ufw enable
# Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
```

_Output:_ `Firewall is active and enabled on system startup`

### 5. Quản Lý ufw

```bash
sudo ufw status numbered     # Xem trạng thái và quy tắc (dạng số)
sudo ufw delete 3            # Xóa quy tắc số 3
sudo ufw disable             # Tắt ufw
```

---

## 6.4 Đọc Nhật Ký (Log) Với journalctl

Mọi hoạt động của hệ thống đều được ghi lại trong **log**. `journalctl` là công cụ để xem chúng.

```bash
journalctl              # Hiển thị tất cả log (nhấn 'q' để thoát)
journalctl -f           # Theo dõi log theo thời gian thực
journalctl -u cron.service   # Log của dịch vụ cron
journalctl -u ssh.service    # Log của dịch vụ ssh
journalctl -b           # Log từ lần khởi động gần nhất
journalctl -k           # Log lỗi của kernel
```

---

## 6.5 Hành Trình Tiếp Theo Của Bạn

Chúc mừng! Bạn đã hoàn thành phần nền tảng vững chắc nhất của Ubuntu. Giờ là lúc mở rộng:

- **Luyện tập Shell Scripting:** Tự động hóa các tác vụ lặp lại bằng vòng lặp, điều kiện và cron.
- **Quản trị máy chủ:** Cài đặt và cấu hình máy chủ web (Nginx, Apache) hoặc cơ sở dữ liệu (PostgreSQL, MySQL).
- **Containers:** Làm quen Docker và Kubernetes – công nghệ trung tâm trong phát triển hiện đại.
- **Cloud:** Áp dụng Linux trên AWS, GCP hoặc Azure.

### Nơi Tìm Trợ Giúp

- `man` và `info`: Tài liệu chính thống đi kèm hệ thống.
- [Ask Ubuntu](https://askubuntu.com): Cộng đồng hỏi đáp lớn nhất về Ubuntu.
- [Arch Wiki](https://wiki.archlinux.org): Kho tài liệu Linux toàn diện nhất.

---

## 6.6 Bài Tập Thực Hành

1. Cập nhật hệ thống:

   ```bash
   sudo apt update && sudo apt upgrade
   ```

2. Kiểm tra trạng thái ufw:

   ```bash
   sudo ufw status
   ```

3. Thêm quy tắc cho phép SSH:

   ```bash
   sudo ufw allow ssh
   ```

4. Bật ufw:

   ```bash
   sudo ufw enable
   ```

5. Kiểm tra lại quy tắc:

   ```bash
   sudo ufw status verbose
   ```

6. Xem 50 dòng log cuối cùng của cron:

   ```bash
   journalctl -u cron.service -n 50
   ```

---

## 6.7 Lời Kết

Bạn đã đi từ những lệnh cơ bản như `ls` và `cd` đến cấu hình tường lửa và đọc log hệ thống. Sức mạnh của Linux nằm ở dòng lệnh — và giờ bạn đã nắm chìa khóa để làm chủ nó.

> **Hãy tiếp tục tò mò, thử nghiệm (trong máy ảo!), và học hỏi.**
