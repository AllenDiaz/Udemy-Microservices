

```markdown
# Updating Deployment Images in Kubernetes

## 🔑 Key Concepts
- **Deployments**: Manage Pods, ensure desired state, handle updates, and provide self‑healing.
- **Image Updates**: Two main approaches to updating container images in deployments:
  - **Method 1**: Specify exact version in the config file.
  - **Method 2**: Use `latest` tag and trigger rollout restarts.
- **Best Practice**: Method 2 is preferred in professional environments for efficiency and reduced risk of errors.

---

## ⚙️ Method 1 — Update via Config File

### Steps
1. **Make Code Changes**  
   - Edit application code (e.g., add `console.log("Version 20")`).

2. **Rebuild Docker Image**  
   ```bash
   docker build -t <docker-id>/posts:0.0.5 .
   ```
   - Tag with a new version (e.g., `0.0.5`).

3. **Update Deployment Config File**  
   ```yaml
   image: <docker-id>/posts:0.0.5
   ```

4. **Apply Updated Config**  
   ```bash
   kubectl apply -f posts-depl.yaml
   ```
   - Output: *“deployment configured”* (not *“created”*).

5. **Verify**  
   - `kubectl get deployments` → shows updated deployment.  
   - `kubectl get pods` → new Pod created.  
   - `kubectl logs <pod>` → confirms new version running.

### Limitations
- Requires **manual edits** to YAML for every update.
- Risk of typos or incorrect version numbers.
- Deployment files grow complex over time.
- Not common in professional workflows.

---

## ⚙️ Method 2 — Use `latest` Tag + Rollout Restart

### Steps
1. **Set Deployment to Use Latest**  
   - In config file, set image to `latest` or omit version:
     ```yaml
     image: <docker-id>/posts:latest
     ```

2. **Apply Config**  
   ```bash
   kubectl apply -f posts-depl.yaml
   ```

3. **Update Code & Rebuild Image**  
   - Make changes (e.g., `console.log("Version 55")`).  
   - Rebuild image:
     ```bash
     docker build -t <docker-id>/posts .
     ```

4. **Push Image to Docker Hub**  
   ```bash
   docker push <docker-id>/posts
   ```

5. **Restart Deployment**  
   ```bash
   kubectl rollout restart deployment posts-depl
   ```

6. **Verify**  
   - `kubectl get pods` → new Pod created with fresh age.  
   - `kubectl logs <pod>` → confirms latest version running.

### Advantages
- No need to edit YAML for every update.
- Reduces risk of typos or config errors.
- Streamlined workflow: rebuild → push → rollout restart.
- Preferred in professional environments.

---

## 📊 Comparison

| **Method** | **Process** | **Pros** | **Cons** |
|------------|-------------|----------|----------|
| **1. Config File Update** | Edit YAML → Apply | Explicit version control | Manual edits, error‑prone |
| **2. Latest + Rollout Restart** | Push latest image → Restart deployment | Faster, less error‑prone, no YAML edits | Relies on Docker Hub push |

---

## 📚 Key Takeaways
- **Method 1**: Good for learning, but inefficient for production.  
- **Method 2**: Recommended approach — always use `latest` and trigger rollout restarts.  
- Deployments ensure Pods are updated and self‑heal automatically.  
```
