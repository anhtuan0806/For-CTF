# Chapter 2 – Quy Trình Làm Việc Cơ Bản: Add và Commit

## 2.1 Mục Đích

Trong chương 1, bạn đã khởi tạo kho chứa (`git init`). Bây giờ là lúc tìm hiểu quy trình làm việc cốt lõi của Git. Chương này giúp bạn:

- Hiểu và sử dụng `git status` để kiểm tra trạng thái dự án.
- Theo dõi các tệp tin mới bằng `git add`.
- Đưa các thay đổi vào Khu vực chuẩn bị (Staging Area).
- Lưu các “ảnh chụp” (snapshot) của dự án bằng `git commit`.
- Xem lại lịch sử các thay đổi bằng `git log`.
- Biết cách phớt lờ (ignore) các tệp tin không mong muốn bằng `.gitignore`.

---

## 2.2 `git status` – Người Bạn Thân Nhất Của Bạn

Lệnh `git status` là lệnh bạn sẽ sử dụng nhiều nhất. Nó cho bạn biết chính xác điều gì đang xảy ra trong ba khu vực (Working Directory, Staging Area, Repository).

Hãy đi vào thư mục `git_practice` bạn đã tạo ở Chương 1:

```bash
$ cd git_practice
$ git status
```

Kết quả:

```
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

Thông báo “No commits yet” và “nothing to commit” là hoàn toàn bình thường.

---

## 2.3 `git add` – Theo Dõi và Chuẩn Bị Tệp Tin

Bây giờ, hãy tạo một tệp tin mới:

```bash
$ echo "Đây là dự án Git thực hành." > README.md
```

Kiểm tra trạng thái:

```bash
$ git status
```

Kết quả:

```
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
```

Giải thích:

- **Untracked files**: Git thấy tệp `README.md`, nhưng chưa theo dõi nó.
- **Staging Area**: Để lưu tệp này, bạn cần thêm vào khu vực chuẩn bị.

Thêm tệp vào Staging Area:

```bash
$ git add README.md
```

Kiểm tra lại:

```bash
$ git status
```

Kết quả:

```
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md
```

Tệp `README.md` đã sẵn sàng để được commit.

---

## 2.4 `git commit` – Lưu “Ảnh Chụp”

Một commit là hành động lưu “ảnh chụp” của Staging Area vào lịch sử kho chứa.

```bash
$ git commit -m "Thêm tệp README.md ban đầu"
```

Kết quả:

```
[master (root-commit) a1b2c3d] Thêm tệp README.md ban đầu
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

Kiểm tra lại trạng thái:

```bash
$ git status
```

```
On branch master
nothing to commit, working tree clean
```

“working tree clean” nghĩa là mọi thay đổi đã được lưu.

---

## 2.5 Quy Trình Làm Việc Tiêu Chuẩn

Quy trình làm việc cơ bản trong Git là một vòng lặp gồm 3 bước:

1. **Modify (Sửa đổi)** – thay đổi tệp tin.
2. **Add (Thêm)** – thêm thay đổi vào Staging Area.
3. **Commit (Cam kết)** – lưu lại lịch sử.

Ví dụ:

```bash
# 1. MODIFY
$ echo "Thêm một dòng mới." >> README.md

# 2. ADD
$ git add README.md

# 3. COMMIT
$ git commit -m "Cập nhật README với dòng thứ hai"
```

Kết quả:

```
[master 4e5f6a7] Cập nhật README với dòng thứ hai
 1 file changed, 1 insertion(+)
```

---

## 2.6 `git log` – Xem Lại Lịch Sử

Xem các “ảnh chụp” bạn đã lưu:

```bash
$ git log
```

Hoặc phiên bản tóm tắt:

```bash
$ git log --oneline
```

Ví dụ:

```
4e5f6a7 Cập nhật README với dòng thứ hai
a1b2c3d Thêm tệp README.md ban đầu
```

---

## 2.7 Phớt Lờ Tệp Tin (`.gitignore`)

Một số tệp bạn không muốn Git theo dõi (vd: log, `node_modules`, `.env`).

Tạo tệp `.gitignore`:

```bash
$ nano .gitignore
```

Nội dung mẫu:

```
*.log
node_modules/
.env
```

Thêm và commit `.gitignore`:

```bash
$ git add .gitignore
$ git commit -m "Thêm .gitignore để phớt lờ tệp log"
```

---

## 2.8 Bài Tập Thực Hành

1. Tạo hai tệp: `index.html` và `style.css`.
2. Dùng `git status` để xem 2 tệp “untracked”.
3. Chỉ `add` tệp `index.html`.
4. Kiểm tra lại trạng thái.
5. Commit `index.html` với thông điệp “Thêm trang chủ”.
6. Add và commit `style.css` trong commit riêng.
7. Sửa đổi cả hai tệp.
8. Dùng `git add .` để thêm tất cả thay đổi.
9. Commit với thông điệp “Cập nhật style cho trang chủ”.
10. Dùng `git log --oneline` để xem toàn bộ lịch sử.
11. Tạo tệp `.gitignore` và thêm `temp/` vào đó. Commit tệp này.

---

## 2.9 Kết Luận

Bạn đã nắm được quy trình làm việc cốt lõi:

- **Sửa đổi → git add → git commit**
- `git status` kiểm tra trạng thái.
- `git log` xem lịch sử.
- `.gitignore` loại trừ tệp không cần thiết.

Ở chương tiếp theo, bạn sẽ học cách tạo **nhánh (branch)** để làm việc song song mà không ảnh hưởng đến dòng chính.
