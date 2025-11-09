## Chapter 4 – Làm Việc với Kho Chứa Từ Xa (Remotes)

### 4.1 Mục Đích

Ở các chương trước, mọi thao tác (commit, branch, merge) đều diễn ra trên máy tính cá nhân của bạn (local repository). Trong chương này, bạn sẽ học cách đưa dự án của mình lên mạng để lưu trữ và hợp tác với người khác.

Chương này giúp bạn:

- Hiểu kho chứa từ xa (Remote Repository) là gì (ví dụ: GitHub, GitLab).
- Sao chép (`git clone`) một dự án từ remote về máy.
- Kết nối (`git remote add`) kho chứa local với một remote.
- Đẩy (`git push`) các commit từ local lên remote.
- Kéo (`git pull`) các thay đổi từ remote về local.

### 4.2 Kho Chứa Từ Xa (Remote) Là Gì?

Một **Remote** là một phiên bản của kho chứa Git của bạn, nhưng được lưu trữ trên một máy chủ trên Internet hoặc trong mạng nội bộ. GitHub, GitLab, và Bitbucket là các dịch vụ lưu trữ remote phổ biến nhất.

Làm việc với remote cho phép bạn:

- **Sao lưu:** Giữ một bản sao dự phòng an toàn cho dự án của bạn.
- **Hợp tác:** Nhiều người có thể cùng đóng góp vào một dự án bằng cách push (đẩy) và pull (kéo) các thay đổi từ remote chung.

### 4.3 Lấy Dự Án Từ Remote (`git clone`)

Đây là cách phổ biến nhất để bạn tham gia một dự án đã tồn tại. `git clone` thực hiện hai việc:

1. Tải (download) toàn bộ kho chứa (bao gồm tất cả lịch sử) về máy bạn.
2. Tự động kết nối kho chứa local mới này với remote (đặt tên remote mặc định là `origin`).

**Cách làm:**

```bash
# Di chuyển ra ngoài thư mục git_practice của bạn
$ cd ..

# Clone dự án
$ git clone https://github.com/facebook/react.git
Cloning into 'react'...
remote: Enumerating objects: ..., done.
remote: Counting objects: ..., done.
remote: Compressing objects: ..., done.
remote: Total ... (delta ...), reused ... (delta ...), pack-reused ...
Receiving objects: 100% (...), ...
Resolving deltas: 100% (...), done.

# Bây giờ bạn có một thư mục mới tên là 'react'
$ cd react
```

### 4.4 Kết Nối Kho Chứa Local Với Remote (`git remote`)

Đây là trường hợp của bạn: Bạn đã có một dự án local (`git_practice`) và bây giờ bạn muốn đưa nó lên GitHub.

**Bước 1:** Tạo một kho chứa _rỗng_ trên GitHub

- Đăng nhập GitHub.
- Nhấn dấu `+` ở góc trên bên phải, chọn **New repository**.
- Đặt tên (ví dụ: `my-git-practice`).
- **Quan trọng:** Không tick vào _Add a README file_ hay _.gitignore_. Bạn muốn một kho chứa hoàn toàn rỗng.
- Nhấn **Create repository**.

**Bước 2:** Lấy URL của remote

Ví dụ: `https://github.com/anhnguyen/my-git-practice.git`

**Bước 3:** Kết nối remote vào dự án local

```bash
# 1. Thêm remote với tên là 'origin'
$ git remote add origin https://github.com/anhnguyen/my-git-practice.git

# 2. Kiểm tra xem remote đã được thêm chưa
$ git remote -v
origin  https://github.com/anhnguyen/my-git-practice.git (fetch)
origin  https://github.com/anhnguyen/my-git-practice.git (push)
```

> `origin` là tên gọi tắt (alias) tiêu chuẩn cho remote chính của dự án.

### 4.5 Lưu Ý Quan Trọng: `main` vs `master`

- Trước đây, Git mặc định gọi nhánh chính là `master`.
- Gần đây, GitHub và cộng đồng đã chuyển sang dùng `main` làm tên mặc định.

Kho chứa `git_practice` của bạn đang dùng nhánh `master`. Kho chứa mới trên GitHub đang chờ nhánh `main`. Bạn nên đổi tên nhánh local của mình để đồng bộ:

