# Bài 7 — Pull Request, review và quyết định cách gộp

[Quay lại Bài 6](bai-6.md)

## Mục tiêu

- Tạo Pull Request (PR) để đề xuất nội dung từ nhánh đóng góp vào main.
- Phân biệt nhánh nguồn, nhánh nhận, review và merge.
- Đóng vai dev, tester và người duyệt.
- So sánh Merge commit, Squash and merge, Rebase and merge.
- Đưa quyết định chấp nhận thay đổi vào lịch sử GitHub.

Bài 5–6 dùng repo bản sao để luyện nhánh và conflict. Bài này quay lại repo học chính tại `C:\Users\Hello\Desktop\gogit`.

Bạn làm bước 1–4 trước, đọc phần thay đổi rồi mới sang thao tác gửi PR trên GitHub. Bước xác nhận merge dành cho bạn trong vai người quản lý repo.

## 1. Xác định đúng repo và nhánh

```powershell
cd C:\Users\Hello\Desktop\gogit
```

`cd` chuyển thư mục làm việc của terminal. Sau đó kiểm tra repo.

```bash
git status --short --branch
git remote -v
git branch -vv
```

| Phần | Ý nghĩa |
|---|---|
| `status --short --branch` | Hiện nhánh đang làm cùng trạng thái file dạng ngắn. |
| `remote -v` | Hiện tên remote và địa chỉ lấy, gửi dữ liệu. |
| `branch -vv` | Hiện các nhánh local, commit và nhánh theo dõi nếu có. |

Tại lúc viết bài, origin trỏ tới `https://github.com/sapoche-dev/gogit.git` và nhánh đang mở là `codex/lession-4-remote`. Đây là địa chỉ và cách viết tên nhánh thực tế đang có trong repo của bạn.

Các bài đầu dùng tên lenky-dev/learngit. Trước khi push hoặc tạo PR, hãy đối chiếu địa chỉ hiện tại với repo bạn muốn dùng. Nếu hai địa chỉ khác nhau, xác định repo cần đóng góp trước khi tiếp tục gửi dữ liệu.

Ở các bước dưới, nhánh nguồn là `codex/lession-4-remote` và nhánh nhận là `main`. Nếu bạn đang dùng tên khác, thay bằng tên thực tế trong các lệnh.

## 2. Cập nhật thông tin từ GitHub và xem phần đóng góp

```bash
git fetch origin
git branch -r
git merge-base origin/main HEAD
```

| Phần | Ý nghĩa |
|---|---|
| `fetch origin` | Tải dữ liệu và cập nhật các mốc theo dõi remote origin. |
| `branch -r` | Liệt kê các mốc nhánh remote Git đang theo dõi trên máy. |
| `merge-base origin/main HEAD` | Tìm commit tổ tiên chung của origin/main và commit hiện tại. |

Cần có `origin/main` trong danh sách nhánh và lệnh merge-base cần in một mã commit. Nếu Git báo thiếu nhánh hoặc hai lịch sử chưa có tổ tiên chung, hãy cùng kiểm tra lại repo và nhánh trước khi mở PR.

```bash
git --no-pager log --oneline origin/main..HEAD
git --no-pager diff --stat origin/main...HEAD
```

| Phần | Ý nghĩa |
|---|---|
| `--no-pager` | In trực tiếp vào terminal. |
| `log --oneline` | Hiện mỗi commit trên một dòng ngắn. |
| `origin/main..HEAD` | Chọn commit có trong nhánh hiện tại mà origin/main chưa có. |
| `diff --stat` | Xem thống kê file thay đổi. |
| `origin/main...HEAD` | So sánh từ tổ tiên chung tới HEAD để xem phần thay đổi của nhánh đóng góp. |

Các lệnh này đọc phần đã commit. File đang sửa và file mới chưa commit được liệt kê qua git status.

Nếu các commit luyện practice.txt cũng nằm trong nhánh nguồn, chúng sẽ đi cùng PR. Hãy đọc toàn bộ danh sách để biết chính xác phạm vi đề xuất.

## 3. Lưu các bài hướng dẫn mới trên nhánh đóng góp

```bash
git switch codex/lession-4-remote
git diff --staged
```

`switch` chuyển sang nhánh đã tồn tại. `diff --staged` hiện toàn bộ nội dung đã được chọn cho commit. Nếu staging area có phần khác cần lưu riêng, xử lý phần đó trước khi chọn tài liệu dưới đây.

```bash
git add -- tutorials/bai-0.md tutorials/bai-4.md tutorials/bai-5.md tutorials/bai-6.md tutorials/bai-7.md tutorials/bai-8.md
git diff --staged
```

`add` chọn nội dung hiện tại của những file được chỉ rõ. `--` đánh dấu phần đường dẫn. Các file đã sạch sẽ vắng mặt trong diff.

Đọc các phần đã stage và xác nhận chúng đúng với thay đổi bạn muốn gửi.

