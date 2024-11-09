---
title: "Sử dụng Fluent Bit"
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

```bash
kubectl get all -n aws-for-fluent-bit
```
Đầu ra:
```
NAME                           READY   STATUS    RESTARTS   AGE
pod/aws-for-fluent-bit-vfsbe   1/1     Running   0          99m
pod/aws-for-fluent-bit-kmvnk   1/1     Running   0          99m
pod/aws-for-fluent-bit-rxhs7   1/1     Running   0          100m

NAME                                DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
daemonset.apps/aws-for-fluent-bit   2         2         2       2            2           <none>          104m
```

![FluentBit](../../../../images/0006/00012.png?featherlight=false&width=90pc)

ConfigMap cho aws-for-fluent-bit được cấu hình để stream nội dung của các tệp trong thư mục /var/log/containers/*.log từ mỗi node đến nhóm log CloudWatch /eks-workshop/worker-fluentbit-logs:

```bash
kubectl describe configmaps -n aws-for-fluent-bit
```
Đầu ra:
```
Name:         aws-for-fluent-bit
Namespace:    aws-for-fluent-bit
Labels:       app.kubernetes.io/instance=aws-for-fluent-bit
              app.kubernetes.io/managed-by=Helm
              app.kubernetes.io/name=aws-for-fluent-bit
              app.kubernetes.io/version=2.21.5
              helm.sh/chart=aws-for-fluent-bit-0.1.18
Annotations:  meta.helm.sh/release-name: aws-for-fluent-bit
              meta.helm.sh/release-namespace: aws-for-fluent-bit

Data
====
fluent-bit.conf:
----
[SERVICE]
    Parsers_File /fluent-bit/parsers/parsers.conf

[INPUT]
    Name              tail
    Tag               kube.*
    Path              /var/log/containers/*.log
    DB                /var/log/flb_kube.db
    Parser            docker
    Docker_Mode       On
    Mem_Buf_Limit     5MB
    Skip_Long_Lines   On
    Refresh_Interval  10

[FILTER]
    Name                kubernetes
    Match               kube.*
    Kube_URL            https://kubernetes.default.svc.cluster.local:443
    Merge_Log           On
    Merge_Log_Key       data
    Keep_Log            On
    K8S-Logging.Parser  On
    K8S-Logging.Exclude On
[OUTPUT]
    Name                  cloudwatch
    Match                 *
    region                us-east-1
    log_group_name        /eks-workshop/worker-fluentbit-logs
    log_stream_prefix     fluentbit-
    auto_create_group     true

...........
```

![FluentBitConfig](../../../../images/0006/00013.png?featherlight=false&width=90pc)
