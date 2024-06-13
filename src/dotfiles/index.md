# Dotfiles

Thông thường các ứng dụng cho người dùng hay lưu trữ config ở `home` đối với Linux, BSD hay MacOS. Bằng cách sử dụng [git](../tools/git.md) để quản lý dotfiles không chỉ để theo dõi thay đổi config dễ dàng hơn mà còn có thể push code lên remote và dùng config này ở nhiều thiết bị khác nhau.

## tats does dotfiles

[↗ link github repo](https://github.com/tatsupro/dotfiles)

Đây là dotfiles tối giản chứa config, các script để setup và quản lý các package chạy trên cả MacOS và Arch Linux. Nó được cấu hình để phù hợp với luồng làm việc của tôi và để dễ dàng setup trên bất kì thiết bị chạy 2 hệ điều hành nêu trên, với mục tiêu đảm bảo config tối giản & có đủ những thứ để sử dụng, không tùy biến quá sâu để giúp bạn dù dùng Arch nhưng vẫn đảm bảo cho bạn có một cuộc đời và có thời gian đi kiếm người yêu thay vì ngồi nhà phí thời gian vào configuring rapid hole.

Để sử dụng lại thì bạn có thể fork lại repo này, chỉnh sửa lại các variable trong các file script, thêm hoặc loại bỏ config hoặc các package theo nhu cầu cá nhân (như loại bỏ các config liên quan tới Mac nếu bạn không dùng).

### Các chương trình có trong dotfiles này

- [**zsh**](/tools/shell.html#zsh): Tương tác với máy tính qua dòng lệnh.
  - Xem danh sách các plugin [ở đây](/tools/shell.html#các-plugins).
- [**neovim**](/tools/neovim.html): Text Editor dạng dòng lệnh.
- [**lf**](/tools/lf.html): Trình duyệt và quản lý file dạng dòng lệnh.
- [**git**](/tools/git.html): Version Control Tool
- [**fzf**](/tools/fzf.html): Để tìm nhiều thứ và tương tác qua dòng lệnh.
- [**docker** và **docker compose**](/container/docker.html): Container Tool.
- [**kubectl** và **kubectx**](/container/kubectl.html): Kubernetes Tool.
- [**terraform**](/terraform/index.html): Infra as Code Tool.

## Liên kết ngoài

- [Dotfiles - Arch Wiki](https://wiki.archlinux.org/title/Dotfiles)
- [Dotfiles - Missing Semester](https://missing.csail.mit.edu/2019/dotfiles)
- [GitHub does dotfiles](https://dotfiles.github.io)
