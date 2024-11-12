---
title: "Xem tài nguyên trên EKS console"
weight: 3
chapter: false
pre: "<b> 3. </b>"
---

{{% notice tip %}}
Chuẩn bị môi trường thực hành:
```bash
$ prepare-environment
```
{{% /notice %}}

Trong phòng thí nghiệm này, chúng ta sẽ xem tất cả các loại tài nguyên API Kubernetes bằng AWS Management Console cho Amazon EKS. Bạn sẽ có thể xem và khám phá tất cả các loại tài nguyên API Kubernetes chuẩn như tài nguyên cấu hình, tài nguyên ủy quyền, tài nguyên chính sách, tài nguyên dịch vụ, v.v. [Chế độ xem tài nguyên Kubernetes](https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html) được hỗ trợ cho tất cả các cụm Kubernetes do Amazon EKS lưu trữ. Bạn có thể sử dụng [Amazon EKS Connector](https://docs.aws.amazon.com/eks/latest/userguide/eks-connector.html) để đăng ký và kết nối bất kỳ cụm Kubernetes nào tuân thủ với AWS và trực quan hóa cụm đó trong bảng điều khiển Amazon EKS.

Chúng ta sẽ xem các tài nguyên do ứng dụng mẫu tạo ra. Lưu ý rằng bạn sẽ chỉ thấy các tài nguyên mà bạn có [quyền RBAC](https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html#view-kubernetes-resources-permissions) để truy cập, được tạo trong quá trình chuẩn bị môi trường.
