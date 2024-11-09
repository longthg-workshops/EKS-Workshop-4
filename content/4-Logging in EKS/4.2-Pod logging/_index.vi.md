---
title: "Ghi log từ pod"
weight: 2
chapter: false
pre: "<b> 4.2 </b>"
---

### Trước khi bắt đầu

Chuẩn bị môi trường cho phần này:
```bash
prepare-environment observability/logging/pods
```
Lệnh này sẽ thực hiện các thay đổi sau vào môi trường lab của bạn:

- **_Cài đặt AWS cho Fluent Bit trong cụm Amazon EKS._**

![Prepare](../../../../../images/0006/00011.png?featherlight=false&width=90pc)

Bạn có thể xem Terraform áp dụng các thay đổi này [ở đây](https://github.com/aws-samples/eks-workshop-v2/tree/stable/manifests/modules/observability/logging/pods/.workshop/terraform).

---

### Giới thiệu về lab

Theo nguyên tắc Twelve-Factor App manifesto, cung cấp tiêu chuẩn vàng cho việc thiết kế các ứng dụng hiện đại, các ứng dụng container nên đầu ra log của chúng ra stdout và stderr. Điều này cũng được coi là thực hành tốt nhất trong Kubernetes và các hệ thống thu thập log cấp cụm được xây dựng dựa trên tiền đề này.

Kiến trúc ghi log của Kubernetes xác định ba cấp độ phân biệt:

- Ghi log cấp cơ bản: khả năng lấy log của các pod bằng cách sử dụng kubectl (ví dụ: kubectl logs myapp - trong đó myapp là một pod đang chạy trong cụm của tôi)
- Ghi log cấp node: Container engine ghi lại log từ stdout và stderr của ứng dụng, và ghi chúng vào một tập tin log.
- Ghi log cấp cụm: Xây dựng trên cơ sở ghi log cấp node; một đại diện ghi log chạy trên mỗi node. Đại diện này thu thập log trên hệ thống tệp cục bộ và gửi chúng đến một đích ghi log tập trung như Elasticsearch hoặc CloudWatch. Đại diện thu thập hai loại log:
  - Log của container được ghi lại bởi container engine trên node.
  - Log hệ thống.

Kubernetes, mặc định, không cung cấp một giải pháp tự nhiên để thu thập và lưu trữ log. Nó cấu hình runtime của container để lưu log dưới định dạng JSON trên hệ thống tệp cục bộ. Runtime của container - như Docker - chuyển hướng luồng stdout và stderr của container sang một trình điều khiển ghi log. Trong Kubernetes, log của container được ghi vào /var/log/pods/*.log trên node. Kubelet và runtime của container ghi log của chúng vào /var/logs hoặc vào journald, trong các hệ điều hành có systemd. Sau đó, các hệ thống thu thập log trên toàn cụm như Fluentd có thể theo dõi các tệp log này trên node và gửi log để lưu trữ. Các hệ thống thu thập log này thường chạy như DaemonSets trên các node worker.

Trong lab này, chúng tôi sẽ chỉ ra cách một đại diện ghi log có thể được thiết lập để thu thập log từ các node trong EKS và gửi chúng đến CloudWatch Logs.

{{% notice info %}}
Nếu bạn đang dùng CDK Observability Accelerator, hãy xem qua [AWS for Fluent Bit Addon](https://aws-quickstart.github.io/cdk-eks-blueprints/addons/aws-for-fluent-bit/). **_AWS for FluentBit Addon_** có thể được cấu hình để chuyển tiếp log tới nhiều nơi khác nhau, bao gồm CloudWatch, Amazon Kinesis, và AWS OpenSearch.
{{% /notice %}}
