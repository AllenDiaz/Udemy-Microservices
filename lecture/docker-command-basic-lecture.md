# Basic Docker Commands Overview

This lesson provides a refresher on essential Docker commands that will be frequently used, with later variants applied in Kubernetes.

---

## 🔨 Building Images
- **Command:** `docker build -t <tag> .`
- **Purpose:** Builds a new image from the current directory.
- **Notes:**
  - `-t` flag tags the image (e.g., `docker build -t stephengrider/post .`).
  - Images can be referenced by **ID** or **tag**.

---

## 🚀 Running Containers
- **Command:** `docker run <image>`
- **Purpose:** Creates and starts a container from an image.
- **Variants:**
  - Override default command:  
    `docker run -it <image> <command>`
    - Example: `docker run -it stephengrider/post sh` → starts a shell inside the container instead of running `npm start`.

---

## 📋 Listing Containers
- **Command:** `docker ps`
- **Purpose:** Displays running containers with details:
  - Container ID
  - Image used
  - Command executed
  - Creation time
  - Status

---

## 🖥️ Executing Commands in Containers
- **Command:** `docker exec -it <container_id> <command>`
- **Purpose:** Runs arbitrary commands inside a running container.
- **Example:**  
  `docker exec -it <container_id> sh` → opens a shell inside the container.

---

## 📜 Viewing Logs
- **Command:** `docker logs <container_id>`
- **Purpose:** Shows logs emitted by the container’s primary process.
- **Use Case:** Helpful for debugging and monitoring processes like `npm start`.

---

## ✅ Key Takeaways
- **Build images** with `docker build`.
- **Run containers** with `docker run`.
- **Override commands** using `-it`.
- **Inspect running containers** with `docker ps`.
- **Execute commands inside containers** with `docker exec`.
- **View logs** with `docker logs`.

These commands form the foundation for working with Docker and will later be extended into Kubernetes workflows.
