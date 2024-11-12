---
title: "Enabling Container Insights"
weight: 1
chapter: false
pre: "<b> 6.1 </b>"
---

In this section of the lab, we’ll learn how to enable CloudWatch Container Insights metrics using ADOT Collector for an EKS cluster.

1. Now, let’s create the resources to give ADOT Collector the permissions it needs. We’ll start with the ClusterRole, which gives the collector permission to access the Kubernetes API:

**~/environment/eks-workshop/modules/observability/container-insights/adot/clusterrole.yaml**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: otel-ci-role
rules:
  - apiGroups: [""]
    resources: ["pods", "nodes", "endpoints"]
    verbs: ["list", "watch", "get"]
  - apiGroups: ["apps"]
    resources: ["replicasets"]
    verbs: ["list", "watch", "get"]
  - apiGroups: ["batch"]
    resources: ["jobs"]
    verbs: ["list", "watch"]
  - apiGroups: [""]
    resources: ["nodes/proxy"]
    verbs: ["get"]
  - apiGroups: [""]
    resources: ["nodes/stats", "configmaps", "events"]
    verbs: ["create", "get"]
  - apiGroups: [""]
    resources: ["configmaps"]
    verbs: ["update"]
  - apiGroups: [""]
    resources: ["configmaps"]
    resourceNames: ["otel-container-insight-clusterleader"]
    verbs: ["get","update", "create"]
  - apiGroups: ["coordination.k8s.io"]
    resources: ["leases"]
    verbs: ["create","get", "update"]
  - apiGroups: ["coordination.k8s.io"]
    resources: ["leases"]
    resourceNames: ["otel-container-insight-clusterleader"]
    verbs: ["get","update", "create"]
```

2. We'll use the CloudWatchAgentServerPolicy management IAM policy to provide the collector with the necessary IAM permissions via IAM Roles for Service Accounts.

```bash
aws iam list-attached-role-policies \
  --role-name eks-workshop-adot-collector-ci | jq .
```

3. This IAM role will be added to the ServiceAccount for the collector:

**~/environment/eks-workshop/modules/observability/container-insights/adot/serviceaccount.yaml**

``` yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: adot-collector-ci
  annotations:
    eks.amazonaws.com/role-arn: ${ADOT_IAM_ROLE_CI}
```

4. Create the required resources

```bash
kubectl kustomize ~/environment/eks-workshop/modules/observability/container-insights/adot \
  | envsubst | kubectl apply -f-
kubectl rollout status -n other daemonset/adot-container-ci-collector --timeout=120s
```

5. The specs for the collector are too long to show here, but you can view it with the following command:

```bash
kubectl -n other get opentelemetrycollector adot-container-ci
```

6. Let's break it down into parts to better understand what was implemented. Here is the configuration of the OpenTelemetry collector:

```bash
kubectl -n other get opentelemetrycollector adot-container-ci -o jsonpath='{.spec.config}'
```

7. Below is an OpenTelemetry pipeline configuration with the following structure:

**Receivers**

- Receiver Container Insights designed to collect performance log events using the Embedded Metric Format.

**Processors**

- Group metrics into 60-second intervals.

**Exporters**

- CloudWatch EMF exporter sends metrics to the CloudWatch API.

This collector is also configured to run as a DaemonSet with a collector agent running on each node.

You can check that:

```bash
kubectl -n other get opentelemetrycollector adot-container-ci -o jsonpath='{.spec.mode}{"\n"}'
```

8. We can confirm that by checking the Pods collecting ADOT Container Insights metrics running:

```bash
kubectl get pods -n other
```

**Output:**
```
NAME                               READY   STATUS    RESTARTS   AGE
adot-container-ci-collector-5lp5g  1/1     Running   0          15s
adot-container-ci-collector-ctvgs  1/1     Running   0          15s
```
If the output of this command includes multiple pods in the Running state as shown (above), the collector is running and collecting metrics from the cluster. The collector creates a log group named aws/containerinsights/cluster-name/performance and sends the metrics data as performance log events in EMF format.