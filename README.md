# Udemy-Microservices

A microservices learning project from a Udemy course — a simple **Blog Application** built with multiple independent services communicating via an Event Bus.

## Architecture

```
┌──────────┐    ┌──────────────┐    ┌─────────────┐
│  Client   │───▶│  Posts (4000) │───▶│             │
│ (React)   │    └──────────────┘    │  Event Bus  │
│  Port 3000│    ┌──────────────┐    │  (4005)     │
│           │───▶│Comments(4001)│───▶│             │
│           │    └──────────────┘    │             │
│           │    ┌──────────────┐    │             │
│           │◀──▶│ Query (4002) │◀──▶│             │
│           │    └──────────────┘    │             │
│           │    ┌──────────────┐    │             │
│           │    │Moderation    │◀──▶│             │
│           │    │  (4003)      │    └─────────────┘
└──────────┘    └──────────────┘
```

## Services

| Service | Port | Description |
|---------|------|-------------|
| **Posts** | 4000 | Creates new posts (`POST /posts`, `GET /posts`) |
| **Comments** | 4001 | Creates comments on posts (`POST /posts/:id/comments`, `GET /posts/:id/comments`) |
| **Query** | 4002 | Aggregates posts + comments into a read-optimized store; listens for events |
| **Moderation** | 4003 | Moderates comments (approves/rejects based on content); listens for `CommentCreated` events |
| **Event Bus** | 4005 | Simple event broker — receives events via `POST /events` and forwards to all services |
| **Client** | 3000 | React frontend for creating/viewing posts and comments |

## Event Flow

1. **Posts Service** emits `PostCreated` → Event Bus → Query Service
2. **Comments Service** emits `CommentCreated` → Event Bus → Moderation + Query
3. **Moderation Service** emits `CommentModerated` → Event Bus → Comments Service
4. **Comments Service** emits `CommentUpdated` (with status) → Event Bus → Query Service

The Event Bus stores all events in memory so services can replay missed events via `GET /events`.

## Kubernetes / Docker

Infrastructure configs are in `blog-boilerplate/infra/k8s/`:
- `posts-depl.yaml` — Deployment for the Posts service
- `post-srv.yaml` — NodePort Service exposing Posts on port 30040

The Posts service includes a `Dockerfile` for containerization.

## Lecture Notes

The `lecture/` folder contains study notes on:
- **Docker** — Commands, Dockerfiles, container execution
- **Kubernetes** — Pods, Deployments, Services (NodePort, ClusterIP), networking, common commands
- **Event Bus Design** — Architecture and patterns
- **Handling Service Downtime** — Strategies for missed events

## Getting Started

### Prerequisites
- Node.js (v14+)
- Docker Desktop (optional, for containerized runs)
- kubectl + minikube/Docker Desktop K8s (optional)

### Run Locally

```bash
# From blog-boilerplate directory, start each service:
cd posts && npm install && npm start
cd comments && npm install && npm start
cd query && npm install && npm start
cd moderation && npm install && npm start
cd event-bus && npm install && npm start
cd client && npm install && npm start
```

### Run with Kubernetes

```bash
cd blog-boilerplate/infra/k8s
kubectl apply -f posts-depl.yaml
kubectl apply -f post-srv.yaml
```

## Tech Stack

- **Frontend:** React, Axios
- **Backend:** Node.js, Express
- **Containerization:** Docker, Kubernetes
- **Communication:** Custom Event Bus (HTTP-based)