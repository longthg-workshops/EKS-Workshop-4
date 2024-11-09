---
title: "Observability"
weight: 1
chapter: false
---

# Observability (khả năng quan sát) trên EKS

**Observability** (khả năng quan sát và theo dõi)  là một yếu tố cơ bản của một môi trường EKS được thiết kế tốt. **AWS** cung cấp các giải pháp theo dõi cơ bản (CloudWatch) và quản lý mã nguồn mở (Amazon Managed Service for Prometheus, Amazon Managed Grafana và AWS Distro for OpenTelemetry) cho việc theo dõi, ghi log, cảnh báo và tạo bảng điều khiển của các môi trường EKS.

Trong chương này, chúng ta sẽ tìm hiểu cách bạn có thể sử dụng các giải pháp theo dõi của AWS tích hợp với EKS để cung cấp khả năng nhìn thấy vào:

- **Các Tài nguyên Kubernetes** trong giao diện EKS console
- Ghi log **Control Plane và Pod** sử dụng Fluentbit
- Theo dõi **Metrics** với CloudWatch Container Insights
- Theo dõi **Metrics EKS** với AMP và ADOT.

{{% notice info %}}
Tìm hiểu một bộ các module Cơ sở hạ tầng dưới dạng Mã _(Infrastructure as Code - **IaC**)_ kèm ý kiến đánh giá nhằm thiết lập khả năng quan sát và theo dõi cho môi trường AWS của mình trong [AWS Observability Accelerator for CDK](https://aws-observability.github.io/cdk-aws-observability-accelerator/) và [AWS Observability Accelerator for Terraform](https://aws-observability.github.io/terraform-aws-observability-accelerator/). Các module này hoạt động với các dịch vụ thuộc AWS (Amazon CloudWatch) và các dịch vụ quan sát - theo dõi được AWS quản lý (Amazon Managed Service for Prometheus, Amazon Managed Grafana và AWS Distro for OpenTelemetry (ADOT)).
{{% /notice %}}

### Kiến trúc hệ thống quan sát thuộc AWS
![AWS Native Observability](../images/home/cloud-native-obs-architecture.webp)

### Kiến trúc hệ thống quan sát sử dụng phần mềm mã nguôn mở (OSS - Do AWS quản lý):
![Open Source Managed Observability ](../images/home/oss-architecture.webp)