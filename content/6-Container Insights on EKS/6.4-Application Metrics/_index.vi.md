---
title: "Các thông số ứng dụng trên Amazon CloudWatch"
weight: 4
chapter: false
pre: "<b> 6.4 </b>"
---

1. Trong phần này, chúng ta sẽ tìm hiểu cách nhận được thông tin về các chỉ số được tiết lộ bởi các công việc của chúng ta và hiển thị các chỉ số đó bằng cách sử dụng Amazon CloudWatch Insights Prometheus. Một số ví dụ về các chỉ số này có thể là:

- Các chỉ số hệ thống như chỉ số Java heap hoặc trạng thái của bể kết nối cơ sở dữ liệu
- Các chỉ số ứng dụng liên quan đến các chỉ số KPI kinh doanh

2. Hãy xem cách nhập các chỉ số ứng dụng bằng cách sử dụng AWS Distro cho OpenTelemetry và hiển thị các chỉ số bằng cách sử dụng Amazon CloudWatch. Mỗi thành phần trong bộ công cụ này đã được trang bị để cung cấp các chỉ số Prometheus bằng cách sử dụng các thư viện phù hợp với ngôn ngữ lập trình hoặc framework cụ thể. Chúng ta có thể xem một ví dụ về các chỉ số này từ dịch vụ đặt hàng như sau:

```bash
kubectl -n orders exec deployment/orders -- curl http://localhost:8080/actuator/prometheus
[...]
# HELP jdbc_connections_idle Number of established but idle connections.
# TYPE jdbc_connections_idle gauge
jdbc_connections_idle{name="reader",} 10.0
jdbc_connections_idle{name="writer",} 10.0
[...]
# HELP watch_orders_total The number of orders placed
# TYPE watch_orders_total counter
watch_orders_total{productId="510a0d7e-8e83-4193-b483-e27e09ddc34d",} 2.0
watch_orders_total{productId="808a2de1-1aaa-4c25-a9b9-6612e8f29a38",} 1.0
watch_orders_total{productId="*",} 3.0
watch_orders_total{productId="6d62d909-f957-430e-8689-b5129c0bb75e",} 1.0
```
Kết quả từ lệnh này là được hiển thị toàn bộ, cho mục đích của bài lab này, hãy tập trung vào chỉ số - watch_orders_total:

- **watch_orders_total**: Chỉ số ứng dụng - Số đơn hàng đã được đặt qua cửa hàng bán lẻ

3. Bạn có thể thực hiện các yêu cầu tương tự đối với các thành phần khác, ví dụ như dịch vụ thanh toán:

```bash
kubectl -n checkout exec deployment/checkout -- curl http://localhost:8080/metrics
[...]
# HELP nodejs_heap_size_total_bytes Process heap size from Node.js in bytes.
# TYPE nodejs_heap_size_total_bytes gauge
nodejs_heap_size_total_bytes 48668672
[...]
```

4. Bạn sẽ nhận ra bộ thu thập mà chúng ta đã triển khai là DaemonSet, nghĩa là nó chạy trên mọi nút. Điều này trái với mong muốn khi thu thập số liệu từ các Pod trong cụm vì chúng ta sẽ có số liệu trùng lặp. Bây giờ chúng ta sẽ triển khai một bộ thu thập thứ hai chạy dưới dạng Deployment với một bản sao duy nhất.

Đầu tiên, chúng ta phải thu thập các số liệu từ các pod ứng dụng. Cấu hình OpenTelemetry để thực hiện điều này được mô tả chi tiết dưới đây:

**~/environment/eks-workshop/modules/observability/container-insights/adot-deployment/opentelemetrycollector.yaml**

