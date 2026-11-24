# Bài 3 — Đọc lịch sử commit và dùng .gitignore

[Quay lại Bài 2](bai-2.md)

## Mục tiêu

- Đọc mã và thông điệp của các commit đã tạo.
- Xem một commit thay đổi những file và dòng nào.
- Đọc bản cũ của file bằng lệnh.
- Viết thông điệp commit rõ nghĩa.
- Dùng `.gitignore` cho file tạm trong repo học tập.

Mở terminal ở thư mục gốc của repo, nơi chứa `practice.txt` và thư mục `tutorials`. Làm từng bước, đọc kết quả rồi mới sang bước tiếp theo.

## 1. Xem trạng thái trước khi đọc lịch sử

```bash
git status --short --branch
```

| Phần | Ý nghĩa |
|---|---|
| `git status` | Xem trạng thái repo và các file thay đổi. |
| `--short` | Hiện mỗi file trên một dòng với hai cột trạng thái. |
| `--branch` | Thêm dòng thông tin nhánh ở đầu. |

Dòng `## main...origin/main [ahead 6]` có nghĩa nhánh local có sáu commit nhiều hơn mốc `origin/main` Git đang lưu trên máy. Con số của bạn có thể khác.

Nếu thấy các file `tutorials/bai-*.md` có chữ `M`, đó có thể là phần tài liệu vừa được cập nhật. Các bước 2–5 đọc lịch sử đã commit nên vẫn thực hành được khi còn file đang sửa.

## 2. Liệt kê năm commit gần nhất

```bash
git --no-pager log --oneline -5
```

| Phần | Ý nghĩa |
|---|---|
| `git` | Chạy chương trình Git. |
| `--no-pager` | In trực tiếp vào terminal để bạn xem kết quả ngay. Tùy chọn này đứng trước `log`. |
| `log` | Liệt kê lịch sử commit, mặc định bắt đầu từ commit hiện tại. |
| `--oneline` | Hiện mỗi commit trên một dòng, gồm mã rút gọn và tiêu đề commit. |
| `-5` | Hiện tối đa năm commit. |

Một dòng trong repo học của bạn có thể hiện như sau.

```text
6cfcddd Refactor folders
```

`6cfcddd` là mã rút gọn của commit, còn `Refactor folders` là tiêu đề do người tạo commit nhập. Git thường liệt kê commit gần nhất ở dòng đầu.

**Thực hành** Ghi lại mã commit ở dòng đầu và đọc thông điệp của nó. Nếu muốn xem thêm, đổi `-5` thành `-10`.

## 3. Xem commit gần nhất đã thay đổi gì

```bash
git --no-pager show --stat HEAD
```

| Phần | Ý nghĩa |
|---|---|
| `show` | Hiển thị thông tin của đối tượng Git được chọn, ở đây là một commit. |
| `--stat` | Hiện danh sách file cùng thống kê số dòng thay đổi. |
| `HEAD` | Chỉ commit hiện tại. Khi đang ở nhánh `main`, đó thường là commit ở đầu nhánh `main`. |

Kết quả gồm mã commit, tác giả, thời điểm tạo, thông điệp và thống kê file. Các dòng trong working directory đang sửa sẽ được đọc bằng `git diff`.

Tiếp theo, xem phần thay đổi chi tiết của commit.

```bash
git --no-pager show HEAD
```

Ở dạng này, Git hiện cả phần diff. Với các dòng nội dung, `+` chỉ dòng được thêm và `-` chỉ dòng bị bỏ. Phần `---` và `+++` ở đầu diff đặt tên bản cũ và bản mới của file.

**Thực hành** Tìm tên một file đã thay đổi và đọc nội dung thay đổi của nó.

Để xem commit khác, thay `HEAD` bằng mã bạn lấy ở bước 2. Ví dụ dưới đây xem commit `6cfcddd` trong repo học.

```bash
git --no-pager show --stat 6cfcddd
```

## 4. Tìm các commit đã thay đổi practice.txt

```bash
git --no-pager log --oneline -5 -- practice.txt
```

