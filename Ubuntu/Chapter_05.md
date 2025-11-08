# Chapter 5 – Mạng, Ổ Đĩa và Nén Tệp

## 5.1 Mục Đích

Chương này sẽ trang bị cho bạn các kỹ năng thiết yếu để quản lý các tài nguyên cốt lõi của hệ thống:

- Kiểm tra cấu hình mạng và chẩn đoán các sự cố kết nối cơ bản.
- Giám sát dung lượng ổ đĩa và xem tệp tin nào đang chiếm dung lượng.
- Nén (lưu trữ) và giải nén các tệp tin bằng các công cụ dòng lệnh phổ biến như **tar** và **zip**.

---

## 5.2 Quản Lý Mạng (Networking)

Đây là các lệnh cơ bản để xem hệ thống của bạn đang kết nối với mạng như thế nào.

### 1. `ip` – Lệnh quản lý mạng chính

Lệnh `ip` là công cụ hiện đại thay thế cho các lệnh cũ như `ifconfig` và `route`.

```bash
# Hiển thị tất cả các giao diện mạng và địa chỉ IP
$ ip address show
# (Hoặc viết tắt)
$ ip a
```

Ví dụ:

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc ...
    inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic ...
       valid_lft 85652sec preferred_lft 85652sec
```

- **lo**: Giao diện loopback (chính nó), luôn là `127.0.0.1`.
- **ens33**: Giao diện mạng vật lý (tên có thể khác), có IP là `192.168.1.100`.

### 2. `ping` – Kiểm tra kết nối

Dùng để kiểm tra xem bạn có thể kết nối đến một máy chủ khác không.

```bash
# Ping đến máy chủ DNS của Google
$ ping 8.8.8.8
```

Kết quả mẫu:

```
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=118 time=10.5 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=118 time=10.2 ms
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss
```

Bạn cũng có thể ping bằng tên miền:

```bash
$ ping google.com
```

Nhấn **Ctrl + C** để dừng lệnh `ping`. Nếu `0% packet loss` nghĩa là kết nối tốt.

### 3. `dig` hoặc `nslookup` – Kiểm tra DNS

Dùng để kiểm tra phân giải tên miền (DNS): xem một tên miền được dịch sang địa chỉ IP nào.

```bash
$ dig google.com
```

Kết quả ví dụ:

```
; <<>> DiG 9.18.18-0ubuntu0.24.04.1-Ubuntu <<>> google.com
;; ANSWER SECTION:
google.com.             111     IN      A       142.250.204.14
```

Lệnh `nslookup google.com` cũng cho kết quả tương tự và đơn giản hơn.

---

## 5.3 Quản Lý Ổ Đĩa

Biết được ổ đĩa còn trống bao nhiêu và tệp nào đang chiếm dung lượng là rất quan trọng.

### 1. `df` (Disk Free)

Hiển thị dung lượng trống và đã dùng của các hệ thống tệp.

```bash
# -h (human-readable): hiển thị theo đơn vị dễ đọc (KB, MB, GB)
$ df -h
```

Ví dụ:

```
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           1.6G  1.8M  1.6G   1% /run
/dev/sda1        50G   20G   28G  42% /
tmpfs           7.8G     0  7.8G   0% /dev/shm
```

Dòng quan trọng nhất thường là `/` (thư mục gốc), cho thấy ổ đĩa chính của bạn đã dùng **42% (20GB / 50GB)**.

### 2. `du` (Disk Usage)

Ước tính dung lượng mà các tệp tin và thư mục đang sử dụng.

```bash
# -s: chỉ hiển thị tổng dung lượng
# -h: hiển thị dễ đọc
# * : áp dụng cho tất cả các mục trong thư mục hiện tại
$ du -sh *
```

Ví dụ:

```
4.0K    Documents
1.2G    Downloads
350M    Pictures
8.0K    lab1
```

Lệnh này rất hữu ích để tìm xem thư mục nào chiếm nhiều dung lượng nhất.

### 3. `lsblk` (List Block Devices)

Liệt kê các thiết bị khối (ổ đĩa và phân vùng) dưới dạng cây.

```bash
$ lsblk
```

Kết quả ví dụ:

```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0   50G  0 disk
├─sda1   8:1    0   49G  0 part /
└─sda2   8:2    0    1G  0 part [SWAP]
sr0     11:0    1 1024M  0 rom
```

Cho thấy bạn có một ổ đĩa `sda` (50GB) được chia làm hai phân vùng `sda1` (cho hệ thống `/`) và `sda2` (cho SWAP).

---

## 5.4 Nén và Giải Nén Tệp Tin

Nén tệp tin (**archiving**) giúp gộp nhiều tệp thành một và giảm dung lượng lưu trữ, rất hữu ích khi sao lưu hoặc gửi file.

### 1. `tar` (Tape Archive)

`tar` là công cụ tiêu chuẩn của Linux/UNIX. Bản thân nó chỉ gộp tệp tin; nó cần các tùy chọn `-z` (gzip) hoặc `-j` (bzip2) để nén.

| Tùy chọn | Ý nghĩa                              |
| -------- | ------------------------------------ |
| `c`      | Tạo tệp lưu trữ mới                  |
| `x`      | Giải nén                             |
| `v`      | Hiển thị quá trình                   |
| `f`      | Chỉ định tên tệp lưu trữ (bắt buộc)  |
| `z`      | Nén/giải nén bằng gzip (`.tar.gz`)   |
| `j`      | Nén/giải nén bằng bzip2 (`.tar.bz2`) |

#### Ví dụ thao tác:

| Thao tác                                    | Lệnh                          |
| ------------------------------------------- | ----------------------------- |
| Nén thư mục `lab1` thành `lab1.tar.gz`      | `tar -zcvf lab1.tar.gz lab1/` |
| Giải nén tệp `lab1.tar.gz`                  | `tar -zxvf lab1.tar.gz`       |
| Xem nội dung `lab1.tar.gz` (không giải nén) | `tar -ztvf lab1.tar.gz`       |

### 2. `zip` và `unzip`

Nếu bạn làm việc với người dùng Windows/macOS, `zip` là định dạng rất phổ biến.

```bash
# Cài đặt (nếu chưa có)
$ sudo apt install zip unzip

