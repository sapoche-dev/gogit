# Bài 6 — Merge và xử lý conflict

[Quay lại Bài 5](bai-5.md)

## Mục tiêu

- Hiểu nhánh nguồn và nhánh nhận khi chạy merge.
- Thực hành fast-forward.
- Tạo merge commit khi hai nhánh có commit riêng.
- Tạo conflict có chủ đích rồi sửa nội dung và hoàn thành merge.
- Biết hủy lần merge đang dở để làm lại.

Bài này tiếp tục trong bản sao `learngit-branch-lab` đã tạo ở Bài 5. Mở terminal tại thư mục bản sao. Bạn cần có lab-base và lab-note, trong đó lab-note chứa commit thêm `branch-note.txt`.

## 1. Kiểm tra trước khi gộp

```bash
git status --short --branch
git branch
```

`status --short --branch` hiện nhánh cùng trạng thái file. `branch` liệt kê nhánh local và đặt dấu `*` trước nhánh đang chọn.

Working directory cần sạch để bạn phân biệt rõ thay đổi do merge tạo ra. Nếu đang sửa file, hãy lưu công việc đó thành commit phù hợp trước khi bắt đầu.

Chuyển sang nhánh nhận.

```bash
git switch codex/lab-base
```

`switch` chọn nhánh đã tồn tại. Trong bài, lab-base đóng vai trò nhánh tích hợp để thử các đóng góp.

Với lệnh `git merge TEN_NHANH`, Git gộp lịch sử của TEN_NHANH vào nhánh bạn đang đứng. Vì vậy hãy đọc nhánh hiện tại trước khi merge.

## 2. Gộp bằng fast-forward

```bash
git merge --ff-only codex/lab-note
```

| Phần | Ý nghĩa |
|---|---|
| `merge` | Gộp lịch sử của nhánh được chọn vào nhánh hiện tại. |
| `--ff-only` | Chỉ chấp nhận khi nhánh nhận có thể tiến thẳng tới commit của nhánh nguồn. |
| `codex/lab-note` | Nhánh nguồn chứa commit thêm file từ Bài 5. |

Trước thao tác, lab-base ở commit A và lab-note ở commit B nằm sau A.

```text
A --- B
^     ^
base  note
```

Sau fast-forward, hai tên nhánh cùng trỏ tới B.

```text
A --- B
      ^
      base, note
```

Bạn sẽ thấy thông báo `Fast-forward` và file `branch-note.txt` xuất hiện trên lab-base. Git đưa nhánh nhận tới B. Lịch sử của bài tập vẫn dùng commit đã có từ lab-note.

Kiểm tra kết quả.

```bash
git branch --show-current
git --no-pager log --oneline --decorate -3
```

`branch --show-current` cần in `codex/lab-base`. Trong lệnh log, `--no-pager` in trực tiếp, `--oneline` rút gọn mỗi commit còn một dòng, `--decorate` hiện tên nhánh và `-3` giới hạn ba commit.

Nếu thấy `Already up to date.`, lab-base đã chứa lịch sử lab-note từ lần merge trước.

## 3. Cho hai nhánh có các commit riêng

Tạo nhánh đóng góp tiếp theo.

```bash
git switch -c codex/lab-review
```

`-c` tạo nhánh và chuyển sang nhánh mới tại commit hiện tại.

Trên lab-review, tạo file `merge-note.txt` ở gốc bản sao với nội dung sau. Lưu file với ký tự xuống dòng cuối.

```text
Nguoi kiem tra da doc huong dan.
```

```bash
git add -- merge-note.txt
git diff --staged
git commit -m "Add reviewer practice note"
```

`add -- merge-note.txt` chọn nội dung file. `diff --staged` giúp kiểm tra toàn bộ staging area. `commit -m` lưu commit cùng thông điệp trong ngoặc kép.

Chuyển về lab-base.

```bash
git switch codex/lab-base
```

Sửa toàn bộ nội dung `branch-note.txt` thành một dòng sau rồi lưu.

```text
Trang thai: cho kiem tra.
```

```bash
git diff -- branch-note.txt
git add -- branch-note.txt
git diff --staged
git commit -m "Mark branch note ready for review"
```

`diff -- branch-note.txt` so sánh file trên đĩa với staging area để xem phần sửa chưa stage. Các lệnh add và commit lưu thay đổi trên lab-base.

Lab-review đã thêm merge-note.txt, còn lab-base đã sửa branch-note.txt. Hai nhánh giờ có commit riêng sau một commit chung.

