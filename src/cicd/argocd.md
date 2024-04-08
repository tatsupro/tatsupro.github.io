# ArgoCD

Argo**CD** là [GitOps](gitops.md) tool để automate deploy (nhìn vào chữ **CD** bên cạnh tên nó đi bro) và sync changes ở Kubernetes với những gì được define ở Git. Người ta thường dùng nó để phòng trường hợp điển hình như khi có ông Ops bất kì thay đổi resource của app trên k8s mà khác so với những gì ghi trong file manifest được lưu cùng source code.

![The ArgoCD Flow](images/argocd-sync-flow.png)

Vì dựa trên ý tưởng của Git nên ngoài việc đảm bảo Infra có state đã được define thì Argo kế thừa những tinh hoa của Git dành cho Infra như tạo, khôi phục, revert change của config hoặc resource trong 1 state nhất định (như save game vậy).
