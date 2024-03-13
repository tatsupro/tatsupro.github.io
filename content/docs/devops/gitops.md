---
title: GitOps
layout: "layouts/home.njk"
eleventyNavigation:
  key: GitOps
  parent: DevOps
---

# GitOps

Chỉ là một tiêu đề khó hiểu cho việc sử dụng VCS làm 1 nơi thống nhất để quản lý source code và mấy file manifest của Kubernetes. Có thể bạn đang thắc mắc: **Tại sao có cả Kubernetes?**

Đó là vì khi cần deploy 1 ứng dụng lên 1 cụm k8s, bạn cần tạo resource để app của bạn *"có thể là 1 trang Wordpress"* chạy trên đó và để cho k8s biết được resource bạn muốn tạo thì chúng ta dùng `kubectl` hoặc 𝓓𝓮𝓬𝓵𝓪𝓻𝓪𝓽𝓲𝓿𝓮 hơn thì apply bằng file manifest. Vậy sau khi tạo resource và đẩy app lên xong rồi vứt đống file manifest vào hư vô? Tất nhiên là không, vì vậy cần tạo 1 folder trong source code đó tên là `k8s` và vứt tất cả những gì bạn vừa viết vào đó. Sau đó bạn nhận ra sẽ khá là mệt nếu cứ mỗi lần muốn deploy app bạn phải `kubectl rollout`, vậy nên bạn setup 1 luồng CI/CD và viết 1 file shell script nào đó để bớt những công việc tay chân đi.

Vô hình trung, bạn đã vừa provision resource bằng mấy file YAML được lưu ở trên Git thông qua CI/CD + Shell Script và người ta gọi đó là **GitOps**.