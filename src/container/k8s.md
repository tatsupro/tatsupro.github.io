# Kubernetes

Kubernetes là orchestration framework (tức nó dùng để điều phối và quản trị các container). k8s cung cấp các thứ cho container chạy ở môi trường production theo scale tuỳ thích. Kubernetes giúp quản lý một lượng lớn container dễ dàng hơn.

## Kiến trúc

![](/assets/k8s-1.png)

### Control Plane

Tên gọi khác là Master Node, là một tập hợp các thành phần quản lý chính cluster chứa nó.

![](/assets/k8s-2.png)

- **Kube API**: Đây là API server, có thể coi nó như interface tương tác với control plane (cũng như cluster) của chính nó.
- **Etcd:** Đây là server tương tác với 1 database key-value để lưu những thông tin liên quan tới state của cluster.
- **Scheduler:** Đây là thành phần quản lý việc scheduling, quản lý việc chọn node nào khả dụng trên cluster nào để chạy container.
- **Control Manager:** Đây là thành phần chạy nhiều thành phần con trong đó, quản lý nhiều task tự động hóa với cluster.
- **Cloud Control Manager:** Dùng làm interface giữa Kubernetes và cloud provider, không cần quan tâm ngay bây giờ (đang nói thằng viết câu này).

### Node

Có thể gọi là Worker Node, là một máy bị quản lý và chạy các container bởi 1 cluster nào đó, 1 cluster có thể có nhiều node.

![](/assets/k8s-3.png)

