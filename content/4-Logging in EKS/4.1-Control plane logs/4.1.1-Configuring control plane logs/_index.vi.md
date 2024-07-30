---
title: "Control plane logs"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b> 4.1.1 </b>"
---

#### Configuring control plane logs


Để kích hoạt các nhật ký điều khiển EKS được thực hiện trên mỗi cụm thông qua API EKS. Thông thường, điều này sẽ được cấu hình bằng Terraform hoặc CloudFormation, nhưng trong lab này chúng ta sẽ sử dụng AWS CLI để kích hoạt chức năng này:

```

aws eks update-cluster-config \
    --region $AWS_REGION \
    --name $EKS_CLUSTER_NAME \
    --logging '{"clusterLogging":[{"types":["api","audit","authenticator","controllerManager","scheduler"],"enabled":true}]}'
sleep 30
aws eks wait cluster-active --name $EKS_CLUSTER_NAME

```

Như bạn có thể thấy, chúng ta có thể kích hoạt từng loại log của cluster một cách riêng biệt, và trong bài lab này, chúng ta đang kích hoạt tất cả mọi thứ.

Hãy xem cấu hình này trên bảng điều khiển EKS:

[https://console.aws.amazon.com/eks/home#/clusters/eks-workshop?selectedTab=cluster-logging-tab](https://console.aws.amazon.com/eks/home#/clusters/eks-workshop?selectedTab=cluster-logging-tab)

Tab Logging hiển thị cấu hình hiện tại cho log điều khiển cho cluster:


Bạn có thể điều chỉnh cấu hình ghi nhật ký bằng cách nhấp vào nút Quản lý: Manage