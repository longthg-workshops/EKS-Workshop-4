---
title: "Services and Endpoints"
weight: 3
chapter: false
pre: "<b> 3.3 </b>"
---

1. To view the Kubernetes service and networking resources, click on the **_Resources_** tab. Drill down to the **_Service and Networking_** section and you can view several the Kubernetes API resource types that are part of service and networking. This lab exercise details ways to expose an application running on a set of Pods as **_Service_**, **_Endpoints_** and **_Ingresses_**.

2. [Service](https://kubernetes.io/docs/concepts/services-networking/service/) resource view displays all the services that expose applications running on set of pods in a cluster.

![EKS workshop](/images/0005/00019.png?featherlight=false&width=90pc)

3. If you select the service `cart` the view displayed will have details about the service in Info section including selector(The set of pods targeted by a service is usually determined by a selector), the protocol and port it is running on and any labels and annotations.

4. Pods expose themselves through endpoints to a service. An endpoint is an resource that gets an IP address and port of pods assigned dynamically to it. An endpoint is reference by a Kubernetes service.

![EKS workshop](/images/0005/00020.png?featherlight=false&width=90pc)

5. For this sample application, click on `Endpoints` and explore the details of the IP address and port associated with the endpoint along with Info, Labels and Annotations sections.

![EKS workshop](/images/0005/00021.png?featherlight=false&width=90pc)