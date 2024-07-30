---
title: "Các Bước Chuẩn Bị"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b> 2. </b>"
---

#### Các Bước Chuẩn Bị

Chuẩn bị môi trường cho phần này:

```bash timeout=300 wait=30
$ prepare-environment
```

Trong phần thực hành này, chúng ta sẽ xem tất cả các loại tài nguyên API Kubernetes bằng cách sử dụng Bảng điều khiển Quản lý AWS cho Amazon EKS. Bạn sẽ có thể xem và khám phá tất cả các loại tài nguyên API Kubernetes tiêu chuẩn như cấu hình, tài nguyên phân quyền, tài nguyên chính sách, tài nguyên dịch vụ và nhiều hơn nữa. [Xem tài nguyên Kubernetes](https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html) được hỗ trợ cho tất cả các cụm Kubernetes được lưu trữ bởi Amazon EKS. Bạn có thể sử dụng [Amazon EKS Connector](https://docs.aws.amazon.com/eks/latest/userguide/eks-connector.html) để đăng ký và kết nối bất kỳ cụm Kubernetes phù hợp nào vào AWS và hiển thị nó trong bảng điều khiển Amazon EKS.

Chúng ta sẽ xem các tài nguyên được tạo ra bởi ứng dụng mẫu. Lưu ý rằng bạn chỉ sẽ thấy các tài nguyên mà bạn có [quyền RBAC](https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html#view-kubernetes-resources-permissions) để truy cập, được tạo ra trong quá trình chuẩn bị môi trường.

![EKS](/images/0008/0001.jpg?featherlight=false&width=90pc)

