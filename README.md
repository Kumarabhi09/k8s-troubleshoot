# k8s-troubleshoot

 Kubernetes Troubleshoot Toolkit

A comprehensive, multi-interface toolkit designed to identify and diagnose common Kubernetes pod issues such as CrashLoopBackOff, ImagePullBackOff, Error, Pending, and more.

# File: README.md

## 🔧 Kubernetes Troubleshoot Toolkit

A comprehensive, multi-interface toolkit designed to identify and diagnose common Kubernetes pod issues such as `CrashLoopBackOff`, `ImagePullBackOff`, `Error`, `Pending`, and more.

---

### 📁 Components

#### 1. **CLI Script (`k8s-troubleshoot.sh`)**
- Bash script using `kubectl` to:
  - List all pod statuses in a namespace.
  - Detect failure states like CrashLoopBackOff, ErrImagePull, etc.
  - Display `kubectl describe` and logs for each container in failing pods.

#### 2. **Dockerized CLI (`Dockerfile.k8s-troubleshoot`)**
- Wraps the CLI script inside a lightweight kubectl Docker image.

#### 3. **Web API with Flask (`app.py`)**
- REST endpoints:
  - `/troubleshoot/<namespace>` → JSON of failing pod details.
  - `/export/csv/<namespace>` → CSV download of failing pod info.
- Built using the official Kubernetes Python client.

#### 4. **Dockerized Web App (`Dockerfile.web`)**
- Lightweight Python-based Docker image for the Flask app.

#### 5. **Helm Chart (`helm-chart/`)**
- Deploys the web app via Helm.
- Exposes the Flask service within your cluster (default: ClusterIP).

#### 6. **GitHub Actions CI/CD (`.github/workflows/docker-ci.yml`)**
- On push to main:
  - Builds CLI & Web Docker images.
  - Pushes to DockerHub using provided secrets.

---

### 🚀 Use Cases
- Identify pods in trouble across any namespace.
- Quickly gather `describe` output and logs.
- Export reports for audit or support purposes.
- Provide a UI for less technical users (via API or integrated frontend).

---

### 🛠️ Requirements
- `kubectl` access to your cluster (for CLI).
- Kubernetes Python client installed (for API).
- DockerHub credentials for CI/CD.

---

### 📦 Future Enhancements
- Web UI with dashboard and filters.
- Email/Slack alerts on critical failures.
- In-cluster service discovery.

---

This toolkit saves time for SREs and DevOps teams by automating manual steps in debugging Kubernetes workloads.

---

# Existing project files follow below...

# File: k8s-troubleshoot.sh
#!/bin/bash

NAMESPACE=${1:-default}
