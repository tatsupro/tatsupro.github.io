# Linux

> ⚠️ Trang còn sơ sài, cần cập nhật

1. Linux và GNU là 2 thứ khác nhau
2. Có nhiều distro nhưng tất cả đều là Linux, không khác gì nhau ngoài package manager
3. Tìm hiểu thêm kiến trúc OS: [link](https://en.wikipedia.org/wiki/Linux#Design)
4. GUI và CLI là 2 thứ khác nhau
5. Shell, Prompt và Terminal là những thứ khác nhau

![Linux API](https://upload.wikimedia.org/wikipedia/commons/4/43/Linux_API.svg)

## Tổ chức File hệ thống

Tổ chức như nhánh cây, thư mục level cao nhất là root `/`.

```sh
bin # chứa binary thiết yếu cho toàn OS, ls /bin để bt thêm
dev # các files tương tác với phần cứng như driver
etc # các files config của toàn OS
home # nơi người dùng lưu file
lib # các thư viện cho toàn hệ thống
mnt # chứa các file của các thiết bị được mount
proc # các thứ liên quan tới các luồng chương trình đang vận hành
root # như home nhưng dành cho người dùng root
sbin # các binary thiết yếu cho sysadmin
tmp # nơi lưu trữ file tạm thời
usr # nơi lưu binary,config,... người dùng sử dụng
```

## Lệnh Linux

### Navigate File System

```sh
$ pwd # in ra path của working directory
/your/current/directory
$ cd /your/directory # thay working directory
$ ls # in ra tên file và thư mục của working directory
abc xyz dcm vcl
```

### Chỉnh sửa file

```sh
$ touch # vcl anh tạo 1 file để chạm vào trái tim em
$ mkdir # tạo thư mục mới
$ rm # remove duh
$ cp # copy duh
```

### Xem file text

```sh
$ cat # in toàn bộ nội dung file
$ head # phần đầu nội dung file
$ tail # phần cuối nội dung file
$ more # giống cat nhưng chỉ hiện thị hết 1 màn hình, có thể lướt qua lướt lại được
$ less # cải tiến của more
```

### Echo

```sh
$ echo bonjour!
bonjour!
```

### Xử lý text

```sh
$ grep "1" test.txt
$ sed 's/<text_to_replace>/<replacement_text>/' <file_name>
# sed để thay thế text bất kì
$ sort # để sắp xếp nội dung trong file
```

### I/O Redirection

```sh
# để chuyển output của 1 cmd về 1 file bất kì
$ echo "Hello World" > hello.txt
# dùng pipe để pass output sang làm argument của cmd khác
$ sort numbers.txt | uniq
```

## Sysadmin

### Users/Groups Management

- Linux users có UID, home directory và login shell
- Group bao gồm nhiều user, có UID và share được quyền cho chung 1 nhóm

```sh
$ id # hiển thị UID, GID, và các group mà user thuộc về.
uid=0(root) gid=0(root) groups=0(root)
$ whoami # hiển thị tên người dùng hiện tại
root
```

Root user (hay superuser) là user có quyền cao nhất, có UID là 0

| File          | Mô tả                                                    |
| ------------- | -------------------------------------------------------- |
| `/etc/passwd` | Chứa username, uid, gid, home directory, login shell etc |
| `/etc/shadow` | Mật khẩu của username tương ứng                          |
| `/etc/group`  | Các group user đang có                                   |

### Users Manage Command

```sh
$ useradd <tên user> # tạo user mới
$ passwd <tên user> # tạo hoặc sửa user password

$ usermod <tên user> # sửa user attributes
$ usermod -h # help info & list attributes
$ usermod -gm wheel -d /home/username -a <tên người dùng>
# thêm người dùng vào group bất kì `-g`
# tạo home dir cho người dùng
# append chứ ko remove khỏi group khác `-a`
# tạo home dir cho người dùng

$ userdel <tên user> # xóa user
```

#### Thêm pseudo root access cho người dùng

Đôi khi trong lúc setup, một số bản Linux không setup sẵn quyền `sudo` cho người dùng. Khi đó ta phải tự setup tay như sau:

1. Cài package `sudo`
2. Thêm người dùng vào group `wheel` như ở ví dụ trên
3. Sửa file `/etc/sudoers`, thêm hoặc bỏ comment dòng sau:

```
%wheel ALL=(ALL) ALL
```

Sau đó khi user thường chạy lệnh cần quyền root, chỉ cần thêm `sudo` trước lệnh đó là chạy được.

### Group Manage Command

```sh
$ groupadd <tên group> # tạo group mới
$ groupmod <tên group> # sửa group atributes
$ groupdel <tên group> # xóa group
$ gpasswd <tên group> # sửa group password
```

### File Management

![](/assets/linux-perms.png)

| Ký hiệu | Mô tả                                                        |
| ------- | ------------------------------------------------------------ |
| 1       | Kiểu file:<br>`-` là file thông thường<br>`d` là một thư mục |
| 2       | Quyền của owner                                              |
| 3       | Quyền của owner group                                        |
| 4       | Quyền cho toàn bộ người dùng                                 |
| 5       | Owner                                                        |
| 6       | Owner group                                                  |

```
r (read) | w (write) | x (execute)
```

| Quyền          | `rwx` | Binary | Decimal |
| -------------- | ----- | ------ | ------- |
| Đọc, ghi, chạy | `rwx` | 111    | 7       |
| Đọc, ghi       | `rw-` | 110    | 6       |
| Đọc, chạy      | `r-x` | 101    | 5       |
| Chỉ đọc        | `r--` | 100    | 4       |
| Ghi, chạy      | `-wx` | 011    | 3       |
| Chỉ ghi        | `-w-` | 010    | 2       |
| Chỉ chạy       | `--x` | 001    | 1       |
| Không có gì    | `---` | 000    | 0       |

1. `chmod` để sửa quyền truy cập vào file và thư mục, có thể dùng decimal như trên

```sh
# Example
$ touch test_file
$ ls -l test_file
-rw-r--r--  1 root wheel # Đang là 644
$ chmod 664 test_file # Thêm quyền ghi cho group wheel
$ ls -l test_file
-rw-rw-r--  1 root wheel
```

2. `chown` để thay đổi owner của file

```sh
$ chown <owner mới> <tên file>
```

3. `chgrp` để thay đổi group của file

```sh
$ chown <owner mới> <tên file>
```

### SSH

Remote things bruh, phân biệt 2 host như sau:

- client: máy đang sử dụng để remote máy khác
- remote: máy được remote tới

#### Passwordless Auth

1. Cài `openssh`
2. Tạo key mới: `ssh-keygen` trên client vì là RSA nên sẽ có 2 khóa (public và private - public key có thể gửi cho bất kì ai và private key phải giữ bí mật nhất có thể).

```sh
$ ls -l ~/.ssh
-rw------- 1 root root 2602 Jan  6 08:18 id_rsa
-rw-r--r-- 1 root root  571 Jan  6 08:18 id_rsa.pub
# private key chỉ cho root user đọc ghi
# public key cho phép all user đọc
```

3. Gửi public key tới remote

```sh
$ ssh-copy-id username@<host_ip_address>
```

4. `ssh` vào máy đích

```sh
$ ssh username@<host_ip_address>
```

5. Để kiểm tra lại xem public key được gửi lên chưa, kiểm tra trong file `~/.ssh/authorized_keys`
   Vì lý do bảo mật, có thể setup cho máy đích chỉ chấp nhận public key auth bằng cách thêm 2 dòng sau vào file này `/etc/ssh/sshd_config`:

```
PasswordAuthentication no
AuthenticationMethods publickey
```

Và không cho phép ssh bằng root user:

```
PermitRootLogin no
```

Khởi động lại để áp dụng config mới:

```sh
$ sudo systemctl restart sshd
```

#### Chạy lệnh trên remote

```
$ ssh username@<host_ip_address> 'ps aux | head'
```

#### Truyền file lên remote

```
$ scp test_file username@<host_ip_address>:/home/username
```

### Package Manager

Dùng để cài và quản lý phần mềm, mỗi distro có thể dùng 1 cái khác như Ubuntu/Debian: `apt (.deb)`, Fedora/CentOS/Red Hat: `dnf/yum (rpm)`, OpenSUSE/SLE: `ZYpp`, Arch: `pacman`, etc.

```ssh
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install vim
$ sudo apt remove vim
$ apt search vim
```

### Monitor

Có 2 lệnh cơ bản để monitor process: `ps` và `top`.
Nếu không có lệnh `ps` thì cài package `procps`

```sh
$ ps
  PID TTY          TIME CMD
   17 pts/1    00:00:00 sh
   66 pts/1    00:00:00 ps
# để hiện nhiều thông tin hữu ích hơn
$ ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0  19224  2816 pts/0    Ss+  08:17   0:00 bash
root        17  0.0  0.0  12060  3072 pts/1    Ss   08:17   0:00 /bin/sh
root        67  0.0  0.0  44708  3328 pts/1    R+   09:11   0:00 ps aux
$ ps aux | grep -i "bash"
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0  19224  2816 pts/0    Ss+  08:17   0:00 bash
root        81  0.0  0.0   9092   896 pts/1    R+   09:13   0:00 grep -i bash
```

`top` giống `ps` về chức năng nhưng chi tiết hơn và hỗ trợ check real-time.

```sh
$ free -h # hiện memory đang dùng/đang còn trống, buffers, cache
$ vmstat # giống cái trên nhưng hiện cả io/cpu usage
$ df # disk free
$ du # disk usage
```

#### Daemon

Là chương trình chạy nền (như `httpd`, `sshd`), người dùng không tương tác được.

#### Systemd

Là service manager, quản lý theo unit (có file config trong `/usr/lib/systemd/system`)

```sh
$ systemctl start name.service # chạy service
$ systemctl stop name.service # dừng service
$ systemctl restart name.service # khởi động lại service
$ systemctl status name.service # check service status
$ systemctl reload name.service # reload service config
```

#### Logs

Thường log hay lưu ở mấy chỗ sau:

- `/var/log/*`: các daemon process và system logs
  - `/var/log/messages`: system errors, booting, shutdown, system config change etc
  - `/var/log/authlog`: system auth related
  - `/var/log/lastlog`: login gần đây của toàn bộ user
- `dmesg` Kernel log, muốn chuyên sâu hơn có thể đọc Arch Wiki
- `journalctl` với hệ thống dùng systemd có thể check qua journal
  - Ví dụ check log 1 unit cụ thể `journalctl -u name.service`
