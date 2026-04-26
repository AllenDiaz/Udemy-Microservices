# Kubernetes — Load Balancer Service vs Ingress Controller

## Key Concepts
- **Load Balancer Service**
  - Kubernetes object that provisions a **cloud provider load balancer** (AWS, GCP, Azure).
  - Exists **outside the cluster**.
  - Directs external traffic into a **single pod** inside the cluster.

- **Ingress / Ingress Controller**
  - A **pod inside the cluster** with **routing rules**.
  - Works alongside the Load Balancer Service.
  - Routes traffic to the correct **ClusterIP service → pod** based on request path.
  - Simplifies client apps (e.g., React) by handling routing logic internally.

---

## Important Definitions
- **Load Balancer Service**: Tells Kubernetes to provision a load balancer with the cloud provider to forward traffic into the cluster.
- **Ingress Controller**: Pod that inspects incoming request paths and forwards traffic to the appropriate service/pod.

---

## Practical Examples
- **Without Ingress**  
  - Load Balancer Service forwards traffic to **one pod only**.  
  - Not sufficient when multiple pods (e.g., posts, comments, query) need traffic.

- **With Ingress**  
  - Load Balancer forwards traffic to the **Ingress Controller**.  
  - Ingress applies routing rules:  
    - `/posts` → Posts pod  
    - `/comments` → Comments pod  
    - `/query` → Query pod  

---

## Step-by-Step Process
1. **Create Load Balancer Service Config**  
   - Define YAML config for LoadBalancer Service.  
   - Apply with `kubectl apply`.

2. **Cluster Requests Cloud Provider**  
   - Kubernetes provisions a load balancer (AWS/GCP/Azure).  
   - Load balancer exists outside the cluster.

3. **Traffic Flow Without Ingress**  
   - External request → Load Balancer → Single Pod.

4. **Traffic Flow With Ingress**  
   - External request → Load Balancer → Ingress Controller → Correct ClusterIP Service → Target Pod.

---

## Key Takeaways
- **Load Balancer Service** = Entry point into the cluster.  
- **Ingress Controller** = Smart router inside the cluster.  
- Together, they enable scalable, flexible routing for multiple microservices.  
- Deep technical details are simplified here — focus on understanding the **relationship** between Load Balancer and Ingress.
