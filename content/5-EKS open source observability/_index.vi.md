---
title: "EKS open source observability"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: "<b> 5. </b>"
---

#### EKS open source observability

Mở **CloudWatch Logs console** để kiểm tra các log này có xuất hiện:

### TRƯỚC KHI BẮT ĐẦU
Chuẩn bị môi trường cho phần này:

```
prepare-environment observability/oss-metrics
```


Điều này sẽ thực hiện các thay đổi sau vào môi trường lab của bạn:

- Cài đặt phần bổ sung quản lý EKS cho AWS Distro cho OpenTelemetry
- Tạo một vai trò IAM cho bộ thu thập ADOT để truy cập Amazon Managed Prometheus

Bạn có thể xem Terraform áp dụng các thay đổi này tại [đây](link).

Trong lab này, chúng ta sẽ thu thập các số liệu từ ứng dụng bằng cách sử dụng AWS Distro cho OpenTelemetry, lưu trữ các số liệu trong Amazon Managed Service for Prometheus và trực quan hóa bằng cách sử dụng Amazon Managed Grafana.

AWS Distro cho OpenTelemetry là một bản phân phối được hỗ trợ bởi AWS, an toàn và sẵn sàng cho sản xuất của dự án OpenTelemetry. Là một phần của Cloud Native Computing Foundation, OpenTelemetry cung cấp các API, thư viện và agent mã nguồn mở để thu thập các dấu vết phân tán và số liệu cho việc giám sát ứng dụng. Với AWS Distro cho OpenTelemetry, bạn chỉ cần instrument ứng dụng của mình một lần để gửi các số liệu và dấu vết tương quan đến nhiều giải pháp giám sát của AWS và đối tác. Sử dụng các agent tự động instrument để thu thập các dấu vết mà không cần thay đổi mã của bạn. AWS Distro cho OpenTelemetry cũng thu thập dữ liệu về siêu dữ liệu từ tài nguyên AWS và các dịch vụ quản lý, giúp bạn liên kết dữ liệu hiệu suất ứng dụng với dữ liệu cơ sở hạ tầng bên dưới, giảm thiểu thời gian trung bình để giải quyết vấn đề. Sử dụng AWS Distro cho OpenTelemetry để instrument các ứng dụng đang chạy trên Amazon Elastic Compute Cloud (EC2), Amazon Elastic Container Service (ECS), và Amazon Elastic Kubernetes Service (EKS) trên EC2, AWS Fargate, và AWS Lambda, cũng như on-premises.

Amazon Managed Service for Prometheus là một dịch vụ giám sát cho các số liệu tương thích với dự án mã nguồn mở Prometheus, giúp bạn dễ dàng giám sát môi trường container một cách an toàn hơn. Amazon Managed Service for Prometheus là một giải pháp để giám sát các container dựa trên dự án Prometheus của Cloud Native Computing Foundation (CNCF). Amazon Managed Service for Prometheus giảm bớt công việc nặng nề cần thiết để bắt đầu giám sát ứng dụng trên Amazon Elastic Kubernetes Service và Amazon Elastic Container Service, cũng như các cụm Kubernetes tự quản lý.