```yaml
apiVersion: opentelemetry.io/v1beta1
kind: OpenTelemetryCollector
metadata:
  name: adot-container-ci-deploy
  namespace: other
spec:
  image: public.ecr.aws/aws-observability/aws-otel-collector:v0.40.0
  mode: deployment
  serviceAccount: adot-collector-ci
  config:
    receivers:
      prometheus:
        config:
          global:
            scrape_interval: 60s
            scrape_timeout: 15s
            external_labels:
              cluster: ${EKS_CLUSTER_NAME}
              account_id: ${AWS_ACCOUNT_ID}
              region: ${AWS_REGION}
          scrape_configs:
            - job_name: "kubernetes-pods"
              honor_labels: true

              kubernetes_sd_configs:
                - role: pod
              relabel_configs:
                - source_labels:
                    [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
                  action: keep
                  regex: true
                - source_labels:
                    [__meta_kubernetes_pod_annotation_prometheus_io_scrape_slow]
                  action: drop
                  regex: true
                - source_labels:
                    [__meta_kubernetes_pod_annotation_prometheus_io_scheme]
                  action: replace
                  regex: (https?)
                  target_label: __scheme__
                - source_labels:
                    [__meta_kubernetes_pod_annotation_prometheus_io_path]
                  action: replace
                  target_label: __metrics_path__
                  regex: (.+)
                - action: labelmap
                  regex: __meta_kubernetes_pod_annotation_prometheus_io_param_(.+)
                  replacement: __param_$$1
                - action: labelmap
                  regex: __meta_kubernetes_pod_label_(.+)
                - source_labels: [__meta_kubernetes_namespace]
                  action: replace
                  target_label: namespace
                - source_labels: [__meta_kubernetes_pod_name]
                  action: replace
                  target_label: pod
                - source_labels: [__meta_kubernetes_pod_phase]
                  regex: Pending|Succeeded|Failed|Completed
                  action: drop

    processors:
      batch/metrics:
        timeout: 60s

    exporters:
      awsemf/prometheus:
        namespace: ContainerInsights/Prometheus
        log_group_name: "/aws/containerinsights/${EKS_CLUSTER_NAME}/prometheus"
        log_stream_name: "${K8S_POD_NAME}"
        region: ${AWS_REGION}
        resource_to_telemetry_conversion:
          enabled: true
        dimension_rollup_option: NoDimensionRollup
        metric_declarations:
          - dimensions: [[pod, productId]]
            metric_name_selectors:
              - "^watch_orders_total$"

    extensions:
      health_check: {}

    service:
      pipelines:
        metrics:
          receivers: [prometheus]
          processors: [batch/metrics]
          exporters: [awsemf/prometheus]
      extensions: [health_check]

  env:
    - name: K8S_NODE_NAME
      valueFrom:
        fieldRef:
          fieldPath: spec.nodeName
    - name: HOST_IP
      valueFrom:
        fieldRef:
          fieldPath: status.hostIP
    - name: HOST_NAME
      valueFrom:
        fieldRef:
          fieldPath: spec.nodeName
    - name: K8S_NAMESPACE
      valueFrom:
        fieldRef:
          fieldPath: metadata.namespace
    - name: "K8S_POD_NAME"
      valueFrom:
        fieldRef:
          fieldPath: "metadata.name"
```

5. Tạo các tài nguyên chúng ta đã tìm hiểu ở trên:

```bash
kubectl -n other get opentelemetrycollector adot-container-ci -o jsonpath='{.spec.config}' \
  | yq '.receivers.prometheus.config.scrape_configs[2]'
```
Cấu hình này tận dụng cơ chế khám phá dịch vụ Kubernetes của Prometheus để tự động phát hiện tất cả các pod có các chú thích cụ thể. Cấu hình cụ thể này sẽ phát hiện bất kỳ pod nào có chú thích prometheus.io/scrape và sẽ làm giàu các số liệu mà nó thu thập được với dữ liệu siêu dữ liệu Kubernetes như namespace và tên pod.

6. Để xác nhận việc triển khai, chạy lệnh sau:
```bash
$ kubectl get pod -n other -l app.kubernetes.io/name=adot-container-ci-deploy-collector
NAME                                      READY   STATUS    RESTARTS   AGE
adot-container-ci-deploy-collector-5lp5g  1/1     Running   0          15s
```

7. Để hỗ trợ CloudWatch Container Insights Prometheus, chúng ta xuất các số liệu với các kích thước cụ thể trong Định dạng Số liệu Nhúng - EMF. Bộ xuất CloudWatch EMF chuyển đổi dữ liệu số liệu thành các sự kiện nhật ký hiệu suất với EMF và sau đó gửi nó trực tiếp đến một nhóm nhật ký CloudWatch bằng cách sử dụng API PutLogEvents.

