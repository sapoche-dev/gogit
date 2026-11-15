# Vibe & Git — sổ tay luyện tập từ số 0

> Mục tiêu: hiểu điều mình đang làm, gõ được lệnh mà không học vẹt, và lặp lại quy trình đủ nhiều để thành phản xạ.
>
> Cách học: mỗi lần chỉ học một bài. Đọc mục tiêu, tự gõ lệnh (đừng chỉ chép), dự đoán kết quả, chạy lệnh, rồi đối chiếu. Nếu sai, dùng `git status` để quan sát trước khi sửa.

## Bản đồ hành trình

Các phần sau sẽ được bổ sung từng bài một. Đừng nhảy đến lệnh nâng cao khi vòng lặp cơ bản chưa rõ.

1. **Bài 0 — Git là gì, repo là gì, kiểm tra Git** (bài hôm nay)
2. **Bài 1 — Ba vùng quan trọng:** thư mục làm việc → staging area → lịch sử commit
3. **Bài 2 — Xem thay đổi và sửa nhầm:** `diff`, bỏ stage, khôi phục file
4. **Bài 3 — Lịch sử:** `log`, `show`, commit tốt, `.gitignore`
5. **Bài 4 — GitHub và remote:** `clone`, `remote`, `fetch`, `pull`, `push`
6. **Bài 5 — Nhánh:** tạo, chuyển, đổi tên, xóa branch; HEAD
7. **Bài 6 — Gộp công việc:** merge, fast-forward, conflict và cách giải quyết
8. **Bài 7 — Pull Request:** vai trò dev, tester, reviewer/approver; CI và checklist
9. **Bài 8 — Làm việc nhóm:** đồng bộ, review thay đổi, xử lý nhánh lỗi thời
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

- **File**: tài liệu/code, ví dụ `vibeandgit.md`.
- **Thư mục làm việc** (working directory): các file bạn đang nhìn và sửa.
- **Repository / repo**: thư mục được Git quản lý, có thư mục ẩn `.git` chứa lịch sử và cấu hình.
- **Commit**: một mốc lưu trong lịch sử Git, có mã định danh và lời nhắn.

Git chạy trên máy bạn. GitHub là một nơi có thể chứa bản sao repo để chia sẻ, lưu trữ, review và tự động chạy kiểm tra. Có Git không có nghĩa là đã kết nối GitHub.

## 2. Kiểm tra Git đã cài chưa

Mở Terminal/PowerShell tại thư mục dự án và tự gõ:

```bash
git --version
```

Bạn sẽ thấy phiên bản, ví dụ `git version ...`. Nếu báo không nhận ra lệnh `git`, hãy cài Git trước rồi mở lại terminal.

## 3. Kiểm tra thư mục hiện tại

```bash
pwd
git status
```

Trong PowerShell, `pwd` cũng dùng được. `git status` cho biết nhánh hiện tại và các file Git nhận ra. Đây là lệnh quan sát an toàn, hãy tập thói quen dùng nó khi chưa chắc chuyện gì xảy ra.

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

Cấu hình này ghi tên/email vào các commit mới. `--global` áp dụng cho mọi repo của tài khoản máy hiện tại. Email hiển thị trong commit có thể công khai trên GitHub; chọn email bạn muốn dùng cho hoạt động GitHub.

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

# Bài 1 — Từ file đến commit đầu tiên

> Làm bài này trong repo học tập. Không dùng `reset --hard`, không xóa `.git`, và chưa cần kết nối GitHub.

## Mô hình cần thuộc

```text
Thư mục làm việc  --git add-->  Staging area  --git commit-->  Lịch sử local
      sửa file                     chọn nội dung                mốc đã lưu
```

- **Working directory**: nơi bạn sửa file.
- **Staging area / index**: vùng chọn chính xác phần thay đổi sẽ đi vào commit kế tiếp.
- **Commit history**: các mốc đã commit. Commit lưu ở máy trước; chưa tự gửi lên GitHub.

Câu thần chú: **sửa → xem → stage → xem lại → commit → xem lịch sử**.

## 1. Xem tình trạng trước khi làm

```bash
git status
```

Đọc trạng thái trước. Nếu repo đã có file khác đang sửa, đừng vô tình đưa file đó vào bài tập.

## 2. Tạo file luyện tập

Tạo file `practice.txt` trong repo với đúng một dòng:

```text
Lan dau hoc Git.
```

Lưu file, rồi chạy:

```bash
git status
```

Git thường báo `practice.txt` là **untracked** — file mới chưa được Git theo dõi. Git chưa đưa nó vào lịch sử.

## 3. Xem nội dung sắp thay đổi

```bash
git diff -- practice.txt
```

Với file untracked, `git diff` thường chưa hiện nội dung vì file chưa được stage. Đây là điểm dễ nhầm: hãy stage file rồi xem staged diff ở bước 5.

## 4. Đưa đúng file vào staging area

```bash
git add practice.txt
git status
```

