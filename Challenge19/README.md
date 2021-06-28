# Popeye - A Kubernetes Cluster Sanitizer

Kịch bản này chủ yếu là thực hiện việc scan k8s Cluster bằng cách scan k8s cluster trực tiếp và báo cáo các vấn đề tiềm ẩn với các tài nguyên và cấu hình đã triển khai.

Dể bắt đầu với kịch bản này, bạn có thể chạy lệnh sau để khởi động hacker-container với các đặc quyền administrator cluster (chẳng hạn như tiller)

```sh
kubectl run -n kube-system --serviceaccount=tiller --rm --restart=Never -it --image=madhuakula/hacker-container -- bash
```

# Solution

`Popeye` là một công cụ scan k8s cluster trực tiếp và báo cáo các vấn đề tiềm ẩn với các tài nguyên và cấu hình đã triển khai. Nó sẽ sanitize cluster của bạn dựa trên những gì đã được triển khai chứ không phải những gì nằm trên đĩa. Bằng cách scanning cluster, nó phát hiện ra các cấu hình sai và giúp bạn đảm bảo best practices áp dụng ở đó. Do đó sẽ giúp bạn ngăn ngừa các hiểm họa trong tương lai.

Dưới đây là danh sách một số saninizer có sẵn:

- Node
- Namespace
- Pod
- Service
- ServiceAccount
- Secrets
- ConfigMap
- Deployment
- StatefulSet
- DaemonSet
- PersistentVolume
- PersistentVolumeClaim
- HorizontalPodAutoscaler
- PodDisruptionBudget
- ClusterRole
- ClusterRoleBinding
- Role
- RoleBinding
- Ingress
- NetworkPolicy
- PodSecurityPolicy

Tham khảo thêm  https://github.com/derailed/popeye  để hiểu rõ về project

```sh
popeye
```

![popeye](images1.PNG)

