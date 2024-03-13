---
layout: "layouts/home.njk"
eleventyNavigation:
  key: GitOps
  parent: DevOps
---

# GitOps

Chỉ là một tiêu đề khó hiểu cho việc sử dụng VCS làm 1 nơi thống nhất để quản lý source code và mấy file manifest của Kubernetes. Có thể bạn đang thắc mắc: **Tại sao có cả Kubernetes?**

Đó là vì khi cần deploy 1 ứng dụng lên 1 cụm k8s, chúng ta cần tạo resource trong đó để app của bạn *"có thể là 1 trang Wordpress"* chạy trên đó bằng file manifest, vậy nên chỉ đơn giản là tạo xong rồi vứt nó vào hư vô? Tất nhiên là không và chúng ta nên tạo 1 folder trong source code đó tên là `k8s` và vứt tất cả những gì chúng ta vừa viết vào đó. Sau đó bạn nhận ra sẽ khá là mệt nếu cứ mỗi lần muốn deploy app bạn phải `kubectl rollout`, vậy nên bạn setup 1 luồng CI/CD và viết 1 file shell script nào đó để bớt những công việc tay chân đi.

Vô hình trung, bạn đã vừa provision resource bằng mấy file YAML được lưu ở trên Git thông qua CI/CD + Shell Script và người ta gọi đó là **GitOps**.