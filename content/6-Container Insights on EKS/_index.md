---
title: "Container Insights on EKS"
weight: 6
chapter: false
pre: "<b> 6. </b>"
---

### Before you start

Prepare your environment for this section:

```bash
$ prepare-environment observability/container-insights
```

This will make the following changes to your lab environment:

- Install the OpenTelemetry operator
- Create an IAM role for the ADOT collector to access CloudWatch

You can view the Terraform that applies these changes [here](https://github.com/aws-samples/eks-workshop-v2/tree/stable/manifests/modules/observability/container-insights/.workshop/terraform).

### Lab introduction
In this lab, we'll see how to enable and use [CloudWatch Container Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ContainerInsights.html) to collect, aggregate, and summarize metrics and logs from your containerized applications and microservices. Container Insights is available for Amazon Elastic Container Service (Amazon ECS), Amazon Elastic Kubernetes Service (Amazon EKS), and Kubernetes platforms on Amazon EC2. Amazon ECS support includes support for Fargate.

The metrics include utilization for resources such as CPU, memory, disk, and network. Container Insights also provides diagnostic information, such as container restart failures, to help you isolate issues and resolve them quickly. You can also set CloudWatch alarms on metrics that Container Insights collects.

The metrics that Container Insights collects are available in CloudWatch automatic dashboards. You can analyze and troubleshoot container performance and logs data with CloudWatch Logs Insights.

Operational data is collected as performance log events. These are entries that use a structured JSON schema that enables high-cardinality data to be ingested and stored at scale. From this data, CloudWatch creates aggregated metrics at the cluster, node, pod, task, and service level as CloudWatch metrics.

We'll set up Container Insights to collect metrics from Amazon EKS cluster by using the [AWS Distro for OpenTelemetry collector](https://aws-otel.github.io/).