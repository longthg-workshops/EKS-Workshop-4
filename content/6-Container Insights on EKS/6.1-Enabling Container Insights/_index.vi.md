---
title: "Container Insights on EKS"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b> 6.1 </b>"
---

#### Kích hoạt Container Insights sử dụng AWS Distro for OpenTelemetry

Trong bài thực hành này, chúng ta sẽ tìm hiểu cách kích hoạt các số liệu Container Insights của CloudWatch bằng cách sử dụng ADOT Collector (bộ thu thập ADOT) cho một cụm EKS.

1. Bây giờ, hãy tạo các tài nguyên để cấp quyền cho ADOT Collector theo đúng những gì nó cần. Chúng ta sẽ bắt đầu với ClusterRole, cung cấp quyền cho bộ thu thập để truy cập API của Kubernetes:

**~/environment/eks-workshop/modules/observability/container-insights/adot/clusterrole.yaml**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: otel-ci-role
rules:
  - apiGroups: [""]
    resources: ["pods", "nodes", "endpoints"]
    verbs: ["list", "watch", "get"]
  - apiGroups: ["apps"]
    resources: ["replicasets"]
    verbs: ["list", "watch", "get"]
  - apiGroups: ["batch"]
    resources: ["jobs"]
    verbs: ["list", "watch"]
  - apiGroups: [""]
    resources: ["nodes/proxy"]
    verbs: ["get"]
  - apiGroups: [""]
    resources: ["nodes/stats", "configmaps", "events"]
    verbs: ["create", "get"]
  - apiGroups: [""]
    resources: ["configmaps"]
    verbs: ["update"]
  - apiGroups: [""]
    resources: ["configmaps"]
    resourceNames: ["otel-container-insight-clusterleader"]
    verbs: ["get","update", "create"]
  - apiGroups: ["coordination.k8s.io"]
    resources: ["leases"]
    verbs: ["create","get", "update"]
  - apiGroups: ["coordination.k8s.io"]
    resources: ["leases"]
    resourceNames: ["otel-container-insight-clusterleader"]
    verbs: ["get","update", "create"]

```

2. Chúng ta sẽ sử dụng chính sách IAM quản lý CloudWatchAgentServerPolicy để cung cấp cho bộ thu thập các quyền IAM cần thiết thông qua IAM Roles for Service Accounts.

```

aws iam list-attached-role-policies \
  --role-name eks-workshop-adot-collector-ci | jq .

```

3. Vai trò IAM này sẽ được thêm vào ServiceAccount cho bộ thu thập:

**~/environment/eks-workshop/modules/observability/container-insights/adot/serviceaccount.yaml**

``` yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: adot-collector-ci
  annotations:
    eks.amazonaws.com/role-arn: ${ADOT_IAM_ROLE_CI}

```

4. Tạo tài nguyên:

```

kubectl kustomize ~/environment/eks-workshop/modules/observability/container-insights/adot \
  | envsubst | kubectl apply -f-
kubectl rollout status -n other daemonset/adot-container-ci-collector --timeout=120s

```


5. Thông số kỹ thuật cho bộ thu thập quá dài để hiển thị ở đây, nhưng bạn có thể xem nó như sau:

```

kubectl -n other get opentelemetrycollector adot-container-ci

```

6. Hãy chia nhỏ thành từng phần để hiểu rõ hơn về những gì đã được triển khai. Đây là cấu hình của bộ thu thập OpenTelemetry:

```

kubectl -n other get opentelemetrycollector adot-container-ci -o jsonpath='{.spec.config}'

```

7. Dưới đây là cấu hình một OpenTelemetry pipeline với cấu trúc sau:

**Receivers**

- Receiver Container Insights được thiết kế để thu thập sự kiện nhật ký hiệu suất bằng Embedded Metric Format.

**Processors**

- Gom nhóm các chỉ số vào các khoảng thời gian 60 giây.

**Exporters**

- CloudWatch EMF exporter gửi các chỉ số đến API CloudWatch.

Bộ thu thập này cũng được cấu hình để chạy như một DaemonSet với một đại lý thu thập chạy trên mỗi nút.


```

kubectl -n other get opentelemetrycollector adot-container-ci -o jsonpath='{.spec.mode}{"\n"}'

```

8. Chúng tôi có thể xác nhận điều đó bằng cách kiểm tra các Pods thu thập ADOT Container Insights metrics đang chạy:

``` bash

~$ kubectl get pods -n other

NAME                               READY   STATUS    RESTARTS   AGE
adot-container-ci-collector-5lp5g  1/1     Running   0          15s
adot-container-ci-collector-ctvgs  1/1     Running   0          15s

```


Nếu kết quả của lệnh này bao gồm nhiều pods ở trạng thái Running như hiển thị (phía trên), bộ thu thập đang chạy và thu thập các số liệu từ cụm. Bộ thu thập tạo một nhóm log có tên là aws/containerinsights/cluster-name/performance và gửi dữ liệu số liệu dưới dạng sự kiện log hiệu suất theo định dạng EMF.