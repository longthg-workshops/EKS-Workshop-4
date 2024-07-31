---
title: "Số liệu Container Insights trong Amazon CloudWatch"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b> 6.2 </b>"
---

#### Số liệu Container Insights trong Amazon CloudWatch

1. Trong bài thực hành này, chúng ta sẽ trực quan hóa các số liệu Container Insights bằng cách sử dụng CloudWatch.

2. Để xem các số liệu, trước tiên hãy mở bảng điều khiển CloudWatch và điều hướng đến Container Insights: https://console.aws.amazon.com/cloudwatch/home#container-insights:infrastructure


3. Chọn "Performance Monitoring" từ menu dropdown ở đầu trang, và sau đó chọn loại tài nguyên mà bạn muốn xem. Ví dụ, chọn "EKS Pods" để có các số liệu cơ bản như CPU, bộ nhớ và sự sử dụng cho các Pods đang chạy trong cụm EKS của chúng ta:

![EKS workshop](/EKS-Workshop-4/images/0007/0001.jpg?featherlight=false&width=90pc)

Bạn có thể thiết lập một cảnh báo CloudWatch trên bất kỳ số liệu nào mà Container Insights thu thập.






