---
title: "Kiểm tra log trên CloudWatch"
weight: 2
chapter: false
pre: "<b> 4.2.2 </b>"
---

Trong bài thực hành này, chúng ta sẽ tìm hiểu cách kiểm tra các nhật ký pod Kubernetes được chuyển tiếp bởi Fluent Bit agent triển khai trên mỗi nút tới Amazon CloudWatch Logs. Các thành phần ứng dụng đã triển khai ghi lại nhật ký vào stdout, được lưu trữ trong đường dẫn /var/log/containers/*.log trên mỗi nút.

Đầu tiên, hãy tái sử dụng các pod cho thành phần ui để đảm bảo nhật ký mới được ghi từ khi chúng ta bật Fluent Bit:

```bash
kubectl delete pod -n ui --all
kubectl rollout status deployment/ui \
  -n ui --timeout 30s
```

Bây giờ chúng ta có thể kiểm tra rằng thành phần giao diện người dùng của chúng ta đang tạo ra các nhật ký bằng cách sử dụng trực tiếp kubectl logs:

```bash
kubectl logs -n ui deployment/ui 
```

Mở Console CloudWatch Logs để kiểm tra các log này xuất hiện:

[https://console.aws.amazon.com/cloudwatch/home?#logsV2:log-groups](https://console.aws.amazon.com/cloudwatch/home?#logsV2:log-groups)

![EKS workshop](/EKS-Workshop-4/images/0006/00016.png?featherlight=false&width=90pc)

Bộ lọc cho fluentbit-cloudwatch để tìm các nhóm log được tạo bởi Fluent Bit:

![EKS workshop](/EKS-Workshop-4/images/0006/00017.png?featherlight=false&width=90pc)

Chọn `/aws/eks/fluentbit-cloudwatch/workload/ui` để xem các luồng log, mỗi luồng tương ứng với một pod cá nhân:

![EKS workshop](/EKS-Workshop-4/images/0006/00018.png?featherlight=false&width=90pc)

Bạn có thể mở rộng một trong các mục log để xem toàn bộ dữ liệu JSON đầy đủ:

![EKS workshop](/EKS-Workshop-4/images/0006/00019.png?featherlight=false&width=90pc)
