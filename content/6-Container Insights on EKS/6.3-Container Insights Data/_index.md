---
title: Container Insights data with CloudWatch Logs Insights"
weight: 3
chapter: false
pre: "<b> 6.3 </b>"
---

Container Insights collects metrics using performance log events using the Embedded Metric Format stored in CloudWatch Logs. CloudWatch automatically generates some metrics from the logs, which you can view in the CloudWatch console. You can also perform deeper analysis on the collected performance data using CloudWatch Logs Insights queries.

1. Open the [CloudWatch Log Insights Console](https://console.aws.amazon.com/cloudwatch/home#logsV2:logs-insights).

2. Near the top of the page is the query editor. When you first open CloudWatch Logs Insights, this box contains a default query that returns the 20 most recent log events.

3. When you select a log group and run the query, CloudWatch Logs Insights automatically discovers fields in the data in the log group and displays them in **Discovered Fields** on the right. It also displays a bar chart of the log events in this log group over time. This bar chart shows the distribution of events in the log group that match your query and time range, not just the events shown in the table. Select the log group for your EKS cluster that ends in **/performance**.

4. In the query editor, replace the default query with the following query and select **Run query**.

```sql
STATS avg(node_cpu_utilization) as avg_node_cpu_utilization by NodeName
| SORT avg_node_cpu_utilization DESC 
```

![EKS workshop](../../../../images/0007/0002.jpg?featherlight=false&width=90pc)

5. This query shows a list of nodes, sorted by the node's average CPU usage.

To try with another example, replace that query with another query and select **Run query**.

```
STATS avg(number_of_container_restarts) as avg_number_of_container_restarts by PodName
| SORT avg_number_of_container_restarts DESC
```

![EKS workshop](../../../../images/0007/0003.jpg?featherlight=false&width=90pc)

This query shows a list of your pods, sorted by their average container restarts.