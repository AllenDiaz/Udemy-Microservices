```markdown
# Kubernetes Lesson Summary

## ✅ Verifying Kubernetes Installation
- Open a **new terminal window** (important step).
- Run the command:
  ```bash
  kubectl version
  ```
- Output should show **client version** and **server version**.
- Any version is fine; Kubernetes emphasizes **stability** over cutting-edge features.

---

## 🗂 Workflow Overview
### Step 1: Build Docker Image
- Start with **source code + Dockerfile**.
- Use Docker to create an **image** (blueprint).
- Containers are instances of this image.

### Step 2: Deploy to Kubernetes
- Kubernetes requires **images ready to go**.
- Create a **configuration file**:
  - Specify number of copies (e.g., 2 containers).
  - Define networking rules for accessibility.
- Apply config using:
  ```bash
  kubectl apply -f <config-file>
  ```

---

## 🖥️ Kubernetes Cluster & Nodes
- **Node** = virtual machine running containers.
- Local setup usually has **1 node**.
- Cloud deployments may have **multiple nodes**.
- Kubernetes distributes containers across nodes.

---

## 📦 Pods and Containers
- **Pod** = wrapper around a container.
- For this course: treat **pod ≈ container**.
- A pod can technically hold multiple containers.
- Example: Requesting 2 copies → Kubernetes creates **2 pods**, each with a container.

---

## 🔄 Deployment
- **Deployment** manages pods:
  - Ensures correct number of pods.
  - Automatically recreates pods if they crash.
- Reads instructions from the configuration file.

---

## 🌐 Service
- **Service** in Kubernetes ≠ microservice.
- Provides **network access** to pods.
- Abstracts away:
  - IP addresses
  - Ports
- Makes pods accessible via simple URLs (e.g., `post-service`).
- Example: Event bus can reach posts via the service instead of direct container IPs.

---

## 📌 Key Terminology
- **Image**: Blueprint for containers.
- **Container**: Instance of an image.
- **Pod**: Kubernetes wrapper around containers.
- **Deployment**: Controller ensuring pods run correctly.
- **Service**: Networking abstraction for accessing pods.

---

## 🎯 Practical Takeaways
- Always verify Kubernetes setup with `kubectl version`.
- Build Docker images before deploying to Kubernetes.
- Use configuration files to define deployments and services.
- Pods and containers are interchangeable in this course context.
- Services simplify networking between microservices.

---
```