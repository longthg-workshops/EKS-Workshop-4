---
title: "Application Metrics"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: "<b> 5.5 </b>"
---

#### Application Metrics
Trong phần này, chúng ta sẽ tìm hiểu cách nhận thông tin từ các chỉ số do các công việc của chúng ta tiết lộ. Một số ví dụ về chúng có thể là:

- Chỉ số hệ thống như chỉ số Java heap hoặc trạng thái hồ bơi kết nối cơ sở dữ liệu
- Chỉ số ứng dụng liên quan đến các KPI kinh doanh

Hãy xem cách nhập chỉ số ứng dụng bằng cách sử dụng AWS Distro cho OpenTelemetry và biểu đồ hóa các chỉ số bằng cách sử dụng Grafana.

1. Mỗi thành phần trong bộ công cụ này đã được đặt dụng để cung cấp các chỉ số Prometheus bằng cách sử dụng thư viện phù hợp với ngôn ngữ lập trình hoặc framework cụ thể. Chúng ta có thể xem một ví dụ về các chỉ số từ dịch vụ đặt hàng như sau:

```

kubectl -n orders exec deployment/orders -- curl http://localhost:8080/actuator/prometheus

```

2. Đầu ra từ lệnh này có tính chi tiết nên ví dụ trên đã được rút gọn để hiển thị:

- Thông số hệ thống - Số lượng kết nối JDBC đang trống
- Thông số ứng dụng - Số lượng đơn hàng đã được đặt qua cửa hàng bán lẻ

3. Bạn có thể thực hiện các yêu cầu tương tự đối với các thành phần khác, ví dụ như dịch vụ thanh toán:

```

kubectl -n checkout exec deployment/checkout -- curl http://localhost:8080/metrics

```

4. Trong bài thực hành này, chúng ta sẽ tận dụng ADOT để tiếp nhận các chỉ số cho tất cả các thành phần và khám phá một bảng điều khiển để hiển thị số lượng đơn hàng đã được đặt. Hãy nhìn vào cấu hình OpenTelemetry được sử dụng để thu thập các chỉ số từ các pod ứng dụng, cụ thể là phần này:


```

kubectl -n other get opentelemetrycollector adot -o jsonpath='{.spec.config}' \
  | yq '.receivers.prometheus.config.scrape_configs[2]'

```

5. Cấu hình này tận dụng cơ chế phát hiện dịch vụ Prometheus trên Kubernetes để tự động khám phá tất cả các pod có các chú thích cụ thể. Cấu hình cụ thể này sẽ khám phá bất kỳ pod nào có chú thích prometheus.io/scrape, và sẽ bổ sung thông tin về chỉ số mà nó khám phá bằng dữ liệu siêu dữ liệu Kubernetes như namespace và tên pod.

Chúng ta có thể kiểm tra các chú thích trên các pod của thành phần đặt hàng:


```

kubectl get -o yaml -n orders deployment/orders | yq '.spec.template.metadata.annotations'

```

6. Như chúng ta đã thấy trong phần về các chỉ số của cluster, những chỉ số của pod này cũng sẽ được gửi đến AMP bằng cách sử dụng cùng một công cụ xuất dữ liệu OpenTelemetry.

- Tiếp theo, sử dụng đoạn mã dưới đây để chạy một công cụ tạo tải, sẽ đặt các đơn hàng thông qua cửa hàng và tạo ra các chỉ số ứng dụng:

```

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

7. Mở Grafana như chúng ta đã làm trong phần trước:

![EKS workshop](/EKS-Workshop-4/images/0007/0007.png?featherlight=false&width=90pc)


8. Đi đến trang bảng điều khiển và nhấp vào bảng điều khiển Order Service Metrics để xem lại các bảng trong bảng điều khiển:

![EKS workshop](/EKS-Workshop-4/images/0007/0008.png?featherlight=false&width=90pc)

9. Chúng ta có thể xem cách bảng điều khiển được cấu hình để truy vấn AMP bằng cách di chuột qua tiêu đề của bảng Orders by Product và nhấp vào nút "Edit":

![EKS workshop](/EKS-Workshop-4/images/0007/0009.png?featherlight=false&width=90pc)

10. Truy vấn PromQL được sử dụng để tạo bảng điều này được hiển thị ở cuối trang:

![EKS workshop](/EKS-Workshop-4/images/0007/00010.png?featherlight=false&width=90pc)

11. Trong trường hợp này, chúng ta đang sử dụng truy vấn:

```
sum by(productId) (watch_orders_total{productId!="*"})

```

12. Truy vấn này thực hiện các bước sau:

- Truy vấn cho chỉ số watch_orders_total
- Bỏ qua các chỉ số có giá trị productId là *
- Tổng hợp các chỉ số này và nhóm chúng theo productId
- Bạn cũng có thể khám phá các bảng điều khiển khác để hiểu cách chúng được tạo ra.

13. Khi bạn hài lòng với việc quan sát các chỉ số, bạn có thể dừng máy tạo tải bằng lệnh dưới đây.

```

kubectl delete pod load-generator -n other

```
