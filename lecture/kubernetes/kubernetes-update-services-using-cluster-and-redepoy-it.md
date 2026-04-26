```markdown
# Kubernetes Lesson: Updating Service URLs and Redeploying

## Key Concepts
- **Service Name Usage**: Replace `localhost` references with Kubernetes service names for inter‑pod communication.
- **ClusterIP Services**: Each microservice (Posts, Event Bus, future Comments/Moderation/Query) requires its own ClusterIP service.
- **Deployment Updates**: Code changes require rebuilding Docker images and restarting deployments.

## Practical Applications
- **Posts → Event Bus**:
  - Old: `http://localhost:4005`
  - New: `http://event-bus-srv:4005`
- **Event Bus → Posts**:
  - Old: `http://localhost:4000`
  - New: `http://posts-clusterip-srv:4000`

## Step-by-Step Process

### 1. Update Code
- In **Posts `index.js`**:
  - Change requests from `localhost:4005` → `event-bus-srv:4005`.
- In **Event Bus `index.js`**:
  - Change requests from `localhost:4000` → `posts-clusterip-srv:4000`.
  - Temporarily comment out requests to unimplemented services (Comments, Moderation, Query).

### 2. Verify Service Names
- Run:
  ```bash
  kubectl get services
  ```
- Double‑check spelling and ports:
  - Event Bus → `event-bus-srv:4005`
  - Posts → `posts-clusterip-srv:4000`

### 3. Rebuild and Push Images
- Event Bus:
  ```bash
  docker build -t <docker-id>/event-bus .
  docker push <docker-id>/event-bus
  ```
- Posts:
  ```bash
  docker build -t <docker-id>/posts .
  docker push <docker-id>/posts
  ```

### 4. Restart Deployments
- Check deployment names:
  ```bash
  kubectl get deployments
  ```
- Restart:
  ```bash
  kubectl rollout restart deployment posts-depl
  kubectl rollout restart deployment event-bus-depl
  ```

### 5. Verify Pods
- Run:
  ```bash
  kubectl get pods
  ```
- Confirm old pods terminate and new pods start successfully.

### 6. Test Communication
- Use **Postman** to issue a POST request to the Posts service.
- Check logs of Posts and Event Bus to confirm events are exchanged.

## Takeaway
- Always replace `localhost` with **service names + ports** for Kubernetes communication.
- Every new microservice (Comments, Moderation, Query) will require:
  1. A ClusterIP service.
  2. Updated URLs in code.
  3. Rebuilt images and redeployed pods.
```
