---
title: "Container Insights metrics in Amazon CloudWatch"
weight: 2
chapter: false
pre: "<b> 6.2 </b>"
---

1. In this lab, we will visualize Container Insights metrics using CloudWatch.

2. To view the metrics, first open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/home#container-insights:infrastructure) and navigate to Container Insights.

3. Select "Performance Monitoring" from the dropdown menu at the top of the page, and then select the type of resource you want to view. For example, select "EKS Pods" to get basic metrics like CPU, memory, and utilization for the Pods running in our EKS cluster:

![EKS workshop](/EKS-Workshop-4/images/0007/0001.jpg?featherlight=false&width=90pc)

You can set up a CloudWatch alarm with any metric that Container Insights collects.