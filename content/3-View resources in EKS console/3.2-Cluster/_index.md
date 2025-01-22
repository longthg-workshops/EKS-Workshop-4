---
title: "Cluster"
weight: 2
chapter: false
pre: "<b> 3.2 </b>"
---

### Your Cluster

To view the Kubernetes cluster resources, click on the Resources  tab. Drill down to the  Cluster  section and you can view several Kubernetes API resource types that are part of cluster. Cluster view details all the components of the cluster architecture like Nodes, Namespaces and API Services that run the workloads.

### Nodes

Kubernetes runs your workload by placing containers into pods to run on [Nodes](https://kubernetes.io/docs/concepts/architecture/nodes/). A node may be a virtual or physical machine, depending on the cluster. The eks-workshop is running 3 nodes on which the workloads are deployed. Click on the Nodes drill down to list the nodes.

If you click on any of the node names, you will find the Info section that has a lot of details of the node - OS, container runtime, instance type, EC2 instance and [Managed node group](https://docs.aws.amazon.com/eks/latest/userguide/managed-node-groups.html) (that make it easy to provision compute capacity for the cluster). The next section, Capacity allocation shows usage and reservation of various resources on EC2 worker nodes connected to the cluster.

The console also details all the pods provisioned on the node and any applicable [Taints](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/), labels, and annotations.

![EKS workshop](/images/0005/00014.png?featherlight=false&width=90pc)


![EKS workshop](/images/0005/00015.png?featherlight=false&width=90pc)


![EKS workshop](/images/0005/00016.png?featherlight=false&width=90pc)

### Namespaces

[Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces) are a mechanism to organize clusters which can be very helpful when different teams or projects share a Kubernetes cluster. In our sample application we have microservices - carts, checkout, catalog, assets which all share the same cluster using the namespace construct.


![EKS workshop](/images/0005/0018.png?featherlight=false&width=90pc)