- **Kubelet:** là 1 agent chạy trên mỗi node, thường tương tác với [[#Control Plane]] để đảm bảo các Container chạy trên các Node hoạt động theo đúng chỉ dẫn từ Control Plane. Kubelet đồng thời lo report lại trang thái của Container và các thông tin khác về Control Plane.
- **Container runtime:** không phải là một phần của k8s. Nó là một phần mềm khác dùng để chạy các container mà k8s quản lý, có thể là [[Docker]] hoặc là containerd.
- **Kube Proxy:** là Network Proxy chạy trên mỗi node để lo một số thứ dạng như thông mạng giữa các container và service trong cluster.

## Dựng 1 cụm k8s

Ở đây tôi dựng 3 máy ảo AWS, 1 master node và 2 worker node. Phải đảm bảo cả 3 máy đều chung một network.

1. **Sửa file hosts**
   Cho dễ phân biệt với nhau thì có thể dùng lệnh `sudo hostnamectl set-hostname` để đổi tên lại các host. Xong thêm 3 dòng sau vào file hosts:

```
/etc/hosts
172.31.84.97 k8s-control
172.31.92.249 k8s-worker1
172.31.83.141 k8s-worker2
```

Format là `<IP> <Tên host>`. Tên host phía trên tương ứng với những host mà tôi tự đặt. 2. **Bật một số module trong kernel**
Để mỗi lần khởi động hệ thống, `overlay` và `br_netfilter` sẽ được bật.

```bash
cat << EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF
```

Chạy luôn mà không cần khởi động:

```bash
sudo modprobe overlay
sudo modprobe br_netfilter
```

3. **Thêm một số system config cho phần network của k8s**

```bash
cat << EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF
```

Và chạy lệnh sau để apply config ngay lật tức.

```bash
sudo sysctl --system
```

4. **Cài containerd**

```bash
sudo apt-get update && sudo apt-get install -y containerd
```

5. **Viết config cho cotnainerd**

```bash
# Tạo thư mục cho config
sudo mkdir -p /etc/containerd
# Generate ra config
sudo containerd config default | sudo tee /etc/containerd/config.toml
# Khởi động lại containerd
sudo systemctl restart containerd
# Kiểm tra containerd hoạt động chưa
sudo systemctl status containerd
```

6. Tắt swap đi

```bash
sudo swapoff -a
```

7. Cài Kubernetes
   Đây là doc chính thức nếu có gì đó thay đổi: [doc](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/).

```bash
# cài các deps cần thiết cho k8s
sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl
# thêm key cho repo của k8s
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
# thêm config cho repo
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

Lặp lại từ bước 2 tới bước 7 cho mỗi Node. 8. Tạo cluster

```bash
sudo kubeadm init --pod-network-cidr 192.168.0.0/16
```

9. Setup kubeconfig
   Làm theo hướng dẫn sau khi cluster được init xong.
10. Setup network
    Có nhiều cái lắm nhưng cứ tàu nhanh calico vậy:

```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

11. Cho các worker node join vào cụm

```bash
kubeadm token create --print-join-command
```

Rồi paste cái lệnh nó in ra vào mấy worker node và `kubectl get nodes` để check xem.

## Cách kết nối tới cluster

1. Đối với cloud như GCP, [cài gcloud & auth](https://cloud.google.com/sdk/docs/install) để lấy credentials.
2. Cài kubectl: [MacOS](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos)
3. (Tùy chọn) Cài kubectx/kubens để thay đổi cluster/namespace dễ dàng hơn
4. Đối với GCP, lấy credentials như sau:

```bash
gcloud container clusters get-credentials <example-cluster>
--region <place-cluster-region-here>
--project example-project
--internal
```

Tài liệu tham chiếu: https://vinid-team.atlassian.net/wiki/spaces/1MGTEC/pages/1750401610/D+ng+resources+tr+n+c+m+K8S+m+i

## Các khái niệm cơ bản

### Cách các Object đc define

Thường một file yaml sẽ trông như thế này

```yaml
apiVersion:
kind:
metadata:

spec:
```

Ví dụ tạo một Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
	name: myapp-pod
	labels:
		app: my-app
spec:
	containers:
		- name: nginx-container
		  image: nginx
```

Và dùng lệnh sau để áp dụng

```bash
kubectl create -f dat-fucking.yml
```

### Pod

Pod là đơn vị object nhỏ nhất mà kubernetes (đại diện cho một instance của một nhóm container) cho người dùng deploy một hay nhiều container chạy trong một node. Kubernetes không trực tiếp deploy container, mà gián tiếp thông qua việc deploy pod.

1. Tạo và chạy một pod (CLI): `kubectl run`
2. Tạo pod (CLI hoặc YAML): `kubectl create`
3. Tạo hoặc cập nhật thay đổi (CLI hoặc YAML): `kubectl apply`

Pod chỉ là một layer trừu tượng, chỉ là ý tưởng chứ không phải là thực thể về một nhóm của một loại tài nguyên bao hàm 1 hay nhiều container cùng dùng chung 1 địa chỉ IP và cùng chung 1 cơ chế deploy.
Khi có thay đổi liên quan tới pod thì nó chỉ kill pod cũ đi và tạo cái mới.

- Để xem các pods đang có

```bash
kubectl get pods
```

- Để xem thông tin chi tiết về 1 pod

```
kubectl describe pod dat-pod-name
```

### ReplicaSet

< Cái này để viết sau đi ... >

### Namespace

Namespace là tính năng giúp cho mình tạo những nhóm các object ([[#Pod]], Container, ...) của k8s vào các nhóm mà chúng có thể hoạt động trong môi trường tách biệt nhau, gần giống như việc object ở trong các cluster ảo được tạo ra trong 1 cluster thật vậy.

- Liệt kê namespace đang có trong cluster: `kubectl get namespaces`
- Liệt kê những tài nguyên trong namespace, ví dụ như pod:
  `kubectl get pods --namespace my-namespace`
  có thể viết tắt đi bằng `-n`
- Tạo namespace: `kubectl create namespace my-namespace`

### Service

Là 1 tài nguyên cung cấp network endpoint cho một hay nhiều [[#Pod]]. Các pods sẽ nhận được traffic được config qua selector, tức là tên của các Pod đó.

![](/assets/k8s-4.png)

#### Các loại Service

- **ClusterIP:** Loại mặc định, giới hạn kết nối của service trong các cluster đã được define sẵn.
- **NodePort:** Sẽ tạo ra 1 dải port mở cố định trên node mà service được deploy. Có thể truy cập từ bên ngoài cluster qua `NodeIP:nodePort`.
- **LoadBalancer:** Service này sẽ dùng load balancer ngoài, cụ thể là LB của cloud.
- **ExternalName:** Loại service này thay vì sử dụng selector để trỏ tới một service ngoài như trên thì nó dùng DNS để trỏ tới một service cụ thể khác với các type trên thay vì thông qua proxy hay forwarding thì type này điều hướng thông qua DNS.

#### Cách tạo Service

abcxyz

### Ingress

Là cách kiểm soát mọi băng thông thông qua một thành phần của k8s, được lựa chọn là phương án để expose dịch vụ ở môi trường production. Thay vì phải dùng tới Load Balancer bên ngoài thì Ingress đã có sẵn ngay trong cluster (chạy dưới dạng các pod).

### ConfigMap

abcxyz

### Secret

abcxyz

## Kiến trúc network

abcxyz

## Kubectl cơ bản

[Kubectl Dictionary](https://kubernetes.io/docs/reference/kubectl/cheatsheet) (dùng Cmd + F để tìm lệnh cần dùng)

## Tip & Tricks

1. Tìm tên các image đang được sử dụng

```sh
kubectl get pods --all-namespaces -o jsonpath="{.items[*].spec['initContainers', 'containers'][*].image}" |\
tr -s '[[:space:]]' '\n' |\
sort |\
uniq -c |\
grep "<image-name-here>"
```
