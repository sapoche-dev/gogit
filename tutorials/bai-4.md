# Bài 4 — Kết nối GitHub và đồng bộ repo

[Quay lại Bài 3](bai-3.md)

## Mục tiêu

- Đọc cấu hình remote và phân biệt `main` với `origin/main`.
- Dùng `fetch` để cập nhật thông tin từ GitHub.
- Đưa các commit luyện tập lên một nhánh đóng góp.
- Hiểu `pull` cập nhật nhánh hiện tại như thế nào.
- Biết `clone` tạo bản sao repo trên máy khác.

Thực hành từ thư mục gốc `C:\Users\Hello\Desktop\gogit`. Hôm nay bạn có thể chỉ làm bước 1–3 trước rồi gửi kết quả để cùng đọc. Các bước sau hướng dẫn đưa đóng góp lên GitHub.

## 1. Kiểm tra đang ở nhánh nào và kết nối đến đâu

```bash
git status --short --branch
git remote -v
```

| Phần | Ý nghĩa |
|---|---|
| `status` | Xem trạng thái nhánh và file trong repo. |
| `--short` | Hiện mỗi file thay đổi trên một dòng ngắn. |
| `--branch` | Thêm thông tin nhánh hiện tại ở đầu kết quả. |
| `remote` | Quản lý các địa chỉ repo từ xa. |
| `-v` | Hiện cả tên remote và địa chỉ của nó. |

Trong repo này, bạn cần thấy địa chỉ sau ở hai dòng.

```text
origin  https://github.com/lenky-dev/learngit.git (fetch)
origin  https://github.com/lenky-dev/learngit.git (push)
```

`origin` là tên Git đặt cho một kết nối remote. `(fetch)` là địa chỉ lấy dữ liệu về, `(push)` là địa chỉ gửi commit lên. Hai dòng này thường cùng trỏ đến một repo GitHub.

Các lệnh để thiết lập remote, dùng khi trạng thái của bạn khớp với tình huống tương ứng.

| Tình huống | Lệnh |
|---|---|
| Chưa có remote tên origin | `git remote add origin https://github.com/lenky-dev/learngit.git` |
| Origin đã tồn tại và cần đổi địa chỉ | `git remote set-url origin https://github.com/lenky-dev/learngit.git` |

`add` tạo kết nối mới. `set-url` thay địa chỉ của kết nối đã có. Kiểm tra lại bằng `git remote -v` sau khi thay cấu hình.

## 2. Lấy thông tin mới từ GitHub bằng fetch

```bash
git fetch origin
git status --short --branch
```

| Phần | Ý nghĩa |
|---|---|
| `fetch` | Tải các đối tượng Git cần thiết và cập nhật các mốc theo dõi remote. |
| `origin` | Tên remote được chọn để lấy dữ liệu. |

Sau `fetch`, mốc `origin/main` trên máy phản ánh commit mới nhất Git vừa thấy trên nhánh `main` ở GitHub. Nhánh `main` local tiếp tục trỏ đến commit local hiện tại. Nếu remote đã khớp với thông tin đang có, `fetch` có thể trả về trống.

| Tên | Nằm ở đâu và dùng làm gì |
|---|---|
| `main` local | Nhánh trên máy bạn đang dùng để lưu commit. |
| `origin/main` | Mốc trên máy ghi nhớ nhánh main của remote origin sau lần đồng bộ gần nhất. |
| `main` trên GitHub | Nhánh ở repo từ xa, được cập nhật khi có push hoặc merge vào nhánh ấy. |

Nếu trước đó bạn thấy `ahead 6`, sau fetch con số có thể thay đổi tùy dữ liệu mới ở GitHub.

## 3. Xem commit nào đã có ở mỗi bên

```bash
git --no-pager log --oneline origin/main..main
git --no-pager log --oneline main..origin/main
```

| Phần | Ý nghĩa |
|---|---|
| `--no-pager` | In trực tiếp vào terminal. |
| `log --oneline` | Liệt kê commit, mỗi commit một dòng ngắn. |
| `A..B` | Chọn các commit có trong lịch sử B mà chưa có trong lịch sử A. |
| `origin/main..main` | Các commit ở main local mà origin/main chưa có. |
| `main..origin/main` | Các commit ở origin/main mà main local chưa có. |

Đọc kết quả sau khi đã fetch.

| Kết quả | Trạng thái |
|---|---|
| Chỉ lệnh đầu hiện commit | Local đi trước. Các commit đó cần được gửi lên khi bạn sẵn sàng đóng góp. |
| Chỉ lệnh thứ hai hiện commit | Local đi sau. Có thể cập nhật local bằng pull ở bước 7 khi thư mục làm việc đã sạch. |
| Cả hai đều trống | Hai nhánh đang trỏ tới cùng commit. |
| Cả hai đều hiện commit | Lịch sử đã phân kỳ. Gửi kết quả cho người hướng dẫn trước khi tiếp tục phần đồng bộ. |

Tại lúc soạn bài, `main` local của repo học đang đi trước mốc `origin/main` sáu commit. Đó là những commit bạn đã luyện từ các bài trước. Bước 4 tạo nhánh từ trạng thái local hiện tại để đề xuất chúng cho người quản lý repo.

