# Cloudflare

> ⚠️ Trang còn sơ sài, cần cập nhật

Uhm chẳng biết tóm tắt như thế nào về những sản phẩm chúng nó bán ra, nhưng nhìn chung chúng nó nhờ có mạng lưới máy tính rộng lớn trên thế giới mà bán được rất nhiều dịch vụ khác nhau. Nổi tiếng nhất là DNS, cứ nhắc tới Cloudflare là nhớ ngay tới DNS.

Service thì nhiều nhưng cái chung chung cần biết + ổn định để dùng thì có: DNS, Proxy, Page Rules, Firewall, WARP/Zero Trust, Worker, One, ...

![](/assets/cf-1.png)

- DNS Only chỉ đơn giản là resolve DNS, còn Proxy thì khi trỏ tới máy chủ của Cloudflare, thông qua đó mới thông sang máy thật được. Cái này hay dùng để che địa chỉ IP của máy thật.
  Qua Proxied thì cũng có active thêm các service như Speed: Cache, CDN, cũng có Image Resizing luôn, Firewall, Page/API Shield.
  Workers giống Plugin của Kong, dùng để chạy một đoạn code can thiệp vào request nào đó.
- Page Rules như tên gọi.
- WARP/Zero Trust bản thân nó giống như 1 VPN trung gian giúp cho mọi người kết nối được vào mạng nội bộ của công ty, tuy nhiên khác biệt thì đường mạng này mọi người phải được verify IAM thì mới vào được.

![](/assets/cf-2.png)
