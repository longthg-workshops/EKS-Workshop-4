---
title: "Application Metrics in Amazon CloudWatch"
weight: 4
chapter: false
pre: "<b> 6.4 </b>"
---

####  Đo lường Ứng dụng trong Amazon CloudWatch

1. Trong phần này, chúng ta sẽ tìm hiểu cách nhận được thông tin về các chỉ số được tiết lộ bởi các công việc của chúng ta và hiển thị các chỉ số đó bằng cách sử dụng Amazon CloudWatch Insights Prometheus. Một số ví dụ về các chỉ số này có thể là:

- Các chỉ số hệ thống như chỉ số Java heap hoặc trạng thái của bể kết nối cơ sở dữ liệu
- Các chỉ số ứng dụng liên quan đến các chỉ số KPI kinh doanh

2. Hãy xem cách nhập các chỉ số ứng dụng bằng cách sử dụng AWS Distro cho OpenTelemetry và hiển thị các chỉ số bằng cách sử dụng Amazon CloudWatch.

3. Mỗi thành phần trong bộ công cụ này đã được trang bị để cung cấp các chỉ số Prometheus bằng cách sử dụng các thư viện phù hợp với ngôn ngữ lập trình hoặc framework cụ thể. Chúng ta có thể xem một ví dụ về các chỉ số này từ dịch vụ đặt hàng như sau:

```

kubectl -n orders exec deployment/orders -- curl http://localhost:8080/actuator/prometheus

```
Kết quả từ lệnh này là verbose, cho mục đích của bài lab này, hãy tập trung vào chỉ số - watch_orders_total:

- **watch_orders_total**: Chỉ số ứng dụng - Số đơn hàng đã được đặt qua cửa hàng bán lẻ

4. Bạn có thể thực hiện các yêu cầu tương tự đối với các thành phần khác, ví dụ như dịch vụ thanh toán:


```

kubectl -n checkout exec deployment/checkout -- curl http://localhost:8080/metrics

```

Trong bài thực hành này, chúng ta sẽ tận dụng CloudWatch Container Insights Prometheus Support với AWS Distro for OpenTelemetry để thu thập các số liệu từ tất cả các thành phần và xây dựng Bảng điều khiển Amazon CloudWatch để hiển thị số lượng đơn hàng đã được đặt. Có hai phần cần giải quyết để tích hợp Prometheus với CloudWatch Container Insights Prometheus. Phần đầu tiên là lấy các số liệu từ Prometheus từ các pod ứng dụng. Vấn đề thứ hai là hiển thị chúng trong định dạng đặc biệt của CloudWatch với các bộ siêu dữ liệu đúng.

6. Đầu tiên, chúng ta phải thu thập các số liệu từ các pod ứng dụng. Cấu hình OpenTelemetry để thực hiện điều này được mô tả chi tiết dưới đây:

```

kubectl -n other get opentelemetrycollector adot-container-ci -o jsonpath='{.spec.config}' \
  | yq '.receivers.prometheus.config.scrape_configs[2]'

```

7. Cấu hình này tận dụng cơ chế khám phá dịch vụ Kubernetes của Prometheus để tự động phát hiện tất cả các pod có các chú thích cụ thể. Cấu hình cụ thể này sẽ phát hiện bất kỳ pod nào có chú thích prometheus.io/scrape và sẽ làm giàu các số liệu mà nó thu thập được với dữ liệu siêu dữ liệu Kubernetes như namespace và tên pod.

8. Để hỗ trợ CloudWatch Container Insights Prometheus, chúng tôi xuất các số liệu với các kích thước cụ thể trong Định dạng Số liệu Nhúng - EMF. Bộ xuất CloudWatch EMF chuyển đổi dữ liệu số liệu thành các sự kiện nhật ký hiệu suất với EMF và sau đó gửi nó trực tiếp đến một nhóm nhật ký CloudWatch bằng cách sử dụng API PutLogEvents.

```

kubectl -n other get opentelemetrycollector adot-container-ci -o jsonpath='{.spec.config}' \
   | yq '.exporters.awsemf/prometheus'

```

9. Pipeline được xác định trong tập tin cấu hình `opentelemetrycollector.yaml`. Một pipeline xác định luồng dữ liệu trong bộ thu thập OpenTelemetry và bao gồm việc nhận, xử lý và xuất các chỉ số. Trong mỗi giai đoạn, có thể có nhiều thành phần và chúng có thể chạy tuần tự (bộ xử lý) hoặc song song (bộ nhận, bộ xuất). Bên trong, tất cả các thành phần giao tiếp bằng cách sử dụng các mô hình dữ liệu thống nhất của OpenTelemetry để các thành phần từ các nhà cung cấp khác nhau có thể làm việc cùng nhau. Các bộ nhận thu thập dữ liệu từ các hệ thống nguồn và chuyển đổi chúng thành các mô hình nội bộ. Các bộ xử lý có thể lọc và sửa đổi các chỉ số. Các bộ xuất chuyển đổi dữ liệu sang các schema khác và gửi đến các hệ thống đích. Từ cấu hình này, các chỉ số của bạn từ đơn hàng sẽ được cung cấp dưới không gian tên chỉ số CloudWatch `ContainerInsights/Prometheus` với các kích thước pod và productId theo cài đặt cấu hình của bộ xuất.


```

kubectl -n other get opentelemetrycollector adot-container-ci -o jsonpath='{.spec.config}' \
   | yq '.service'

```


10. Bây giờ khi chúng ta đã hoàn thành cài đặt, chúng ta sẽ sử dụng đoạn mã dưới đây để chạy một công cụ tạo tải mà sẽ đặt các đơn hàng thông qua cửa hàng và tạo ra các chỉ số ứng dụng:

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

11. Mở bảng điều khiển CloudWatch và điều hướng đến phần Dashboards:

![EKS workshop](/EKS-Workshop-4/images/0007/0004.jpg?featherlight=false&width=60pc)

12.  Chọn bảng điều khiển Order-Service-Metrics để xem các panel trong bảng điều khiển đó:

![EKS workshop](/EKS-Workshop-4/images/0007/0005.jpg?featherlight=false&width=90pc)


13.  Chúng ta có thể xem cách bảng điều khiển được cấu hình để truy vấn CloudWatch bằng cách di chuột qua tiêu đề của bảng **Orders by Product** và nhấp vào nút **Edit**:


![EKS workshop](/EKS-Workshop-4/images/0007/0006.jpg?featherlight=false&width=90pc)

14. Đoạn truy vấn được sử dụng để tạo bảng điều này được hiển thị ở cuối trang:


```
SELECT COUNT(watch_orders_total) FROM "ContainerInsights/Prometheus" WHERE productId != '*' GROUP BY productId
```

Đoạn truy vấn này thực hiện các bước sau:

- Truy vấn cho metric watch_orders_total
- Bỏ qua các metric có giá trị productId là *
- Tổng hợp các metric này và nhóm chúng theo productId
- Khi bạn hài lòng với việc quan sát các metric, bạn có thể dừng load generator bằng cách sử dụng lệnh sau.

```

kubectl delete pod load-generator -n other

```

