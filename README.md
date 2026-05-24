# CIS602-HW3: Containerization Assignment

This repository contains a containerized Flask-Python web application built using Docker for CIS 602 Homework 3.

Two use cases are given as examples:
1. A standard **Web Stack** (Frontend + Backend + DB).
2. A **Microservices/Caching Stack** (API + Redis Cache).

---

## Technical Report

### 1. Importance of Docker in Modern Software Development

Docker has fundamentally changed modern software development and DevOps workflows by resolving the classic "it works on my machine" problem. By encapsulating an application, its system configuration, runtime environments, and binary dependencies into a single immutable image, Docker guarantees identical behavior across development machines, staging environments, and production servers.

* **Example 1 (Dependency Isolation):** A development team can run multiple isolated applications on the exact same host server—such as a legacy tool running on Python 2.7 alongside a modern microservice built on Python 3.12—without dealing with global system dependency conflicts.
* **Example 2 (CI/CD Optimization):** Automated continuous integration (CI/CD) pipelines can instantly spin up an exact, clean mirror of the target infrastructure to execute integration tests on new code commits, completely removing manual environment configuration mistakes.

### 2. How Docker Containers Differ from Virtual Machines

The structural difference between Docker containers and Virtual Machines (VMs) lies in their architectural approach to virtualization and how they utilize host hardware resources.

* **Example 1 (Hypervisor vs. Shared Kernel):** Virtual machines rely on a heavy Hypervisor layer to emulate physical hardware. Each VM must load a fully independent Guest Operating System (such as an entirely separate instance of Linux or Windows) to execute code, which easily consumes gigabytes of memory. Docker containers, alternatively, share the host system’s operating system kernel directly through Linux namespaces and cgroups, remaining highly lightweight and consuming minimal RAM.
* **Example 2 (Performance & Initialization Speed):** Because a virtual machine must boot an entire virtualized operating system from scratch, it often takes several minutes to initialize. A Docker container runs simply as a sandboxed process on the existing host kernel, allowing it to spin up or scale down almost instantly within milliseconds.

### 3. Use Case for Docker Compose: High-Traffic Caching Architecture

Docker Compose is an orchestration utility designed to define, launch, and manage multi-container application environments using a single, unified YAML configuration script instead of executing tedious individual terminal setup commands.

A clear, real-world example of Docker Compose is managing a **High-Traffic Web Application with a Caching Layer**. Imagine building a high-volume news website. If millions of users hit the site simultaneously during a breaking news story, pulling the article data from a traditional database every single time would crash the system. 

To solve this, developers run a two-part infrastructure stack:
1. **The Web Server (Flask):** A container that handles incoming web traffic from readers.
2. **The Cache Layer (Redis):** A super-fast, in-memory storage container. When the first user loads the breaking news story, the Flask container saves a copy of the text inside the Redis container. For the next million readers, Flask pulls the article instantly out of the fast cache instead of stressing the main database.

**Why Docker Compose is essential here:** The Flask app and the Redis cache are completely separate pieces of software. Docker Compose allows a developer to spin up both independent containers side-by-side using one command (`docker compose up`). Furthermore, Compose automatically sets up an isolated internal network so that the Flask container can securely talk to the caching container by simply using its container network name (e.g., connecting directly to `redis-cache`).

#### Project Directory Structure for this Use Case:
```text
├── high-traffic-app/
│   ├── backend-flask/
│   │   ├── app.py
│   │   └── Dockerfile
│   └── docker-compose.yml