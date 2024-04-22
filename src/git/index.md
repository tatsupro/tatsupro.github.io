# Git

Version control hay source control system giúp theo dõi và điều khiển thay đổi của các file theo thời gian. Các phần mềm kiểu này thường bao gồm khả năng lưu các phiên bản của file, tạo nhánh/tách nhánh file riêng biệt và so sánh các phiên bản của file. Hữu ích trong việc xác định ai là người thay đổi code. Git là 1 trong số những công cụ kiểu này.

## Kiến thức cơ bản

Khi tạo 1 git repository, git sẽ tạo ra 1 folder ẩn tên là `.git` dùng để lưu trữ cấu hình, thông tin và các theo dõi thay đổi tại thư mục hiện tại.

![](/assets/git-stage-graph.png)

Khi có một file mới trong môi trường làm việc, git sẽ không để ý thay đổi cho tới khi dùng lệnh `git add` để thêm file đó vào một vùng để theo dõi (stage area - cái này mang tính ý niệm hơn là một folder vật lý). File đó sẽ được lưu vào vào main branch hay branch nào đó khác sau khi được `commit`.

### Trong file `.git` có những gì?

```
$ ls .git/
COMMIT_EDITMSG # chứa các commit message dưới dạng plain text

HEAD # thông tin reference liên quan tới branch đang làm việc

config # thông tin config của repo (như user name vs email)

description # chứa tên của repo

hooks # thư mục chứa scripts, có thể tự tạo script
để automate một số tasks

index # file theo dõi các item ở stage area và cả file đã commit

info # thường bên trong có file liệt kê những file cần ignore

logs # như tên gọi, thư mục chứa log

objects # thư mục chứa database, các file được commit sẽ được nén và lưu dưới dạng hash trong database này

refs # thư mục chứa các file reference, dùng cho branch với tag
```

## Tạo git repo

Có 2 cách tạo mới 1 git repo, 1 là cứ tạo như bình thường và 2 là git bare. Khi tạo git thì 1 là nó sẽ chọn thư mục hiện tại hoặc tạo mới 1 thư mục khác, với git bare thì khác vì nó không tạo dựa trên 1 thư mục làm việc cụ thể vì để dành cho dự án lớn phức tạp hơn và khi tạo thì nó lấy luôn thư mục tạo git làm thư mục lưu thông tin về git repo chứ không tạo thêm `.git` nữa.

```sh
git init /path/to/directory
git init --bare /path/to/directory
```

## Thêm sửa config

Config của git được lưu dưới dạng key-value, để thêm mới hoặc sửa config thì ta có thể dùng lệnh `git config`.

```sh
git config user.name "tatsu"
git config --global user.name "tatsu"
# thêm --global để áp dụng config ở khắp mọi nơi
```

Liệt kê các config:

```sh
git config --list
```

```
credential.helper=osxkeychain
init.defaultbranch=main
user.name=tatsu
user.email=tatsu@mail.com
user.username=tatsu
init.defaultbranch=main
core.editor=/usr/bin/vim
```

## Thêm file và check status của repo

Khi có 1 file mới xuất hiện, git sẽ đếu quan tâm cho tới khi mình dùng `git add`. Sau khi `git add` một file nào đó thì để biết nó đang được track chưa thì dùng lệnh `git status`.

```sh
git add README.md
git status
```

