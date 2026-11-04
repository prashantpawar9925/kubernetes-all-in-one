## Taints & Tolerations
**What is Taints & Tolerations:**
- **Taint**: It is way to tell your Kubernetes cluster to not running a pods on this particular Nodes.
- **Tolerations**: If the particular nodes are Taint still your running the pods into that taint nodes

## Implementation of Taints and Tolerations:
- Taint a Node
```yaml
kubectl taint node node1 prod=true:NoSchedule
```
``Note: The above command will taint the node1 with key "prod", without the appropriate tolerations no pods will schedule to node1.``
- To remove the taint , add - at the end of the command
```yaml
kubectl taint nodes node1 prod=true:NoSchedule-
```