`--` đánh dấu phần đường dẫn ở cuối lệnh. `practice.txt` giới hạn lịch sử vào các commit liên quan tới file này. Các tùy chọn `--oneline` và `-5` giữ ý nghĩa như bước 2.

Sao chép mã commit đầu tiên trong danh sách đó. Dùng nó thay chữ `MA_COMMIT` trong lệnh sau trước khi chạy.

```bash
git --no-pager show MA_COMMIT -- practice.txt
```

Git hiện thông tin commit được chọn và phần thay đổi của riêng `practice.txt`.

**Thực hành** Tìm commit đã thêm dòng `Toi biet xem trang thai.`. Có thể cần xem vài mã trong lịch sử của file.

## 5. Đọc nội dung file tại một commit

```bash
git --no-pager show HEAD:practice.txt
```

Trong `HEAD:practice.txt`, phần trước dấu `:` chọn commit và phần sau chọn đường dẫn bên trong commit đó. Kết quả là toàn bộ nội dung `practice.txt` đã lưu trong commit hiện tại.

Đọc bản ở commit liền trước.

```bash
git --no-pager show "HEAD~1:practice.txt"
```

`HEAD~1` chỉ commit cha thứ nhất của commit hiện tại. Ngoặc kép bao quanh cả biểu thức giúp truyền nó thành một đối số cho Git trong PowerShell. Lệnh cần lịch sử có ít nhất một commit cha và file tồn tại trong commit được chọn.

Hai kết quả có thể giống nhau nếu commit mới nhất chỉ thay đổi tài liệu. Bạn cũng có thể thay `HEAD~1` bằng mã commit tìm được ở bước 4.

**Thực hành** Đọc `practice.txt` trong commit `52a0685`, là commit thêm file đầu tiên của repo học.

```bash
git --no-pager show 52a0685:practice.txt
```

Nếu Git mở trình xem ở các lệnh bạn tự gõ thiếu `--no-pager`, nhấn `q` để quay lại terminal.

## 6. Viết thông điệp commit rõ nghĩa

Trong lịch sử có một số commit mang tiêu đề `Describe this change`. Đó là câu ví dụ của Bài 1 mà bạn đã dùng khi luyện tập. Với các commit tiếp theo, hãy viết cụ thể nội dung vừa sửa.

| Nội dung bạn vừa làm | Tiêu đề gợi ý |
|---|---|
| Thêm hướng dẫn về git log và git show | `Add lesson 3 on Git history` |
| Giải thích hai cột trong git status | `Explain status columns in lesson 2` |
| Thêm quy tắc bỏ qua file tạm | `Ignore local practice note` |

Cú pháp dưới đây là mẫu tham khảo khi bạn đã kiểm tra phần được stage.

```bash
git commit -m "Ignore local practice note"
```

`commit` lưu nội dung đã stage thành một mốc mới. `-m` lấy chuỗi trong ngoặc kép làm thông điệp của commit. Bạn sẽ dùng mẫu này khi muốn lưu quy tắc ở cuối bài.

## 7. Tạo file tạm và bỏ qua bằng .gitignore

`.gitignore` là file chứa các mẫu đường dẫn mà Git bỏ qua đối với file chưa được theo dõi. Nó giúp giữ các file tạm khỏi danh sách file mới khi bạn làm việc.

Tạo file `git-local-note.tmp` ở thư mục gốc repo bằng trình soạn thảo. Ghi một dòng sau rồi lưu.

```text
Ghi chu tam de luyen .gitignore.
```

```bash
git status --short -- git-local-note.tmp
```

Bạn cần thấy dòng sau.

```text
?? git-local-note.tmp
```

`??` nghĩa là file chưa được Git theo dõi. Nếu file đã tồn tại trước bài học hoặc đã được theo dõi, hãy chọn một tên file tạm mới và dùng tên đó nhất quán trong phần còn lại.

Tạo file tên chính xác là `.gitignore` ở thư mục gốc repo. Nếu đã có file này, thêm dòng sau vào cuối và giữ các quy tắc hiện có.

```gitignore
/git-local-note.tmp
```