```bash
git commit -m "Add Git lessons on branching and collaboration"
git push -u origin codex/lession-4-remote
```

| Phần | Ý nghĩa |
|---|---|
| `commit -m` | Tạo commit từ staging area với thông điệp trong ngoặc kép. |
| `push` | Gửi commit và cập nhật nhánh ở remote. |
| `-u` | Thiết lập nhánh theo dõi cho local sau khi push thành công. |
| `origin` | Remote nhận thay đổi. |
| `codex/lession-4-remote` | Nhánh local được gửi lên nhánh cùng tên. |

Nếu đã commit các bài này ở lần trước, Git có thể báo nothing to commit. Kiểm tra log để xác nhận rồi tiếp tục theo trạng thái thực tế.

## 4. Mở Pull Request trên GitHub — vai dev

Mở repo đã xác nhận ở bước 1 trong trình duyệt.

1. Chọn tab **Pull requests**, rồi **New pull request**.
2. Đặt **base** là main, tức nhánh nhận nội dung được chấp nhận.
3. Đặt **compare** là codex/lession-4-remote, tức nhánh nguồn đang đề xuất.
4. Đọc danh sách commit và file thay đổi.
5. Nếu đã có PR mở cho cùng nhánh nguồn và nhánh nhận, vào PR đó để tiếp tục. Commit vừa push sẽ được đưa vào PR đang mở.
6. Nếu chưa có PR, nhập tiêu đề và mô tả rồi chọn **Create pull request**. Khi vẫn muốn hoàn thiện nội dung, chọn dạng **Draft** nếu giao diện cung cấp.

Mô tả gợi ý, bạn cần điều chỉnh theo phần diff thực tế và những bước đã tự kiểm tra.

```markdown
## Nội dung
Bổ sung các bài hướng dẫn Git về nhánh, merge, Pull Request và làm việc nhóm.

## Phạm vi cần duyệt
- Các bài học mới và liên kết giữa các bài.
- Các commit luyện tập trước đó nếu xuất hiện trong phần Commits.

## Kiểm tra đã thực hiện
- Ghi rõ bài và bước đã tự chạy.
- Ghi rõ kết quả thực tế.
- Liệt kê phần còn cần tester kiểm tra.
```

Một PR là nơi đề xuất, thảo luận và ghi nhận quyết định về thay đổi. Ghi lại số PR và địa chỉ của nó để dùng khi trao đổi với người kiểm tra.

## 5. Đọc thay đổi và kiểm tra — vai tester

Trên PR, xem lần lượt các phần giao diện.

| Phần | Cần đọc gì |
|---|---|
| Conversation | Mục tiêu, phản hồi và các quyết định đã trao đổi. |
| Commits | Những mốc thay đổi thuộc PR. |
| Files changed | Nội dung từng file được đề xuất. |
| Checks hoặc khu vực trạng thái kiểm tra | Các kiểm tra tự động được cấu hình và kết quả của chúng. |

Với tài liệu này, tester chọn một bài, chạy từng bước trong đúng repo luyện tập rồi đối chiếu với kết quả mong đợi. Ghi báo cáo có thể làm lại được.

```text
Commit đã kiểm tra: <mã commit thực tế>
Bài và bước: Bài 5, bước 5
Thao tác: chuyển từ lab-note sang lab-base
Mong đợi: branch-note.txt biến mất trên lab-base trước khi merge
Thực tế: <kết quả bạn quan sát>
Kết luận: Đạt / Cần sửa
```

Nếu cần lấy mã commit hiện tại trên bản đang kiểm tra, chạy `git rev-parse --short HEAD`. `rev-parse` đọc giá trị tham chiếu, `--short` rút gọn mã, `HEAD` chọn commit hiện tại.

