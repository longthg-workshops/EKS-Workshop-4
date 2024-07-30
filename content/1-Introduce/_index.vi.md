---
title: "Giới thiệu"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b> 1. </b>"
---

#### Giới thiệu

**Observability là một phần cơ bản của một môi trường EKS được thiết kế tốt**

**Observability** là một yếu tố cơ bản của một môi trường EKS được thiết kế tốt. **AWS** cung cấp các giải pháp Observability cơ bản (CloudWatch) và quản lý mã nguồn mở (Amazon Managed Service for Prometheus, Amazon Managed Grafana và AWS Distro for OpenTelemetry) cho việc theo dõi, ghi nhật ký, cảnh báo và tạo bảng điều khiển của các môi trường EKS.

Trong chương này, chúng ta sẽ tìm hiểu cách bạn có thể sử dụng các giải pháp Observability của AWS tích hợp với EKS để cung cấp khả năng nhìn thấy vào:

- **Các Tài nguyên Kubernetes** trong giao diện EKS console
- Ghi nhật ký **Control Plane và Pod** sử dụng Fluentbit
- Theo dõi **Metrics** với CloudWatch Container Insights
- Theo dõi **Metrics EKS** với AMP và ADOT.