# Nén thư mục 'Documents' thành 'docs.zip'
# -r: bao gồm cả các thư mục con
$ zip -r docs.zip Documents/

# Giải nén tệp 'docs.zip'
$ unzip docs.zip
```

---

## 5.5 Bài Tập Thực Hành

1. Dùng `ip a` để tìm địa chỉ IP của máy bạn.
2. Dùng `ping` để kiểm tra kết nối tới `vnexpress.net`.
3. Dùng `df -h` để xem ổ đĩa `/` còn trống bao nhiêu %.
4. Vào thư mục `/home`, dùng `du -sh *` để xem thư mục nào chiếm nhiều dung lượng nhất.
5. Dùng `tar` để nén thư mục `lab1` thành `lab1_backup.tar.gz`.
6. Tạo thư mục `test_unzip`, di chuyển `lab1_backup.tar.gz` vào đó và giải nén.

---

## 5.6 Kết Luận

Bạn đã học được các lệnh cơ bản để:

- Chẩn đoán mạng (`ip`, `ping`)
- Quản lý dung lượng ổ đĩa (`df`, `du`)
- Lưu trữ và nén tệp tin (`tar`, `zip`)

Những công cụ này là nền tảng cho việc quản trị và bảo trì bất kỳ hệ thống Linux nào.

Ở chương tiếp theo, chúng ta sẽ xem xét các chủ đề nâng cao hơn — bao gồm an ninh cơ bản, quản lý tường lửa (**firewall**), và các bước tiếp theo để bạn tiếp tục hành trình Linux của mình.
