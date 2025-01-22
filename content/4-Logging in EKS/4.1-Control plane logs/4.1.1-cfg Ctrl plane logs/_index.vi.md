---
title: "Cấu hình log cho Control plane"
weight: 1
chapter: false
pre: "<b> 4.1.1 </b>"
---

Chúng ta có thể kích hoạt từng loại log của cluster một cách riêng biệt. Tuy nhiên, trong bài lab này, chúng ta sẽ kích hoạt tất cả mọi thứ.

[Truy cập trang điều khiển EKS](https://console.aws.amazon.com/eks/home#/clusters)

Tab **Logging** cho thấy cấu hình hiện tại của việc ghi log tầng điều khiển cụm:

![EKS Console Logging Tab](/images/4/1/1/0004-eks-cluster-logging-tab.webp?featherlight=false&width=90pc)

Bạn có thể chỉnh sửa cấu hình log bằng cách nhấp chuột vào nút **Manage**:

![Enable Logging](/images/0006/0005.png?featherlight=false&width=90pc)

Việc kích hoạt các log tầng điều khiển EKS có thể được thực hiện trên mỗi cụm thông qua API EKS. Thông thường, điều này sẽ được cấu hình bằng Terraform hoặc CloudFormation, nhưng trong lab này chúng ta sẽ sử dụng AWS CLI để kích hoạt chức năng này:

```bash
aws eks update-cluster-config \
    --region $AWS_REGION \
    --name $EKS_CLUSTER_NAME \
    --logging '{"clusterLogging":[{"types":["api","audit","authenticator","controllerManager","scheduler"],"enabled":true}]}'
sleep 30
aws eks wait cluster-active --name $EKS_CLUSTER_NAME
```

![EKS workshop](/images/0006/0003.png?featherlight=false&width=90pc)

{{% notice info %}}
Nếu bạn đang dùng CDK Observability Accelerator, hãy xem qua [CDK Observability Builder](https://aws-quickstart.github.io/cdk-eks-blueprints/builders/observability-builder/#supported-methods). Công cụ này hỗ trợ việc kích hoạt toàn bộ tính năng ghi log tầng điều khiển (Control Plane) cho các cụm EKS và lưu vào CloudWatch.
{{% /notice %}}
