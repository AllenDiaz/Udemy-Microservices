# Kubernetes vs Docker Commands — Practical Equivalents

## 🔑 Key Concepts
- **Docker CLI vs kubectl**:  
  - Docker manages individual containers.  
  - Kubernetes manages groups of containers (Pods).  
  - Once using Kubernetes, `kubectl` replaces most Docker CLI usage.
- **Pod Management**: Pods wrap containers and are controlled via config files and `kubectl` commands.
- **Command Equivalence**: Many Docker commands have direct Kubernetes counterparts.

---

## 📊 Command Equivalents

| **Docker Command**        | **Kubernetes Equivalent**       | **Purpose** |
|----------------------------|---------------------------------|-------------|
| `docker ps`               | `kubectl get pods`              | List running containers/pods |
| `docker exec -it <id>`    | `kubectl exec -it <pod> <cmd>`  | Run a command inside a container/pod |
| `docker