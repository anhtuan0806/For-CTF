# Chương 6 — Lô-gíc và Vòng Lặp

Chào mừng bạn đến với phần **trung cấp** của series Bash! Ở 5 chương trước, bạn đã học các lệnh cơ bản và viết script đơn giản. Giờ là lúc làm cho script thông minh hơn bằng **if/elif/else**, **for** và **while**.

---

## 1. Câu lệnh `if` (Ra quyết định)

Cú pháp cơ bản:

```bash
if [ <điều_kiện> ]; then
    # làm khi đúng
elif [ <điều_kiện_khác> ]; then
    # làm khi điều kiện khác đúng
else
    # làm khi mọi thứ sai
fi
```

**Lưu ý:** phải có dấu cách trong `[` `]`, và dùng `fi` để kết thúc.

### Các phép kiểm tra phổ biến

**Kiểm tra file/thư mục**

- `[ -f "/path/to/file" ]` — là file
- `[ -d "/path/to/dir" ]` — là thư mục
- `[ -e "/path/to/file" ]` — tồn tại
- `[ -r "/path/to/file" ]` — có quyền đọc
- `[ -w "/path/to/file" ]` — có quyền ghi
- `[ -x "/path/to/file" ]` — có quyền thực thi

**Kiểm tra chuỗi**

- `[ "$str1" == "$str2" ]` — bằng nhau
- `[ "$str1" != "$str2" ]` — khác nhau
- `[ -z "$str" ]` — chuỗi rỗng
- `[ -n "$str" ]` — chuỗi không rỗng

**Kiểm tra số (integer)**

- `[ $num1 -eq $num2 ]` — bằng
- `[ $num1 -ne $num2 ]` — khác
- `[ $num1 -gt $num2 ]` — lớn hơn
- `[ $num1 -lt $num2 ]` — nhỏ hơn
- `[ $num1 -ge $num2 ]` — lớn hơn hoặc bằng
- `[ $num1 -le $num2 ]` — nhỏ hơn hoặc bằng

### Ví dụ script (kiểm tra file/dir)

```bash
#!/bin/bash
TARGET=$1
if [ -z "$TARGET" ]; then
    echo "Loi: Ban phai nhap ten file/thu muc!"
    exit 1
fi

if [ -f "$TARGET" ]; then
    echo "$TARGET la mot file."
    file "$TARGET"
elif [ -d "$TARGET" ]; then
    echo "$TARGET la mot thu muc."
    ls -la "$TARGET"
else
    echo "$TARGET khong ton tai hoac la loai file khac."
fi
```

---

## 2. Vòng lặp `for` (Lặp qua danh sách)

**Cú pháp 1 — lặp qua danh sách:**

```bash
for bien in item1 item2 item3; do
    echo "Muc hien tai: $bien"
done
```

**Ví dụ — ping nhiều host:**

```bash
HOSTS="google.com facebook.com 8.8.8.8"
for host in $HOSTS; do
    echo "--- DANG PING $host ---"
    ping -c 1 $host
    echo "--------------------------"
done
```

**Cú pháp 2 — lặp qua file bằng wildcard:**

```bash
for file in *.txt; do
    echo "Tim password trong file $file:"
    grep -i "password" "$file"
done
```

**Cú pháp 3 — for kiểu C (đếm):**

```bash
for (( i=1; i<=5; i++ )); do
    echo "Lap lan thu: $i"
done
```

**Ví dụ CTF (fuzzing):**

```bash
URL="http://victim.com/api/user.php?id="
for (( id=1; id<=100; id++ )); do
    echo "Dang thu User ID: $id"
    curl "$URL$id"
done
```

---

## 3. Vòng lặp `while` (Lặp khi điều kiện đúng)

**Cú pháp cơ bản:**

```bash
i=1
while [ $i -le 5 ]; do
    echo "While loop lan: $i"
    i=$((i + 1))
done
```

**Đọc file dòng từng dòng (vô cùng hữu dụng trong CTF):**

```bash
FILE="hosts.txt"
while read -r dong; do
    echo "--- DANG PING $dong ---"
    ping -c 1 "$dong"
done < "$FILE"
```

Giải thích: `read -r` đọc từng dòng, `-r` giữ nguyên dấu `\` nếu có. Dấu `< "$FILE"` chuyển nội dung file vào stdin cho vòng lặp.

---

## 4. Bài tập thực hành

1. Viết `check_root.sh` dùng `if` để kiểm tra `$USER`:

   - Nếu `$USER` là `root`, in: `Ban la ROOT! (Quyen luc toi thuong)`.
   - Ngược lại, in: `Ban la nguoi dung thuong ($USER)`.

2. Viết `fuzzer.sh` dùng `for` (kiểu C) lặp từ 1 đến 10 và dùng `curl` để truy cập `http://httpbin.org/status/[so]`.

3. Tạo file `wordlist.txt` chứa 3–4 mật khẩu (mỗi dòng 1 password).

   - Viết `read_wordlist.sh` dùng `while read` để đọc từng dòng và in `Dang thu password: [password]`.

---

### Kết thúc Chương 6

Bạn đã nắm được xương sống của lập trình kịch bản: `if`, `for`, và `while`. Script giờ đã có thể **ra quyết định** và **tự động hoá** các tác vụ lặp lại — rất cần cho các bài tập CTF.

Chương tiếp theo sẽ là **Chương 7: Xử lý văn bản nâng cao (cut, sort, uniq, awk, sed)**.