CI là kiểm tra tích hợp liên tục. Với GitHub Actions, repo có thể cấu hình workflow chạy khi có PR hoặc push. Nếu PR chưa có workflow hay check, ghi rõ phần tự động chưa được cấu hình và nêu những bước đã kiểm tra thủ công. [Tài liệu GitHub Actions](https://docs.github.com/en/actions/get-started/understand-github-actions)

## 6. Gửi review — vai người duyệt

Trong **Files changed**, đọc từng file và ghi phản hồi ở đúng dòng cần chỉnh. Mở **Review changes** rồi chọn loại phản hồi phù hợp.

| Loại review | Ý nghĩa |
|---|---|
| Comment | Gửi nhận xét hoặc câu hỏi. |
| Request changes | Yêu cầu tác giả sửa nội dung trước khi chấp nhận. |
| Approve | Ghi nhận việc chấp nhận phiên bản đang được review. |

Tác giả PR bị GitHub chặn tự Approve PR của mình. Nếu chỉ dùng một tài khoản để đóng các vai, ghi phản hồi bằng comment và thực hiện quyết định merge bằng quyền maintainer khi repo cho phép. Để thực hành review Approve độc lập, dùng người khác có tài khoản và quyền phù hợp. Việc Request changes có chặn merge hay không phụ thuộc ruleset và quyền trong repo. [Tài liệu review](https://docs.github.com/en/pull-requests/how-tos/review-pull-requests/reviewing-proposed-changes-in-a-pull-request)

Ví dụ yêu cầu sửa cụ thể.

```text
Bài 6, bước 7 cần nêu rõ phải lưu file sau khi bỏ các dấu conflict.
Vui lòng bổ sung hướng dẫn rồi kiểm tra lại bước này.
```

## 7. Dev sửa theo phản hồi và cập nhật cùng PR

Trong repo local, ở đúng nhánh nguồn của PR, sửa file cần chỉnh rồi lưu.

```bash
git switch codex/lession-4-remote
git diff -- tutorials/bai-6.md
git add -- tutorials/bai-6.md
git diff --staged
git commit -m "Clarify saving resolved conflict files"
git push origin codex/lession-4-remote
```

`diff -- <file>` xem phần chưa stage của file. Chuỗi add, kiểm tra staged, commit và push tạo một bản cập nhật trên cùng nhánh nguồn. Thông điệp commit trong ví dụ cần khớp với sửa đổi thực tế bạn làm.

Mở lại PR và kiểm tra commit mới đã xuất hiện. Tester kiểm tra phiên bản mới rồi ghi lại mã commit đã kiểm tra.

## 8. Người duyệt chọn cách đưa thay đổi vào main

Ba cách gộp tạo lịch sử khác nhau.

| Cách gộp | Kết quả trên main | Khi nào hợp với bài tập |
|---|---|---|
| Create a merge commit | Giữ các commit của nhánh nguồn và thêm một merge commit. | Muốn đọc lại toàn bộ các commit đã luyện. |
| Squash and merge | Gom phần thay đổi của PR thành một commit mới. | Muốn ghi một mốc cho toàn bộ phần tài liệu được chấp nhận. |
| Rebase and merge | Đưa từng commit lên main theo lịch sử thẳng, GitHub tạo mã commit mới. | Các commit đã rõ mục đích và muốn giữ từng commit theo thứ tự. |

Người quản lý chọn theo lịch sử muốn lưu. [Tài liệu các cách gộp](https://docs.github.com/en/pull-requests/reference/pull-request-merges)

Khi đã chấp nhận nội dung và các điều kiện của repo đã đạt, người duyệt thực hiện trên trang PR.

1. Nếu PR đang Draft, chuyển sang **Ready for review**.
2. Đọc lại commit mới nhất, phản hồi còn mở và trạng thái checks.
3. Mở danh sách cách gộp, chọn cách muốn dùng.
4. Kiểm tra thông điệp commit nếu giao diện yêu cầu.
5. Bấm nút xác nhận merge của cách đã chọn.
6. Xác nhận PR hiện trạng thái **Merged**.

Nếu chưa chấp nhận, tiếp tục yêu cầu sửa hoặc đóng PR và ghi lý do. Nút merge và các phương thức khả dụng phụ thuộc quyền cùng cấu hình repo. [Tài liệu merge PR](https://docs.github.com/en/pull-requests/how-tos/merge-and-close-pull-requests/merging-a-pull-request)

## 9. Đọc kết quả sau khi merge

Trong repo local, lấy thông tin mới rồi xem lịch sử remote main.

```bash
git fetch origin
git --no-pager log --oneline --graph --decorate -10 origin/main
```

`--graph` vẽ quan hệ commit, `--decorate` hiện tên tham chiếu và `-10` giới hạn mười commit. Lệnh này đọc trực tiếp mốc origin/main sau fetch.

Main local có thể còn các commit đã luyện trước đó. Sau squash hoặc rebase, các mã commit trên main có thể khác các mã local. Bài 8 sẽ tạo bản clone mới từ main đã được chấp nhận để bắt đầu phần làm việc nhóm.

## Tự kiểm tra

1. Base và compare có vai trò gì?
2. Khi dev push commit mới vào nhánh của PR đang mở, cập nhật nằm ở đâu?
3. Approve ghi nhận điều gì, và thao tác nào thực sự đưa thay đổi vào main?
4. Ba cách gộp giữ các commit khác nhau như thế nào?
5. Tester cần ghi mã commit vì sao?

<details>
<summary>Xem đáp án sau khi tự trả lời</summary>

1. Base nhận thay đổi, compare cung cấp thay đổi.
2. Trong PR đang mở cho nhánh nguồn đó.
3. Approve ghi nhận chấp nhận phiên bản được review. Xác nhận merge cập nhật main khi GitHub thực hiện gộp.
4. Merge commit giữ các commit và thêm điểm gộp. Squash tạo một commit. Rebase đưa từng commit lên lịch sử thẳng với mã mới.
5. Để biết chính xác phiên bản đã được kiểm tra và khi nào cần kiểm tra lại.

</details>

**Đọc tiếp** [Bài 8 — Phối hợp nhóm và cập nhật nhánh đóng góp](bai-8.md).
