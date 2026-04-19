```markdown
# Kubernetes Services — ClusterIP

## 🔑 Key Concepts
- **ClusterIP**: Default service type in Kubernetes.
  - Exposes Pods **only within the cluster**.
  - Enables Pod-to-Pod communication without relying on unpredictable Pod IPs.
- **Problem**: Pods are assigned dynamic IPs, making direct communication unreliable.
- **Solution**: Use ClusterIP services to provide a stable internal endpoint.

---

## 📄 Scenario: Posts ↔ Event Bus

- Current setup:
  - **Posts Pod**: Handles post creation.
  - **Event Bus Pod**: Broadcasts events to other services.
- Communication challenge:
  - Posts must send events to Event Bus.
  - Event Bus must send events back to Posts.
- Direct Pod-to-Pod communication is unreliable → requires **ClusterIP services**.

---

## ⚙️ Planned Architecture

1. **Deployments**
   - Create a Deployment for **Posts**.
   - Create a Deployment for **Event Bus**.

2. **ClusterIP Services**
   - **Posts Service**: Governs access to the Posts Pod.
   - **Event Bus Service**: Governs access to the Event Bus Pod.

3. **Communication Flow**
   - Posts → Event Bus Service → Event Bus Pod.
   - Event Bus → Posts Service → Posts Pod.

---

## 📄 Example Config (ClusterIP Service)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: eventbus-srv
spec:
  type: ClusterIP
  selector:
    app: eventbus
  ports:
    - name: eventbus
      protocol: TCP
      port: 4005
      targetPort: 4005
```

### Explanation
- **type: ClusterIP** → Internal-only service.
- **selector** → Matches Pods labeled `app: eventbus`.
- **ports**:
  - **port**: Service port exposed inside the cluster.
  - **targetPort**: Container’s listening port (e.g., `app.listen(4005)`).

---

## 📚 Key Takeaways
- ClusterIP is essential for **internal communication** between Pods.
- Provides a **stable endpoint** regardless of Pod IP changes.
- Each Pod that needs to communicate with others should have a **matching ClusterIP service**.
- In this example:
  - Posts and Event Bus communicate through their respective services.
  - This pattern scales as more Pods (e.g., Comments, Query) are added later.
```