```bash
# -M: Di chuyển/Đổi tên nhánh
$ git branch -M master main
```

### 4.6 Đẩy Code Lên Remote (`git push`)

Bây giờ kho chứa local (nhánh `main`) và remote (`origin`) đã sẵn sàng. `git push` là lệnh gửi các commit của bạn từ local lên remote.

```bash
# Cú pháp: git push <tên_remote> <tên_nhánh>

# Lần đẩy đầu tiên, dùng -u (set upstream)
$ git push -u origin main
```

Kết quả:

```
Enumerating objects: 15, done.
Counting objects: 100% (15/15), done.
Delta compression using up to 8 threads
Compressing objects: 100% (10/10), done.
Writing objects: 100% (15/15), 1.34 KiB | 1.34 MiB/s, done.
Total 15 (delta 2), reused 0 (delta 0), pack-reused 0
...
To https://github.com/anhnguyen/my-git-practice.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

> `-u` (hoặc `--set-upstream`) chỉ cần dùng lần đầu. Nó liên kết nhánh `main` local với nhánh `main` trên `origin`.

Từ giờ, để đẩy các commit mới, bạn chỉ cần gõ:

```bash
$ git push
```

Sau đó F5 trang GitHub của bạn — tất cả các tệp (README.md, index.html, v.v.) đã xuất hiện!

### 4.7 Kéo Code Từ Remote (`git pull`)

Giả sử đồng nghiệp của bạn cũng push một thay đổi lên `origin`. Kho chứa local của bạn giờ đã bị "cũ". Bạn cần cập nhật nó.

`git pull` lấy các thay đổi mới nhất từ remote và tự động gộp (merge) chúng vào nhánh hiện tại của bạn.

```bash
# Cú pháp: git pull <tên_remote> <tên_nhánh>
# (Hoặc chỉ cần 'git pull' nếu bạn đã 'set upstream')
$ git pull origin main
```

Ví dụ kết quả:

```
From https://github.com/anhnguyen/my-git-practice
 * branch            main       -> FETCH_HEAD
Updating 9a8b7c6..f0e1d2c
Fast-forward
 teamate-file.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 teamate-file.txt
```

> Quy tắc vàng: **Luôn `git pull` trước khi bạn bắt đầu code một tính năng mới** để đảm bảo bạn đang làm việc trên phiên bản mới nhất.

### 4.8 Bài Tập Thực Hành

1. Tạo tài khoản GitHub (nếu chưa có).
2. Tạo một kho chứa mới rỗng trên GitHub, đặt tên là `git-learn`.
3. Vào thư mục `git_practice` trên máy của bạn.
4. Chạy `git remote -v`. Nếu có remote cũ, xóa bằng `git remote remove origin`.
5. Thêm remote mới trỏ đến kho chứa `git-learn`.
6. Đổi tên nhánh `master` thành `main` (nếu chưa làm):

   ```bash
   git branch -M main
   ```

7. Đẩy nhánh `main` lên origin:

   ```bash
   git push -u origin main
   ```

8. Mở trình duyệt, vào repo `git-learn` và xác nhận các tệp của bạn đã ở đó.
9. **Thử thách:**
   - Trên GitHub, nhấn **Add a file → Create new file**.
   - Tạo tệp `LICENSE` và commit trực tiếp.
   - Quay lại Terminal, chạy `git pull` để lấy tệp `LICENSE` về máy local của bạn.

### 4.9 Kết Luận

Bạn đã học được cách kết nối thế giới local và thế giới remote:

- `git clone`: Lấy dự án đã có.
- `git remote add`: Kết nối dự án đang làm.
- `git push`: Gửi thay đổi lên.
- `git pull`: Nhận thay đổi về.

Quy trình làm việc cơ bản khi hợp tác:

> **Pull → Làm việc (trên nhánh mới) → Push → Tạo Pull Request (sẽ học sau).**

Ở chương tiếp theo, chúng ta sẽ tìm hiểu cách _quay ngược thời gian_ và sửa chữa các lỗi sai bằng `git reset`, `git revert`, và `git checkout -- <file>`.