```
On branch main
Your branch is up to date with 'origin/main'.

You are in a sparse checkout with xx% of tracked files present.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Ngoài ra cũng có thêm 1 số flag hữu ích nếu muốn quan tâm:

```sh
# để hiện thị gọn hơn
git status -s
```

```
M README.md
```

```sh
# để hiển thị đầy đủ (verbose) hơn
git status -v
```

```
# như cái git status thường nhưng giờ có thêm cả diff
```

Muốn xoá file thì dùng git rm (xoá thật, real đó):

```sh
git rm <file-nào-đó>
```

Nhưng trong trường hợp không muốn xoá file, chỉ muốn git không biết đến sự tồn tại của file này:

```sh
git rm --cached <file-nào-đó>
```

## Commit file vào repo

Đến lúc này bạn cảm thấy tự tin và đẹp zai với công việc của mình rồi, giờ đến lúc tạo cái snapshot cho nó trong 1 branch thôi.
`git commit` sẽ mở file editor lên để bạn viết commit message cho những file bạn vừa add.
Cảm thấy hơi bị lười? Hãy dùng `-m` để đỡ khỏi gọi cái editor nào ở đây:

```sh
git commit -m "đây là commit message"
```

Đây là cách ai cũng dùng.
Để lười hơn nữa thì dùng `-a`, giờ bất cứ file nào thay đổi sẽ được commit luôn, đỡ khỏi add lại:

```sh
git commit -am "đây là commit message"
```

## Ignore một số file

Thường sẽ có file, folder hoặc kiểu file bạn không muốn git track lại... điển hình là `node_modules`. Khi đó ta sẽ dùng file `.gitignore`.
Uhm dù sẽ chẳng mấy khi động tới bao giờ, nhưng cũng nên để ý là file `.git/info/exclude` cũng giống `.gitignore` chỉ khác ở vị trí để file thôi.
Ghi các pattern file mà muốn git né (chắc là regex) vào 1 trong 2 file này là git sẽ né chúng nó ra. Để kiểm tra pattern đúng không (vì mục đích debug), dùng lệnh `git check-ignore`.

## Tags

Chưa bao giờ dùng tới, Tag là chức năng đặt tên một cách đơn giản của Git, nó cho phép ta xác định một cách rõ ràng các phiên bản mã nguồn (code) của dự án. Ta có thể coi tag như một branch không thay đổi được. Một khi nó được tạo (gắn với 1 commit cụ thể) thì ta không thể thay đổi lịch sử commit ấy được nữa.
Có 2 loại tag là **annotated** và **lightweight**.

**Lightweight** tag thực chất chỉ là đánh dấu (bookmark) cho một commit, vì chúng chỉ lưu trữ hash của commit mà chúng tham chiếu. **Annotated** thì ngoài tên còn có thể lưu trữ dữ liệu bổ sung như Tên tác giả (-s), tin nhắn (-m: message), và ngày dưới dạng các đối tượng đầy đủ trong cơ sở dữ liệu Git. Tất cả thông tin ấy quan trọng cho việc release dự án.

```sh
# tạo annotated tag
git tag -a v1.0.0 -m "Releasing version v1.0.0"
git tag # xem tất cả các tag
# tạo lightweight tag
git tag v1.0.0 -m "Releasing version v1.0.0"
git tag -d v1.0.0 # xóa tag
```

## Branch

Quá quen thuộc, không có gì xa lạ.

```sh
git branch # liệt kê các branch
git branch branch-name # tạo branch mới có tên là branch-name
git checkout branch-name # chuyển sang branch-name
git merge # gộp thay đổi từ nhánh khác vào nhánh hiện tại
git branch -d branch-name # xóa branch
```

Nếu việc merge thuận lời thì nhánh main sẽ fast-forward tới thay đổi mới nhất của nhánh dev như hình dưới:

![](/assets/git-branch.png)

Còn không thì sẽ bị dính M̵̈́͜E̵͈͒R̸͉͂G̷͈͂E̵͚͒ ̷̡͆C̶͎̆O̵̢͆N̷̺̒F̵͙̓L̵͉̀Ȋ̵̙C̵̉ͅT̸̩́. Khi dính merge conflict thì phải resolve bằng tay thôi.
Cũng là gộp nhánh khác vào, nhưng rebase khác fast forward ở chỗ nó áp thẳng (replay) các commit từ nhánh khác thay vì fast-forward tới. **Thì?:** Nó sẽ lưu lại lịch sử trong log đẹp hơn, dùng rebase cho code của mình thôi, đừng dùng cho thằng khác.

## Lệnh

```sh
git revert # quay về một commit nào đó
git diff # so sánh thay đổi giữa commit/branch/etc
git gc # Garbage Collection, để dọn dẹp database khi có object không còn được tham chiếu tới nữa trong database
git gc --prune # như trên nhưng chỉ dọn mấy cái cũ hơn 2 tuần theo mặc định. có thể tùy chỉnh thời gian.
git config gc.pruneexpire "30 days"
git log # xem log
git log # cũng xem log nhưng đẹp hơn
git log --stat
git log --pretty=format:
git log --since="4 days ago"

# cái này gọi là phê, đọc bớt hại mắt hẳn
git shortlog
git log --oneline
git log --oneline --graph

# lệnh phê nhưng hơi dài
git log --graph --pretty="%C(bold blue)%h %Cred%s %C(Yellow)by %an"
git log --graph --pretty=format:"%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset" --abbrev-commit

# đến lúc này thì nên lười và alias nó thành một lệnh nào đó
git config --global alias.l0g 'log --color --graph --pretty="%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset" --abbrev-commit'

# lệnh log thì có nhiều thứ để bàn tới lắm, nhưng cứ dùng GUI nào đó như GitLens cho nhanh thay vì nhớ đi

git clone <repo link> # để clone
# lưu ý chút thì ngoài khả năng clone repo trên remote, git có thể clone được cả ở local
git clone <local repo folder> <new repo>
git remote -v # liệt kê mấy remote server đang theo dõi thay đổi
git fetch # lấy các commit mới từ remote về
# để upload thay đổi lên remote, dùng git push
git push <remote> <local branch>
git pull <remote> <local branch> # dùng để kéo branch từ remote
```

## Tham khảo thêm

- Học thêm ở đây: [KillerCoda - Git foundations](https://killercoda.com/pawelpiwosz/course/gitFundamentals)
