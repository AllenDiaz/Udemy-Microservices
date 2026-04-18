## Kubernetes Terminology Summary

A reference‑friendly overview of the core Kubernetes terms introduced in the lesson.

---

## 🧩 Kubernetes Cluster
A **cluster** is the full set of machines and control components that run your application.

- **Definition** — A collection of *nodes* managed by a *master/control plane*.
- **Purpose** — Provides the infrastructure where all containers and workloads run.
- **Scale** — Can have a single node (like local setups) or hundreds/thousands in production.

---

## 🖥️ Node
A **node** is a virtual machine (or physical machine) inside the cluster.

- **Role** — Runs the containers assigned to it.
- **Types** — Worker nodes (run workloads); master/control-plane nodes (manage the cluster).
- **Example** — Your local Kubernetes setup typically starts with **one node**, but production clusters scale to many.

---

## 📦 Pod
A **pod** is the smallest deployable unit in Kubernetes.

- **Definition** — A wrapper around one or more containers.
- **Course Simplification** — Treated as a **1:1 mapping** with containers for simplicity.
- **Reality** — Pods *can* contain multiple tightly coupled containers, but this course won’t use that pattern.

---

## 🔁 Deployment
A **deployment** manages and maintains a set of identical pods.

- **Purpose** — Ensures the desired number of pods are always running.
- **Self‑healing behavior** — If a pod crashes or stops, the deployment automatically replaces it.
- **Use case** — Running multiple replicas of the same application for reliability and scaling.

---

## 🌐 Service (Kubernetes Service)
A **service** provides stable networking and a consistent URL for accessing pods.

- **Definition** — A networking abstraction that exposes pods to other pods or external clients.
- **Why it matters** — Pods come and go; services give them a **permanent address**.
- **Potential confusion** — The term *service* is also used in microservices architecture.  
  - *Application service* → your program  
  - *Kubernetes service* → networking layer inside the cluster

---

## 📝 Key Takeaways
- A **cluster** contains nodes; **nodes** run pods; **pods** run containers.
- A **deployment** keeps pods healthy and consistent.
- A **service** makes pods discoverable and reachable.
- Expect the term **service** to be context‑dependent (Kubernetes vs. application).

