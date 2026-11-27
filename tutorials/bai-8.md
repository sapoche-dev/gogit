# Bài 8 — Phối hợp nhóm và cập nhật nhánh đóng góp

[Quay lại Bài 7](bai-7.md)

## Mục tiêu

- Thực hành vòng dev viết, tester kiểm tra, dev sửa và maintainer quyết định.
- Dùng hai bản clone để phân biệt công việc trên hai máy.
- Cập nhật nhánh đóng góp khi main đã nhận một PR khác.
- Nhận biết vì sao push bị từ chối và kiểm tra lại sau khi đồng bộ.

Bài này bắt đầu sau khi bạn đã quyết định và merge PR tài liệu ở Bài 7. Repo remote cần có nhánh main và tài khoản của bạn có quyền push các nhánh luyện tập.

Các ví dụ dùng địa chỉ origin đang có lúc soạn bài là `https://github.com/sapoche-dev/gogit.git`. Nếu đã đổi repo, lấy địa chỉ thực tế bằng `git remote -v` tại repo học chính rồi thay trong hai lệnh clone.

## 1. Chuẩn bị hai bản clone từ main

Mở terminal tại Desktop.

```powershell
cd C:\Users\Hello\Desktop
```

Tạo hai thư mục mới có tên chưa tồn tại.

```bash
git clone --branch main https://github.com/sapoche-dev/gogit.git gogit-dev-lab
git clone --branch main https://github.com/sapoche-dev/gogit.git gogit-tester-lab
```

| Phần | Ý nghĩa |
|---|---|
| `clone` | Tạo repo local từ nguồn remote. |
| `--branch main` | Mở nhánh main làm nhánh local ban đầu. |
| Địa chỉ HTTPS | Repo GitHub đang thực hành. |
| `gogit-dev-lab` | Bản làm việc của dev. |
| `gogit-tester-lab` | Bản dùng để tester kiểm tra và tạo một đóng góp riêng. |

Hai bản clone bắt đầu từ main trên GitHub, nên tiếp nối nội dung đã được chấp nhận ở Bài 7.

Nếu luyện lại, dùng hai tên thư mục mới và thay đường dẫn cd tương ứng trong bài. Nếu đóng các vai bằng một tài khoản GitHub, phần review độc lập vẫn chịu giới hạn tự Approve đã học ở Bài 7.

## 2. Dev tạo nhánh và đề xuất thay đổi

Chuyển tới bản của dev.

```powershell
cd C:\Users\Hello\Desktop\gogit-dev-lab
```

```bash
git status --short --branch
git switch -c codex/team-note
```

`status --short --branch` kiểm tra nhánh và trạng thái file. `switch -c` tạo nhánh đóng góp từ main hiện tại rồi chuyển sang nhánh mới.

Tạo file mới `team-practice.txt` ở thư mục gốc bản dev. Nội dung sau cố ý sắp xếp sai thứ tự kiểm tra để tester phát hiện.

```text
Quy trinh: dev -> merge -> tester.
```

Lưu file với ký tự xuống dòng cuối.

```bash
git add -- team-practice.txt
git diff --staged
git commit -m "Add team workflow practice"
git push -u origin codex/team-note
```

| Phần | Ý nghĩa |
|---|---|
| `add -- team-practice.txt` | Chọn nội dung file mới. |
| `diff --staged` | Xem toàn bộ nội dung đang được chọn cho commit. |
| `commit -m` | Lưu commit với thông điệp trong ngoặc kép. |
| `push -u origin codex/team-note` | Gửi nhánh lên GitHub và thiết lập upstream cùng tên. |

Mở PR theo Bài 7, với base main và compare codex/team-note. Trong mô tả, ghi tiêu chí chấp nhận là thứ tự dev viết → tester kiểm tra → người duyệt đánh giá → merge.

Giữ PR này mở để thực hiện các vòng phản hồi trong bài.

## 3. Tester lấy đúng nhánh cần kiểm tra

Chuyển sang bản của tester.

