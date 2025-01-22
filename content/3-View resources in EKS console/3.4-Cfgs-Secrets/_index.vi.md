---
title: "Cấu hình (ConfigMaps) và Khóa (Secrets)"
weight: 4
chapter: false
pre: "<b> 3.4 </b>"
---

Để xem tài nguyên Kubernetes ConfigMap và Secrets, hãy nhấp vào tab Resources. Tiếp tục vào phần Config và secrets và bạn có thể xem một số loại tài nguyên API Kubernetes mà là phần của ConfigMap và Secrets API. Bài thực hành lab này mô tả các tài nguyên mà Kubernetes cung cấp để cấu hình các workloads với dữ liệu không bảo mật sử dụng ConfigMap và dữ liệu nhạy cảm sử dụng Secrets.

### ConfigMaps

**ConfigMaps** là một đối tượng tài nguyên của Kubernetes được sử dụng để lưu trữ dữ liệu cấu hình dưới định dạng key-value. ConfigMaps hữu ích để lưu trữ biến môi trường, tham số dòng lệnh, cấu hình ứng dụng có thể được truy cập bởi các ứng dụng triển khai trong các pod. ConfigMaps cũng có thể được lưu trữ dưới dạng các tệp cấu hình trong một khối lưu trữ. Điều này giúp tách biệt dữ liệu cấu hình khỏi mã ứng dụng.

Nhấp vào ConfigMap drill down và bạn có thể xem tất cả các cấu hình cho cụm máy chủ.

Nếu bạn nhấp vào ConfigMap checkout, bạn có thể xem các thuộc tính liên quan đến nó, trong trường hợp này, key REDIS_URL với giá trị của địa chỉ endpoint của redis. Như bạn thấy, giá trị không được mã hóa và ConfigMaps không nên được sử dụng để lưu trữ bất kỳ cặp key-value nào có tính bảo mật.

![EKS workshop](/EKS-Workshop-4/images/0005/00022.png?featherlight=false&width=90pc)

### Secrets

**Secrets** là một đối tượng tài nguyên Kubernetes dùng để lưu trữ các dữ liệu nhạy cảm như tên người dùng, mật khẩu, token và các thông tin chứng thực khác. Secrets hữu ích để tổ chức và phân phối thông tin nhạy cảm qua các pod trong một cụm. Secrets có thể được sử dụng theo nhiều cách khác nhau, như được gắn kết như các thư mục dữ liệu hoặc được tiết lộ như các biến môi trường để sử dụng bởi một container trong một Pod.

Nhấp vào liên kết **Secrets** để xem tất cả các secrets cho cụm.

Nếu bạn nhấp vào **Secrets** checkout-config, bạn có thể xem các secrets liên quan đến nó. Trong trường hợp này, lưu ý token được mã hóa. Bạn nên thấy giá trị giải mã cũng với nút chuyển đổi giải mã.


![EKS workshop](/EKS-Workshop-4/images/0005/00023.png?featherlight=false&width=90pc)
