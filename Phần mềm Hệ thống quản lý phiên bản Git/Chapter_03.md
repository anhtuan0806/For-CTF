# Chapter 3 – Phân Nhánh và Gộp Nhánh (Branching & Merging)

## 3.1 Mục Đích

Ở chương 2, bạn đã làm việc trên một "dòng thời gian" duy nhất, gọi là `master`. Đây là một trong những khái niệm mạnh mẽ nhất của Git: **làm việc song song**. Chương này giúp bạn:

- Hiểu được Branch (Nhánh) là gì và tại sao nó là cứu cánh.
- Hiểu con trỏ HEAD.
- Tạo và chuyển đổi giữa các nhánh (`git branch`, `git checkout`).
- Gộp các thay đổi từ nhánh này sang nhánh khác (`git merge`).
- Xóa nhánh sau khi hoàn thành.
- Hiểu sơ lược về Merge Conflict (Xung đột khi gộp) và cách giải quyết.

---

## 3.2 Branch (Nhánh) Là Gì?

Hãy tưởng tượng `master` là **phiên bản chính thức (production)** của sản phẩm. Bạn không bao giờ muốn làm hỏng nó.

Khi bạn muốn phát triển một tính năng mới (ví dụ: "trang liên hệ") hoặc sửa một lỗi, bạn không làm việc trực tiếp trên `master`. Thay vào đó, bạn **tạo ra một nhánh (branch) mới**, tách ra từ `master`.

Nhánh này là một **vũ trụ song song**, một bản sao của dự án nơi bạn có thể thoải mái thay đổi, commit, thậm chí phá hỏng mọi thứ mà không ảnh hưởng đến `master`.

Sau khi tính năng của bạn hoàn thiện và đã được kiểm thử, bạn sẽ **gộp (merge)** nó trở lại vào `master`.

---

## 3.3 HEAD Là Gì?

Git dùng một con trỏ đặc biệt tên là **HEAD** để biết bạn hiện đang ở đâu.

- `HEAD` trỏ đến commit mới nhất của nhánh mà bạn đang làm việc.
- Nếu bạn checkout sang nhánh `master`, HEAD sẽ trỏ đến `master`.
- Nếu bạn checkout sang nhánh `feature`, HEAD sẽ trỏ đến `feature`.

---

## 3.4 Tạo và Chuyển Nhánh

Hãy xem chúng ta đang ở đâu:

```bash
# -v (verbose) hiển thị commit mới nhất của mỗi nhánh
$ git branch -v
* master 4e5f6a7 Cập nhật README với dòng thứ hai
```

Dấu `*` cho biết bạn đang ở nhánh `master` (tức `HEAD` đang trỏ đến `master`).

### 1. Tạo nhánh mới

```bash
$ git branch feature/login
```

Lệnh này chỉ tạo nhánh, nhưng bạn vẫn đang ở `master`.

```bash
$ git branch -v
  feature/login 4e5f6a7 Cập nhật README với dòng thứ hai
* master        4e5f6a7 Cập nhật README với dòng thứ hai
```

### 2. Chuyển sang nhánh mới

```bash
$ git checkout feature/login
Switched to branch 'feature/login'

$ git branch -v
* feature/login 4e5f6a7 Cập nhật README với dòng thứ hai
  master        4e5f6a7 Cập nhật README với dòng thứ hai
```

Dấu `*` đã di chuyển. HEAD bây giờ trỏ đến `feature/login`.

### 3. Shortcut: Tạo và chuyển nhánh cùng lúc

```bash
# Lệnh này = (git branch feature/contact) + (git checkout feature/contact)
$ git checkout -b feature/contact
Switched to a new branch 'feature/contact'
```

> 💡 Trong Git phiên bản mới, bạn có thể dùng `git switch -c <tên-nhánh>`.

---

## 3.5 Làm Việc Trên Nhánh Mới

Bây giờ bạn đang ở nhánh `feature/login`. Hãy tạo một tệp mới và commit:

```bash
# 1. Tạo tệp
$ echo "<html>Trang login</html>" > login.html

# 2. Add và Commit
$ git add login.html
$ git commit -m "Thêm trang login.html"
[feature/login 9a8b7c6] Thêm trang login.html
 1 file changed, 1 insertion(+)
 create mode 100644 login.html
```

Kiểm tra lịch sử của nhánh này:

