# Gaining environment information

Mỗi môi trường trong k8s cluster sẽ có rất nhiều thông tin cần thiết để giúp bạn đi sâu vào hệ thống hơn. Chẳng hạn như secret_key, api kkeys, configs, services và nhiều thứ khác. Kịch bản này sẽ giúp bạn có thêm kiến thức bổ sung khi recon một container.

# Solution

```cat /proc/self/cgroup
cat /etc/hosts
mount
ls -la /home/
```

```sh
printenv
```

# Kết luận

Quá trình recon này có hiệu quả hay không phụ thuốc vào kiến thức hiểu sâu về k8s của bạn như thế nào. Tuy nhiên, `printenv` thường sẽ có nhiều sessitive data dành cho bạn.