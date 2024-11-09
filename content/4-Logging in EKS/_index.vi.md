---
title: "Tạo log trên EKS"
weight: 4
chapter: false
pre: "<b> 4. </b>"
---

Trong Kubernetes, việc ghi log có thể được chia thành ghi log của control plane, ghi log của node và ghi log của ứng dụng. [Control plane của Kubernetes](https://kubernetes.io/docs/concepts/overview/components/#control-plane-components) là một bộ các thành phần quản lý các cụm Kubernetes và tạo ra các log được sử dụng cho mục đích kiểm toán và chẩn đoán. Với Amazon EKS, bạn có thể bật log cho các thành phần control plane khác nhau và gửi chúng đến Amazon CloudWatch.

Các container được nhóm lại thành các Pod trong một cụm Kubernetes và được lên lịch để chạy trên các node Kubernetes của bạn. Hầu hết các ứng dụng container viết vào standard output và standard error, và engine của container chuyển hướng đầu ra đến một driver ghi log. Trong Kubernetes, các log của container được tìm thấy trong thư mục `/var/log/pods` trên một node. Bạn có thể cấu hình CloudWatch và Container Insights để ghi lại các log này cho mỗi Pod của Amazon EKS của bạn.

Trong lab này, chúng ta sẽ:

- Làm thế nào để bật log Control Plane của EKS và xác minh nó trong Amazon CloudWatch
- Làm thế nào để thiết lập agent ghi log (Fluent Bit) để stream các log của Pod đến Amazon CloudWatch
