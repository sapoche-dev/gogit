# Bài 2 — Xem thay đổi, bỏ stage và khôi phục file

[Quay lại Bài 1](bai-1.md)

## Chuẩn bị

Mở terminal ở thư mục gốc của repo, nơi có `practice.txt` từ Bài 1. Các lệnh trong bài dùng đường dẫn tính từ thư mục đó.

```bash
git ls-files -- practice.txt
git status --short -- practice.txt
```

`git ls-files -- practice.txt` liệt kê đường dẫn khớp trong danh sách file Git theo dõi. Lệnh cần in `practice.txt` để xác nhận file từ Bài 1 đã được theo dõi.

Đọc `git status --short -- practice.txt` theo từng phần.

| Phần | Ý nghĩa |
|---|---|
| `git` | Chạy chương trình Git. |
| `status` | Xem trạng thái thay đổi của các file. |
| `--short` | Hiện kết quả dạng ngắn, mỗi file một dòng. |
| `--` | Đánh dấu chỗ kết thúc phần tùy chọn. Trong lệnh này, Git đọc phần sau dấu đó là đường dẫn. |
| `practice.txt` | Chỉ xem trạng thái của file này, tính từ thư mục đang đứng. |

Khi `practice.txt` đã được theo dõi và lệnh `status` trả về trống, file đã sẵn sàng để luyện. Nếu lệnh in một dòng trạng thái, hãy kiểm tra và lưu lại phần sửa cần giữ trước khi bắt đầu bài.

Trong kết quả dạng ngắn, hai cột đầu cho biết file thay đổi ở đâu. `M` là *modified* (đã sửa).

```text
 M practice.txt    M ở cột phải: thư mục làm việc có phần sửa chưa stage
M  practice.txt    M ở cột trái: staging area có phần sửa đã stage
MM practice.txt    Cả hai cột: cùng một file có phần đã stage và phần chưa stage
```

## 1. Sửa một dòng rồi xem thay đổi

Thêm dòng sau vào cuối `practice.txt` và lưu file.

```text
Dang luyen git diff.
```

```bash
git status --short -- practice.txt
git diff -- practice.txt
```

`diff` hiển thị sự khác nhau giữa hai bản nội dung. Trong `git diff -- practice.txt`, Git so sánh file trên đĩa với bản trong staging area, nên bạn sẽ thấy phần sửa chưa stage.

Bạn sẽ thấy ` M practice.txt` và dòng `+Dang luyen git diff.`. Trong diff, `+` ở đầu một dòng nội dung chỉ dòng được thêm, còn `-` chỉ dòng bị bỏ. Nếu Git mở màn hình xem diff, nhấn `q` để quay lại terminal.

## 2. Stage dòng thứ nhất

```bash
git add -- practice.txt
git status --short -- practice.txt
git diff --staged -- practice.txt
```

- `git add -- practice.txt` chọn toàn bộ nội dung hiện tại của file này cho commit tiếp theo. Bạn sẽ thấy `M  practice.txt`.
- Trong `git diff --staged -- practice.txt`, `--staged` bảo Git so sánh staging area với commit hiện tại. Kết quả hiện dòng vừa được stage.

Hãy thử chạy lại `git diff -- practice.txt`. Kết quả trống vì nội dung file trên đĩa đang giống bản trong staging area.

## 3. Thêm dòng thứ hai sau khi stage

Giữ dòng thứ nhất, thêm dòng sau vào cuối file rồi lưu.

```text
Dang luyen git restore.
```

```bash
git status --short -- practice.txt
git diff -- practice.txt
git diff --staged -- practice.txt
```

Bạn sẽ thấy `MM practice.txt`. Trong hai diff, hãy tìm các dòng nội dung bắt đầu bằng `+`.

- `git diff` so sánh file trên đĩa với bản đã stage. Dòng có dấu `+` là dòng thứ hai.
- `git diff --staged` so sánh bản đã stage với commit hiện tại. Dòng có dấu `+` là dòng thứ nhất.

Nếu tạo commit ở thời điểm này bằng `git commit` với đúng phần đã stage, commit sẽ chứa dòng thứ nhất. Trong bài này, cứ tiếp tục sang bước bỏ stage.

## 4. Bỏ stage và giữ cả hai dòng

```bash
git restore --staged -- practice.txt
git status --short -- practice.txt
git diff -- practice.txt
git diff --staged -- practice.txt
```

`restore` khôi phục nội dung từ một bản Git đã giữ. Với `restore --staged`, tùy chọn `--staged` chỉ định nơi cần khôi phục là staging area. Git lấy nội dung từ commit hiện tại để cập nhật staging area và giữ file trên đĩa.

Trạng thái trở lại ` M practice.txt`, cả hai dòng xuất hiện trong diff chưa stage, còn diff đã stage trống.

## 5. Xóa hai dòng thử để làm lại

Chạy `git diff -- practice.txt` và xác nhận phần sửa chỉ gồm hai dòng luyện tập. Sau đó chạy:

```bash
git restore -- practice.txt
git status --short -- practice.txt
```

Trong `git restore -- practice.txt`, `restore` mặc định khôi phục file trên đĩa từ staging area. Lệnh ghi đè phần sửa chưa stage. Sau bước 4, staging area đã khớp với commit hiện tại, nên hai dòng luyện tập biến mất và lệnh kiểm tra trạng thái trả về trống.

## Tự luyện lại

Làm lại bước 1–5. Sau mỗi thao tác, đoán vị trí của chữ `M` rồi mới chạy `git status --short -- practice.txt` để đối chiếu.

1. Ở bước 3, lệnh nào cho xem phần sẽ vào commit? **`git diff --staged -- practice.txt`**.
2. Nếu chỉ muốn bỏ stage mà giữ phần sửa, dùng lệnh nào? **`git restore --staged -- practice.txt`**.
3. Nếu chạy `git restore -- practice.txt` ngay ở bước 3, dòng nào còn? **Dòng thứ nhất đã stage**. Git chép bản đã stage vào file, nên dòng thứ hai biến mất và trạng thái thành `M  practice.txt`.

**Đọc tiếp** [Bài 3 — Đọc lịch sử commit và dùng .gitignore](bai-3.md).
