---
title: "Managing Application Logs"
weight: 2
chapter: false
pre: "<b> 1.2 </b>"
---

Bắt đầu với logging trong Docker:

```
Logs - Kubernetes
apiVersion: v1
kind: Pod
metadata:
  name: event-simulator-pod
spec:
  containers:
  - name: event-simulator
    image: kodekloud/event-simulator

```

Để xem logs:

```
$ kubectl logs -f event-simulator-pod

```

Nếu có nhiều containers trong một pod, bạn phải chỉ định tên của container một cách rõ ràng trong lệnh.

```
$ kubectl logs -f <pod-name> <container-name>
$ kubectl logs -f even-simulator-pod event-simulator

```

#### K8s Reference Docs
https://kubernetes.io/blog/2015/06/cluster-level-logging-with-kubernetes/