## 4. Tạo nhánh đóng góp

Trước khi làm, xem staging area.

```bash
git diff --staged
```

Lệnh hiện toàn bộ nội dung đang được chọn cho commit kế tiếp. Nếu có phần sửa của bài khác đang stage, hãy kiểm tra và quyết định cách lưu phần đó trước khi thêm tài liệu của bài này.

Khi đang ở nhánh `main` local của bài học, tạo nhánh mới.

```bash
git switch -c codex/lesson-4-remote
git status --short --branch
```

| Phần | Ý nghĩa |
|---|---|
| `switch` | Chuyển sang một nhánh để làm việc. |
| `-c` | Tạo nhánh mới rồi chuyển sang nhánh đó. |
| `codex/lesson-4-remote` | Tên nhánh đóng góp của bài này. |

Nhánh mới bắt đầu từ commit hiện tại, nên chứa cả sáu commit local đã luyện. Những file tài liệu đang sửa và file mới vẫn xuất hiện trong working directory để bạn lưu tiếp trên nhánh mới.

Nếu nhánh này đã được tạo trong lần luyện trước, lệnh `-c` sẽ báo tên đã tồn tại. Dùng `git switch codex/lesson-4-remote` để tiếp tục nhánh đó sau khi kiểm tra công việc hiện tại bằng `git status`.

Đọc lại các file bài học đã sửa hoặc tạo. Với phần tài liệu hiện có, chọn đúng năm file sau.

```bash
git add -- tutorials/bai-0.md tutorials/bai-1.md tutorials/bai-2.md tutorials/bai-3.md tutorials/bai-4.md
git diff --staged
```

`add` đưa nội dung hiện tại của các file được chỉ định vào staging area. Dấu `--` đánh dấu phần đường dẫn, các đường dẫn cách nhau bằng dấu cách. `diff --staged` cho xem nội dung sẽ đi vào commit.

Đọc hết diff. Các file đã được commit và đang sạch có thể vắng mặt trong diff, đó là trạng thái bình thường. Khi phần đã stage đúng với nội dung bạn muốn lưu, tạo commit.

```bash
git commit -m "Update Git tutorials through lesson 4"
git status --short --branch
```

`commit` tạo mốc lịch sử từ staging area. `-m` nhận thông điệp trong ngoặc kép. Nếu Git báo `nothing to commit`, kiểm tra lại trạng thái vì có thể nội dung đã được lưu ở một lần luyện trước.

## 5. Kiểm tra phần sẽ đề xuất lên GitHub

```bash
git --no-pager log --oneline origin/main..HEAD
git --no-pager diff --stat origin/main...HEAD
```

`HEAD` chỉ commit hiện tại trên nhánh đóng góp. Lệnh `log` liệt kê các commit mà origin/main chưa có. Trong lệnh `diff`, `--stat` hiện thống kê file, còn `origin/main...HEAD` so sánh từ tổ tiên chung của hai nhánh đến HEAD để xem phần nhánh đóng góp đã thay đổi.

Bài tập này có thể bao gồm cả thay đổi `practice.txt` từ các commit trước và phần cập nhật tài liệu. Hãy đọc danh sách file để xác nhận toàn bộ phạm vi đóng góp.

## 6. Đẩy nhánh đóng góp lên GitHub bằng push

```bash
git push -u origin codex/lesson-4-remote
```

| Phần | Ý nghĩa |
|---|---|
| `push` | Gửi các commit cần thiết và yêu cầu cập nhật nhánh ở remote. |
| `-u` | Thiết lập nhánh remote được theo dõi, còn gọi là upstream, sau khi push thành công. |
| `origin` | Remote nhận dữ liệu. |
| `codex/lesson-4-remote` | Nhánh local sẽ được gửi lên nhánh cùng tên trên GitHub. |

Sau khi thành công, local `codex/lesson-4-remote` theo dõi `origin/codex/lesson-4-remote`. Kiểm tra bằng hai lệnh sau.

```bash
git branch -vv
git status --short --branch
```

`branch -vv` liệt kê nhánh local với commit gần nhất và nhánh upstream nếu có. Trên nhánh hiện tại, dòng trạng thái cần thể hiện tên nhánh đóng góp ở cả local và remote.

