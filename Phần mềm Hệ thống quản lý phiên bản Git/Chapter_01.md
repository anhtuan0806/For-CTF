# Chapter 1 – Giới Thiệu về Git và Cài Đặt

## 1.1 Mục Đích

Chào mừng bạn đến với loạt bài hướng dẫn về Git! Chương này sẽ cung cấp cho bạn nền tảng cần thiết để bắt đầu:

- Hiểu được "Kiểm soát phiên bản" (Version Control) là gì và tại sao nó quan trọng.
- Phân biệt giữa mô hình Tập trung (Centralized) và Phân tán (Distributed).
- Cài đặt Git trên các hệ điều hành phổ biến (Linux, Windows, macOS).
- Thực hiện cấu hình ban đầu (`git config`) cho tên và email của bạn.
- Học cách tạo (khởi tạo) một kho chứa (repository) Git đầu tiên.

## 1.2 Kiểm Soát Phiên Bản (VCS) Là Gì?

Hệ thống Kiểm soát Phiên bản (Version Control System - VCS) là một phần mềm giúp bạn theo dõi và quản lý mọi thay đổi đối với các tệp tin của mình theo thời gian.

Hãy tưởng tượng bạn đang viết một tài liệu quan trọng. Bạn có thể lưu các phiên bản như:

```
report_v1.doc
report_v2_final.doc
report_v2_final_REALLY_final.doc
```

Cách làm này rất lộn xộn và không hiệu quả. VCS giải quyết vấn đề này bằng cách cho phép bạn:

- **Lưu "ảnh chụp" (snapshots):** Ghi lại trạng thái của dự án tại một thời điểm.
- **Quay lại (Revert):** Dễ dàng quay trở lại một phiên bản cũ nếu bạn làm sai.
- **Phân nhánh (Branch):** Thử nghiệm các tính năng mới mà không ảnh hưởng đến phiên bản chính.
- **Hợp tác (Collaborate):** Nhiều người có thể làm việc chung trên cùng một dự án.

## 1.3 Tập Trung (Centralized) vs. Phân Tán (Distributed)

Có hai mô hình VCS chính:

**Tập trung (Centralized - CVCS):** (Ví dụ: SVN, Perforce)

- Có một máy chủ trung tâm duy nhất chứa toàn bộ lịch sử dự án.
- Lập trình viên "check out" tệp tin từ máy chủ đó.
- **Nhược điểm:** Nếu máy chủ trung tâm bị lỗi, không ai có thể làm việc hoặc lưu phiên bản.

**Phân tán (Distributed - DVCS):** (Ví dụ: Git, Mercurial)

- Mỗi lập trình viên có một bản sao đầy đủ của toàn bộ lịch sử dự án trên máy cá nhân.
- Bạn có thể lưu phiên bản (commit) offline.
- Chỉ cần kết nối mạng khi muốn đồng bộ (push/pull) với các kho chứa khác (ví dụ: GitHub).

> **Git** là hệ thống kiểm soát phiên bản phân tán (DVCS) phổ biến nhất thế giới.

## 1.4 Cài Đặt Git

Bạn cần cài đặt Git trước khi có thể sử dụng.

### 1. Trên Linux (Ubuntu/Debian)

Sử dụng apt (như đã học trong series về Ubuntu):

```bash
sudo apt update
sudo apt install git
```

### 2. Trên Windows

Cách dễ nhất là tải về và cài đặt **Git for Windows** từ trang web chính thức:
[https://git-scm.com/downloads](https://git-scm.com/downloads)

Bản cài đặt này cung cấp cả công cụ dòng lệnh `git` và **Git Bash** (một Terminal giống Linux).

### 3. Trên macOS

Nếu bạn đã cài đặt Xcode Command Line Tools, Git có thể đã có sẵn. Bạn có thể cài đặt hoặc cập nhật nó bằng:

```bash
xcode-select --install
```

Hoặc nếu bạn dùng Homebrew:

```bash
brew install git
```

### Kiểm tra cài đặt

Sau khi cài đặt, mở Terminal (hoặc Git Bash trên Windows) và gõ:

```bash
git --version
```

Ví dụ:

```
git version 2.45.1
```

Nếu bạn thấy thông báo phiên bản, bạn đã cài đặt thành công.

## 1.5 Cấu Hình Git Lần Đầu Tiên

Đây là bước bắt buộc, bạn chỉ cần làm **một lần duy nhất trên mỗi máy**. Git cần biết bạn là ai để gắn thông tin vào mỗi thay đổi (**commit**) bạn tạo ra.

```bash
git config --global user.name "Nguyen Van Anh"
git config --global user.email "anh.nguyen@example.com"
```

> `--global` nghĩa là áp dụng cho **tất cả** các dự án Git trên máy.

Kiểm tra lại cấu hình:

```bash
git config --list
```

Ví dụ:

```
user.name=Nguyen Van Anh
user.email=anh.nguyen@example.com
```

## 1.6 Kho Chứa (Repository) Là Gì?

Một **Repository (repo)** là thư mục dự án mà Git đang theo dõi.

Git hoạt động với 3 khu vực chính:

1. **Working Directory:** Nơi chứa tệp tin bạn đang chỉnh sửa.
2. **Staging Area:** Nơi chọn những thay đổi sẽ được lưu vào commit.
3. **Repository (.git):** Thư mục ẩn chứa toàn bộ lịch sử và cấu hình của dự án.

## 1.7 Tạo Kho Chứa Đầu Tiên (git init)

Có hai cách chính để bắt đầu một dự án Git:

- `git init`: Biến thư mục hiện có thành một kho chứa Git.
- `git clone`: Sao chép một kho chứa đã tồn tại (ví dụ: từ GitHub).

### Ví dụ:

```bash
mkdir du-an-moi
cd du-an-moi
git init
```

Đầu ra:

```
Initialized empty Git repository in /home/anh/du-an-moi/.git/
```

Kiểm tra thư mục `.git`:

```bash
ls -a
```

Đầu ra:

```
.  ..  .git
```

> ⚠️ **Không bao giờ chỉnh sửa trực tiếp thư mục `.git` trừ khi bạn biết chính xác mình đang làm gì.**

## 1.8 Bài Tập Thực Hành

1. Cài đặt Git và xác minh bằng `git --version`.
2. Chạy `git config --global` để thiết lập `user.name` và `user.email`.
3. Dùng `git config --list` để kiểm tra cấu hình.
4. Tạo thư mục mới `git_practice`.
5. Di chuyển vào thư mục đó và khởi tạo Git bằng `git init`.
6. Dùng `ls -a` để xác nhận `.git` đã được tạo.

## 1.9 Kết Luận

Bạn đã:

- Hiểu Git là gì và tại sao nó quan trọng.
- Cài đặt và cấu hình Git trên máy của mình.
- Học cách khởi tạo một kho chứa mới bằng `git init`.

> Ở chương tiếp theo, bạn sẽ tìm hiểu quy trình làm việc cơ bản nhất của Git: **Thêm (`git add`) và Cam kết (`git commit`)** các thay đổi.
