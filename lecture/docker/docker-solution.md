# Lesson Summary: Introduction to Docker

## Key Concept
Docker is a tool that simplifies running applications by packaging them into **containers**—isolated environments that include everything needed to run a program.

---

## What is a Container?
- An isolated computing environment.
- Contains all dependencies required to run a single program.
- Each service in an application runs in its own container (e.g., events, posts, comments).
- Multiple instances of a service can be created by starting additional containers.

---

## Problems Docker Solves
1. **Environment Assumptions**
   - Running applications currently assumes tools like **Node.js** and **NPM** are installed locally.
   - These implicit requirements can cause issues when deploying to different environments.

2. **Startup Complexity**
   - Each service requires precise commands (e.g., `npm start`).
   - Managing multiple services becomes confusing and error-prone.

---

## How Docker Helps
- Wraps all dependencies (Node, NPM, etc.) inside the container.
- Includes instructions on how to start and run the program.
- Eliminates environment-specific assumptions.
- Makes running any program (not just Node.js) straightforward and consistent.

---

## Why It Matters
- Docker makes applications easier to run and deploy, especially in production.
- It pairs naturally with **Kubernetes**, which will be discussed in the next lesson.

---
