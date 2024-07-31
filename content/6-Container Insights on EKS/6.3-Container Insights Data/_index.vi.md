---
title: "Dữ liệu Container Insights với CloudWatch Logs Insights"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b> 6.3 </b>"
---

#### Dữ liệu Container Insights với CloudWatch Logs Insights

Container Insights thu thập các số liệu bằng cách sử dụng các sự kiện nhật ký hiệu suất bằng cách sử dụng Embedded Metric Format được lưu trữ trong CloudWatch Logs. CloudWatch tự động tạo ra một số số liệu từ các nhật ký mà bạn có thể xem trong bảng điều khiển CloudWatch. Bạn cũng có thể tiến hành phân tích sâu hơn về dữ liệu hiệu suất được thu thập bằng cách sử dụng các truy vấn CloudWatch Logs Insights.

1. Đầu tiên, mở bảng điều khiển CloudWatch Log Insights:

    [https://console.aws.amazon.com/cloudwatch/home#logsV2:logs-insights](https://console.aws.amazon.com/cloudwatch/home#logsV2:logs-insights)

2. Gần đầu trang là trình soạn thảo truy vấn. Khi bạn lần đầu tiên mở CloudWatch Logs Insights, hộp này chứa một truy vấn mặc định trả về 20 sự kiện nhật ký gần đây nhất.

3. Khi bạn chọn một nhóm nhật ký và chạy truy vấn, CloudWatch Logs Insights tự động phát hiện các trường trong dữ liệu trong nhóm nhật ký và hiển thị chúng trong **Discovered Fields** (Các trường đã khám phá) ở bên phải. Nó cũng hiển thị một biểu đồ cột của các sự kiện nhật ký trong nhóm nhật ký này theo thời gian. Biểu đồ cột này hiển thị phân phối của các sự kiện trong nhóm nhật ký phù hợp với truy vấn và phạm vi thời gian của bạn, không chỉ các sự kiện được hiển thị trong bảng. Chọn nhóm nhật ký cho cụm EKS của bạn kết thúc bằng **/performance**.

4. Trong trình soạn thảo truy vấn, thay thế truy vấn mặc định bằng truy vấn sau đây và chọn **Run query**.

```
STATS avg(node_cpu_utilization) as avg_node_cpu_utilization by NodeName
| SORT avg_node_cpu_utilization DESC 
```

![EKS workshop](/EKS-Workshop-4/images/0007/0002.jpg?featherlight=false&width=90pc)

5. Đoạn truy vấn này hiển thị danh sách các node, được sắp xếp theo mức sử dụng trung bình của CPU của node.

    Để thử một ví dụ khác, thay thế truy vấn đó bằng một truy vấn khác và chọn **Run query**.

```
STATS avg(number_of_container_restarts) as avg_number_of_container_restarts by PodName
| SORT avg_number_of_container_restarts DESC
```

![EKS workshop](/EKS-Workshop-4/images/0007/0003.jpg?featherlight=false&width=90pc)


Truy vấn này hiển thị danh sách các pods của bạn, được sắp xếp theo số lần khởi động lại trung bình của container.