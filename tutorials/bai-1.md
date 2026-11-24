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

Tạo file `practice.txt` ở thư mục gốc của repo với đúng một dòng:

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

Trong lệnh trên, `--` đánh dấu rằng `practice.txt` phía sau là một đường dẫn. Git chỉ hiển thị phần sửa chưa stage của file được chỉ định.

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

### Ai là tác giả commit? (`user.name` và `user.email`)

Git ghi `user.name` và `user.email` vào metadata của commit. Đây là tên/email tác giả hiển thị trong lịch sử; chúng không phải username hay mật khẩu đăng nhập GitHub, và không quyết định quyền `push`.

Kiểm tra cấu hình đang có và nguồn cấu hình:

```bash
git config --show-origin --get-regexp "^user\.(name|email)$"
git log -1 --format="%an <%ae>"
```

- `--show-origin` in nguồn của mỗi giá trị cấu hình, chẳng hạn đường dẫn tới file cấu hình. `--get-regexp` lọc tên các mục theo mẫu trong ngoặc kép, ở đây là `user.name` và `user.email`.
- `git log` đọc lịch sử commit. `-1` chỉ lấy một commit. `--format` đặt cách hiển thị, với `%an` là tên tác giả và `%ae` là email tác giả.

`--global` áp dụng mặc định cho mọi repo trên máy. Nếu chỉ muốn đổi danh tính cho repo hiện tại, bỏ `--global`:

```bash
# Mọi repo trên máy
git config --global user.name "Tên hiển thị"
git config --global user.email "email@example.com"

# Chỉ repo hiện tại; ghi đè cấu hình global tại đây
git config user.name "Tên cho repo này"
git config user.email "email@example.com"
```

GitHub dùng email của commit để liên kết commit với tài khoản. Dùng email đã thêm vào GitHub, hoặc địa chỉ `noreply` GitHub cấp nếu muốn ẩn email cá nhân. Email mới chỉ ảnh hưởng commit tạo sau khi đổi; commit cũ giữ nguyên tác giả/email đã ghi lúc tạo.

## 7. Xác nhận kết quả

```bash
git status
git log --oneline -5
```

Trong `git log --oneline -5`, `--oneline` in mỗi commit trên một dòng và `-5` giới hạn kết quả ở tối đa năm commit. Commit vừa tạo sẽ xuất hiện đầu danh sách. Nếu commit báo chưa cấu hình tên/email, quay lại [Bài 0, mục 4](bai-0.md#4-thiết-lập-tên-hiển-thị-cho-commit-chỉ-làm-nếu-chưa-thiết-lập).

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

## Ghi chú — `main` và `origin/main`

Hai tên này thường trỏ đến hai mốc khác nhau:

- `main` là nhánh local trên máy bạn. Đây là nhánh bạn đang làm việc.
- `origin` là tên viết tắt của remote GitHub đã cấu hình trong repo.
- `origin/main` là mốc local Git dùng để ghi nhớ commit mới nhất mà nó đã thấy trên nhánh `main` ở `origin`. Nó không phải bản thân nhánh trên GitHub và không tự cập nhật khi GitHub thay đổi.

Khi hai bên đồng bộ:

```text
Máy bạn                              GitHub (remote tên origin)
main       ───────────────┐          main
origin/main ──────────────┴────────▶ commit A
```

Thực tế, `main` và `origin/main` là hai nhãn local cùng trỏ đến commit A; nhánh `main` trên GitHub cũng đang ở A.

### Khi thay đổi xảy ra

- Bạn commit trên máy: `main` tiến lên commit B; `origin/main` vẫn ở A cho đến khi đồng bộ. `git status -sb` có thể báo `ahead 1`.
- GitHub nhận commit mới từ nơi khác: GitHub `main` tiến lên B, nhưng máy bạn vẫn nhớ `origin/main` ở A cho đến khi chạy `git fetch origin`. Sau fetch, `origin/main` cập nhật thành B; `main` local chưa tự thay đổi. `git status -sb` có thể báo `behind 1`.
- `git push` gửi commit local lên GitHub.
- `git pull` lấy thay đổi mới về và tích hợp vào nhánh hiện tại; về khái niệm, nó gồm fetch rồi tích hợp (merge hoặc rebase tùy cấu hình).

### Bộ lệnh quan sát

```bash
git status -sb
git branch -vv
git fetch origin
git status -sb
```

- `git status -sb` ghép `-s` (dạng ngắn) và `-b` (hiện thông tin nhánh).
- `git branch -vv` liệt kê các nhánh local kèm commit gần nhất, lời nhắn commit và nhánh theo dõi nếu có.
- `git fetch origin` lấy dữ liệu từ remote tên `origin` và cập nhật các mốc theo dõi như `origin/main`. So sánh kết quả `git status -sb` trước và sau khi chạy để xem nhánh local đi trước hay đi sau.

**Mẹo nhớ:** `main` = nhánh local mình làm; `origin/main` = dấu mốc local về nhánh main trên remote.

Khi hai nhánh cùng trỏ đến một commit, chúng đã đồng bộ theo thông tin Git đang có. Việc stage file chỉ cập nhật staging area. Tạo commit mới trên nhánh local thì mốc commit của nhánh đó mới tiến lên.

---

## Nhật ký luyện tập

Sau mỗi buổi, ghi ngày và tự chấm 0–2 điểm cho mỗi mục: 0 = chưa làm được; 1 = cần nhìn tài liệu; 2 = tự làm và giải thích được.

| Ngày | Đọc status trước | Xem diff | Stage đúng | Kiểm tra staged | Commit rõ nghĩa | Giải thích được ba vùng |
|---|---:|---:|---:|---:|---:|---:|
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |

Bạn có thể dùng bảng này để theo dõi tiến bộ qua các buổi thực hành.

## Quy tắc an toàn trong các bài tiếp theo

1. Đọc `git status` trước và sau thao tác quan trọng.
2. Đọc diff trước khi commit hoặc gửi lên GitHub.
3. Không chạy lệnh phá lịch sử nếu chưa hiểu chính xác nó sẽ bỏ/thay đổi điều gì.
4. Commit nhỏ, một mục đích mỗi commit.
5. Không commit mật khẩu, token, khóa riêng, dữ liệu cá nhân hay file cấu hình chứa bí mật.

**Đọc tiếp** [Bài 2 — Xem thay đổi và sửa nhầm an toàn](bai-2.md).
