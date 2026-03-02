## Kubernetes Lesson Summary: Creating Your First Pod with a Config File

This section breaks down the full process of writing your first Kubernetes configuration file, building the container image, and applying the config to create a pod inside your cluster.

---

## 🧱 Goal of the Lesson
Create **one standalone pod** (no Deployment, no Service) that runs the **posts** container image you built locally.

This is your first hands‑on example of how Kubernetes uses configuration files to create objects inside the cluster.

---

## 🐳 Step 1 — Rebuild the Docker Image
You rebuild the image to attach a new label/version that will be referenced in the Kubernetes config.

**Command:**
```bash
docker build -t <your-docker-id>/posts:0.0.1 .
```

**Key points:**
- The `:0.0.1` tag uniquely identifies this version.
- The `.` tells Docker to build from the current directory.
- Kubernetes will later pull this exact image name and tag.

---

## 📁 Step 2 — Create the Infrastructure Folder Structure
Inside your project:

- Create an `infra/` directory → holds all deployment-related configuration.
- Inside it, create a `k8s/` directory → holds Kubernetes YAML files.
- Inside `k8s/`, create a file named **posts.yaml**.

This organizes your Kubernetes configs cleanly for future scaling.

---

## 📄 Step 3 — Write the Pod Configuration File
This YAML file defines a **Pod** object and the container it should run.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: posts
spec:
  containers:
    - name: posts
      image: <your-docker-id>/posts:0.0.1
```

### Important details:
- `apiVersion: v1` — Pods belong to the core API group.
- `kind: Pod` — Tells Kubernetes what object to create.
- `metadata.name` — The pod’s name inside the cluster.
- `spec.containers` — A list of containers inside the pod.
- Indentation **must be exact** (YAML is whitespace-sensitive).

If indentation is wrong, Kubernetes will reject the file.

---

## 🚀 Step 4 — Apply the Config to Create the Pod
Navigate into the `infra/k8s` directory and run:

```bash
kubectl apply -f posts.yaml
```

### What this does:
- Sends your YAML file to Kubernetes.
- Kubernetes reads the config and creates the pod.
- If you see an error, it’s almost always:
  - A YAML typo, or
  - Kubernetes is not running.

---

## 🔍 Step 5 — Inspect the Pod
To list all pods running in your cluster:

```bash
kubectl get pods
```

You should see something like:

```
NAME     READY   STATUS    RESTARTS   AGE
posts    1/1     Running   0          50s
```

### What this output tells you:
- **NAME** — The pod is named `posts`.
- **READY** — 1 container is running and ready.
- **STATUS** — The pod is successfully running.
- **RESTARTS** — How many times Kubernetes restarted it.
- **AGE** — How long ago it was created.

---

## 🧠 Key Concepts Reinforced
- Kubernetes objects are created using **YAML config files**.
- A **Pod** is the smallest deployable unit and can run one or more containers.
- `kubectl apply -f` is how you submit configs to Kubernetes.
- `kubectl get pods` lets you inspect what’s running.
- YAML indentation is critical—one wrong space breaks everything.
