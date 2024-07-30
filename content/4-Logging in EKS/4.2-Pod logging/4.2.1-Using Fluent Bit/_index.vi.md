---
title: "Using Fluent Bit"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b> 4.2.1 </b>"
---

#### Sử dụng Fluent Bit
Đối với các thành phần của cụm Kubernetes chạy trong các pod, chúng ghi vào các tệp trong thư mục /var/log, bỏ qua cơ chế ghi nhật ký mặc định. Chúng ta có thể triển khai ghi nhật ký ở mức pod bằng cách triển khai một đại diện ghi nhật ký ở mức nút dưới dạng DaemonSet trên mỗi nút, chẳng hạn như Fluent Bit.

Fluent Bit là một trình xử lý và chuyển tiếp nhật ký nhỏ gọn cho phép bạn thu thập dữ liệu và nhật ký từ các nguồn khác nhau, làm phong phú chúng bằng bộ lọc và gửi chúng đến nhiều địa điểm như CloudWatch, Kinesis Data Firehose, Kinesis Data Streams và Dịch vụ Amazon OpenSearch.

AWS cung cấp một ảnh Fluent Bit với các plugin cho cả CloudWatch Logs và Kinesis Data Firehose. Ảnh AWS cho Fluent Bit có sẵn trên Amazon ECR Public Gallery.

Trong phần tiếp theo, bạn sẽ thấy cách xác nhận rằng đại diện Fluent Bit đang chạy như một DaemonSet để gửi nhật ký của các container/pod đến CloudWatch Logs.

Trước tiên, chúng ta có thể xác nhận các tài nguyên được tạo cho Fluent Bit bằng cách nhập lệnh sau. Mỗi nút nên có một pod:

```

kubectl get all -n aws-for-fluent-bit

```

ConfigMap cho aws-for-fluent-bit được cấu hình để stream nội dung của các tệp trong thư mục /var/log/containers/*.log từ mỗi node đến nhóm log CloudWatch /eks-workshop/worker-fluentbit-logs:

```

kubectl describe configmaps -n aws-for-fluent-bit

```