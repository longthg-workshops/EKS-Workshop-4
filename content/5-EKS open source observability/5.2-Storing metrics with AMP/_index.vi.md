---
title: "Lưu trữ số liệu với AMP"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b> 5.2 </b>"
---

#### Lưu trữ số liệu với AMP

 Một không gian làm việc Amazon Managed Service for Prometheus đã được tạo sẵn cho bạn. Bạn có thể xem nó trong bảng điều khiển tại đây:

[https://console.aws.amazon.com/prometheus/home#/workspaces](https://console.aws.amazon.com/prometheus/home#/workspaces)

Để xem không gian làm việc, hãy nhấp vào tab Tất cả các Không gian làm việc ở bảng điều khiển điều khiển bên trái. Chọn không gian làm việc bắt đầu bằng eks-workshop và bạn có thể xem nhiều tab khác nhau dưới không gian làm việc như quản lý quy tắc, quản lý cảnh báo, v.v.

Hãy xác nhận việc chúng ta đã thành công trong việc nhập các số liệu:

```

awscurl -X POST --region $AWS_REGION --service aps "${AMP_ENDPOINT}api/v1/query?query=up" | jq '.data.result[1]'

```

