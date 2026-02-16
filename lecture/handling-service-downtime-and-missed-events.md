# Handling Service Downtime and Missed Events

## The Problem

Microservices architecture faces two critical challenges:

1. **Service Downtime**: When a service goes down temporarily, it misses events that were emitted during its downtime
   - Example: Moderation service was down when a comment was created → comment stuck in "pending" state forever

2. **Late Service Creation**: When a new service is brought online after the system has been running
   - Example: Query service created one year after Posts and Comments services have been collecting data
   - How does the new service sync with existing data?

## Time Sequence Visualization

```
Time flows vertically ↓

Event Bus: ████████████████████  (always running)
Posts:     ████████████████████  (always running)
Comments:  ████████████████████  (always running)
Query:     ████████████████████  (always running)
Moderation: ████░░░░████████████  (red = downtime)
            ↑       ↑
         Event 1  Events 2 & 3 LOST!
```

## Solution Options

### Option 1: Synchronous Requests ❌

**Approach**: When a new service comes online, it makes direct HTTP requests to other services

```
Query Service → [GET all posts] → Posts Service
Query Service → [GET all comments] → Comments Service
```

**Downsides**:
- Falls back to synchronous communication (defeats microservices purpose)
- Requires implementing special "bulk fetch" endpoints in other services
- Creates tight coupling between services

---

### Option 2: Direct Database Access ❌

**Approach**: Give the new service direct access to other services' databases

```
Query Service → directly queries → Posts Database (MySQL)
Query Service → directly queries → Comments Database (MongoDB)
```

**Downsides**:
- Violates the "each service owns its database" principle
- Requires the new service to know implementation details of other databases
- Need to write code to interface with multiple database types

---

### Option 3: Event Store ✅ (Chosen Solution)

**Approach**: Event Bus stores ALL events it receives in a persistent data store

#### How It Works

1. **Event Storage**: Every event emitted is stored by the Event Bus
   ```
   Event 1 (PostCreated) → Event Bus → [stores] → continues to services
   Event 2 (CommentCreated) → Event Bus → [stores] → continues to services
   Event 3 (CommentModerated) → Event Bus → [stores] → continues to services
   ```

2. **Service Recovery/Initialization**: When a service comes online (new or after downtime):
   ```
   Query Service: "Give me all past events"
   Event Bus: "Here are events 1, 2, 3, 4, ..., N"
   Query Service: Processes all events and gets up to sync
   ```

3. **Handling Missed Events**: Service tracks last received event
   ```
   Moderation: "I was down. Last event I got was Event 1"
   Event Bus: "Here are Events 2 and 3 that you missed"
   ```

#### Advantages

- ✅ No additional code needed in services
- ✅ Reuses existing event handlers
- ✅ Maintains loose coupling
- ✅ Solves both downtime AND late service creation
- ✅ Services can replay events to rebuild state

#### Trade-offs

- Event store grows large over time (but storage is cheap)
- Event Bus becomes a critical component
- Need persistent storage (database) not just in-memory

## Key Takeaways

1. **Event Sourcing Pattern**: Storing all events creates an immutable log of everything that happened
2. **Replay Capability**: Services can rebuild their state by replaying events
3. **Resilience**: System can recover from temporary failures automatically
4. **Scalability**: New services can be added at any time without special migration code

## Implementation Strategy

```javascript
// Event Bus now stores events
app.post('/events', (req, res) => {
  const event = req.body;
  
  events.push(event); // Store event
  
  // Send to all services
  axios.post('http://posts:4000/events', event);
  axios.post('http://comments:4001/events', event);
  // ... etc
});

// New endpoint to get all past events
app.get('/events', (req, res) => {
  res.send(events);
});
```

---

**Cost Consideration**: Even with millions of events, cloud storage costs are minimal (e.g., ~$15/month for 100 million records)
