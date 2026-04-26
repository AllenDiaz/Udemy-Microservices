# Kubernetes Lesson: Integrating the React Application with a Load Balancer

## Key Concepts
- **React Dev Server in Kubernetes**:
  - Runs inside a pod as a container.
  - Serves initial **HTML, CSS, and JavaScript** to the browser.
  - Does **not** directly communicate with other microservices (Posts, Comments, Query, Moderation).
  - All subsequent requests for data are made from the **browser** to backend services.

- **Service Communication**:
  - Browser → React Dev Server → HTML/CSS/JS delivered.
  - Browser React App → ClusterIP services (Posts, Comments, Query) via Load Balancer.

## Options for External Access
### Option 1: NodePort Services (❌ Not Recommended)
- Expose each microservice (Posts, Comments, Query, Moderation) via NodePort.
- Problems:
  - Random port assignment (unstable).
  - Requires updating React app code if ports change.
  - Not scalable or production-ready.

### Option 2: Load Balancer Service (✅ Recommended)
- **Single point of entry** into the cluster.
- Routes incoming requests to the correct ClusterIP service.
- Provides stable, production-ready access for the React app.
- Workflow:
  1. Browser requests → Load Balancer.
  2. Load Balancer forwards to appropriate ClusterIP service.
  3. Response flows back through Load Balancer → Browser.

## Step-by-Step Strategy
1. **Build Docker Image** for the React application (Create React App dev server).
2. **Create Deployment**:
   - Pod running the React dev server.
   - Container built from the React Docker image.
3. **Create Load Balancer Service**:
   - Acts as gateway for external traffic.
   - Routes requests to ClusterIP services for Posts, Comments, Query.
4. **Configure Routing Logic**:
   - Ensure requests to `/posts`, `/comments`, `/query` are forwarded correctly.
5. **Deploy to Cluster**:
   - Apply YAML configs for React deployment and Load Balancer.
   - Verify external access via browser.

## Takeaway
- The **React Dev Server** only serves static assets (HTML, CSS, JS).
- All **data requests** come from the browser, routed through the **Load Balancer**.
- **Best Practice**: Use a Load Balancer for a single, stable entry point into the cluster instead of multiple NodePorts.