```bash
kubectl -n other get opentelemetrycollector adot-container-ci -o jsonpath='{.spec.config}' \
   | yq '.exporters.awsemf/prometheus'
```

8. Pipeline được xác định trong tập tin cấu hình `opentelemetrycollector.yaml`. Một pipeline xác định luồng dữ liệu trong bộ thu thập OpenTelemetry và bao gồm việc nhận, xử lý và xuất các chỉ số. Trong mỗi giai đoạn, có thể có nhiều thành phần và chúng có thể chạy tuần tự (bộ xử lý) hoặc song song (bộ nhận, bộ xuất). Bên trong, tất cả các thành phần giao tiếp bằng cách sử dụng các mô hình dữ liệu thống nhất của OpenTelemetry để các thành phần từ các nhà cung cấp khác nhau có thể làm việc cùng nhau. Các bộ nhận thu thập dữ liệu từ các hệ thống nguồn và chuyển đổi chúng thành các mô hình nội bộ. Các bộ xử lý có thể lọc và sửa đổi các chỉ số. Các bộ xuất chuyển đổi dữ liệu sang các schema khác và gửi đến các hệ thống đích. Từ cấu hình này, các chỉ số của bạn từ đơn hàng sẽ được cung cấp dưới không gian tên chỉ số CloudWatch `ContainerInsights/Prometheus` với các kích thước pod và productId theo cài đặt cấu hình của bộ xuất.


```bash
kubectl -n other get opentelemetrycollector adot-container-ci -o jsonpath='{.spec.config}' \
   | yq '.service'
```


9. Bây giờ khi chúng ta đã hoàn thành cài đặt, chúng ta sẽ sử dụng đoạn mã dưới đây để chạy một công cụ tạo tải mà sẽ đặt các đơn hàng thông qua cửa hàng và tạo ra các chỉ số ứng dụng:

```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: load-generator
  namespace: other
spec:
  containers:
  - name: artillery
    image: artilleryio/artillery:2.0.0-31
    args:
    - "run"
    - "-t"
    - "http://ui.ui.svc"
    - "/scripts/scenario.yml"
    volumeMounts:
    - name: scripts
      mountPath: /scripts
  initContainers:
  - name: setup
    image: public.ecr.aws/aws-containers/retail-store-sample-utils:load-gen.0.4.0
    command:
    - bash
    args:
    - -c
    - "cp /artillery/* /scripts"
    volumeMounts:
    - name: scripts
      mountPath: "/scripts"
  volumes:
  - name: scripts
    emptyDir: {}
EOF
```

10. Mở bảng điều khiển CloudWatch và điều hướng đến phần [Dashboards](https://console.aws.amazon.com/cloudwatch/home#dashboards).

![EKS workshop](../../../../images/0007/0004.jpg?featherlight=false&width=60pc)

11.  Chọn bảng điều khiển Order-Service-Metrics để xem các panel trong bảng điều khiển đó:

![Application Metrics](../../../../images/0007/0004.jpg?featherlight=false&width=60pc)


12.  Chúng ta có thể xem cách bảng điều khiển được cấu hình để truy vấn CloudWatch bằng cách di chuột qua tiêu đề của bảng **Orders by Product** và nhấp vào nút **Edit**:


![Edit Panel](../../../../images/0007/0005.jpg?featherlight=false&width=90pc)

13. Đoạn truy vấn được sử dụng để tạo bảng điều này được hiển thị ở cuối trang:

```sql
SELECT COUNT(watch_orders_total) FROM "ContainerInsights/Prometheus" WHERE productId != '*' GROUP BY productId
```

Đoạn truy vấn này thực hiện các bước sau:

- Truy vấn cho metric watch_orders_total
- Bỏ qua các metric có giá trị productId là *
- Tổng hợp các metric này và nhóm chúng theo productId
- Khi bạn hài lòng với việc quan sát các metric, bạn có thể dừng load generator bằng cách sử dụng lệnh sau.

```bash
kubectl delete pod load-generator -n other
```
14. Khi bạn đã hài lòng với các số liệu quan sát, bạn có thể dừng trình tạo tải bằng lệnh bên dưới:

```bash timeout=180 test=false
$ kubectl delete pod load-generator -n other
```