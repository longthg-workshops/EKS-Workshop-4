---
title: "Xem các khối công việc (Workloads)"
weight: 1
chapter: false
pre: "<b> 3.1 </b>"
---

Để xem các tài nguyên Kubernetes, nhấp vào tab Resources. Tiến sâu vào phần Workload và bạn có thể xem một số loại tài nguyên API Kubernetes nằm trong phần công việc. Công việc bao gồm các container đang chạy trong cụm của bạn, và bao gồm Pods, ReplicaSets, Deployments, và DaemonSets. Đây là các khối cơ bản cho việc chạy các container trong một cụm.

### Workloads

#### **Pods**

Pods resource view hiển thị tất cả các pods đại diện cho đối tượng Kubernetes nhỏ nhất và đơn giản nhất.

1. Mặc định, tất cả các loại tài nguyên API Kubernetes được hiển thị, nhưng bạn có thể lọc theo namespace hoặc tìm kiếm các giá trị cụ thể để nhanh chóng tìm thấy điều bạn đang tìm kiếm. Dưới đây, bạn sẽ thấy các pods được lọc theo `namespace=catalog`.

![EKS workshop](/EKS-Workshop-4/images/0005/0002.png?featherlight=false&width=90pc)

2. Xem tài nguyên cho tất cả các loại tài nguyên API Kubernetes, cung cấp hai chế độ xem - cấu trúc và raw. Chế độ xem cấu trúc cung cấp một biểu diễn hình ảnh của tài nguyên để giúp truy cập dữ liệu cho tài nguyên. Trong ví dụ này (dưới đây), bạn có thể thấy một chế độ xem cấu trúc cho pod catalog phân chia thông tin pod thành các phần Info, Containers, Labels và Annotations. Nó cũng mô tả replicaset, namespace và node liên quan.

![EKS workshop](/EKS-Workshop-4/images/0005/0003.png?featherlight=false&width=90pc)

3. Chế độ xem raw hiển thị đầu ra JSON đầy đủ từ API Kubernetes, có ích để hiểu cấu hình và trạng thái của các loại tài nguyên không có sự hỗ trợ xem cấu trúc trong bảng điều khiển Amazon EKS. Trong ví dụ xem raw, chúng tôi hiển thị chế độ xem raw cho pod catalog.

![EKS workshop](/EKS-Workshop-4/images/0005/0004.png?featherlight=false&width=90pc)

#### **ReplicaSets**

Một ReplicaSet là một đối tượng Kubernetes đảm bảo một tập hợp ổn định các replica pods luôn chạy. Do đó, nó thường được sử dụng để đảm bảo sẵn có một số lượng xác định các pods giống nhau. Trong ví dụ này (dưới đây), bạn có thể thấy 2 replicasets cho namespace orders. ReplicaSet cho orders-d6b4566fc xác định cấu hình cho số lượng pods mong muốn và hiện tại.

Nhấp vào replicaset orders-d6b4566fc và khám phá cấu hình. Bạn sẽ thấy các cấu hình dưới Info, Pods, nhãn và chi tiết của replica tối đa và mong muốn.

![EKS workshop](/EKS-Workshop-4/images/0005/0005.png?featherlight=false&width=90pc)

![EKS workshop](/EKS-Workshop-4/images/0005/0006.png?featherlight=false&width=90pc)


![EKS workshop](/EKS-Workshop-4/images/0005/0007.png?featherlight=false&width=90pc)


![EKS workshop](/EKS-Workshop-4/images/0005/0008.png?featherlight=false&width=90pc)

#### **DaemonSet**


**DaemonSet** đảm bảo rằng tất cả (hoặc một số) Nodes chạy một bản sao của một pod. Trong ứng dụng mẫu, chúng ta có DaemonSet chạy trên mỗi node như được hiển thị (dưới đây).

Nhấp vào daemonset kube-proxy và khám phá cấu hình. Bạn sẽ thấy các cấu hình dưới phần Thông tin, các pod đang chạy trên mỗi node, nhãn và chú thích.


![EKS workshop](/EKS-Workshop-4/images/0005/0009.png?featherlight=false&width=90pc)


![EKS workshop](/EKS-Workshop-4/images/0005/00010.png?featherlight=false&width=90pc)


#### **Deployments**

Deployment là một đối tượng Kubernetes cho phép cập nhật khai báo cho các pods và replicaSets. Nó cho Kubernetes biết cách tạo ra hoặc sửa đổi các phiên bản của pods. Các Deployment giúp tự động mở rộng số lượng replica pods và cho phép phát hành hoặc quay lùi phiên bản triển khai một cách có kiểm soát. Trong ví dụ này (dưới đây), bạn có thể thấy 2 deployments cho namespace carts.

Nhấp vào deployment orders và khám phá cấu hình. Bạn sẽ thấy chiến lược triển khai dưới mục Info, chi tiết pod dưới mục Pods, các nhãn và phiên bản triển khai.


![EKS workshop](/EKS-Workshop-4/images/0005/00011.png?featherlight=false&width=90pc)


![EKS workshop](/EKS-Workshop-4/images/0005/00012.png?featherlight=false&width=90pc)


![EKS workshop](/EKS-Workshop-4/images/0005/00013.png?featherlight=false&width=90pc)

    