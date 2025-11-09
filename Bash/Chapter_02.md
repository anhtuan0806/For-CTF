# Chương 2 — Tìm Kiếm và Quyền Truy Cập

Trong **CTF**, file "flag" hoặc file cấu hình quan trọng thường bị giấu sâu trong hệ thống. Chương này dạy bạn cách _săn_ file và hiểu về _quyền truy cập_ — rào cản chính cần vượt qua.

---

## 1. `find` — Thợ săn file

Lệnh `find` dùng để tìm file hoặc thư mục dựa trên tiêu chí bạn đưa ra.

**Cú pháp:**

```
find [nơi_bắt_đầu_tìm] [tiêu_chí]
```

**Tiêu chí (options) phổ biến:**

- `-name "tên"`: Tìm theo tên (wildcard `*` được hỗ trợ).
- `-iname "tên"`: Giống `-name` nhưng không phân biệt hoa thường.
- `-type f`: Chỉ tìm **file**.
- `-type d`: Chỉ tìm **thư mục**.
- `-user <tên_user>`: Tìm file thuộc sở hữu user.
- `-perm <quyền>`: Tìm file có permission cụ thể.

**Ví dụ:**

```bash
# Tìm mọi file có tên chứa "flag" từ thư mục gốc
find / -name "*flag*"

# Bỏ qua lỗi Permission denied
find / -name "*flag*" 2>/dev/null

# Tìm file có bit SUID (rất quan trọng trong CTF)
find / -perm -u=s -type f 2>/dev/null
```

> **Ghi chú:** `2>/dev/null` chuyển hướng _standard error_ (mã 2) vào /dev/null — tức là giấu thông báo lỗi.

---

## 2. `grep` — Thợ săn nội dung

`grep` tìm **nội dung** trong file.

**Cú pháp:**

```
grep "chuỗi_cần_tìm" [file | thư_mục]
```

**Tùy chọn hữu ích:**

- `-i`: Không phân biệt hoa thường.
- `-r` hoặc `-R`: Tìm đệ quy trong thư mục.
- `-n`: Hiển thị số dòng.

**Ví dụ:**

```bash
# Tìm chữ "password" trong /etc (không phân biệt hoa thường)
grep -ir "password" /etc 2>/dev/null

# Kết hợp với pipe
history | grep "find"
```

---

## 3. Quyền truy cập (File Permissions)

Khi chạy `ls -la`, bạn thấy cột như `-rwxr-xr-x`.

- Ký tự đầu: `-` = file, `d` = directory.
- 9 ký tự tiếp theo chia làm 3 cụm (user | group | others), mỗi cụm 3 ký tự:

  - `r` = read (4)
  - `w` = write (2)
  - `x` = execute (1)

**Ví dụ:** `drwxr-xr-x`:

- `d`: thư mục
- `rwx` (user): chủ sở hữu có đọc, ghi, thực thi
- `r-x` (group): nhóm có đọc, thực thi
- `r-x` (others): người khác có đọc, thực thi

---

## 4. `chmod` — Thay đổi quyền

**Cách 1: Dùng số (octal)**

- `r=4, w=2, x=1`.
- Ví dụ `chmod 755 file` → user=7 (rwx), group=5 (r-x), others=5 (r-x).

**Cách 2: Dùng chữ (symbolic)**

- `u` (user), `g` (group), `o` (others), `a` (all).
- `+` thêm quyền, `-` bỏ quyền, `=` gán quyền.
- Ví dụ: `chmod u+x script.sh` — thêm quyền thực thi cho chủ sở hữu.

**CTF tip:** Nếu chạy `./exploit` báo `Permission denied`, thường là file chưa có quyền thực thi.

```bash
# Kiểm tra
ls -l exploit
# Thêm quyền
chmod u+x exploit
# Hoặc chmod 700 exploit
```

---

## 5. Quyền đặc biệt — SUID (Set User ID)

Khi bạn thấy `s` thay vì `x` ở phần user: `-rwsr-xr-x`, đó là **SUID**.

**Ý nghĩa:** Khi ai đó chạy file này, chương trình sẽ chạy với quyền của _chủ sở hữu file_ (thường là `root`) thay vì quyền của người chạy.

**Tại sao quan trọng trong CTF:** Nếu file SUID thuộc sở hữu `root` có lỗ hổng hoặc cho phép chạy lệnh tùy ý, kẻ tấn công có thể leo thang lên `root` — đây là lỗ hổng leo thang đặc quyền cổ điển.

**Tìm file SUID:**

```bash
find / -perm -4000 -type f 2>/dev/null
# hoặc
find / -perm -u=s -type f 2>/dev/null
```

---

## 6. Bài tập thực hành

Thực hiện trên hệ thống (trong thư mục home):

```bash
cd ~
mkdir -p ctf_practice/level1/level2
echo "flag{tim_thay_roi_nhe}" > ctf_practice/level1/level2/secret.txt
echo "Password admin la: 123456" > ctf_practice/level1/config.ini
# Tạo script không có quyền thực thi
echo 'echo "Script da chay!"' > runme.sh

# Quay về home và bắt đầu tìm
cd ~

# Thử thách 1: Tìm secret.txt
find ~/ctf_practice -name "secret.txt"

# Thử thách 2: Dùng grep để tìm "123456"
grep -ir "123456" ~/ctf_practice

# Thử thách 3: Chạy script sẽ bị Permission denied
./runme.sh

# Thử thách 4: Kiểm tra quyền
ls -l runme.sh

# Thử thách 5: Cấp quyền thực thi
chmod u+x runme.sh

# Thử thách 6: Chạy lại
./runme.sh
```

---

### Kết thúc Chương 2

Bạn đã có hai công cụ mạnh: `find` và `grep` để _săn file/nội dung_, đồng thời hiểu cách _quyền truy cập_ hoạt động (và cách thay đổi chúng bằng `chmod`). Ở chương sau, ta sẽ học về **xử lý output**, **redirection**, và **sử dụng sudo** để thực hành leo thang đặc quyền an toàn.

---
