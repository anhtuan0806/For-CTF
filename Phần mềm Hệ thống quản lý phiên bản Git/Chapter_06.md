## Chapter 6 – Quy Trình Hợp Tác: Fork và Pull Request

### 6.1 Mục Đích

Trong Chapter 4, bạn đã học cách push và pull từ một kho chứa `origin` mà bạn có quyền ghi (write access). Nhưng làm thế nào để bạn đóng góp cho một dự án mã nguồn mở (open-source) mà bạn **không có quyền ghi**?

Chương cuối cùng này sẽ dạy bạn quy trình làm việc quan trọng nhất để cộng tác trên GitHub:

- Hiểu sự khác biệt giữa **origin** và **upstream**.
- Học cách **Fork (Rẽ nhánh)** một kho chứa.
- Biết cách **đồng bộ hóa (sync)** fork của bạn với dự án gốc.
- Tạo một **Pull Request (PR)** để đề xuất các thay đổi của bạn.
- Cập nhật Pull Request dựa trên phản hồi (review).

---

### 6.2 Fork Là Gì?

Một **Fork (Rẽ nhánh)** là một bản sao cá nhân của kho chứa của người khác, nằm trên tài khoản GitHub của bạn.

- Bạn có **toàn quyền (admin)** trên bản fork của mình.
- Bạn có thể thoải mái push, pull, tạo nhánh, hay thậm chí phá hỏng bản fork mà không ảnh hưởng gì đến dự án gốc.
- Bản fork này là nơi bạn **chuẩn bị các thay đổi** của mình trước khi gửi chúng cho dự án gốc.

---

### 6.3 Pull Request (PR) Là Gì?

Một **Pull Request (Yêu cầu gộp)** là một yêu cầu chính thức bạn gửi đến chủ sở hữu dự án gốc (gọi là **upstream**).

> Nghĩa là: _"Chào bạn, tôi đã thực hiện một số cải tiến trên nhánh `my-feature` trong bản fork của tôi. Vui lòng xem xét và kéo (pull) các thay đổi này vào dự án chính của bạn."_

PR là nơi diễn ra các **cuộc thảo luận**, **đánh giá code (code review)**, và **kiểm tra tự động** trước khi code được gộp (merge).

---

### 6.4 Quy Trình Làm Việc Đầy Đủ

Đây là quy trình chuẩn khi đóng góp cho bất kỳ dự án mã nguồn mở nào.

#### 🪄 Bước 1: Fork Kho Chứa (Upstream)

1. Truy cập kho chứa của dự án gốc (ví dụ: `github.com/owner-project/the-project`).
2. Nhấn nút **Fork** ở góc trên bên phải.
3. GitHub sẽ tạo một bản sao của dự án về tài khoản của bạn (ví dụ: `github.com/your-username/the-project`).

#### 🧩 Bước 2: Clone Kho Chứa (Fork) Của Bạn Về Máy

Không clone dự án gốc — hãy clone bản fork của bạn:

```bash
git clone https://github.com/your-username/the-project.git
cd the-project
```

#### 🔗 Bước 3: Kết Nối Với `Upstream` (Quan trọng)

Hiện tại, kho chứa local của bạn chỉ biết về `origin` (bản fork của bạn). Bạn cần thêm `upstream` để lấy các cập nhật mới nhất từ dự án gốc.

```bash
git remote add upstream https://github.com/owner-project/the-project.git

# Kiểm tra lại các remote
git remote -v
```

Kết quả:

```
origin    https://github.com/your-username/the-project.git (fetch)
origin    https://github.com/your-username/the-project.git (push)
upstream  https://github.com/owner-project/the-project.git (fetch)
upstream  https://github.com/owner-project/the-project.git (push)
```

#### 🧭 Bước 4: Đồng Bộ (Sync) Trước Khi Bắt Đầu

```bash
git checkout main
# Kéo thay đổi mới nhất từ dự án GỐC
git pull upstream main
# (Tùy chọn) Đẩy thay đổi này lên fork của bạn
git push origin main
```

#### 🌿 Bước 5: Tạo Nhánh Mới

Không bao giờ làm việc trực tiếp trên `main`:

```bash
git checkout -b fix/typo-in-readme
```

#### 🛠️ Bước 6: Sửa Đổi, Add, và Commit

```bash
nano README.md
git add README.md
git commit -m "Sửa lỗi chính tả trong phần giới thiệu"
```

#### 🚀 Bước 7: Đẩy (Push) Lên Fork Của Bạn (origin)

```bash
git push -u origin fix/typo-in-readme
```

#### 📨 Bước 8: Tạo Pull Request (Trên GitHub)

1. Truy cập `github.com/your-username/the-project`.
2. Nhấn **Compare & pull request**.
3. Đảm bảo:

   - **base repository** → `owner-project/the-project`
   - **head repository** → `your-username/the-project`

4. Viết tiêu đề và mô tả rõ ràng.
5. Nhấn **Create pull request**.

---

### 6.5 Cập Nhật Một Pull Request

Nếu maintainer yêu cầu sửa đổi:

```bash
git checkout fix/typo-in-readme
nano README.md
git commit -m "Cập nhật README theo yêu cầu review"
git push origin fix/typo-in-readme
```

Pull Request sẽ tự động được cập nhật.

---

### 6.6 Dọn Dẹp Sau Khi PR Được Gộp

```bash
git checkout main
git pull upstream main
git branch -d fix/typo-in-readme
```

Bạn có thể xóa nhánh đó trên GitHub để giữ repo sạch sẽ.

---

### 6.7 Bài Tập Thực Hành

1. Tìm một dự án mã nguồn mở nhỏ trên GitHub.
2. Fork dự án đó.
3. Clone bản fork về máy.
4. Thêm remote upstream.
5. Tạo nhánh mới (`feature/update-docs`).
6. Sửa lỗi chính tả trong tài liệu.
7. Commit và push.
8. Mở Pull Request gửi đến upstream.

---

### 6.8 Kết Luận Series

🎉 Chúc mừng bạn đã hoàn thành loạt bài hướng dẫn về Git!

Bạn đã học:

1. `git init` – khởi tạo dự án (Chapter 1)
2. `git add` & `git commit` – quy trình cốt lõi (Chapter 2)
3. `branch` & `merge` – làm việc song song (Chapter 3)
4. `push` & `pull` – kết nối với origin (Chapter 4)
5. `restore`, `revert`, `amend` – sửa lỗi sai (Chapter 5)
6. `fork` & `pull request` – cộng tác với cộng đồng (Chapter 6)

Git là công cụ mạnh mẽ – thực hành là chìa khóa để thành thạo. Tiếp theo, hãy tìm hiểu thêm về `git rebase`, `git stash`, và các công cụ như **VS Code GitLens**.

> Chúc bạn may mắn trên hành trình lập trình của mình! 💪