```powershell
cd C:\Users\Hello\Desktop\gogit-tester-lab
```

```bash
git fetch origin
git switch --track -c codex/team-review origin/codex/team-note
git rev-parse --short HEAD
```

| Phần | Ý nghĩa |
|---|---|
| `fetch origin` | Cập nhật các mốc theo dõi nhánh trên GitHub. |
| `switch` | Chuyển sang nhánh để kiểm tra. |
| `--track` | Thiết lập nhánh upstream từ điểm bắt đầu đã chọn. |
| `-c codex/team-review` | Tạo nhánh local cho công việc kiểm tra. |
| `origin/codex/team-note` | Điểm bắt đầu là phiên bản nhánh của dev vừa fetch. |
| `rev-parse --short HEAD` | In mã rút gọn của commit hiện tại để ghi vào báo cáo. |

Nhánh local tester tên team-review theo dõi nhánh remote team-note. Bản này dùng để đọc nội dung và đối chiếu kết quả.

Mở team-practice.txt, so sánh với tiêu chí của PR rồi gửi báo cáo vào PR.

```text
Commit đã kiểm tra: <mã thực tế vừa đọc>
Tiêu chí: tester kiểm tra và người duyệt đánh giá trước khi merge
Thực tế: file ghi merge trước tester
Kết luận: Cần sửa thứ tự trong team-practice.txt
```

Nếu dùng tài khoản tester độc lập, có quyền review phù hợp, có thể gửi Request changes theo Bài 7. Khi đóng các vai bằng tài khoản tác giả PR, ghi phản hồi dưới dạng comment.

## 4. Dev sửa và push lên cùng nhánh

Trở lại bản dev.

```powershell
cd C:\Users\Hello\Desktop\gogit-dev-lab
```

```bash
git branch --show-current
```

`branch --show-current` phải in `codex/team-note`. Sửa team-practice.txt thành dòng sau và lưu.

```text
Quy trinh: dev -> tester -> nguoi duyet -> merge.
```

```bash
git diff -- team-practice.txt
git add -- team-practice.txt
git diff --staged
git commit -m "Fix team workflow review order"
git push origin codex/team-note
```

`diff -- <file>` xem phần sửa chưa stage. Các lệnh tiếp theo chọn, kiểm tra, commit và gửi bản sửa. PR team-note nhận commit mới từ cùng nhánh nguồn.

Tester trở lại bản kiểm tra.

```powershell
cd C:\Users\Hello\Desktop\gogit-tester-lab
```

```bash
git status --short
git pull --ff-only
git rev-parse --short HEAD
```

Khi trạng thái sạch, `pull --ff-only` lấy và tích hợp phiên bản mới từ upstream origin/codex/team-note. Nó chỉ tiến thẳng khi lịch sử cho phép. Ghi mã commit mới, mở file rồi gửi lại kết quả kiểm tra.

Mẫu nhận xét có thể ghi “Đạt tiêu chí thứ tự trên commit <mã mới>”. Tiếp tục giữ PR mở để luyện cập nhật main ở bước 6.

## 5. Tester tạo một đóng góp khác vào main

Trong bản tester, chuyển về main và đồng bộ.

```bash
git switch main
git pull --ff-only
git switch -c codex/team-checklist
```

`switch main` chọn nhánh main local. Pull lấy upstream của main. Nhánh team-checklist bắt đầu từ main đã được cập nhật.

Tạo file mới `team-checklist.md` ở gốc repo.

```markdown
# Checklist trước khi gộp

- [ ] Đọc đúng phiên bản commit cần kiểm tra.
- [ ] Đối chiếu kết quả với tiêu chí của PR.
- [ ] Người duyệt quyết định cách merge.
```

```bash
git add -- team-checklist.md
git diff --staged
git commit -m "Add team review checklist"
git push -u origin codex/team-checklist
```

Tạo PR thứ hai với base main và compare codex/team-checklist. Người quản lý đọc checklist và quyết định gộp PR checklist trước. Xác nhận PR thứ hai hiện Merged.

