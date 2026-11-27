# Bài 5 — Tạo, chuyển, đổi tên và xóa nhánh

[Quay lại Bài 4](bai-4.md)

## Mục tiêu

- Biết nhánh hiện tại là nhánh nào và HEAD đang ở đâu.
- Tạo một nhánh từ commit hiện tại.
- Quan sát file thay đổi khi chuyển nhánh.
- Đổi tên nhánh và xóa nhánh local đã hoàn thành.
- Chuẩn bị một nhánh có commit để thực hành merge ở Bài 6.

Mỗi lần chỉ làm một bước. Đoán kết quả trước, chạy lệnh rồi đối chiếu.

## 1. Kiểm tra repo đang học

Mở terminal ở thư mục gốc `C:\Users\Hello\Desktop\gogit`.

```bash
git status --short --branch
git branch -vv
```

| Phần | Ý nghĩa |
|---|---|
| `status` | Xem nhánh hiện tại và thay đổi của file. |
| `--short` | Hiện các file với hai cột trạng thái. |
| `--branch` | Thêm thông tin nhánh ở đầu kết quả. |
| `branch` | Liệt kê nhánh local khi dùng dạng này. |
| `-vv` | Hiện thêm commit và nhánh upstream nếu có. |

Dấu `*` trong danh sách branch đánh dấu nhánh đang làm việc.

Khi soạn bài, nhánh đang mở là `codex/lession-4-remote`. Bạn đã dùng chữ `lession` trong tên nhánh, nên hãy đọc đúng tên thực tế khi thao tác. Các tên nhánh của bài tập dưới đây dùng tiền tố `codex/lab-`.

## 2. Tạo bản sao luyện tập riêng

Chạy từ repo `gogit`.

```bash
git clone --no-hardlinks . ../learngit-branch-lab
cd ../learngit-branch-lab
git remote -v
```

| Phần | Ý nghĩa |
|---|---|
| `clone` | Tạo bản sao repo cùng lịch sử commit đã lưu. |
| `--no-hardlinks` | Sao chép các file dữ liệu Git thành các file độc lập khi clone từ repo local. |
| `.` | Repo nguồn ở thư mục hiện tại, tức gogit. |
| `../learngit-branch-lab` | Thư mục mới nằm cạnh gogit trong Desktop. |
| `cd` | Lệnh terminal chuyển thư mục làm việc. |
| `remote -v` | Xem tên và địa chỉ remote của bản sao. |

Clone từ repo local lấy các commit đã lưu. Những sửa đổi còn trong working directory của repo nguồn cần được commit trước nếu bạn muốn đưa chúng vào bản sao này. Bạn có thể tiếp tục đọc các file hướng dẫn trong cửa sổ soạn thảo đang mở tại repo nguồn.

Origin trong bản sao vừa tạo trỏ tới repo local nguồn. Gỡ kết nối đó trong bản sao luyện tập.

```bash
git remote remove origin
git remote -v
```

`remote remove origin` gỡ cấu hình remote tên origin và các mốc theo dõi thuộc remote đó. Lệnh tiếp theo trả về trống, xác nhận bài tập dùng một repo local độc lập.

Nếu thư mục `learngit-branch-lab` đã có từ lần trước, vào thư mục đó và kiểm tra `git status` để tiếp tục. Để làm lại từ đầu, dùng một tên thư mục mới như `learngit-branch-lab-02` trong cả lệnh clone và cd.

## 3. Tạo nhánh nền cho bài tập

Chạy bên trong bản sao luyện tập.

```bash
git switch -c codex/lab-base
git branch --show-current
```

| Phần | Ý nghĩa |
|---|---|
| `switch` | Chuyển sang một nhánh. |
| `-c` | Tạo nhánh mới và chuyển sang nhánh đó. |
| `codex/lab-base` | Nhánh nền dùng để nhận các thay đổi trong bài tập. |
| `branch --show-current` | In tên nhánh hiện tại. |

Bạn cần thấy `codex/lab-base`.

Nhánh là một tên trỏ tới commit. Khi tạo nhánh mới, Git đặt tên mới tại commit hiện tại. Khi commit trên nhánh đó, Git đưa nhánh tiến tới commit mới.

Thông thường, HEAD trỏ tới nhánh đang được chọn. Vì vậy khi đang ở lab-base, HEAD đi theo lab-base.

## 4. Tạo nhánh để làm một thay đổi nhỏ

```bash
git switch -c codex/lab-note
git branch --show-current
```

Nhánh mới `codex/lab-note` bắt đầu từ cùng commit với `codex/lab-base`.

Tạo file `branch-note.txt` trong thư mục gốc của bản sao luyện tập, nhập một dòng sau và lưu. Nhấn Enter sau dòng cuối để file có ký tự xuống dòng cuối file.

```text
Trang thai: ban nhap.
```

```bash
git status --short -- branch-note.txt
git add -- branch-note.txt
git diff --staged -- branch-note.txt
git commit -m "Add branch practice note"
```

