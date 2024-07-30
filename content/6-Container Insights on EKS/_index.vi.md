---
title: "Container Insights on EKS"
date: "`r Sys.Date()`"
weight: 6
chapter: false
pre: "<b> 6. </b>"
---

#### Container Insights on EKS

1. Chuẩn bị môi trường cho phần này:

``` bash
prepare-environment observability/container-insights

```

2. Lệnh này sẽ thực hiện các thay đổi sau vào môi trường thí nghiệm của bạn:

- Cài đặt tiện ích quản lý EKS cho AWS Distro for OpenTelemetry
- Tạo một vai trò IAM cho bộ sưu tập ADOT để truy cập CloudWatch

3. Trong bài lab này, chúng ta sẽ tìm hiểu cách bật và sử dụng CloudWatch Container Insights để thu thập, tổng hợp và tóm tắt các chỉ số và logs từ các ứng dụng và dịch vụ microservices được container hóa của bạn. Container Insights có sẵn cho Amazon Elastic Container Service (Amazon ECS), Amazon Elastic Kubernetes Service (Amazon EKS) và các nền tảng Kubernetes trên Amazon EC2. Hỗ trợ Amazon ECS bao gồm cả hỗ trợ cho Fargate.

4. Các chỉ số bao gồm việc sử dụng tài nguyên như CPU, bộ nhớ, ổ đĩa và mạng. Container Insights cũng cung cấp thông tin chẩn đoán, như lỗi khởi động lại container, để giúp bạn phân định vấn đề và giải quyết chúng một cách nhanh chóng. Bạn cũng có thể đặt cảnh báo CloudWatch trên các chỉ số mà Container Insights thu thập.

5. Các chỉ số mà Container Insights thu thập có sẵn trong các bảng điều khiển tự động của CloudWatch. Bạn có thể phân tích và gỡ lỗi dữ liệu hiệu suất và logs của container với CloudWatch Logs Insights.

6. Dữ liệu vận hành được thu thập dưới dạng sự kiện log hiệu suất. Đây là các mục nhập sử dụng một lược đồ JSON có cấu trúc cho phép dữ liệu với độ phức tạp cao được hấp thụ và lưu trữ tỉ lệ. Từ dữ liệu này, CloudWatch tạo ra các chỉ số tổng hợp ở cấp độ cluster, node, pod, task và service như là các chỉ số CloudWatch.

7. Chúng ta sẽ thiết lập Container Insights để thu thập các chỉ số từ cluster Amazon EKS bằng cách sử dụng bộ thu thập AWS Distro for OpenTelemetry.