## 4. Tạo merge commit với các thay đổi tự gộp được

Khi đang ở lab-base, chạy lệnh sau.

```bash
git merge --no-ff --no-edit codex/lab-review
```

| Phần | Ý nghĩa |
|---|---|
| `merge` | Gộp nhánh nguồn vào nhánh hiện tại. |
| `--no-ff` | Yêu cầu tạo merge commit khi có lịch sử mới cần gộp, kể cả tình huống có thể fast-forward. |
| `--no-edit` | Dùng thông điệp merge do Git tạo sẵn. |
| `codex/lab-review` | Nhánh nguồn chứa file reviewer đã thêm. |

Git có thể tự gộp vì các commit trong bài sửa hai file riêng. Kết quả trên lab-base có cả `merge-note.txt` và dòng `Trang thai: cho kiem tra.` trong branch-note.txt.

Đọc lịch sử bằng lệnh sau.

```bash
git --no-pager log --graph --oneline --decorate -10
```

`--graph` vẽ các đường liên kết commit bằng ký tự. `-10` giới hạn mười commit. Các tùy chọn còn lại đã dùng ở bước 2.

Merge commit nối tới hai commit cha, tương ứng hai hướng lịch sử vừa được gộp. Hình bên dưới minh họa, các chữ A, B, C và M là tên giả để đọc hình.

```text
    B ------- M   lab-base
   /         /
A --------- C     lab-review
```

M là merge commit, có B và C làm hai commit cha.

## 5. Tạo conflict bằng cách sửa cùng một dòng

Từ lab-base đã gộp xong, tạo nhánh mới.

```bash
git switch -c codex/lab-conflict
```

Sửa `branch-note.txt` thành một dòng sau rồi lưu.

```text
Trang thai: duyet xong.
```

```bash
git add -- branch-note.txt
git diff --staged
git commit -m "Mark practice note approved"
git switch codex/lab-base
```

Quay về lab-base, sửa cùng file thành một dòng khác rồi lưu.

```text
Trang thai: can bo sung vi du.
```

```bash
git add -- branch-note.txt
git diff --staged
git commit -m "Request examples in practice note"
```

Lần này cả hai nhánh cùng sửa một dòng, từ nội dung chung `Trang thai: cho kiem tra.` sang hai nội dung khác nhau.

Bắt đầu merge.

```bash
git merge --no-edit codex/lab-conflict
```

Git sẽ báo conflict tại `branch-note.txt` và để merge ở trạng thái đang dở. Đây là tình huống đã chủ đích tạo để học cách quyết định nội dung cuối.

## 6. Đọc conflict và chọn nội dung cuối

```bash
git status
git diff --name-only --diff-filter=U
```

| Phần | Ý nghĩa |
|---|---|
| `status` | Hiện nhánh, trạng thái merge và các file cần giải quyết. |
| `diff --name-only` | Liệt kê tên file có thay đổi. |
| `--diff-filter=U` | Giới hạn vào file đang ở trạng thái unmerged, tức còn xung đột cần xử lý. |

Mở branch-note.txt. Với kiểu hiển thị conflict thông thường, bạn sẽ thấy các dấu sau.

```text
<<<<<<< HEAD
Trang thai: can bo sung vi du.
=======
Trang thai: duyet xong.
>>>>>>> codex/lab-conflict
```

- Phần từ `<<<<<<< HEAD` tới `=======` là nội dung nhánh hiện tại lab-base.
- Phần từ `=======` tới `>>>>>>> codex/lab-conflict` là nội dung nhánh nguồn.
- Các dòng ký hiệu đánh dấu hai phần cần quyết định.

Nếu Git được cấu hình hiển thị thêm phần tổ tiên chung, bạn có thể thấy dấu `|||||||` kèm nội dung gốc. Bài tập này dùng một file một dòng, nên bạn có thể viết lại toàn bộ file bằng nội dung đã thống nhất dưới đây.

Giả sử người duyệt quyết định cần bổ sung ví dụ trước khi duyệt. Thay toàn bộ file bằng một dòng sau, bỏ các dòng đánh dấu conflict rồi lưu.

```text
Trang thai: can bo sung vi du truoc khi duyet.
```

Bạn vừa đưa ra quyết định về nội dung. Git cần bạn stage bản đã sửa để xác nhận cách giải quyết conflict.

## 7. Stage bản đã giải quyết và hoàn thành merge

```bash
git add -- branch-note.txt
git diff --staged
git status
```

