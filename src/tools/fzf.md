# Fuzzy Finder

Tool tìm kiếm (trong Unix shell) tương tác với nhiều thể loại list (như files, command history, processes, hostnames, bookmarks, git commits, v.v.).

## Cài đặt

1. Xem hướng dẫn ở đầu mục này: [Installation](https://github.com/junegunn/fzf?tab=readme-ov-file#installation)
2. Tích hợp vào shell tương ứng: [Setting up shell integration](https://github.com/junegunn/fzf?tab=readme-ov-file#setting-up-shell-integration)

## Sử dụng

Lệnh `fzf` để mở interactive finder, đơn giản nó sẽ đọc list từ input (`STDIN`) và write phần tử trong list được chọn vào output (`STDOUT`). Ví dụ câu lệnh này:

```sh
find * -type f | fzf > selected
```

- Input là danh sách các file
- Output là giá trị được chọn trong finder sẽ được redirect và write vào file `selected`

### Phím tắt cơ bản

- Tìm và `cd` vào thư mục bất kì: `Alt-C`
- ... chọn tên thư mục hoặc file bất kì: `Crtl-T`
- ... chọn lệnh trong lịch sử: `Crtl-R`
- Di chuyển lên/xuống: `Crtl-K`/`Crtl-J`

Chi tiết về keymap trên và cách thức cấu hình lại: [Key bindings for command-line](https://github.com/junegunn/fzf?tab=readme-ov-file#key-bindings-for-command-line)

Nếu dùng Mac thì cần map lại phím `⌥ Option` thành `Alt`

### zsh/bash completion

```sh
COMMAND [DIRECTORY/][FUZZY_PATTERN]**<TAB>
```

Ví dụ với file hoặc thư mục:

```sh
vim **<TAB>
vim ../**<TAB>
vim ../fzf**<TAB>
vim ~/**<TAB>
cd **<TAB>
cd ~/github/fzf**<TAB>
```

Process IDs:

```sh
kill -9 **<TAB>
```

Host names:

```sh
ssh **<TAB>
telnet **<TAB>
```

Environment variables / Aliases:

```sh
unset **<TAB>
export **<TAB>
unalias **<TAB>
```

## Liên kết ngoài:

- Fuzzy Finder trêm GitHub: [fzf - GitHub](https://github.com/junegunn/fzf)
