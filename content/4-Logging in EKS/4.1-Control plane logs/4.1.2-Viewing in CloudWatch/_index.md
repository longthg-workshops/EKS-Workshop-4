---
title: "Viewing in CloudWatch"
weight: 2
chapter: false
pre: "<b> 4.1.2 </b>"
---

Let's take a look at the logs in the [CloudWatch Logs console](https://console.aws.amazon.com/cloudwatch/home?#logsV2:log-groups).

Filter for **/aws/eks** prefix and select the cluster you want verify the logs:

![EKS workshop](/EKS-Workshop-4/images/0006/0006.png?featherlight=false&width=90pc)

You will be presented with a number of log streams in the group:

![EKS workshop](/EKS-Workshop-4/images/0006/0007.png?featherlight=false&width=90pc)

Select any of these log streams to view the entries being sent to CloudWatch Logs by the EKS control plane.