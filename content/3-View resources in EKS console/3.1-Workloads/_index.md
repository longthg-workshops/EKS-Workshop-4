---
title: "Workloads"
weight: 1
chapter: false
pre: "<b> 3.1 </b>"
---

To view the Kubernetes resources, click on the **_Resources_** tab. Drill down to the **_Workload_** section and you can view several of the Kubernetes API resource types that are part of workloads. Workloads encompass the running containers in your cluster, and include Pods, ReplicaSets, Deployments, and DaemonSets. These are fundamental building blocks for running containers with a cluster.

### Workloads

#### **Pods**

1. **Pods** resource view displays all the pods which represent the smallest and simplest Kubernetes object.
By default, all Kubernetes API resource types are shown, but you can filter by namespace or search for specific values to find what you’re looking for quickly. Below you will see the pods filtered by the `namespace=catalog`.

![EKS workshop](../../images/0005/0002.png?featherlight=false&width=90pc)

2. The resources view for all Kubernetes API resource types, offers two views - structured view and raw view. The structured view provides a visual representation of the resource to help access the data for the resource.

![EKS workshop](../../images/0005/0003.png?featherlight=false&width=90pc)

3. Raw view shows the complete JSON output from the Kubernetes API, which is useful for understanding the configuration and state of resource types that do not have structured view support in the Amazon EKS console.

![EKS workshop](../../images/0005/0004.png?featherlight=false&width=90pc)

#### **ReplicaSets**

A [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) is a Kubernetes object that ensures a stable set of replica pods are running at all times. As such, it is often used to guarantee the availability of a specified number of identical pods. In this example (below), you can see 2 replicasets for namespace `orders`. The replicaset for `orders-d6b4566fc` defines the configuration for desired and current number of pods.

Click on the replicaset `orders-d6b4566fc` and explore the configuration. You will see configurations under Info, Pods, labels and details of max and desired replicas.

![EKS workshop](../../images/0005/0005.png?featherlight=false&width=90pc)

![EKS workshop](../../images/0005/0006.png?featherlight=false&width=90pc)


![EKS workshop](../../images/0005/0007.png?featherlight=false&width=90pc)


![EKS workshop](../../images/0005/0008.png?featherlight=false&width=90pc)

#### **DaemonSet**

A [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) ensures that all (or some) Nodes run a copy of a pod. In the sample application we have DaemonSet running on each node as shown (below).

Click on the daemonset `kube-proxy` and explore the configuration. You will see configurations under Info, pods running on each node, labels, and annotations.

![EKS workshop](../../images/0005/0009.png?featherlight=false&width=90pc)


![EKS workshop](../../images/0005/00010.png?featherlight=false&width=90pc)

#### **Deployments**

A [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) is a Kubernetes object that provides declarative updates to pods and replicaSets. It tells Kubernetes how to create or modify instances of pods. Deployments help to scale the number of replica pods and enable rollout or rollback a deployment version in a controlled manner. In this example (below), you can see 2 deployments for namespace `carts`.

![EKS workshop](../../images/0005/00011.png?featherlight=false&width=90pc)


![EKS workshop](../../images/0005/00012.png?featherlight=false&width=90pc)


![EKS workshop](../../images/0005/00013.png?featherlight=false&width=90pc)