`add` lưu bản bạn chọn vào staging area và đánh dấu conflict của file đã được giải quyết. `diff --staged` giúp bạn đọc lại phần sắp lưu. Git status lúc này cần báo các conflict đã sửa xong và vẫn đang merge.

Khi nội dung đúng, hoàn thành merge commit.

```bash
git commit -m "Merge practice note and require examples before approval"
git status --short --branch
git --no-pager log --graph --oneline --decorate -10
```

Commit lưu kết quả hợp nhất cùng hai hướng lịch sử. Trạng thái file cần sạch và log hiện merge commit mới.

Mở branch-note.txt để kiểm tra dòng cuối đúng với quyết định đã ghi ở bước 6.

## 8. Hủy một lần merge đang dở

Phần này là lựa chọn khi bạn đang ở bước 5 hoặc 6 và muốn bắt đầu xử lý conflict lại.

```bash
git merge --abort
git status
```

`--abort` yêu cầu Git hủy lần merge đang dở và cố khôi phục trạng thái trước merge. Bài tập đã yêu cầu working directory sạch trước merge để việc quay lại trạng thái ban đầu rõ ràng.

Sau khi hủy, các commit của lab-base và lab-conflict vẫn có trong lịch sử. Để tạo lại conflict, đứng ở lab-base rồi chạy lại `git merge --no-edit codex/lab-conflict` và tiếp tục bước 6.

Lệnh abort chỉ dùng khi merge đang dở. Nếu đã tạo merge commit ở bước 7, bạn đã hoàn thành lần merge đó. Cách hoàn tác commit sẽ học ở bài về revert.

## 9. Dọn nhánh bài tập sau khi đã đọc lịch sử

Khi ở lab-base và đã hoàn thành cả ba lần merge, bạn có thể xóa các tên nhánh nguồn.

```bash
git branch -d codex/lab-note
git branch -d codex/lab-review
git branch -d codex/lab-conflict
git branch
```

`branch -d` xóa tên nhánh local khi đủ điều kiện đã gộp. Các commit trong bài vẫn đi tới được qua lịch sử lab-base.

Để luyện lại từ đầu, tạo bản sao với tên thư mục mới theo Bài 5. Muốn quay lại repo học chính, dùng lệnh PowerShell sau.

```powershell
cd C:\Users\Hello\Desktop\gogit
```

`cd` chuyển thư mục làm việc của terminal. Các file hướng dẫn tiếp tục nằm trong `tutorials` của repo học chính.

## Áp dụng vào quy trình nhóm

Trong bản sao luyện tập, bạn đã đóng cả ba vai để hiểu quá trình tích hợp.

| Vai trò | Quyết định hoặc thao tác |
|---|---|
| Dev | Tạo nhánh và commit thay đổi. Khi có conflict, sửa lại nhánh đóng góp theo thống nhất của nhóm. |
| Tester | Kiểm tra lại nội dung sau khi gộp vì kết quả có thể kết hợp thay đổi từ nhiều phía. |
| Người duyệt | Chọn nội dung cuối được chấp nhận và quyết định thời điểm gộp. |

Trong repo GitHub, một Pull Request giúp nhóm xem xét nhánh đóng góp trước khi gộp vào nhánh nhận. Bài 7 sẽ đi qua phần review và các lựa chọn Merge commit, Squash and merge, Rebase and merge.

## Tự kiểm tra

1. Khi đứng ở nhánh A và chạy `git merge B`, nhánh nào nhận thay đổi?
2. Khi nào fast-forward dùng được?
3. Vì sao merge ở bước 4 tự gộp được còn bước 5 sinh conflict?
4. Sau khi sửa file conflict, còn hai thao tác nào để hoàn thành merge?
5. Khi merge đang dở và muốn thử lại từ đầu, dùng lệnh nào?

<details>
<summary>Xem đáp án sau khi tự trả lời</summary>

1. Nhánh A hiện tại.
2. Khi commit của nhánh nhận là tổ tiên của commit nhánh nguồn, nhánh nhận có thể tiến thẳng theo lịch sử.
3. Bước 4 sửa hai file riêng. Bước 5 sửa cùng một dòng ở hai nhánh thành hai nội dung khác nhau.
4. Stage bản đã giải quyết bằng git add, sau đó tạo merge commit bằng git commit.
5. `git merge --abort`.

</details>

**Đọc tiếp** [Bài 7 — Pull Request, review và quyết định cách gộp](bai-7.md).
