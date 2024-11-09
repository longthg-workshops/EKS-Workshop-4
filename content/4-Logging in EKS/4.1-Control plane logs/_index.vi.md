---
title: "Control plane logs"
weight: 1
chapter: false
pre: "<b> 4.1 </b>"
---

{{% notice tip %}}
Chuẩn bị môi trường cho phần này:
```bash
prepare-environment observability/logging/cluster
```
{{% /notice %}}

![Lab prepare](../../../images/0006/0002.png?featherlight=false&width=90pc)

Amazon EKS control plane logging cung cấp nhật ký kiểm tra và chẩn đoán trực tiếp từ lớp điều khiển Amazon EKS đến CloudWatch Logs trong tài khoản của bạn. Nhật ký này giúp bạn dễ dàng bảo mật và vận hành các cụm của mình. Bạn có thể chọn chính xác loại nhật ký bạn cần và nhật ký được gửi dưới dạng luồng nhật ký đến một nhóm cho mỗi cụm Amazon EKS trong CloudWatch.

Bạn có thể kích hoạt hoặc vô hiệu hóa mỗi loại nhật ký trên cơ sở từng cụm bằng cách sử dụng Bảng điều khiển Quản lý AWS, AWS CLI (phiên bản 1.16.139 trở lên) hoặc thông qua API Amazon EKS.

Khi bạn sử dụng nhật ký điều khiển Amazon EKS, bạn sẽ bị tính phí theo giá cơ bản của Amazon EKS cho mỗi cụm bạn chạy cùng với các chi phí tiếp nhận và lưu trữ dữ liệu nhật ký CloudWatch thông thường cho bất kỳ nhật ký nào được gửi đến CloudWatch Logs từ các cụm của bạn.

Các loại nhật ký điều khiển cụm sau đây có sẵn. Mỗi loại nhật ký tương ứng với một thành phần của bảng điều khiển Kubernetes. Để tìm hiểu thêm về các thành phần này, xem Các thành phần Kubernetes trong tài liệu Kubernetes.

- Nhật ký thành phần máy chủ API Kubernetes (api) – Máy chủ API của cụm của bạn là thành phần bảng điều khiển mà tiếp xúc với API Kubernetes.
- Kiểm tra (audit) – Nhật ký kiểm tra Kubernetes cung cấp một bản ghi về các người dùng, quản trị viên hoặc các thành phần hệ thống cá nhân đã ảnh hưởng đến cụm của bạn.
- Trình xác thực (authenticator) – Nhật ký trình xác thực là duy nhất đối với Amazon EKS. Nhật ký này đại diện cho thành phần bảng điều khiển mà Amazon EKS sử dụng cho Xác thực dựa trên Vai trò (RBAC) của Kubernetes bằng cách sử dụng các thông tin xác thực IAM.
- Bộ điều khiển quản lý (controllerManager) – Bộ điều khiển quản lý quản lý các vòng lặp điều khiển cốt lõi được gửi kèm với Kubernetes.
- Lập lịch (scheduler) – Thành phần lập lịch quản lý khi nào và nơi chạy các pod trong cụm của bạn.

