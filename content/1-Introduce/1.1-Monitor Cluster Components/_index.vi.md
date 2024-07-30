---
title: "Giám sát Cluster Kubernetes"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b> 1.1 </b>"
---


#### Giám sát Cluster Kubernetes

Trong phần này, chúng ta sẽ tìm hiểu về việc giám sát cluster Kubernetes.

#### Giám sát Tiêu Thụ Tài Nguyên trong Kubernetes

Làm thế nào bạn có thể giám sát việc tiêu thụ tài nguyên trong Kubernetes? Hoặc quan trọng hơn, bạn muốn giám sát những gì?

#### Heapster vs Metrics Server

Heapster hiện đã được loại bỏ và một phiên bản thu gọn hơn được hình thành được biết đến là metrics server.

#### Metrics Server

Làm thế nào các metric được tạo ra cho các POD trên các node này?

#### Metrics Server - Bắt Đầu

Sao chép metric server từ kho github

```
$ git clone https://github.com/kubernetes-incubator/metrics-server.git

```

Triển khai metric server

```
$ kubectl create -f metric-server/deploy/1.8+/

```

Xem hiệu suất của cluster

```
$ kubectl top node

```

Xem các metric hiệu suất của pod

```
$ kubectl top pod

```