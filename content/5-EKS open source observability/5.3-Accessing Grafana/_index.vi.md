---
title: "Truy cập Grafana"
weight: 3
chapter: false
pre: "<b> 5.3 </b>"
---


1. Một instance của Grafana đã được cài đặt sẵn trong cụm EKS của bạn. Để truy cập nó, bạn cần trước tiên lấy URL:


```bash
kubectl get ingress -n grafana grafana -o=jsonpath='{.status.loadBalancer.ingress[0].hostname}'
```


2. Mở URL này trong trình duyệt sẽ hiển thị màn hình đăng nhập.

![Grafana dashboard](/EKS-Workshop-4/images/0007/0003.png?featherlight=false&width=60pc)


3. Để lấy thông tin xác thực cho người dùng, truy vấn bí mật được tạo bởi biểu đồ helm Grafana:

```bash
kubectl get -n grafana secrets/grafana -o=jsonpath='{.data.admin-user}' | base64 -d
kubectl get -n grafana secrets/grafana -o=jsonpath='{.data.admin-password}' | base64 -d
```

4. Sau khi đăng nhập vào bảng điều khiển Grafana, hãy xem phần nguồn dữ liệu. Bạn sẽ thấy không gian dữ liệu đã được cấu hình làm nguồn dữ liệu cho Amazon Managed Service for Prometheus.

![Amazon Managed Service for Prometheus](/EKS-Workshop-4/images/0007/0004.png?featherlight=false&width=90pc)