---
title: "Cluster"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b> 3.2 </b>"
---


#### Cluster

Để xem tài nguyên của cụm Kubernetes, nhấp vào tab Resources. Tiếp tục vào phần Cluster, bạn có thể xem nhiều loại tài nguyên API Kubernetes nếu chúng là một phần của cụm. Chi tiết xem cụm hiển thị tất cả các thành phần của kiến trúc cụm như Nodes, Namespaces và API Services chạy các công việc.

#### Nodes

Kubernetes chạy công việc của bạn bằng cách đặt các container vào các pod để chạy trên các Node. Một node có thể là máy ảo hoặc máy vật lý, tùy thuộc vào cụm. Cụm eks-workshop đang chạy 3 node với các công việc được triển khai trên đó. Nhấp vào Nodes để liệt kê các node.

Nếu bạn nhấp vào bất kỳ tên node nào, bạn sẽ tìm thấy phần Thông tin có rất nhiều chi tiết về node - hệ điều hành, container runtime, loại instance, EC2 instance và Managed node group (giúp việc cung cấp khả năng tính toán cho cụm trở nên dễ dàng). Phần tiếp theo, Phân bổ dung lượng, hiển thị việc sử dụng và đặt hàng của các tài nguyên khác nhau trên các node worker EC2 được kết nối vào cụm.

Phần tiếp theo, Pods, mô tả tất cả các pod được triển khai trên node. Trong ví dụ này, có 12 pod đang chạy trên node này.

Phần tiếp theo mô tả bất kỳ Taints, nhãn và chú thích nào có liên quan.


![EKS workshop](/EKS-Workshop-4/images/0005/00014.png?featherlight=false&width=90pc)


![EKS workshop](/EKS-Workshop-4/images/0005/00015.png?featherlight=false&width=90pc)


![EKS workshop](/EKS-Workshop-4/images/0005/00016.png?featherlight=false&width=90pc)



![EKS workshop](/EKS-Workshop-4/images/0005/00017.png?featherlight=false&width=90pc)


#### Namespaces

Namespaces là một cơ chế để tổ chức các cụm (clusters), có thể rất hữu ích khi các nhóm hoặc dự án khác nhau chia sẻ một cụm Kubernetes. Trong ứng dụng mẫu của chúng ta, chúng ta có các dịch vụ nhỏ - carts, checkout, catalog, assets mà tất cả đều chia sẻ cùng một cụm sử dụng cấu trúc namespace.

![EKS workshop](/EKS-Workshop-4/images/0005/0018.png?featherlight=false&width=90pc)