Lúc này main trên GitHub có thêm file team-checklist.md. PR team-note đầu tiên vẫn đang mở và bắt đầu từ phiên bản main cũ.

## 6. Dev cập nhật main vào nhánh đang đóng góp

Trong bản dev, kiểm tra đang đứng ở team-note và thư mục làm việc sạch.

```powershell
cd C:\Users\Hello\Desktop\gogit-dev-lab
```

```bash
git status --short --branch
git fetch origin
git --no-pager log --oneline HEAD..origin/main
```

| Phần | Ý nghĩa |
|---|---|
| `fetch origin` | Lấy lịch sử mới, gồm commit checklist vừa vào main. |
| `--no-pager` | In trực tiếp vào terminal. |
| `log --oneline` | Hiện commit trên từng dòng ngắn. |
| `HEAD..origin/main` | Chọn commit có trong origin/main mà HEAD hiện tại chưa chứa. |

Bạn cần thấy commit liên quan tới checklist, có thể kèm merge commit tùy cách người quản lý đã chọn. Lệnh fetch cập nhật origin/main trên máy. Để nhánh team-note nhận những commit đó, gộp nguồn origin/main vào nhánh hiện tại.

```bash
git merge --no-edit origin/main
```

| Phần | Ý nghĩa |
|---|---|
| `merge` | Tích hợp lịch sử nguồn vào nhánh hiện tại team-note. |
| `--no-edit` | Chấp nhận thông điệp merge Git tạo sẵn. |
| `origin/main` | Nguồn dữ liệu đã fetch từ main trên GitHub. |

Trong bài tập, hai PR thêm hai file khác nhau, nên Git thường tự gộp được. Mở file team-checklist.md để xác nhận nhánh dev đã có checklist.

Đọc phần thay đổi còn lại của PR.

```bash
git --no-pager diff --stat origin/main...HEAD
git --no-pager diff origin/main...HEAD -- team-practice.txt
git status --short --branch
git push origin codex/team-note
```

`diff --stat` hiện thống kê file. Phạm vi ba dấu chấm so sánh từ tổ tiên chung đến HEAD. Thêm `-- team-practice.txt` giới hạn vào file thực hành.

Phần diff của PR team-note giờ cần tập trung vào file team-practice.txt. Checklist đã thuộc main nên thường vắng mặt trong phần thay đổi mới của PR.

Nếu có conflict trong công việc thực tế, dùng cách ở Bài 6 để sửa file, stage bản đã thống nhất rồi commit. Khi merge còn dở và muốn quay lại trạng thái trước đó, dùng `git merge --abort`.

