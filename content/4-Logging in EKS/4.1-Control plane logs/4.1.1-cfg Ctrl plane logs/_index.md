---
title: "Configure Control plane logs"
weight: 1
chapter: false
pre: "<b> 4.1.1 </b>"
---

You can enable each of the cluster log types individually, and in this lab we're enabling everything.

Let's take a look this configuration in the EKS console:

[Enter the EKS Console](https://console.aws.amazon.com/eks/home#/clusters)

The **Logging** tab shows the current configuration for control plane logs for the cluster:

![EKS Console Logging Tab](/EKS-Workshop-4/images/4/1/1/0004-eks-cluster-logging-tab.webp?featherlight=false&width=90pc)

You can alter the logging configuration by clicking the **Manage** button:

![Enable Logging](/EKS-Workshop-4/images/0006/0005.png?featherlight=false&width=90pc)

You can also enable EKS control plane logs on a per-cluster basis through the EKS API. This will often be configured using Terraform or CloudFormation, but in this lab we can use the AWS CLI to enable the functionality:

```bash hook=cluster-logging
aws eks update-cluster-config \
    --region $AWS_REGION \
    --name $EKS_CLUSTER_NAME \
    --logging '{"clusterLogging":[{"types":["api","audit","authenticator","controllerManager","scheduler"],"enabled":true}]}'
sleep 30
aws eks wait cluster-active --name $EKS_CLUSTER_NAME
```

![EKS workshop](/EKS-Workshop-4/images/0006/0003.png?featherlight=false&width=90pc)

{{% notice info %}}
If you are using the CDK Observability Accelerator then check out the [CDK Observability Builder](https://aws-quickstart.github.io/cdk-eks-blueprints/builders/observability-builder/#supported-methods) which supports enabling all control plane logging features for EKS clusters and storing them in CloudWatch.
{{% /notice %}}