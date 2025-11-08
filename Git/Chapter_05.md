## Chapter 5 – Quay Ngược Thời Gian: Sửa Lỗi Sai

### 5.1 Mục Đích

Một trong những lý do lớn nhất khiến Git trở nên mạnh mẽ là khả năng **"hoàn tác" (undo)**. Sớm hay muộn, bạn cũng sẽ commit nhầm, add thiếu file, hoặc phát hiện một commit cũ gây lỗi. Chương này giúp bạn:

- Hiểu và "unstage" một tệp tin đã add nhầm (`git restore --staged`).
- Hủy bỏ các thay đổi trong thư mục làm việc (`git restore`).
- Sửa lỗi cho commit gần nhất (`git commit --amend`).
- Hoàn tác (revert) một commit đã được push lên remote một cách an toàn.
- Hiểu sự khác biệt cơ bản giữa **revert** và **reset**.

### 5.2 Kịch Bản 1: add Nhầm Tệp (Unstaging)

Bạn vô tình add một tệp tin nhạy cảm (ví dụ: `config.log`) vào Staging Area.

```bash
$ echo "PASSWORD=12345" > config.log
$ git add config.log

$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   config.log
```

`git status` đã chỉ cho bạn chính xác cách sửa: `git restore --staged <file>`.

```bash
$ git restore --staged config.log

$ git status
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        config.log
```

Tệp tin đã được **unstage** (đưa ra khỏi Staging Area) và quay lại trạng thái _Untracked_.

> (Bây giờ là lúc thêm `*.log` vào `.gitignore` của bạn!)

**Lưu ý:** Lệnh cũ hơn là `git reset HEAD <file>`, nhưng `git restore` dễ hiểu hơn.

### 5.3 Kịch Bản 2: Hủy Bỏ Thay Đổi (Discarding)

Bạn sửa đổi một tệp (`index.html`), nhưng nhận ra tất cả các thay đổi đó đều sai và muốn quay lại phiên bản commit gần nhất.

```bash
# Bạn sửa file index.html...
$ git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   index.html
```

Một lần nữa, `git status` hướng dẫn bạn: `git restore <file>` để hủy bỏ thay đổi.

```bash
$ git restore index.html

$ git status
On branch main
nothing to commit, working tree clean
```

> ⚠️ **CẢNH BÁO:** Lệnh này sẽ xóa **vĩnh viễn** các thay đổi của bạn trong `index.html`. Hãy chắc chắn 100% bạn muốn làm điều này.

**Lưu ý:** Lệnh cũ hơn là `git checkout -- <file>`, nay được thay thế bằng `git restore` để dễ hiểu hơn.

### 5.4 Kịch Bản 3: Sửa Lỗi Commit Gần Nhất (--amend)

Bạn vừa commit, nhưng nhận ra:

a) Bạn gõ sai thông điệp commit.  
b) Bạn quên add một tệp tin.

`git commit --amend` cho phép bạn "sửa" (thực chất là thay thế) commit gần nhất.

#### Trường hợp a: Sửa lỗi thông điệp

```bash
# Bạn vừa commit với lỗi typo
$ git commit -m "Them trang chu inxed.html"

# Sửa lại ngay lập tức
$ git commit --amend -m "Thêm trang chủ index.html"
[main 1a2b3c4] Thêm trang chủ index.html
 Date: Sun Nov 9 10:30:45 2025 +0700
 1 file changed, 1 insertion(+)
 create mode 100644 index.html
```

Commit cũ đã bị thay thế bằng commit mới.

#### Trường hợp b: Thêm tệp tin bị quên

```bash
# 1. Bạn commit file HTML
$ git add index.html
$ git commit -m "Thêm trang chủ"

# 2. Bạn nhận ra quên file CSS
$ git add style.css

# 3. Dùng --amend, --no-edit nghĩa là giữ nguyên thông điệp commit cũ
$ git commit --amend --no-edit
[main 5d6e7f8] Thêm trang chủ
 Date: Sun Nov 9 10:35:10 2025 +0700
 2 files changed, 5 insertions(+)
 create mode 100644 index.html
 create mode 100644 style.css
```

