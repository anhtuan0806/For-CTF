# Chương 7 — Xử lý Văn bản Nâng cao

Trong CTF, dữ liệu thường ở dạng thô: log, output của tool, file config... Chương này dạy bạn cách **cắt**, **sắp xếp** và **biến đổi** văn bản để lọc nhiễu và tìm thông tin quan trọng.

---

## 1. `cut` — Cắt theo cột

`cut` trích xuất cột từ text. Cần chỉ ra:

- `-d` : delimiter (dấu phân cách)
- `-f` : field (cột) muốn lấy

**Ví dụ:** lấy danh sách user từ `/etc/passwd`:

```bash
cat /etc/passwd | cut -d ':' -f 1
```

Lấy nhiều cột:

```bash
cat /etc/passwd | cut -d ':' -f 1,6
```

---

## 2. `sort` — Sắp xếp

Sắp xếp các dòng trong file.

**Ví dụ:**

```bash
sort list.txt
```

Tùy chọn hữu ích:

- `-n` : numeric sort
- `-r` : reverse
- `-k` : chỉ định cột

---

## 3. `uniq` — Loại bỏ trùng lặp

`uniq` lọc các dòng liền kề trùng nhau — **phải `sort` trước** để hoạt động đúng.

**Ví dụ:**

```bash
sort pass.txt | uniq > clean_pass.txt
```

Tùy chọn `-c` đếm số lần xuất hiện:

```bash
sort ip_list.txt | uniq -c | sort -nr
```

---

## 4. `awk` — Con dao Thụy Sĩ

`awk` là ngôn ngữ mini để xử lý text. Cú pháp:

```bash
awk 'pattern { action }' file
```

Biến đặc biệt:

- `$0`: toàn bộ dòng
- `$1`, `$2`, ...: cột (mặc định delimiter là khoảng trắng)
- `-F` : chỉ định delimiter

**Ví dụ:** thay `cut` lấy user và home:

```bash
awk -F ':' '{ print $1, $6 }' /etc/passwd
```

In các user có shell `/bin/bash`:

```bash
awk -F ':' '$7 == "/bin/bash" { print $1 }' /etc/passwd
```

---

## 5. `sed` — Stream Editor (Tìm & Thay thế)

`sed` chỉnh sửa dòng text theo luồng. Cú pháp thay thế:

```bash
sed 's/find/replace/g' file
```

**Ví dụ:** thêm dấu `!` vào cuối mỗi dòng:

```bash
sed 's/$/!/g' pass.txt
```

Dọn output phức tạp bằng chuỗi `sed` nối nhau.

---

## 6. Pipeline Thần Thánh

Kết hợp nhiều công cụ để xử lý log lớn. Ví dụ: tìm 5 URL được truy cập nhiều nhất bởi IP `1.2.3.4`:

```bash
cat access.log | \
  grep "1.2.3.4" | \
  awk '{ print $7 }' | \
  sort | \
  uniq -c | \
  sort -nr | \
  head -n 5
```

Giải thích từng bước: lọc, lấy cột, sắp xếp, đếm, sắp xếp theo số, lấy 5 dòng đầu.

---

## 7. Bài tập thực hành

1. `ls -l | awk '{ print $9, $5 }'` — in ra tên file (cột 9) và kích thước (cột 5).
2. `history | awk '{ print $2 }'` — lấy các lệnh đã gõ (bỏ số thứ tự).
3. Kết hợp: `history | awk '{ print $2 }' | sort | uniq -c | sort -nr | head -n 10` — 10 lệnh dùng nhiều nhất.
4. Tạo `email.txt` với: `user@test.com`, `admin@gmail.com`, `support@test.com`.

   - `cat email.txt | cut -d '@' -f 2` — trích xuất domain.
   - `sed 's/test.com/production.com/g' email.txt` — thay `test.com` thành `production.com`.

---

### Kết thúc Chương 7

Bạn giờ đã có bộ công cụ xử lý văn bản: `cut`, `sort`, `uniq`, `awk`, `sed`. Đây là những công cụ không thể thiếu khi làm việc với log và output trong CTF.

Chương 8 sẽ nói về **Functions** và **Quản lý lỗi** trong script — muốn mình thêm trước phần `xargs` hoặc ví dụ `awk` phức tạp không?
