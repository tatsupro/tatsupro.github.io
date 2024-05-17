# Cloud

**Máy tính của người khác trên Internet**. Theo cách truyền thống, dữ liệu và chương trình của người dùng được lưu và chạy trên máy tính thuộc sở hữu của cá nhân hoặc tổ chức đó. Sau này khi công nghệ điện toán và mạng máy tính phát triển thì về cơ bản 2 cái trên chạy và lưu trên máy tính của cloud service provider.

Một số <u>thể loại cLoUd</u> có thể kể đến như:

1. Hoàn toàn phụ thuộc vào bên khác (thường là những gã khổng lồ luôn nhăm nhe lấy tiền trong ví của bạn)
2. cỜ-lAo tự cung tự cấp
3. Xài kết hợp cả hai
4. Hoặc đơn giản nhất là không dùng :&rpar;

Các dịch vụ điện toán đám mây thường rơi vào 3 <u>thể loại dịch vụ</u> sau:

1. **SaaS (Software as a service)** là các dịch vụ mọi người thường thấy như Gmail, Google Drive (Workspace), OneDrive (365), Dropbox, iCloud, v.v. mà người dùng chỉ cần biết kiến thức tin học phổ thông để sử dụng.
2. **PaaS (Platform as a service)** là dịch vụ mà người dùng bình thường không dùng. Khách hàng chính của các loại dịch vụ này là người mà cần một nền tảng để vận hành ứng dụng mà họ phát triển trên đó mà không cần động tới phần phức tạp của cơ sở hạ tầng (như Firebase, Netlify, Heroku, v.v.)
3. **IaaS (Infrastructure as a service)** là lý do doanh nghiệp của bạn cần đội "Cloud Engineer" sử dụng để cung cấp hạ tầng thiết bị hoặc máy tính từ xa dưới dạng trừu tượng nào đó. Đây là thể loại dịch vụ tuy phức tạp nhưng giúp cho bạn hoàn toàn kiểm soát cơ sở hạ tầng đảm bảo tài nguyên sử dụng cho bạn. 3 dịch vụ phổ biến nhất là Amazon Web Services (AWS), Google Cloud Platform (GCP) và Microsoft Azure.

## Danh sách sản phẩm và dịch vụ IaaS phổ biến

Sau đây là danh sách các sản phẩm và dịch vụ hay sử dụng từ 3 cloud provider phổ biến nhất.

> **ℹ️ Tip:** Dịch vụ tuy có nhiều nhưng quan trọng nhất cũng chỉ là VM hoặc Kubernetes. Các dịch vụ khác chỉ xoay quanh hỗ trợ 2 cái này.

| Tên dịch vụ                    | AWS                                       | Azure                                      | GCP                            |
| ------------------------------ | ----------------------------------------- | ------------------------------------------ | ------------------------------ |
| Máy ảo (VM - Virtual Machine)  | Elastic Compute Cloud (EC2)               | Virtual Machine                            | Compute Engine                 |
| Kubernetes                     | Elastic Kubernetes Service (EKS)          | Azure Kubernetes Service (AKS)             | Google Kubernetes Engine (GKE) |
| Serverless                     | Lambda                                    | Azure Functions                            | Cloud Functions                |
| Lưu trữ (Storage)              | Simple Storage Service (S3)               | Blob Storage                               | Cloud Storage                  |
| Ổ cứng (Disk)                  | Elastic Block Store                       | Managed Disk                               | Persistent Disk                |
| VPC - Virtual Private Cloud    | Virtual Private Cloud                     | Virtual Network                            | Virtual Private Cloud          |
| DNS - Domain Name System       | Route 53                                  | DNS                                        | Cloud DNS                      |
| Load Balancer                  | Elastic Load Balancing                    | Load Balancer                              | Cloud Load Balancing           |
| Tường lửa (Firewall)           | Web Application Firewall                  | Web Application Firewall                   | Cloud Armor                    |
| SQL                            | RDS                                       | SQL Database                               | Cloud SQL                      |
| NoSQL                          | DynamoDB                                  | Cosmos DB                                  | Firebase Database              |
| Dataware house                 | Redshift                                  | Synapse Analytics                          | BigQuery                       |
| Message queue                  | Simple Queuing Service                    | Storage Queues                             | Pub/Sub                        |
| Monitor                        | CloudWatch                                | Monitor                                    | Cloud Monitoring               |
| Identity Service               | IAM                                       | Microsoft Entra ID                         | Cloud Identity                 |
| Key management                 | KMS                                       | Key Vault                                  | Cloud KMS                      |
| Secret                         | Secrets Manager                           | Key Vault                                  | Secret Manager                 |
| Package & Container Repository | Elastic Container Registry & CodeArtifact | Azure Container Registry & Azure Artifacts | Artifact Registry              |

Bọn IT đụt mắt cận chưa bao giờ biết cách đặt tên cho đúng, tên các service như trên chỉ để phục vụ mục đích marketing.

## Tranh cãi

Ừm chẳng có gì là tốt đẹp cả, tự vận hành cơ sở hạ tầng tuy tốn kém tiền của và yêu cầu nhiều nhân lực hơn nhưng được lợi ích lâu dài. Một khi sử dụng cloud thì số phận của bạn thuộc về một bên khác và bạn chẳng làm được gì nhiều với đồ ăn liền.