Dấu `/` ở đầu neo mẫu vào thư mục chứa `.gitignore`. Trong trường hợp này, quy tắc khớp file `git-local-note.tmp` ngay thư mục gốc repo.

Lưu `.gitignore`, rồi chạy lại.

```bash
git status --short -- git-local-note.tmp
git status --short -- .gitignore
```

Lệnh đầu trả về trống vì file tạm được bỏ qua. Lệnh thứ hai cho biết trạng thái của chính `.gitignore`, thường là `??` nếu bạn vừa tạo nó, hoặc ` M` nếu nó đã được theo dõi và bạn vừa sửa.

## 8. Xác định quy tắc nào đang bỏ qua file

```bash
git check-ignore -v -- git-local-note.tmp
```

| Phần | Ý nghĩa |
|---|---|
| `check-ignore` | Kiểm tra đường dẫn có bị một quy tắc ignore bỏ qua hay không. |
| `-v` | Hiện chi tiết file quy tắc, số dòng và mẫu khớp. |
| `--` | Đánh dấu phần đường dẫn. |
| `git-local-note.tmp` | File bạn đang kiểm tra. |

Kết quả có thể có dạng sau, với số dòng phụ thuộc vị trí bạn thêm quy tắc.

```text
.gitignore:1:/git-local-note.tmp    git-local-note.tmp
```

Bạn có thể xóa dòng quy tắc vừa thêm trong trình soạn thảo, lưu lại rồi chạy `git status --short -- git-local-note.tmp` để thấy `??` xuất hiện trở lại. Thêm lại quy tắc và lưu để kết thúc bài.

Nếu file đã được Git theo dõi trước đó, `.gitignore` vẫn để Git tiếp tục theo dõi thay đổi của file ấy. Bài tập dùng file tạm mới để bạn quan sát quy tắc ngay từ lúc file chưa được theo dõi.

## Lưu quy tắc khi bạn đã hiểu bài

Nếu muốn lưu phần cấu hình vừa thực hành, kiểm tra toàn bộ staging area trước.

```bash
git diff --staged
```

Lệnh này cho xem mọi thay đổi đang được chọn cho commit tiếp theo. Hãy xử lý phần đã stage khác nếu bạn muốn chúng vào một commit riêng. Khi staging area đã sẵn sàng, chọn `.gitignore`.

```bash
git add -- .gitignore
git diff --staged
```

Đọc diff và xác nhận phần được chọn đúng với nội dung bạn định lưu. Sau đó tạo commit.

```bash
git commit -m "Ignore local practice note"
git --no-pager log --oneline -3
```

`add -- .gitignore` chọn file quy tắc cho commit. `log --oneline -3` giúp bạn nhìn lại commit mới ở đầu danh sách. File tạm tiếp tục nằm trên máy và được bỏ qua theo quy tắc đã viết.

## Tự kiểm tra

1. Muốn xem năm commit gần nhất trên mỗi dòng, bạn gõ lệnh nào?
2. `git show --stat HEAD` và `git show HEAD` cho bạn xem những mức chi tiết nào?
3. Trong `HEAD:practice.txt`, hai phần trước và sau dấu `:` nghĩa là gì?
4. Muốn tìm quy tắc bỏ qua file tạm, bạn dùng lệnh nào?
5. Nếu file đã được theo dõi, thêm nó vào `.gitignore` có khiến Git dừng theo dõi không?

<details>
<summary>Xem đáp án sau khi tự trả lời</summary>

1. `git --no-pager log --oneline -5`.
2. Dạng `--stat` hiện thống kê file, dạng mặc định hiện phần thay đổi chi tiết của commit.
3. `HEAD` chọn commit hiện tại, `practice.txt` chọn đường dẫn bên trong commit đó.
4. `git check-ignore -v -- git-local-note.tmp`.
5. Git tiếp tục theo dõi file đã được theo dõi.

</details>

Luyện lại các bước 2–5 với ba mã commit khác nhau. Với phần ignore, bỏ rồi thêm lại quy tắc để quan sát file tạm xuất hiện và biến mất trong kết quả `git status`.

**Đọc tiếp** [Bài 4 — Kết nối GitHub và đồng bộ repo](bai-4.md).
