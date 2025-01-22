---
title: "Dịch vụ (Service) và Điểm kết nối (Endpoint)"
weight: 3
chapter: false
pre: "<b> 3.3 </b>"
---

1. **_Service Resource View_** hiển thị tất cả các dịch vụ đang mở kết nối đến ứng dụng đang chạy trên một tập hợp các pod trong một cluster.

![EKS workshop](/EKS-Workshop-4/images/0005/00019.png?featherlight=false&width=90pc)

2. Nếu bạn chọn service cart, phần hiển thị sẽ bao gồm thông tin về dịch vụ trong phần Info bao gồm selector (Tập hợp các pods mà dịch vụ đích đến thường được xác định bởi một selector), giao thức và cổng nó đang chạy và bất kỳ labels và annotations nào.

3. Pods expose chính nó thông qua các endpoints đến một dịch vụ. Một endpoint là một resource mà nhận một địa chỉ IP và cổng của các pods được gán động đến nó. Một endpoint được tham chiếu bởi một dịch vụ Kubernetes.


![EKS workshop](/EKS-Workshop-4/images/0005/00020.png?featherlight=false&width=90pc)

4. Đối với ứng dụng mẫu này, nhấp vào Endpoints và nó sẽ liệt kê tất cả các endpoints cho cluster của bạn.

5. Nhấp vào endpoint của catalog và khi bạn khám phá chi tiết bạn có thể thấy địa chỉ IP và cổng được liên kết với endpoint cùng với các phần Info, Labels và Annotations.

![EKS workshop](/EKS-Workshop-4/images/0005/00021.png?featherlight=false&width=90pc)
