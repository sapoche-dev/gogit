# Vibe & Git — sổ tay luyện tập từ số 0

> Mục tiêu: hiểu điều mình đang làm, gõ được lệnh mà không học vẹt, và lặp lại quy trình đủ nhiều để thành phản xạ.
>
> Cách học: mỗi lần chỉ học một bài. Đọc mục tiêu, tự gõ lệnh (đừng chỉ chép), dự đoán kết quả, chạy lệnh, rồi đối chiếu. Nếu sai, dùng `git status` để quan sát trước khi sửa.

## Bản đồ hành trình

Các phần sau sẽ được bổ sung từng bài một. Đừng nhảy đến lệnh nâng cao khi vòng lặp cơ bản chưa rõ.

1. [**Bài 0 — Git là gì, repo là gì, kiểm tra Git**](bai-0.md)
2. [**Bài 1 — Ba vùng quan trọng**](bai-1.md) — thư mục làm việc → staging area → lịch sử commit
3. [**Bài 2 — Xem thay đổi và sửa nhầm**](bai-2.md) — `diff`, bỏ stage, khôi phục file
4. [**Bài 3 — Lịch sử**](bai-3.md) — `log`, `show`, commit tốt, `.gitignore`
5. [**Bài 4 — GitHub và remote**](bai-4.md) — `clone`, `remote`, `fetch`, `pull`, `push`
6. [**Bài 5 — Nhánh**](bai-5.md) — tạo, chuyển, đổi tên, xóa branch; HEAD
7. [**Bài 6 — Gộp công việc**](bai-6.md) — merge, fast-forward, conflict và cách giải quyết
8. [**Bài 7 — Pull Request**](bai-7.md) — vai trò dev, tester, reviewer/approver; CI và checklist
9. [**Bài 8 — Làm việc nhóm**](bai-8.md) — đồng bộ, review thay đổi, xử lý nhánh lỗi thời
10. **Bài 9 — Cứu hộ:** revert, reset, reflog, stash, cherry-pick, tag
11. **Bài 10 — Git nâng cao và bảo vệ repo:** rebase, remote tracking, quyền, rules, release
12. **Bài 11 — Bài tập mô phỏng quy trình nhóm hoàn chỉnh**

Lộ trình bao phủ Git dùng ở dòng lệnh và quy trình GitHub thường gặp. Không cần thuộc lòng mọi tùy chọn: cần biết cách tìm trợ giúp, dự đoán tác động, và kiểm tra trạng thái sau mỗi bước.

---

# Bài 0 — Làm quen và quan sát

## Mục tiêu

- Hiểu Git là chương trình theo dõi lịch sử thay đổi; GitHub là dịch vụ lưu repo và cộng tác.
- Biết một thư mục có phải Git repository (repo) hay không.
- Biết hỏi Git xem trạng thái hiện tại.

## 1. Phân biệt bốn từ

- **File**: tài liệu/code, ví dụ `tutorials/bai-0.md`.
- **Thư mục làm việc** (working directory): các file bạn đang nhìn và sửa.
- **Repository / repo**: thư mục được Git quản lý, có thư mục ẩn `.git` chứa lịch sử và cấu hình.
- **Commit**: một mốc lưu trong lịch sử Git, có mã định danh và lời nhắn.

Git chạy trên máy bạn. GitHub là một nơi có thể chứa bản sao repo để chia sẻ, lưu trữ, review và tự động chạy kiểm tra. Có Git không có nghĩa là đã kết nối GitHub.

## 2. Kiểm tra Git đã cài chưa

Mở Terminal/PowerShell tại thư mục dự án và tự gõ:

```bash
git --version
```

Tùy chọn `--version` in phiên bản Git đã cài, ví dụ `git version ...`. Nếu báo không nhận ra lệnh `git`, hãy cài Git trước rồi mở lại terminal.

## 3. Kiểm tra thư mục hiện tại

```bash
pwd
git status
```

`pwd` in đường dẫn đến thư mục terminal đang đứng. `git status` cho biết nhánh hiện tại và trạng thái thay đổi của các file. Đây là lệnh quan sát an toàn, hãy tập thói quen dùng nó khi chưa chắc chuyện gì xảy ra.

Thông báo `not a git repository` nghĩa là terminal đang đứng ngoài repo. Di chuyển vào đúng thư mục dự án rồi thử lại. Không chạy `git init` ở một thư mục bất kỳ để làm mất dấu lỗi vị trí.

## 4. Thiết lập tên hiển thị cho commit (chỉ làm nếu chưa thiết lập)

Xem cấu hình:

```bash
git config --global user.name
git config --global user.email
```

Nếu chưa có, điền tên và email của bạn:

```bash
git config --global user.name "Tên của bạn"
git config --global user.email "email@example.com"
```

`git config` đọc hoặc ghi cấu hình. `user.name` và `user.email` là hai mục Git dùng khi tạo commit mới. Chỉ ghi tên mục thì Git đọc giá trị hiện tại. Thêm giá trị trong ngoặc kép thì Git lưu giá trị đó. `--global` chọn cấu hình mặc định của tài khoản trên máy. Email trong commit có thể công khai trên GitHub, nên hãy dùng địa chỉ bạn muốn gắn với commit.

## Bài tập 0 — Nói lại bằng lời của bạn

Không nhìn phần giải thích, trả lời:

1. Git và GitHub khác nhau thế nào?
2. Repo có gì đặc biệt so với thư mục bình thường?
3. Lệnh nào cho biết trạng thái repo?
4. Nếu thấy `not a git repository`, bước đầu tiên là gì?

### Đáp án tự kiểm

1. Git là công cụ quản lý lịch sử phiên bản; GitHub là dịch vụ lưu trữ/cộng tác repo.
2. Repo có vùng dữ liệu `.git` để Git lưu lịch sử/cấu hình.
3. `git status`.
4. Kiểm tra mình đang đứng ở đúng thư mục repo chưa.

---

## Tiến độ

- [x] Bài 0 — quan sát và khái niệm nền
- [x] Bài 1 — sửa, stage, commit, kiểm tra
- [ ] Bài 2 — diff và sửa nhầm
- [ ] Bài 3 — lịch sử và `.gitignore`
- [ ] Bài 4 — kết nối GitHub an toàn
- [ ] Bài 5 trở đi — nhánh, merge, conflict, PR, vai trò nhóm và cứu hộ

**Đọc tiếp** [Bài 1 — Từ file đến commit đầu tiên](bai-1.md).
