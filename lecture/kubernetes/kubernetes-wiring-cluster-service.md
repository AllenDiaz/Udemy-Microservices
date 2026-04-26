# Kubernetes Lesson: Wiring Up ClusterIP Services

## Key Concepts
- **ClusterIP Service**: Provides stable internal communication between pods in a Kubernetes cluster.
- **Service Name as URL**: Pods communicate via the service name instead of `localhost`.
- **Port Mapping**: Requests must specify the correct port exposed by the service.

## Important Definitions
- **ClusterIP**: Default Kubernetes service type that exposes a pod to other pods within the cluster.
- **Service Name Resolution**: Kubernetes DNS allows pods to reach services using their names (e.g., `http://event-bus-srv:4005`).

## Practical Examples
- **Posts → Event Bus Communication**:
  - Old approach: `http://localhost:4005`
  - New approach: `http://event-bus-srv:4005`
- **Event Bus → Posts Communication**:
  - Old approach: `http://localhost:4000`
  - New approach: `http://posts-clusterip-srv:4000`

## Step-by-Step Process
1. **Create ClusterIP Services**:
   - One for the **Event Bus** (`event-bus-srv` on port 4005).
   - One for the **Posts** (`posts-clusterip-srv` on port 4000).
2. **Update Application Code**:
   - Replace `localhost` references with the **service names**.
   - Example: In `index.js` of Posts project, change event bus URL to `http://event-bus-srv:4005`.
   - Example: In Event Bus project, change posts URL to `http://posts-clusterip-srv:4000`.
3. **Verify Service Names**:
   - Run `kubectl get services` to confirm exact service names.
4. **Rebuild and Redeploy**:
   - Rebuild Docker images with updated code.
   - Apply updated deployments to Kubernetes.

## Takeaway
- **Rule of Thumb**: For pod-to-pod communication, always:
  1. Create a **ClusterIP service** for the target pod.
  2. Use `http://<service-name>:<port>` in your code instead of `localhost`.

