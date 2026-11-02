# **Kubernetes HPA & VPA. (Horizontal Pod Autoscaling & Vertical Pod Autoscaling) on KIND/Minikube Cluster** 

## In this demo, we will see how to deploy HPA controller & VPA controller

**HPA**: HPA controller automatically scale the number of pods based on CPU utilization.

**VPA**: VPA controller scales by increasing or decreasing CPU and memory resources within the existing pod containers.

**Pre-requisites to implement this project:**
- 2 CPU, 4GB of RAM (t2.medium) on AWS
- Setup minikube & KIND Cluster.
- To check the metrics like cpu , memory utilization of your each pod and node then we have to enable the metrics server in kube system. If not already installed,   you can install it using:

**If you have on minikube cluster then you can enable the metrics to check cpu, memory utilization each node & pod**
```yaml
minikube addons enable metrics-server
```

**To check the Utilization of Node & Pod**
```yaml
kubectl top pod
```
**or** 
```yaml
kubectl top pod -n nginx
```
**To check Node utilization**
```yaml
kubectl top node
```
**or** 
```yaml
kubectl top node -n nginx
```
**If you are using a Kind cluster install Metrics Server**
```yaml
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
**Edit the Metrics Server Deployment**
```yaml
kubectl -n kube-system edit deployment metrics-server
```
**Add the security bypass to deployment under container.args**
```yaml
- --kubelet-insecure-tls
- --kubelet-preferred-address-types=InternalIP,Hostname,ExternalIP
```
**Restart the deployment**
```yaml
kubectl -n kube-system rollout restart deployment metrics-server
```
**Verify if the metrics server is running**
```yaml
kubectl get pods -n kube-system
```
**To check the metrics**
```yaml
kubectl top nodes
```
# **For HPA activity**.
**In this demo, we will create an deployment & service files for Apache and with the help of HPA, we will automatically scale the number of pods based on CPU utilization.**

**To create directory**
```yaml
mkdir apache
cd apache
```

