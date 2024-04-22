# GitLab

GitLab hand-on. Thông tin đầy đủ hơn: [READ THE DOCS](https://docs.gitlab.com/).

## GitLab Runners

Đây là một chương trình chạy ở một máy tính khác máy tính để host GitLab, bản thân GitLab vận hành khá nhiều chức năng (như lưu code, quản lý và các công việc tự động hóa, lưu các kết quả từ những pipeline) nhưng khi chạy job thì nó sẽ trigger job chạy ở trên 1 runner, phó thác job trong pipeline đó cho runner và chỉ nhận đầu ra là output của các job đó.

### Luồng

GitLab gửi cho Runner hướng dẫn quy trình chạy các job ➡️ Runner pull và chạy docker image về để tạo môi trường chạy job ➡️ Pull file từ git repo trên GitLab ➡️ Chạy tất cả các dòng lệnh được liệt kê trong quy trình ➡️ Trả lại kết quả về GitLab ➡️ Xong xuôi các thứ thì destroy hết đống container vừa dùng 💥.
Về cơ bản để tối ưu thì có thể chạy nhiều hơn một Runner.

## Pipeline

Trong lĩnh vực này thì để chỉ 1 quy trình hay lặp đi lặp lại, cụ thể khi triển khai tích hợp một ứng dụng thường có 3 bước: test, build, deploy. Những quy trình có dạng kiểu này được gọi là một pipeline, tùy theo từng ứng dụng sẽ có những bước khác nhau... nhưng ý tưởng chung là vậy. Xem video của fireship để hiểu nhanh và rõ hơn [tại đây](https://youtube.com/watch?v=scEDHsr3APg).

### Pipeline stages
Bruh, mỗi pipeline có nhiều stage khác nhau... duh. Phân stage ra pls.

```yaml
stages:
	- test
	- build

test1:
	stage: test
	script:
		- test someshit

test2:
	stage: test
	script:
		- test another shit

build:
	stage: build
	script:
		- build someshit
```

## Job artifacts
Job có thể đưa ra ouput là 1 tập hợp các file hoặc thư mục, những file và thư mục đó được gọi là artifact. Artifacts có thể dùng cho nhiều job khác nhau và đó là use case chính của nó.

```yaml
job:
	script: echo "build xyz project"
	artifacts:
		name: "$CI_JOB_NAME" # tùy chọn
		paths:
			- path/*xyz/*
		expire_in: 1 week
```
