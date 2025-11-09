## Chapter 4 – Vòng Lặp, Dịch Vụ và Tác Vụ Tự Động

### 4.1 Mục Đích

Chương này xây dựng dựa trên kiến thức scripting của bạn, tập trung vào việc lặp lại và tự động hóa:

- Hiểu và viết các vòng lặp **for** để duyệt qua danh sách.
- Hiểu và viết các vòng lặp **while** để lặp lại dựa trên điều kiện.
- Làm quen với **systemd** và lệnh **systemctl** để quản lý các dịch vụ (services).
- Học cách lên lịch (schedule) cho các script chạy tự động bằng **cron**.

---

### 4.2 Vòng Lặp for

Vòng lặp `for` rất lý tưởng khi bạn có một danh sách các mục (ví dụ: file, tên, số) và muốn thực hiện cùng một hành động trên từng mục.

#### Cú pháp 1: Lặp qua danh sách

```bash
#!/bin/bash

# Lặp qua một danh sách các chuỗi
for animal in "chó" "mèo" "cá"; do
    echo "Tôi thích $animal"
done
```

#### Cú pháp 2: Lặp qua các tệp tin

Bạn có thể dùng các ký tự đại diện (`*`, `?`) để lặp qua các tệp tin.

```bash
#!/bin/bash

# Lặp qua tất cả các tệp tin .txt trong thư mục hiện tại
for file in *.txt; do
    echo "Đang xử lý tệp $file..."
    cp $file $file.bak
done
```

#### Cú pháp 3: Lặp kiểu C (dùng số)

```bash
#!/bin/bash

# Dùng $((...)) cho các phép toán
for (( i=1; i<=5; i++ )); do
    echo "Số đếm: $i"
done
```

---

### 4.3 Vòng Lặp while

Vòng lặp `while` sẽ tiếp tục chạy miễn là một điều kiện còn ĐÚNG. Nó phù hợp khi bạn không biết trước số lần lặp.

#### Cú pháp cơ bản (Bộ đếm)

```bash
#!/bin/bash

COUNT=1
while [ $COUNT -le 5 ]; do
    echo "Lần lặp while thứ: $COUNT"
    COUNT=$((COUNT + 1))  # Tăng biến đếm
done
```

`$((...))` là cú pháp của shell để thực hiện các phép toán số học.

#### Cú pháp nâng cao: Đọc tệp tin từng dòng

```bash
#!/bin/bash

FILE="test.txt"

# Kiểm tra file có tồn tại không
if [ ! -f "$FILE" ]; then
    echo "Tệp $FILE không tồn tại."
    exit 1
fi

# Dấu < "$FILE" chuyển hướng nội dung của tệp vào vòng lặp while
while read line; do
    echo "Đọc được dòng: $line"
done < "$FILE"
```

---

### 4.4 Quản Lý Dịch Vụ Với systemd

Hầu hết các bản phân phối Linux hiện đại, bao gồm Ubuntu, sử dụng **systemd** để quản lý các dịch vụ (services). Dịch vụ là các chương trình chạy nền (ví dụ: máy chủ web, cơ sở dữ liệu, dịch vụ SSH).

Lệnh chính để tương tác với systemd là **systemctl**. Vì nó thay đổi trạng thái hệ thống, bạn thường phải dùng `sudo`.

| Lệnh                          | Công dụng              |
| ----------------------------- | ---------------------- |
| `sudo systemctl status cron`  | Xem trạng thái dịch vụ |
| `sudo systemctl stop cron`    | Dừng dịch vụ           |
| `sudo systemctl start cron`   | Khởi động dịch vụ      |
| `sudo systemctl restart cron` | Khởi động lại dịch vụ  |

**Kích hoạt dịch vụ khi khởi động:**

| Lệnh                          | Công dụng                          |
| ----------------------------- | ---------------------------------- |
| `sudo systemctl enable cron`  | Tự động chạy khi khởi động         |
| `sudo systemctl disable cron` | Không tự động chạy khi khởi động   |
| `systemctl is-enabled cron`   | Kiểm tra xem có đang bật hay không |

---

### 4.5 Tự Động Hóa Tác Vụ Với cron

`cron` là một dịch vụ (daemon) chạy nền, chuyên dùng để thực thi các lệnh hoặc script theo lịch trình định sẵn. Các lịch trình này được lưu trong tệp **crontab**.

#### Mở và chỉnh sửa crontab:

```bash
$ crontab -e
```

#### Cú pháp crontab

```
phút   giờ   ngày   tháng   thứ_trong_tuần   lệnh_cần_chạy
(0-59) (0-23) (1-31) (1-12)  (0-7)
```

Dấu `*` có nghĩa là “mỗi”.

#### Ví dụ crontab:

```bash
# Chạy mỗi phút
* * * * * echo "Cron đang chạy" >> /home/anh/cron_test.log

# Chạy backup.sh vào 2:30 sáng hàng ngày
30 2 * * * /home/anh/backup.sh

# Chạy lúc 5 giờ chiều Thứ Hai
0 17 * * 1 /home/anh/scripts/weekly_report.sh
```

Lưu ý: `cron` chạy trong môi trường rất tối giản, nên luôn dùng **đường dẫn tuyệt đối**.

---

### 4.6 Bài Tập Thực Hành

1. **Vòng lặp for:** Viết script `create_files.sh` dùng vòng lặp `for ((i=1; i<=5; i++))` để tạo 5 tệp `file1.txt` đến `file5.txt` (gợi ý: `touch "file$i.txt"`).
2. **Vòng lặp while:** Dùng script đọc file ở mục 4.3 để đọc nội dung tệp `test.txt`.
3. **Quản lý dịch vụ:** Dùng `systemctl status ssh` để kiểm tra dịch vụ SSH.
4. **Lên lịch cron:**

   ```bash
   * * * * * /home/user/hello.sh >> /home/user/hello.log
   ```

   - Chờ 2 phút, kiểm tra file `hello.log` có nội dung chưa.
   - Sau đó mở lại crontab và xóa dòng này đi.

---

### 4.7 Kết Luận

- Bạn đã học cách kiểm soát luồng thực thi bằng vòng lặp `for` và `while`.
- Bạn biết quản lý dịch vụ bằng `systemctl`.
- Bạn đã tự động hóa tác vụ bằng `cron`.

Chương tiếp theo sẽ đi vào **quản lý mạng (networking)**, **quản lý ổ đĩa (disk management)**, và **nén/giải nén tệp tin**.