`git add` chụp nội dung hiện tại của file vào staging area. Nếu sửa file sau lệnh `add`, cần `git add` lại để cập nhật nội dung sẽ commit.

Tập chỉ rõ tên file khi mới học. `git add .` đưa mọi thay đổi phù hợp bên dưới thư mục hiện tại vào stage, nên dễ gom nhầm file.

## 5. Kiểm tra phần đã stage

```bash
git diff --staged
```

Đọc kỹ phần hiện ra: đúng file, đúng dòng, không có nội dung bí mật hay file không liên quan? Nếu đúng mới commit.

## 6. Tạo commit

```bash
git commit -m "Add first Git practice file"
```

Commit tạo một mốc trong lịch sử local. `-m` theo sau là thông điệp ngắn mô tả thay đổi. Dùng động từ và nói rõ thay đổi, ví dụ `Add contact page` hoặc `Fix spelling in guide`.

## 7. Xác nhận kết quả

```bash
git status
git log --oneline -5
```

Mong đợi: working tree sạch (không còn thay đổi chưa commit) và commit vừa tạo xuất hiện ở đầu lịch sử. Nếu commit báo chưa cấu hình tên/email, quay lại Bài 0 mục 4.

## Drill A — Lặp lại ba lần

Mỗi vòng, thêm đúng một dòng mới vào `practice.txt`, với nội dung lần lượt:

```text
Toi biet xem trang thai.
Toi biet chon file de stage.
Toi biet tao commit local.
```

Mỗi lần làm đủ chuỗi sau, không bỏ bước kiểm tra:

```bash
git status
git diff -- practice.txt
git add practice.txt
git diff --staged
git commit -m "Describe this change"
git status
git log --oneline -5
```

Ở lần đầu `git diff -- practice.txt` hãy chú ý nó hiển thị thay đổi so với commit gần nhất. Trước khi `git add`, diff là phần chưa stage; sau `add`, dùng `git diff --staged` để xem nội dung đã chọn cho commit.

## Drill B — Tự giải thích không nhìn tài liệu

- `git add` có gửi code lên GitHub không? **Không.** Nó chọn nội dung cho commit kế tiếp.
- `git commit` có gửi code lên GitHub không? **Không.** Nó tạo mốc trong lịch sử local.
- Lệnh nào kiểm tra nội dung đã stage? **`git diff --staged`.**
- Vì sao chạy `git status` sau commit? **Để xác nhận trạng thái và biết còn thay đổi chưa lưu không.**

## Lỗi thường gặp

- **Commit báo “nothing to commit”**: có thể chưa sửa file, chưa `git add`, hoặc không có thay đổi mới. Chạy `git status`.
- **Sửa file rồi nhưng commit không có sửa đổi mới**: chạy lại `git add <tên-file>` sau khi sửa.
- **Stage nhầm file**: chưa commit thì bỏ file khỏi stage bằng `git restore --staged <tên-file>`, rồi xác nhận với `git status`. Lệnh này giữ nội dung trong working directory.
- **Không chắc lệnh làm gì**: dừng lại, chạy `git status`, đọc trợ giúp bằng `git help <lệnh>` (ví dụ `git help add`), rồi mới tiếp tục.

---

## Nhật ký luyện tập

Sau mỗi buổi, ghi ngày và tự chấm 0–2 điểm cho mỗi mục: 0 = chưa làm được; 1 = cần nhìn tài liệu; 2 = tự làm và giải thích được.

| Ngày | Đọc status trước | Xem diff | Stage đúng | Kiểm tra staged | Commit rõ nghĩa | Giải thích được ba vùng |
|---|---:|---:|---:|---:|---:|---:|
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |

Mục tiêu: đạt 2 ở mọi cột trong ba buổi khác nhau trước khi sang Bài 2.

## Quy tắc an toàn trong các bài tiếp theo

1. Đọc `git status` trước và sau thao tác quan trọng.
2. Đọc diff trước khi commit hoặc gửi lên GitHub.
3. Không chạy lệnh phá lịch sử nếu chưa hiểu chính xác nó sẽ bỏ/thay đổi điều gì.
4. Commit nhỏ, một mục đích mỗi commit.
5. Không commit mật khẩu, token, khóa riêng, dữ liệu cá nhân hay file cấu hình chứa bí mật.

---

## Tiến độ

- [x] Bài 0 — quan sát và khái niệm nền
- [x] Bài 1 — sửa, stage, commit, kiểm tra
- [ ] Bài 2 — diff và sửa nhầm
- [ ] Bài 3 — lịch sử và `.gitignore`
- [ ] Bài 4 — kết nối GitHub an toàn
- [ ] Bài 5 trở đi — nhánh, merge, conflict, PR, vai trò nhóm và cứu hộ

**Bài tiếp theo:** Bài 2 — thay đổi nào đang nằm ở đâu, và làm sao bỏ một lựa chọn mà không mất phần còn lại.