> ⚠️ **CẢNH BÁO QUAN TRỌNG:** Tuyệt đối **không dùng** `git commit --amend` cho một commit mà bạn đã push lên remote. `amend` sẽ viết lại lịch sử, có thể gây xung đột cho đồng nghiệp đã pull commit cũ.

### 5.5 Kịch Bản 4: Hoàn Tác Commit Đã Push (`git revert`)

Đây là tình huống **an toàn**. Bạn push một commit (`a1b2c3d`) lên remote, và vài ngày sau phát hiện nó gây lỗi. Bạn **không thể** dùng amend hay reset — bạn phải dùng `git revert`.

`git revert` **không xóa lịch sử**, mà tạo một commit mới có nội dung ngược lại hoàn toàn với commit gây lỗi.

```bash
# Lịch sử của bạn:
$ git log --oneline
a1b2c3d (HEAD -> main, origin/main) Thêm tính năng login gây lỗi
b4c5d6e Cập nhật trang chủ
...

# 1. Revert commit gây lỗi
$ git revert a1b2c3d
```

Git sẽ mở trình soạn thảo văn bản để bạn nhập thông điệp commit revert. Thông điệp mặc định thường là:

```
Revert 'Thêm tính năng login gây lỗi'
```

Sau khi lưu và đóng, kiểm tra lại lịch sử:

```bash
$ git log --oneline
f9e8d7c (HEAD -> main) Revert "Thêm tính năng login gây lỗi"
a1b2c3d (origin/main) Thêm tính năng login gây lỗi
b4c5d6e Cập nhật trang chủ
```

Bây giờ dự án của bạn đã quay lại trạng thái trước `a1b2c3d`, nhưng lịch sử vẫn được giữ nguyên. Bạn có thể push commit "Revert" này lên remote một cách an toàn.

### 5.6 `revert` vs `reset` (Khái niệm quan trọng)

| Lệnh         | Hành động                                                        | An toàn khi đã push?                            |
| ------------ | ---------------------------------------------------------------- | ----------------------------------------------- |
| `git revert` | Tạo một commit mới để hoàn tác commit cũ.                        | ✅ Rất an toàn                                  |
| `git reset`  | Xóa lịch sử. Di chuyển con trỏ HEAD về quá khứ và vứt bỏ commit. | ⚠️ Cực kỳ nguy hiểm (chỉ dùng cho commit local) |

> Lệnh `git reset --hard <commit_hash>` sẽ xóa **vĩnh viễn** các commit và mọi thay đổi chưa được lưu. Hãy tránh xa nó trừ khi bạn biết chính xác mình đang làm gì.

### 5.7 Bài Tập Thực Hành

1. Mở tệp `README.md` và gõ vào đó dòng `Đây là thay đổi tồi`. Lưu tệp.
2. Chạy `git status` để xem tệp đã bị _modified_.
3. Dùng `git restore README.md` để hủy bỏ thay đổi đó. Mở lại tệp để xác nhận nó đã quay về như cũ.
4. Tạo một tệp `temp.txt`.
5. Dùng `git add temp.txt`.
6. Dùng `git restore --staged temp.txt` để _unstage_ nó.
7. Tạo một commit với thông điệp bị typo:
   ```bash
   git commit -m "Bo sung tai lieu" --allow-empty
   ```
8. Dùng `git commit --amend` để sửa thông điệp thành `Bổ sung tài liệu`.
9. Chạy `git log -1` để xem commit gần nhất và xác nhận thông điệp đã được sửa.

### 5.8 Kết Luận

Bạn đã học được 3 cấp độ "hoàn tác" an toàn:

- `git restore --staged <file>`: Bỏ tệp khỏi Staging Area.
- `git restore <file>`: Hủy thay đổi chưa commit trong Working Directory.
- `git revert <hash>`: Hoàn tác commit đã push (an toàn).

Bạn cũng đã học `git commit --amend` để sửa commit cuối cùng (chỉ an toàn khi chưa push).

Ở chương tiếp theo, chúng ta sẽ học về quy trình làm việc cộng tác quan trọng nhất: **Fork (rẽ nhánh)** và **Pull Request (Yêu cầu gộp)**.
