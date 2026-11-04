## Taints & Tolerations
**What is Taints & Tolerations:**
- **Taint**: It is way to tell your Kubernetes cluster to not running a pods on this particular Nodes.
- **Tolerations**: If the particular nodes are Taint still your running the pods into that taint nodes

## Implementation of Taints and Tolerations:
- Taint a Node
```yaml
kubectl taint node name_of_worker_node prod=true:NoSchedule
```
