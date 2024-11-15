---
title: "View resources in EKS console"
weight: 3
chapter: false
pre: "<b> 3. </b>"
---

{{% notice tip %}}
Prepare your environment for this section:
```bash
$ prepare-environment
```
{{% /notice %}}

In this lab, we'll view all Kubernetes API resource types using the AWS Management Console for Amazon EKS. You will be able to view and explore all standard Kubernetes API resource types such as configuration, authorization resources, policy resources, service resources and more. [Kubernetes resource view](https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html) is supported for all Kubernetes clusters hosted by Amazon EKS. You can use [Amazon EKS Connector](https://docs.aws.amazon.com/eks/latest/userguide/eks-connector.html) to register and connect any conformant Kubernetes cluster to AWS and visualize it in the Amazon EKS console.

We'll be viewing the resources created by the sample application. Note that you’ll only see resources that you have [RBAC permissions](https://docs.aws.amazon.com/eks/latest/userguide/view-kubernetes-resources.html#view-kubernetes-resources-permissions) to access, which were created during the environment preparation.
