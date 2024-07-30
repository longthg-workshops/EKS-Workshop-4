---
title: "Scraping metrics sử dụng AWS Distro cho OpenTelemetry"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b> 5.1 </b>"
---

#### Scraping metrics sử dụng AWS Distro cho OpenTelemetry

Trong thí nghiệm này, chúng ta sẽ lưu trữ các metric trong một không gian làm việc Amazon Managed Service for Prometheus đã được tạo sẵn cho bạn. Bạn có thể thấy nó trong bảng điều khiển:

[https://console.aws.amazon.com/prometheus/home#/workspaces](https://console.aws.amazon.com/prometheus/home#/workspaces)

Để xem không gian làm việc, nhấp vào tab All Workspaces trên bảng điều khiển bên trái. Chọn không gian làm việc bắt đầu bằng eks-workshop và bạn có thể xem một số tab khác nhau dưới không gian làm việc như quản lý luật, quản lý cảnh báo, v.v.

Để thu thập các metric từ Cụm Amazon EKS, chúng ta sẽ triển khai một tài nguyên tùy chỉnh OpenTelemetryCollector. Toán tử ADOT chạy trên cụm EKS phát hiện sự hiện diện hoặc thay đổi của tài nguyên này và cho bất kỳ thay đổi nào như vậy, toán tử thực hiện các hành động sau:

- Xác minh rằng tất cả các kết nối cần thiết cho các yêu cầu tạo, cập nhật hoặc xóa này đến máy chủ API Kubernetes có sẵn.
- Triển khai các phiên bản thu thập ADOT theo cách mà người dùng đã biểu diễn trong cấu hình tài nguyên OpenTelemetryCollector.

Bây giờ, hãy tạo tài nguyên để cho phép bộ thu ADOT các quyền mà nó cần. Chúng ta sẽ bắt đầu bằng ClusterRole mà cung cấp các quyền cho bộ thu truy cập vào API Kubernetes:

**~/environment/eks-workshop/modules/observability/oss-metrics/adot/clusterrole.yaml**

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: otel-prometheus-role
rules:
  - apiGroups:
      - ""
    resources:
      - nodes
      - nodes/proxy
      - services
      - endpoints
      - pods
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - extensions
    resources:
      - ingresses
    verbs:
      - get
      - list
      - watch
  - nonResourceURLs:
      - /metrics
    verbs:
      - get

```

Chúng ta sẽ sử dụng chính sách IAM quản lý AmazonPrometheusRemoteWriteAccess để cung cấp cho bộ thu thập quyền IAM mà nó cần thông qua IAM Roles for Service Accounts:


```

aws iam list-attached-role-policies \
  --role-name $EKS_CLUSTER_NAME-adot-collector | jq .

```

IAM role này sẽ được thêm vào ServiceAccount cho collector:

**~/environment/eks-workshop/modules/observability/oss-metrics/adot/serviceaccount.yaml**

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: adot-collector
  annotations:
    eks.amazonaws.com/role-arn: ${ADOT_IAM_ROLE}
```


Tạo các tài nguyên:

```

kubectl kustomize ~/environment/eks-workshop/modules/observability/oss-metrics/adot \
  | envsubst | kubectl apply -f-
kubectl rollout status -n other deployment/adot-collector --timeout=120s

```

Thông số kỹ thuật cho bộ thu là quá dài để hiển thị ở đây, nhưng bạn có thể xem nó như sau:

```

kubectl -n other get opentelemetrycollector adot -o yaml

```

Hãy phân tích thành các phần để hiểu rõ hơn về những gì đã được triển khai. Đây là cấu hình của bộ thu thập OpenTelemetry:

```

kubectl -n other get opentelemetrycollector adot -o jsonpath='{.spec.config}' | yq

```

Dưới đây là cấu hình một  OpenTelemetry pipeline với cấu trúc sau:

- **Receivers**

- **Prometheus receiver**: được thiết kế để thu thập số liệu từ các mục tiêu mà mở ra một điểm cuối Prometheus.

- **Processors**

Không có trong pipeline này.

- **Exporters**

  - Prometheus remote write exporter: gửi số liệu tới một điểm cuối ghi xa Prometheus như AMP.
  
Collector này cũng được cấu hình để chạy như một Deployment với một đại diện của bộ thu thập đang chạy.

```

kubectl -n other get opentelemetrycollector adot -o jsonpath='{.spec.mode}{"\n"}'

```


Chúng ta có thể xác nhận điều này bằng cách kiểm tra các Pods thu thập ADOT đang chạy:


```

kubectl get pods -n other

```