GitHub cũng có thể cung cấp nút Update branch, tùy quyền và trạng thái nhánh. Bài này dùng merge trên máy để bạn thấy rõ hướng cập nhật. [Tài liệu cập nhật nhánh PR](https://docs.github.com/en/pull-requests/how-tos/create-pull-requests/keeping-your-pull-request-in-sync-with-the-base-branch)

## 7. Tester kiểm tra lại phiên bản sau đồng bộ

Trong bản tester, chuyển về nhánh kiểm tra.

```powershell
cd C:\Users\Hello\Desktop\gogit-tester-lab
```

```bash
git switch codex/team-review
git status --short
git pull --ff-only
git rev-parse --short HEAD
```

Upstream của team-review vẫn là origin/codex/team-note. Sau pull, bản kiểm tra có cả file quy trình và checklist vừa được tích hợp.

Ghi lại mã commit và kết quả kiểm tra mới.

| Nội dung kiểm tra | Mong đợi |
|---|---|
| team-practice.txt | Dev → tester → người duyệt → merge. |
| team-checklist.md | Có ba mục kiểm tra từ PR đã gộp. |
| Files changed của PR team-note | Thể hiện phần đóng góp còn lại so với main. |
| Checks của PR | Đọc kết quả phiên bản mới nếu repo có cấu hình kiểm tra. |

Tester gửi báo cáo, người duyệt xem phản hồi và quyết định cách gộp PR team-note theo Bài 7.

## 8. Bắt đầu phần việc tiếp theo từ main mới

Sau khi PR team-note đã Merged, làm trong bản dev khi thư mục làm việc sạch.

```bash
git switch main
git pull --ff-only
git switch -c codex/team-next-task
```

Nhánh main của bản clone bắt đầu từ main remote và chưa có các commit luyện tập riêng. Vì vậy nó có thể tiến tới lịch sử được chấp nhận qua pull, kể cả khi các PR được squash hoặc rebase.

Nhánh mới team-next-task bắt đầu từ main mới, tiếp nối các thay đổi đã được nhóm chấp nhận. Bản repo gốc gogit vẫn có lịch sử local của riêng nó, nên cần xem trạng thái và log trước khi áp dụng lệnh đồng bộ ở đó.

## 9. Nếu push bị từ chối do remote có commit mới

Thông báo non-fast-forward hoặc fetch first có thể xảy ra khi người khác đã cập nhật chính nhánh bạn đang push. Hãy kiểm tra nhánh hiện tại trước, rồi xem hai hướng lịch sử.

Ví dụ khi đang trên codex/team-note trong bản dev.

```bash
git fetch origin
git --no-pager log --oneline HEAD..origin/codex/team-note
git --no-pager log --oneline origin/codex/team-note..HEAD
```

Lệnh log đầu đọc phần remote có thêm. Lệnh thứ hai đọc phần local có thêm. Khi đã đọc và muốn kết hợp cả hai lịch sử, với thư mục làm việc sạch, thực hiện.

```bash
git merge --no-edit origin/codex/team-note
git push origin codex/team-note
```

Giải quyết conflict theo Bài 6 nếu Git yêu cầu, rồi tester kiểm tra lại bản đã hợp nhất. Nếu bị từ chối vì thiếu quyền hoặc quy tắc bảo vệ, hãy đọc thông báo cụ thể vì đồng bộ commit xử lý nhóm lỗi khác.

## Mở rộng cho đóng góp mã nguồn mở qua fork

Với người đóng góp bên ngoài, origin thường trỏ tới fork của họ, còn upstream là tên đặt cho repo gốc.

| Công việc | Nguồn hoặc đích thường dùng |
|---|---|
| Lấy main mới của dự án | Fetch upstream, đọc upstream/main. |
| Gửi nhánh đóng góp | Push origin, tức fork của người đóng góp. |
| Mở PR | Base là repo gốc, head là nhánh trong fork. |
| Quyết định gộp | Maintainer của repo gốc. |

Bài thực hành chính dùng hai nhánh trong cùng repo có quyền ghi. Khi thực hành fork, cần đọc cả tên repo và tên nhánh ở hai phía của PR.

## Tự kiểm tra

1. Vì sao hai bản clone có thể cùng dùng remote origin nhưng file trên máy khác nhau?
2. Vì sao báo cáo tester cần có mã commit?
3. Khi main có commit mới, fetch đã đưa file mới vào nhánh dev chưa?
4. Khi đứng trên team-note, merge origin/main cập nhật nhánh nào?
5. Sau khi squash một PR, vì sao phần việc mới nên bắt đầu từ main đã đồng bộ?

<details>
<summary>Xem đáp án sau khi tự trả lời</summary>

1. Mỗi bản clone có working directory, staging area và nhánh local riêng.
2. Để biết đúng phiên bản được kiểm tra và xác định lần cập nhật cần kiểm tra lại.
3. Fetch cập nhật mốc remote, cần bước tích hợp để nhánh dev nhận lịch sử mới.
4. Team-note hiện tại nhận lịch sử từ origin/main.
5. Main chứa phiên bản được chấp nhận, còn nhánh nguồn cũ có thể mang các mã commit riêng trước squash.

</details>

**Bài tiếp theo** Bài 9 sẽ luyện stash, revert, reset, reflog và cách cứu các tình huống thao tác nhầm.
