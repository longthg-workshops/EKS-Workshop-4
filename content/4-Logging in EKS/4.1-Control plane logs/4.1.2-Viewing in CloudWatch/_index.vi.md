---
title: "Xem trong CloudWatch"
weight: 2
chapter: false
pre: "<b> 4.1.2 </b>"
---

Hãy xem các logs trong bảng điều khiển [CloudWatch Logs](https://console.aws.amazon.com/cloudwatch/home?#logsV2:log-groups).

Lọc theo tiền tố /aws/eks và chọn cluster mà bạn muốn kiểm tra logs (cụ thể là `/aws/eks/eks-workshop/cluster`):

![EKS workshop](/images/0006/0006.png?featherlight=false&width=90pc)

Bạn sẽ thấy một số luồng log trong nhóm:

![EKS workshop](/images/0006/0007.png?featherlight=false&width=90pc)

Chọn bất kỳ luồng log nào để xem các mục được gửi đến CloudWatch Logs bởi EKS control plane.