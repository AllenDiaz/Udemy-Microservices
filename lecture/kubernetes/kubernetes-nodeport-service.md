```markdown
# Kubernetes Services — Creating a NodePort

## 🔑 Key Concepts
- **Service**: Kubernetes object that exposes Pods or connects them internally.
- **NodePort**: Simplest way to expose a Pod to the outside world.
  - Used mainly for **development/testing**.
  - Each node in the cluster opens a specific port and forwards traffic to the Pod.
- **Why NodePort First?**
  - **ClusterIP**: Only useful when multiple Pods need to communicate (not our case yet).
  - **LoadBalancer**: Requires extra cloud provider configuration.
  - **NodePort**: Easy to set up and immediately useful for exposing a single Pod.

---

## 📄 NodePort Service Config File

Example: `posts-srv.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: posts-srv
spec:
  type: NodePort
  selector:
    app: posts
  ports:
    - name: posts
      protocol: TCP
      port: 4000
      targetPort: 4000
```

### Explanation
- **apiVersion**: `v1` → standard Kubernetes objects.
- **kind**: `Service` → defines a service object.
- **metadata.name**: Identifier for the service (`posts-srv`).
- **spec.type**: `NodePort` → exposes the Pod externally.
- **selector**: Matches Pods with label `app: posts`.
  - Labels applied in the Deployment template allow Services to find Pods.
- **ports**:
  - **port**: Port exposed by the Service.
  - **targetPort**: Port inside the container where the app listens (`4000`).
  - **protocol**: TCP (default for most apps).
  - **name**: Arbitrary identifier for logging/debugging.

---

## ⚙️ Port Mapping Diagram

- **Browser (Client)** → request to **NodePort (Service Port)**  
- **Service Port** → forwards traffic to **TargetPort (Pod’s container port)**  
- **TargetPort** → application listens (e.g., `app.listen(4000)`)

> **Note**: `port` and `targetPort` can differ, but usually they are identical.

---

## 🖥️ Applying the Service
```bash
kubectl apply -f posts-srv.yaml
```
- Creates the NodePort service in the cluster.
- Service will route external traffic to the Pod running the application.

---

## 📚 Key Takeaways
- NodePort is the easiest service to start with for development.
- Services use **selectors** to find Pods by labels.
- `port` vs `targetPort`:
  - `port` → service’s external port.
  - `targetPort` → container’s internal listening port.
- For other apps (e.g., Comments service on port 4001), copy the config and adjust the port numbers.
```