```bash
$ git log --oneline
9a8b7c6 (HEAD -> feature/login) Thêm trang login.html
4e5f6a7 (master) Cập nhật README với dòng thứ hai
a1b2c3d Thêm tệp README.md ban đầu
```

Nhánh `feature/login` đã có commit mới. Giờ hãy xem điều kỳ diệu:

```bash
# 1. Quay trở lại nhánh master
$ git checkout master

# 2. Kiểm tra các tệp tin
$ ls
README.md  index.html  style.css  .gitignore

# 3. Kiểm tra lịch sử
$ git log --oneline
4e5f6a7 (HEAD -> master) Cập nhật README với dòng thứ hai
a1b2c3d Thêm tệp README.md ban đầu
```

Tệp `login.html` đã biến mất! Nhưng đừng lo — nó vẫn an toàn trong nhánh `feature/login`.

---

## 3.6 Gộp Nhánh (`git merge`)

Tính năng "login" đã hoàn tất. Giờ là lúc đưa nó vào `master`.

### Quy trình gộp:

1. Chuyển về nhánh nhận (`master`).
2. Gộp nhánh cho (`feature/login`).

```bash
$ git checkout master
$ git merge feature/login
Updating 4e5f6a7..9a8b7c6
Fast-forward
 login.html | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 login.html
```

"Fast-forward" là kiểu merge đơn giản nhất: Git chỉ cần "tua nhanh" con trỏ `master` đến commit mới nhất của `feature/login`.

```bash
$ git log --oneline
9a8b7c6 (HEAD -> master, feature/login) Thêm trang login.html
4e5f6a7 Cập nhật README với dòng thứ hai
a1b2c3d Thêm tệp README.md ban đầu
```

---

## 3.7 Xóa Nhánh

Sau khi đã gộp thành công, nhánh `feature/login` không còn cần thiết nữa.

```bash
# -d (delete): Xóa nhánh đã được gộp an toàn
$ git branch -d feature/login
Deleted branch feature/login (was 9a8b7c6).
```

Nếu bạn cố xóa nhánh chưa gộp, Git sẽ cảnh báo. Dùng `-D` (chữ hoa) để ép, nhưng cẩn thận.

---

## 3.8 Sơ Lược Về Xung Đột (Merge Conflicts)

Giả sử bạn thay đổi cùng một dòng trong cùng một tệp trên hai nhánh:

```html
<!-- master -->
<h1>Chào thế giới!</h1>

<!-- feature -->
<h1>Chào các bạn!</h1>
```

Khi merge, Git không thể chọn — đây gọi là **Merge Conflict**.

Git sẽ chèn cả hai phiên bản vào tệp:

```bash
<<<<<<< HEAD
<h1>Chào thế giới!</h1>
=======
<h1>Chào các bạn!</h1>
>>>>>>> feature
```

### Cách xử lý:

1. Mở tệp có xung đột.
2. Sửa nội dung, xóa các dấu `<<<<<<<`, `=======`, `>>>>>>>`.
3. Lưu tệp.
4. `git add <tên_tệp>`.
5. `git commit` (Git sẽ tự thêm thông điệp merge).

---

## 3.9 Bài Tập Thực Hành

1. Đảm bảo bạn đang ở nhánh `master`.
2. Tạo một nhánh mới tên `feature/footer`.
3. Chuyển sang nhánh `feature/footer`.
4. Sửa tệp `index.html` và thêm dòng `<p>Đây là footer</p>` vào cuối tệp.
5. Add và commit thay đổi đó.
6. Quay lại `master`.
7. Gộp `feature/footer` vào `master`.
8. Kiểm tra xem `index.html` trên `master` đã được cập nhật chưa.
9. Xóa nhánh `feature/footer`.
10. Kiểm tra lại bằng `git branch`.

---

## 3.10 Kết Luận

Bạn đã học quy trình làm việc quan trọng nhất:

> Tạo nhánh → Làm việc → Gộp nhánh → Xóa nhánh.

Luôn làm việc trên **feature branches**, giữ cho `master` sạch sẽ.

Hiện tại, mọi thao tác vẫn diễn ra **cục bộ (local)**.  
Ở chương tiếp theo, bạn sẽ học cách làm việc với **remote repositories** (như GitHub, GitLab) bằng `git clone`, `git push`, và `git pull`.
