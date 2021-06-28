# Secure network boundaries using NSP

(NSP - Network Service Provider)
Kịch bản này xây dựng một network security policy đơn giản cho tài nguyên K8s để tạo các `ranh giới ` network.

Để bắt đầu với kịch bản này, hãy đảm bảo rằng bạn phải sử dụng giải pháp mạng hỗ trợ `NetworkPolicy`

# Solution

- Kịch bản lấy từ https://github.com/ahmetb/kubernetes-network-policy-recipes


Nếu bạn muốn kiểm soát được luồng traffic ở IP hoặc port level ( OSI layer 3 or 4), thì bạn có thể cân nhắc sử dụng K8s NetworkPolicies cho các ứng dụng cụ thể trong cluster của bạn. NetworkPolicies là một application-centic contruct cho phép bạn chỉ định một pod được phép giao tiếp với các `entities` mạng khác qua network.

Các entities mà một Pod cso thể giao tiếp được xác định thông qua sự kết hợp của 3 `indentifier` (điểm nhận dạng) sau:

- Các pod khác được phép giao tiếp khi Namespaces cho phép ( ngoại trừ một pod không thể chặn quyền truy cập )
- IP block
- Khi xác định một pod hoặc namespace dựa trên networkPolicy, bạn sẻ dụng một selector để chỉ định lưu lượng nào được phép đến và đi Pods đó phù hợp vs selector đã chọn.


Trong khi đó, khi mà IP dựa trên NetworkPoloicies được tạo, chúng tôi xác định các chính sách dựa trên các IP block (CIDR range)

- Kịch bạn sẽ tạo DENY tất cả lưu lượng truy cập vào một ứng dụng

`NetworkPolicy sẽ xóa tất cả các traffic tới pods của một ứng dụng, được chọn bằng Pod Selectors`

Use Cases:

- Nó rất phổ biến: Để bắt đầu làm sạch dữ liệu mạng với Network Policies, đầu tiên bạn cần blasklist trafic bằng chính policy này.

- Bạn muốn chạy một Pod và muốn ngăn bắt kỳ Pod nào giao tiếp với nó

- Bạn tạm thời muốn cô lập traffic đến một Service từ các Pods khác.

![appweb](image1.gif)

Ví dụ:

- Chạy một Pod nginx với labels `app=web` và expose nó dưới port `80`

```sh
kubectl run --generator=run-pod/v1 web --image=nginx --labels app=web --expose --port 80
```

- Chạy một temporary Pod và tạo một request tới web Service

```sh
kubectl run --generator=run-pod/v1 --rm -i -t --image=alpine test-$RANDOM -- sh
```

```
# You will see the below output
# wget -qO- http://web
#    <!DOCTYPE html>
#    <html>
#    <head>
#    ...
```

Nó đã hoạt đọng, bây giờ hãy lưu manifest `web-deny-all.yaml` và apply nó vào cluster

```sh
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: web-deny-all
spec:
  podSelector:
    matchLabels:
      app: web
  ingress: []
```

```
kubectl apply -f web-deny-all.yaml
```

Thử lại kết nối:

```sh
kubectl apply -f web-deny-all.yaml
```

Traffic không thể tới được web=app.

## Nhận xét

- Trong file manifest ở trên, chúng ta đã tạo network policy cho Pods với app=web. File manifest này thiếu trường spec.ingress. Do đó, nó không cho phép bất kỳ lương lượng truy cập nào vào Pod.

- Nếu bạn tạo một NetworkPolicy khác chung cấp cho một số Pods quyền truy cập trực tiếp hoặc gián tiếp vào ứng dụng này, Network Policy này sẽ bị bỏ qua.

- Nếu có ít nhất một NetworkPolicy với một rule cho phép lưu lượng traffic, điều đó có nghĩa là trafic sẽ được chuyển đến Pods bất kể các chính sách chặn traffic.

## Cleanup

```sh
kubectl delete pod web
kubectl delete service web
kubectl delete networkpolicy web-deny-all
```


# Cilium Editor - Network Policy Editor

MỘt công cụ/ framework dạy bạn làm thế nào để tạo ra một network policy bằng Editor. Nó giải thích các khái niệm chính sách mạng cơ bản và hướng dẫn bạn các bước cần thiết để đạt được các khái niệm về bảo mật và độ tin cậy thấp nhất mong muốn.

![image](image2.PNG)