| Lệnh hoặc phần lệnh | Ý nghĩa |
|---|---|
| `status --short -- branch-note.txt` | Xem trạng thái riêng file này. File mới hiện `??`. |
| `add -- branch-note.txt` | Chọn nội dung file để đưa vào commit. |
| `diff --staged -- branch-note.txt` | Xem phần đã chọn của file so với commit hiện tại. |
| `commit` | Lưu nội dung staging area thành commit mới. |
| `-m` | Lấy thông điệp commit từ chuỗi trong ngoặc kép. |
| `--` | Đánh dấu phần đường dẫn trong các lệnh hỗ trợ cú pháp này. |

Bạn đã tạo một commit mới trên nhánh lab-note. Nếu staging area đang chứa nội dung khác, đọc toàn bộ bằng `git diff --staged` trước khi commit vì commit lưu tất cả phần được stage.

## 5. Chuyển nhánh và quan sát file

```bash
git status --short
git switch codex/lab-base
git branch --show-current
```

Chỉ chuyển sang bước tiếp khi trạng thái đã sạch. Khi có phần sửa cần giữ, hãy lưu thành commit phù hợp trước để dễ quan sát file của từng nhánh.

`switch codex/lab-base` chuyển về nhánh đã tồn tại. Mở thư mục bản sao bằng trình soạn thảo và quan sát file `branch-note.txt`. File biến mất vì commit ở đầu lab-base chưa chứa file ấy.

```bash
git switch codex/lab-note
```

File `branch-note.txt` xuất hiện trở lại với dòng bạn đã commit. Git cập nhật working directory theo commit của nhánh được chọn.

## 6. Đọc lịch sử hai nhánh

```bash
git --no-pager log --oneline --graph --decorate codex/lab-base codex/lab-note
```

| Phần | Ý nghĩa |
|---|---|
| `--no-pager` | In trực tiếp ra terminal. |
| `log` | Đọc lịch sử commit. |
| `--oneline` | Mỗi commit trên một dòng. |
| `--graph` | Vẽ các đường liên kết lịch sử bằng ký tự. |
| `--decorate` | Hiện tên nhánh và HEAD cạnh commit tương ứng. |
| Hai tên nhánh ở cuối | Xem lịch sử đi tới được từ cả hai nhánh đó. |

Kết quả có dạng sau, với mã commit của bạn sẽ khác.

```text
* abc1234 (HEAD -> codex/lab-note) Add branch practice note
* def5678 (codex/lab-base) ...
```

Đọc `HEAD -> codex/lab-note` là bạn đang ở nhánh lab-note, nhánh này đang trỏ tới commit abc1234. Lab-base vẫn ở commit trước.

## 7. Đổi tên nhánh local

Khi đang ở lab-note, chạy lệnh sau.

```bash
git branch -m codex/lab-note-renamed
git branch --show-current
```

`branch -m` với một tên mới đổi tên nhánh hiện tại. Commit của nhánh tiếp tục được giữ dưới tên mới.

Đổi về tên ban đầu để Bài 6 dùng đúng tên.

```bash
git branch -m codex/lab-note
```

Việc đổi tên nhánh local cần được xử lý thêm ở remote nếu trước đó bạn đã push nhánh đó. Repo luyện tập này đã gỡ remote ở bước 2.

## 8. Tạo và xóa một nhánh tạm

```bash
git switch codex/lab-base
git branch codex/lab-draft
git branch
```

`branch codex/lab-draft` tạo nhánh tại commit hiện tại và bạn tiếp tục đứng ở lab-base. Quan sát dấu `*` để xác nhận.

```bash
git branch -d codex/lab-draft
git branch
```

`-d` xóa nhánh local sau khi Git kiểm tra điều kiện đã được gộp. Trong bài này lab-draft trỏ tới cùng commit với nhánh hiện tại và chưa có upstream, nên thao tác được chấp nhận.

Nếu Git từ chối xóa một nhánh vì có commit chưa được gộp, hãy xem lại lịch sử và mục đích giữ commit đó. Bài 6 sẽ hướng dẫn gộp nhánh.

Cuối Bài 5, bạn cần đứng ở lab-base và còn nhánh lab-note chứa file luyện tập để merge.

## Tự kiểm tra

1. Lệnh nào in tên nhánh hiện tại?
2. `git branch codex/lab-draft` và `git switch -c codex/lab-draft` khác nhau ở thao tác nào?
3. Vì sao branch-note.txt biến mất khi chuyển về lab-base?
4. Trong `HEAD -> codex/lab-note`, bạn đang đứng ở nhánh nào?
5. Lệnh nào đổi tên nhánh local hiện tại?

<details>
<summary>Xem đáp án sau khi tự trả lời</summary>

1. `git branch --show-current`.
2. Dạng branch tạo tên nhánh. Dạng switch -c vừa tạo vừa chuyển sang nhánh mới.
3. Commit ở đầu lab-base chưa chứa file đó.
4. Codex/lab-note.
5. `git branch -m TEN_MOI`, thay TEN_MOI bằng tên bạn chọn.

</details>

**Đọc tiếp** [Bài 6 — Merge và xử lý conflict](bai-6.md).
