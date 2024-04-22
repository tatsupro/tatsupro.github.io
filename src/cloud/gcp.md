# Google Cloud

## Compute Engine

**<u>VMs, GPUs, TPUs, Disks.</u>**

Compute Engine dùng để tạo máy ảo chạy OS và cấu hình tuỳ thích trên Google Infra.

![](/assets/gcp-1.png)

![](/assets/gcp-2.png)

![](/assets/gcp-3.png)

## Kubernetes Engine

GKE là một giải pháp Kubernetes (một nền tảng mã nguồn mở điều phối container) quản lý bởi Google, bên cạnh việc cung cấp k8s thì còn quản lý control plane và nodes.

### Khi nào sử dụng

Khi dùng container. Cần config scale, network, hardware và secutiry.

![](/assets/gcp-4.png)

## Networking

Hạ tầng mạng của Google Cloud được dựa trên cơ cấu mạng [Jupiter network fabric](https://cloud.google.com/blog/products/gcp/a-look-inside-googles-data-center-networks) và ảo hoá mạng bằng [Andromeda](https://www.usenix.org/conference/nsdi18/presentation/dalton).

![](/assets/gcp-5.png)

![](/assets/gcp-6.png)

Andromeda là một SDN (software-defined networking), đây là nền móng của hệ thống mạng ảo. Kiểu nó như là orchestration point cho provisioning, configuring và quản lý mạng ảo và hoạt động packet processing trong mạng.

![](/assets/gcp-7.png)

### Physical Network Organisation

Infra được chia thành từng vùng (region), nhỏ hơn chia thành từng miền (zone).

- 1 vùng có tính chất như 1 khu vực địa lý.
- 1 miền có tính chất là 1 khu vực deploy trong 1 vùng, miền có không gian isolated riêng \
  _Vậy nên nếu có vấn đề xảy ra, 2 máy tính nằm ở vùng hoặc miền khác sẽ không có chung 1 kết quả._

### Networking Services

Có 5 loại dịch vụ:

- Connect
- Secure
- Scale
- Optimize
- Modernize

### Network Service Tiers

Cuộc đời có nhiều cách để tiêu tiền, tiền vào cloud cũng vậy.

![](/assets/gcp-8.png)

![](/assets/gcp-9.png)

![](/assets/gcp-10.png)

### Load Balancing

![](/assets/gcp-11.png)

## SQL

Như tên gọi, dùng để quản lý CSDL quan hệ (MySQL, PostgreSQL, SQL Server).

![](/assets/gcp-12.png)

## Storage

Như tên gọi, toàn mấy dịch vụ để lưu trữ dữ liệu dưới dạng Object storage. Uhm nó là một kiểu lưu trữ mà mỗi file kiểu có metadata vs id cụ thể. Object storage is immutable, vậy nên không chỉnh sửa được mà mỗi lần thay đổi gì đó thì nó sẽ tạo bản sao được chỉnh sửa mới.
Nếu bật versioning thì bản cũ sẽ được giữ lại, còn không thì sẽ bị bản mới ghi đè.

![](/assets/gcp-13.png)

Cloud storage files được tổ chức theo các buckets, mỗi bucket có id và địa điểm lưu trữ địa lý cụ thể.
Storage có thể được quản lý bằng IAM.

### Lifecycle policies

Bộ nhớ sinh ra để bị lấp đầy, để cho bớt nặng thì có thể set policy kiểu:

- Xoá object cũ hơn 365 ngày
- Xoá object được tạo trc MM/DD/YYYY
- Chỉ giữ lại 3 phiên bản gần nhất

### Nên sử dụng kiểu lưu trữ nào?

Tuỳ vào mục đích sử dụng và loại dữ liệu muốn lưu.

![](/assets/gcp-14.png)

### Storage Class

![](/assets/gcp-15.png)

## IAM

Viết tắt của Identity and Access Management, dùng để quản lý quyền truy cập tới các resource trên cloud (cái này thì thằng nào cũng có). Được thiết kế dựa trên các nguyên tắc PoLP (Principle of least privilege): bất kì thành phần nào cũng sẽ chỉ được có quyền truy cập vừa đủ để thực hiện chức năng của nó.

![](/assets/gcp-16.png)

IAM sẽ định nghĩa `ai (who - identity) | có quyền gì (what role) | với tài nguyên nào (where - resource)`.

### Policy

Policy là 1 tập hợp các binding, audit config và metadata.

- Binding định nghĩa việc các cấp quyền truy cập tài nguyên ra sao, role được gán cho những member nào, với các condition binding khác.
- Metadata là tập hợp các thông tin về policy (etag, version).
- Audit config như tên gọi.

![](/assets/gcp-17.png)

![](/assets/gcp-18.png)
