# CIS602-HW3: Containerization Assignment

This repository contains a containerized Flask-Python web application built using Docker for CIS 602 Homework 3.

Two use cases are given as examples:
1. A standard **Web Stack** (Frontend + Backend + DB).
2. A **Microservices/Caching Stack** (API + Redis Cache).

---

## Technical Report

### 1. Importance of Docker in Modern Software Development
Docker has fundamentally changed modern software development and DevOps workflows by resolving the classic "it works on my machine" problem. By encapsulating an application, its system configuration, runtime environments, and binary dependencies into a single immutable image, Docker guarantees identical behavior across development machines, staging environments, and production servers.

* **Example 1 (Dependency Isolation):** A development team can run multiple isolated applications on the exact same host server—such as a legacy tool running on Python 2.7 alongside a modern microservice built on Python 3.12—without dealing with global system dependency conflicts or breaking system-wide packages.
* **Example 2 (CI/CD Optimization):** Automated continuous integration (CI/CD) pipelines can instantly spin up an exact, clean mirror of the target infrastructure to execute integration tests on new code commits. This eliminates configuration drift, speeds up production deployment pipelines, and removes manual deployment friction entirely.

### 2. How Docker Containers Differ from Virtual Machines
The structural difference between Docker containers and Virtual Machines (VMs) lies in their architectural approach to virtualization and how they utilize host hardware resources.

* **Example 1 (Hypervisor vs. Shared Kernel):** Virtual machines rely on a heavy Hypervisor layer to emulate physical hardware. Each VM must load a fully independent Guest Operating System to execute code, which easily consumes gigabytes of memory and duplicates system background tasks. Docker containers, alternatively, share the host system’s operating system kernel directly through Linux namespaces and cgroups, remaining highly lightweight and consuming minimal background RAM.
* **Example 2 (Performance & Initialization Speed):** Because a virtual machine must boot an entire virtualized operating system from scratch, it often takes several minutes to initialize and prepare services. A Docker container runs simply as a sandboxed process on the existing host kernel, allowing it to spin up or scale down almost instantly within milliseconds to handle sudden traffic spikes.

### 3. Use Cases for Docker Compose

#### Use Case 1: Standard Web Application Stack
A standard software configuration requires an isolated frontend interface, a backend microservice API, and a database layer to store persistent data. Docker Compose links all three dependent platforms using one simple configuration script, managing their startup sequence automatically.
```text
├── web-stack-app/
│   ├── frontend/
│   │   └── Dockerfile
│   ├── backend/
│   │   └── Dockerfile
│   └── docker-compose.yml