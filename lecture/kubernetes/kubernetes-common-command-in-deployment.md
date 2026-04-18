# Kubernetes Deployments — Commands and Behavior

## 🔑 Key Concepts
- **Deployment**: Manages Pods, ensures desired state, handles updates, and provides self‑healing.
- **Pod Lifecycle under Deployment**:
  - If a Pod is deleted, the Deployment automatically recreates it.
  - Pods created by Deployments have unique names with generated suffixes.
- **Debugging**: Use `kubectl describe deployment` to view events and diagnose issues.

---

## 📊 Deployment Commands

| **Command**                          | **Purpose** |
|--------------------------------------|-------------|
| `kubectl get deployments`            | Lists all deployments in the cluster. Shows desired vs available Pods. |
| `kubectl get pods`                   | Lists Pods created by deployments. |
| `kubectl delete pod <pod-name>`      | Deletes a Pod. Deployment will recreate it automatically. |
| `kubectl describe deployment <name>` | Prints detailed info and events for debugging. |
| `kubectl apply -f <file>.yaml`       | Creates a deployment from a config file. |
| `kubectl delete deployment <name>`   | Deletes a deployment and all associated Pods. |

---

## ⚙️ Step-by-Step Examples

### 1. Create Deployment
```bash
kubectl apply -f posts-depl.yaml
