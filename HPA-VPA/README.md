# **Kubernetes HPA & VPA. (Horizontal Pod Autoscaling & Vertical Pod Autoscaling) on KIND/Minikube Cluster** 

## In this demo, we will see how to deploy HPA controller & VPA controller

**HPA**: HPA controller automatically scale the number of pods based on CPU utilization.

**VPA**: VPA controller scales by increasing or decreasing CPU and memory resources within the existing pod containers.

**Pre-requisites to implement this project:**
- 2 CPU, 4GB of RAM (t2.medium) on AWS
- Setup minikube & KIND Cluster.
- To check the metrics like cpu , memory utilization of your each pod and node then we have to enable the metrics server in kube system. If not already installed,   you can install it using:

**If you have on minikube cluster then you can enable the metrics to check each node & pod  cpu , memory utilization**
```yaml
minikube addons enable metrics-server
```
