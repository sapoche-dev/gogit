# Bài 2 — Xem thay đổi, bỏ stage và khôi phục file

[Quay lại Bài 1](bai-1.md)

## Chuẩn bị

Mở terminal ở thư mục gốc của repo, nơi có `practice.txt` từ Bài 1. Các lệnh trong bài dùng đường dẫn tính từ thư mục đó.

```bash
git ls-files -- practice.txt
git status --short -- practice.txt
```

Lệnh đầu phải in `practice.txt`, nghĩa là Git đang theo dõi file. Lệnh thứ hai cần trả về trống, nghĩa là file đang ở trạng thái sẵn sàng để luyện. Nếu lệnh thứ hai in một dòng trạng thái, hãy kiểm tra và lưu lại phần sửa cần giữ trước khi bắt đầu bài. Phần `-- practice.txt` giới hạn các lệnh trong bài vào file luyện tập.

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

Bạn sẽ thấy ` M practice.txt` và dòng `+Dang luyen git diff.`. Trong diff, `+` ở đầu một dòng nội dung chỉ dòng được thêm, còn `-` chỉ dòng bị bỏ. Nếu Git mở màn hình xem diff, nhấn `q` để quay lại terminal.

## 2. Stage dòng thứ nhất

```bash
git add -- practice.txt
git status --short -- practice.txt
git diff --staged -- practice.txt
```

`git add` đưa nội dung hiện tại của file vào staging area. Bạn sẽ thấy `M  practice.txt`, và diff của phần đã stage hiện dòng vừa thêm. Hãy thử chạy lại `git diff -- practice.txt`. Lần này kết quả trống vì nội dung file trên đĩa đang giống bản trong staging area.

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

Git đưa bản trong staging area về nội dung ở commit hiện tại và giữ file trên đĩa. Trạng thái trở lại ` M practice.txt`, cả hai dòng xuất hiện trong diff chưa stage, còn diff đã stage trống.

## 5. Xóa hai dòng thử để làm lại

Chạy `git diff -- practice.txt` và xác nhận phần sửa chỉ gồm hai dòng luyện tập. Sau đó chạy:

```bash
git restore -- practice.txt
git status --short -- practice.txt
```

`git restore -- practice.txt` chép bản trong staging area vào file trên đĩa, nên sẽ xóa phần sửa chưa stage của file. Sau bước 4, staging area đã khớp với commit hiện tại. Vì vậy hai dòng luyện tập biến mất và lệnh kiểm tra trạng thái trả về trống.

## Tự luyện lại

Làm lại bước 1–5. Sau mỗi thao tác, đoán vị trí của chữ `M` rồi mới chạy `git status --short -- practice.txt` để đối chiếu.

1. Ở bước 3, lệnh nào cho xem phần sẽ vào commit? **`git diff --staged -- practice.txt`**.
2. Nếu chỉ muốn bỏ stage mà giữ phần sửa, dùng lệnh nào? **`git restore --staged -- practice.txt`**.
3. Nếu chạy `git restore -- practice.txt` ngay ở bước 3, dòng nào còn? **Dòng thứ nhất đã stage**. Git chép bản đã stage vào file, nên dòng thứ hai biến mất và trạng thái thành `M  practice.txt`.

Bài 3 sẽ dùng `git log` và `git show` để đọc lịch sử commit.
