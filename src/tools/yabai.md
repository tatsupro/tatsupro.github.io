# yabai

Titling Window Manager cho MacOS. Lý do chúng ta cần yabai là vì Window Manager mặc định bị đần.

## Cài đặt

1. **<u>Tắt SIP (System Integrity Protection) - không tắt cũng được nhưng có vài thứ sẽ không hoạt động được bình thường.</u>**

_<u>Đối với máy Intel</u>_

1. Tắt nguồn
2. Khởi động lại, nhấn Command + R liên tục cho tới khi Recovery Mode hiện lên
3. Nếu được hỏi, chọn ổ đĩa lưu OS
4. Bật Terminal: `Utilities > Terminal`
5. Chạy lệnh: `csrutil disable`
6. Khởi động lại

_<u>Đối với máy Apple Silicon</u>_

Tôi không dùng, đây là [hướng dẫn](https://support.apple.com/en-us/102518) của Apple.

Tài liệu của yabai hướng dẫn chi tiếp hơn: [Xem tại đây](https://github.com/koekeishiya/yabai/wiki/Disabling-System-Integrity-Protection).

2. **<u>Cài yabai và skhd</u>**

Máy bạn nên có homebrew, nếu không thì đọc tài liệu của yabai.

1. Cài `yabai` với `brew install koekeishiya/formulae/yabai`
2. Cài `skhd` với `brew install koekeishiya/formulae/skhd`

Khởi động cả 2 service để cửa sổ Accessibility hiện lên hỏi quyền truy cập:

```sh
yabai --start-service
skhd --start-service
```

Sau đó vào Settings theo hướng dẫn và allow cho cả 2 chương trình trên.

Trước khi chạy cần config, cái này có thể tùy chỉnh nhưng nếu bạn cần nhanh 1 cái thì có thể dùng của tôi: [yabai](https://github.com/tatsupro/dotfiles/blob/main/.config/yabai/yabairc), [skhd](https://github.com/tatsupro/dotfiles/blob/main/.config/skhd/skhdrc).

3. **<u>Khởi động yabai và skhd</u>**

```sh
yabai --start-service
skhd --start-service
```
## Liên kết ngoài

- Mã nguồn: [GitHub](https://github.com/koekeishiya/yabai)
- SKHD: [GitHub](https://github.com/koekeishiya/skhd)
- [Wiki của yabai](https://github.com/koekeishiya/yabai/wiki)
