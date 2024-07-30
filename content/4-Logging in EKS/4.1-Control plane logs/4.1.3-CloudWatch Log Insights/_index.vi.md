---
title: "Viewing in CloudWatch"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b> 4.1.3 </b>"
---


CloudWatch Logs Insights cho phép bạn tìm kiếm và phân tích dữ liệu log của bạn trong CloudWatch Logs một cách tương tác. Bạn có thể thực hiện các truy vấn để giúp bạn phản ứng một cách hiệu quả hơn và hiệu quả hơn đối với các vấn đề vận hành. Nếu có vấn đề xảy ra, bạn có thể sử dụng CloudWatch Logs Insights để xác định nguyên nhân tiềm ẩn và xác nhận các sửa đổi đã triển khai. Nó bao gồm một ngôn ngữ truy vấn được xây dựng với một số lệnh đơn giản nhưng mạnh mẽ.

Trong bài thực hành này, chúng ta sẽ xem một ví dụ về việc sử dụng CloudWatch Log Insights để truy vấn các logs điều khiển EKS. Trước tiên, điều hướng đến CloudWatch Log Insights trong bảng điều khiển:

[https://console.aws.amazon.com/cloudwatch/home#logsV2:logs-insights](https://console.aws.amazon.com/cloudwatch/home#logsV2:logs-insights)

Bạn sẽ nhìn thấy một màn hình giống như sau:


Một trường hợp sử dụng phổ biến cho CloudWatch Log Insights là xác định các thành phần trong một cụm EKS đang tạo ra một lượng lớn các yêu cầu đến máy chủ API Kubernetes. Một cách để làm điều này là với truy vấn sau:

```
fields userAgent, requestURI, @timestamp, @message
| filter @logStream ~= "kube-apiserver-audit"
| stats count(userAgent) as count by userAgent
| sort count desc
```


Truy vấn này kiểm tra các log kiểm toán Kubernetes và đếm số lượng yêu cầu API được thực hiện được nhóm theo userAgent và sắp xếp chúng theo thứ tự giảm dần. Trong bảng điều khiển Log Insights, chọn nhóm log cho cụm EKS của bạn.

Sao chép truy vấn vào bảng điều khiển và nhấn Chạy truy vấn, điều này sẽ trả về kết quả:

Thông tin này có thể rất quý giá để hiểu rõ các thành phần nào đang gửi yêu cầu đến máy chủ API.
