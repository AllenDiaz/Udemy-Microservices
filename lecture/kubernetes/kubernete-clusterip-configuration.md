```markdown
# Kubernetes Services — ClusterIP for Posts and Event Bus

## 🔑 Key Concepts
- **ClusterIP**: Default service type in Kubernetes.
  - Exposes Pods **only within the cluster**.
  - Provides stable internal endpoints for Pod-to-Pod communication.
- **Why ClusterIP?**
  - Pods have dynamic IPs → direct communication is unreliable.
  - ClusterIP ensures consistent networking between services.

---

## 📄 Architecture Overview

- **Posts Pod**: Handles post creation.
- **Event Bus Pod**: Broadcasts events to other services.
- **Challenge**: Pods cannot reliably communicate directly due to dynamic IPs.
- **Solution**: Create **ClusterIP services** for both Posts and Event Bus.

### Communication Flow
- Posts → Event Bus Service → Event Bus Pod
- Event Bus → Posts Service → Posts Pod

---

## ⚙️ Implementation Steps

### 1. Event Bus Deployment + ClusterIP Service
- Co-locate Deployment and Service in one YAML file.
- Use `---` to separate multiple objects.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: eventbus-srv
spec:
  selector:
    app: event-bus
  ports:
    - name: eventbus
      protocol: TCP
      port: 4005
      targetPort: 4005
```

- **selector**: Matches Pods labeled `app: event-bus`.
- **ports**: Routes traffic to container’s listening port (`4005`).

### 2. Posts Deployment + ClusterIP Service
- Add ClusterIP service to Posts deployment file.
- Use a **different name** than the existing NodePort service.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: posts-clusterip-srv
spec:
  selector:
    app: posts
  ports:
    - name: posts
      protocol: TCP
      port: 4000
      targetPort: 4000
```

- **selector**: Matches Pods labeled `app: posts`.
- **ports**: Routes traffic to container’s listening port (`4000`).

---

## 🖥️ Commands

- Apply configuration:
  ```bash
  kubectl apply -f eventbus-depl.yaml
  kubectl apply -f posts-depl.yaml
  ```
- List services:
  ```bash
  kubectl get services
  ```
  → Shows `eventbus-srv` and `posts-clusterip-srv` with type `ClusterIP`.

---

## 📚 Key Takeaways
- ClusterIP services are essential for **internal Pod communication**.
- Co-locating Deployment and Service configs is common practice (1:1 mapping).
- Always use **unique service names** to avoid conflicts.
- With ClusterIP in place, Posts and Event Bus can communicate reliably inside the cluster.
