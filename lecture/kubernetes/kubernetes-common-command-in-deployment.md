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
| `kubectl get pods`                   | Lists Pods created by deployments.