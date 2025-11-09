# Chương 8 — Functions (Hàm) và Quản lý Lỗi

Đến giờ, script của bạn có thể thực thi theo thứ tự, nhưng để code gọn, tái sử dụng và dễ bảo trì, ta cần **hàm** và **xử lý lỗi** chuyên nghiệp.

---

## 1. Functions (Hàm) là gì?

Hàm là khối mã có tên, gọi bất kỳ lúc nào để thực hiện một tác vụ.

**Cú pháp:**

```bash
# Cách 1
function ten_ham {
    echo "Day la ham"
}

# Cách 2 (phổ biến)
ten_ham() {
    echo "Day la ham"
}

# Gọi hàm
ten_ham
```

**Lợi ích:**

- Tái sử dụng (DRY).
- Dễ đọc, dễ bảo trì.

---

## 2. Hàm với tham số

Hàm nhận tham số giống script: `$1`, `$2`, ...

```bash
phep_cong() {
    tong=$(($1 + $2))
    echo "Tong: $tong"
}

phep_cong 5 10
```

---

## 3. Biến local vs global

- Mặc định, biến trong bash là **global**.
- Dùng `local` trong hàm để tránh làm vấy bẩn scope toàn cục.

```bash
my_func() {
    local tmp="noi bo"
    echo $tmp
}
```

Quy tắc: ưu tiên `local` cho biến trong hàm.

---

## 4. Trả về giá trị

Hai cách:

- **Echo** để trả chuỗi; hứng bằng `$(...)`.
- **return** chỉ trả mã lỗi (0–255).

```bash
lay_ngay() { echo "$(date)"; }
now=$(lay_ngay)

kiemtra() { return 1; }
kiemtra
echo $?  # mã lỗi
```

---

## 5. Xử lý lỗi (Error Handling)

- Mã thoát: `$?` — giá trị trả về của lệnh vừa chạy.
- `set -e`: dừng script khi lệnh nào trả về != 0.
- `set -u`: lỗi khi dùng biến chưa được gán.
- `set -o pipefail`: khiến pipeline trả lỗi nếu một phần tử trong pipeline lỗi.

Bộ ba xanh an toàn: `set -euo pipefail`.

**In lỗi ra stderr:**

```bash
echo "Loi: Khong tim thay file" >&2
```

**Ví dụ kiểm tra lệnh:**

```bash
cmd || { echo "Loi: cmd that bai" >&2; exit 1; }
```

---

## 6. Bài tập thực hành

1. **Tái cấu trúc `web_scan.sh`** (Chương 5):

   - Viết `print_usage()` in cách dùng (`echo "Cach dung: $0 <domain>"`).
   - Viết `check_ping(domain)` dùng `ping -c 4`.
   - Viết `check_curl(domain)` dùng `curl` hoặc `wget`.
   - Nếu `#$# -eq 0` thì gọi `print_usage` và `exit 1`.

2. **Viết `check_service.sh`** nhận `$1` là tên dịch vụ (ví dụ `ssh`):

   - Dùng `ps aux | grep $1 | grep -v grep`.
   - Kiểm tra `$?` để in `Dich vu $1 dang chay` hoặc `Dich vu $1 KHONG chay`.
   - Thêm `set -e` ở đầu script. Thử chèn lệnh sai (ví dụ `ls /khong/ton/tai`) trước khi `ps aux` và quan sát script dừng ngay lập tức.

---

## 7. Mẹo thực tế

- Luôn bắt đầu script quan trọng bằng `#!/bin/bash` và `set -euo pipefail`.
- In ra stderr cho lỗi để dễ redirect: `2> errors.log`.
- Sử dụng hàm `cleanup` và `trap` để xử lý dọn dẹp khi script nhận SIGINT/SIGTERM.

**Ví dụ trap:**

```bash
cleanup() { echo "Dang don dep..."; }
trap cleanup EXIT
```

---

### Kết thúc Chương 8

Bạn đã học cách tổ chức script bằng hàm, quản lý scope biến, trả giá trị, và xử lý lỗi chuyên nghiệp. Script giờ an toàn, dễ đọc và dễ bảo trì — bước chuẩn bị tốt trước khi triển khai các kỹ thuật nâng cao trong CTF.