Mở [repo learngit](https://github.com/lenky-dev/learngit), chọn nhánh `codex/lesson-4-remote` trong bộ chọn nhánh và đọc lại các file. Push nhánh đóng góp cập nhật nhánh được chỉ định trong lệnh. Để đưa đóng góp vào main, người quản lý sẽ xem xét và gộp qua Pull Request trong phần thực hành sau.

Nếu terminal báo `Everything up-to-date`, các commit của nhánh đã được gửi trước đó. Nếu báo lỗi xác thực hoặc thiếu quyền ghi, hãy đọc tên tài khoản và repo trong thông báo. Quyền push phụ thuộc tài khoản xác thực GitHub. `user.name` và `user.email` dùng để ghi danh tính trong commit.

## 7. Pull lấy dữ liệu về rồi tích hợp vào nhánh hiện tại

Thực hành phần này khi thay đổi đang làm đã được lưu và `git status --short` trả về trống.

```bash
git pull --ff-only
```

| Phần | Ý nghĩa |
|---|---|
| `pull` | Fetch dữ liệu rồi tích hợp nhánh upstream vào nhánh hiện tại. |
| `--ff-only` | Chỉ cập nhật khi có thể tiến thẳng theo lịch sử đã có. Lệnh dừng nếu hai bên có commit riêng làm lịch sử phân kỳ. |

Sau bước 6, nhánh hiện tại theo dõi `origin/codex/lesson-4-remote`, nên pull lấy dữ liệu từ nhánh đó. Nếu hai bên đã khớp, bạn sẽ thấy `Already up to date.`. Nếu remote có commit mới nối tiếp commit local, Git cập nhật nhánh và các file tương ứng.

Khi đang ở local main và muốn cập nhật main từ GitHub, dạng chỉ rõ nguồn là `git pull --ff-only origin main`. Hãy đọc nhánh hiện tại bằng `git status` trước khi dùng dạng này vì pull tích hợp vào nhánh đang đứng. Nếu Git báo lịch sử phân kỳ, giữ thông báo để cùng xử lý trong bài về merge và rebase. [Tài liệu git pull](https://git-scm.com/docs/git-pull.html)

## 8. Clone dùng khi bắt đầu ở một thư mục mới

Phần này là bài tập tùy chọn. Mở terminal ở thư mục cha `C:\Users\Hello\Desktop`, rồi dùng một tên thư mục đích chưa tồn tại.

```bash
git clone https://github.com/lenky-dev/learngit.git learngit-copy
cd learngit-copy
git remote -v
git status --short --branch
```

| Phần | Ý nghĩa |
|---|---|
| `clone` | Tạo repo local mới, tải lịch sử và tạo working directory từ repo nguồn. |
| Địa chỉ HTTPS | Repo GitHub muốn lấy về. |
| `learngit-copy` | Tên thư mục local mới. |
| `cd learngit-copy` | Lệnh terminal chuyển vào thư mục vừa tạo. |

Clone thiết lập remote `origin` và thường mở nhánh mặc định trên GitHub, ở đây là main. Khi đóng góp vẫn đang ở nhánh riêng, bản clone mặc định sẽ hiện nội dung main. Bạn sẽ học cách chuyển sang các nhánh khác trong Bài 5.

## Các vai trò trong bài thực hành

| Vai trò | Công việc |
|---|---|
| Dev | Tạo nhánh, sửa tài liệu, commit và push nhánh đóng góp. |
| Tester | Thử các hướng dẫn, ghi lại lệnh, kết quả mong đợi và kết quả thực tế. |
| Người duyệt / maintainer | Đọc thay đổi, yêu cầu sửa nếu cần và quyết định gộp vào main. |

Ta đang dùng repo và tài khoản GitHub của bạn để mô phỏng các vai trò. Trong dự án mã nguồn mở, người đóng góp bên ngoài thường fork repo rồi gửi Pull Request từ nhánh trong fork của họ. Khi có quyền ghi vào repo, họ có thể dùng nhánh trong cùng repo như bài này.

Tác giả PR tự xem xét nội dung của mình được, còn thao tác review Approve cho chính PR đó bị GitHub chặn. Muốn luyện hai vai trò approve độc lập trên giao diện, cần hai tài khoản khác nhau. Quyền merge và các điều kiện review phụ thuộc thiết lập của repo. [Tài liệu review Pull Request](https://docs.github.com/en/pull-requests/how-tos/review-pull-requests/reviewing-proposed-changes-in-a-pull-request)

Trong bài này, sau khi push nhánh thành công, hãy gửi kết quả để kiểm tra. Bài về Pull Request sẽ hướng dẫn bạn chọn nhánh nguồn, nhánh nhận và quyết định cách đưa thay đổi vào main.

## Tự kiểm tra

1. Origin lưu tên kết nối hay tên nhánh?
2. Fetch cập nhật mốc theo dõi nào khi GitHub main có commit mới?
3. Push nhánh `codex/lesson-4-remote` cập nhật nhánh nào ở GitHub?
4. `-u` trong push giúp các lần pull và push sau như thế nào?
5. Muốn biết commit local nào origin/main chưa có, dùng phạm vi log nào?
6. Khi đang ở nhánh đóng góp, `git pull --ff-only` lấy dữ liệu từ đâu sau bước 6?

<details>
<summary>Xem đáp án sau khi tự trả lời</summary>

1. Origin là tên kết nối remote.
2. Origin/main, theo cấu hình fetch của repo này.
3. Nhánh cùng tên codex/lesson-4-remote.
4. Đặt upstream để Git biết nhánh theo dõi. Việc push mặc định còn phụ thuộc cấu hình, bài tập dùng tên nhánh local và remote giống nhau.
5. `origin/main..main`.
6. Nhánh upstream origin/codex/lesson-4-remote.

</details>

**Đọc tiếp** [Bài 5 — Tạo, chuyển, đổi tên và xóa nhánh](bai-5.md).
