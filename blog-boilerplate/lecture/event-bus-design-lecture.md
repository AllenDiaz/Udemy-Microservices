
# 🚌 Event Bus Design (Lesson Summary)

## 🎯 Purpose
- Learn how an **event bus** enables communication between services by broadcasting events.
- Build a **simplified event bus from scratch** using **JavaScript + Express** to understand the fundamentals before moving to production-grade solutions.

## 🔑 Key Concepts
- **Event Bus**: Receives events and publishes them to listeners (other services).
- **Events**: Any piece of information (JSON, string, raw bytes, etc.) shared between services.
- **Listeners**: Services that subscribe to and react to events.

## 🛠 Existing Implementations
- Popular event buses: **RabbitMQ, Kafka, NATS**.
- Each has unique features that make certain tasks easier or harder.
- Choosing an event bus requires evaluating trade-offs (speed, complexity, features).

## 🧑‍💻 Our Approach
- Build a **basic Express-based event bus**:
  - Minimal features, purely educational.
  - Not suitable for production use.
- Later in the course: switch to a **production-grade event bus** used in large-scale deployments.

## ⚙️ How Our Event Bus Works
1. **Routes Setup**  
   - Add `/events` route to **Post Service**, **Comment Service**, and **Query Service**.
   - Each service listens for POST requests at `/events`.

2. **Emitting Events**  
   - When a service needs to emit an event, it sends a POST request to the event bus with event data.

3. **Broadcasting Events**  
   - Event bus receives the event.
   - It forwards the event via POST requests to all services (`localhost:4000/events`, `localhost:4001/events`, `localhost:4002/events`).
   - Even the original sender receives the event back.

4. **Result**  
   - Any service can emit an event → Event bus distributes it to **all services**.
   - Simple echo mechanism to demonstrate event flow.

## 📌 Takeaway
- This **simplistic event bus** helps visualize how services communicate.
- Real-world event buses add advanced features (filtering, persistence, scaling).
- Understanding the basics prepares you for working with **production-ready solutions** later.
