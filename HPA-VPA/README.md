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
To create a ``namespace.yml``

```yaml
kind: Namespace
apiVersion: v1
metadata:
 name: apache
```
To apply the namespace.yml
```yaml
kubectl apply -f namespace.yml
```

To create the ``deployment.yml`` file
```yaml
kind: Deployment
apiVersion: apps/v1
metadata:
  name: apache-deployment
  namespace: apache
spec:
  replica: 1
  selector:
    matchLabels:
      app: apache
  template:
    metadata:
     name: apache
     labels:
       app: apache

    spec: 
     containers:
     - name: apache
       image: http:latest
       ports:
       - containerPort: 80
       resources:
         requests:
           cpu: 100m
           memory: 128Mi
         limits:
           cpu: 200m
           memory: 256Mi
```

**To apply the deployment**
```yaml
kubectl apply -f deployment.yml
```
**To check the pod is running or not**
```yaml
kubectl get pods -n apache
```
To create ``service.yml`` file
```yaml
kind: Service
apiVersion: v1
metadata:
  name: apache-service
  namespace: apache
spec:
  selector:
      app: apache
  ports:
    - protocol: TCP
      port: 80        # Exposed port in the cluster
      targetPort: 80  # container port
  type: ClusterIP
  ```
To apply the service.yml file
```yaml
kubectl apply -f service.yml
```

```yaml
kubectl get all -n apache
```

**port forward to access the Apache service on browser.**
```yaml
kubectl port-forward service/apache-service -n apache 82:80 --address=0.0.0.0
````
**To Create HPA Resources**
- We will create HPA resources for both Apache deployments. The HPA will scale the number of pods based on CPU utilization

```yaml
kind: HorizontalPodAutoscaler
apiVersion: autoscaling/v2
metadata: 
  name: apache-hpa
  namespace: apache
spec:
  scaleTargetRef:
    kind: Deployment
    name: apache-deployment
    apiVersion: apps/v1

  minReplicas: 1
  maxReplicas: 5

  metrics:
   - type: Resource
     resource:
       name: cpu
       target:
         type: Utilization
         averageUtilization: 5
```
- To apply the HPA resources
```yaml
kubectl apply -f hpa.yml
```
- port forward to access the Apache service on browser.
```yaml  
kubectl port-forward service/apache-service -n apache 82:80 --address=0.0.0.0 &  #(continus running)
```
- To check the status of the HPA resources
```yaml
kubectl get hpa -n apache
```
`This will show you the current state of the HPA, including the current and desired number of replicas.`
