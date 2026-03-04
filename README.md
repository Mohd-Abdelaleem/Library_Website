# Library Website — DevOps CI/CD on Kubernetes (kind)

A DevOps-focused project that delivers a **Flask web application** and a **database service** through a complete **CI/CD pipeline** using **GitHub Actions**, deployed on **Kubernetes (kind)**, and monitored with **Prometheus + Grafana**.

> **Goal:** showcase an end-to-end delivery workflow (**test → build → publish → deploy → observe**) in a production-like Kubernetes environment without cloud dependencies.

---

## Contents

- [What you get](#what-you-get)
- [How the pipeline works](#how-the-pipeline-works)
- [Architecture](#architecture)
- [Tech stack](#tech-stack)
- [Repository structure](#repository-structure)
- [Run locally](#run-locally)
- [Deploy on kind](#deploy-on-kind)
- [Monitoring](#monitoring)
- [Rollback](#rollback)
- [Roadmap](#roadmap)
- [Author](#author)

---

## What you get

- CI/CD on every push to `main`
- Unit tests (`pytest`) as a quality gate
- Docker images for **backend** and **database**
- Kubernetes manifests for deployments/services/secrets/PV/PVC (as provided)
- Prometheus + Grafana monitoring stack
- Rollout verification and rollback commands

---

## How the pipeline works

On each push to `main`, GitHub Actions performs:

1. **Checkout** repository  
2. **Run tests** (`pytest`)  
3. **Build** Docker images (backend + database)  
4. **Push** images to a container registry (e.g., Docker Hub)  
5. **Create** a Kubernetes cluster using **kind**  
6. **Deploy** workloads using Kubernetes manifests  
7. **Deploy monitoring** (Prometheus + Grafana)  
8. **Verify** rollout health (`kubectl rollout status`)  

---

## Architecture

```text
GitHub Actions
  ├─ Test (pytest)
  ├─ Build images (Docker)
  ├─ Push images (Registry)
  ├─ Provision cluster (kind)
  ├─ Deploy app (Kubernetes manifests)
  └─ Deploy monitoring (Prometheus + Grafana)

kind Kubernetes cluster
  ├─ Backend (Flask)
  ├─ Database service
  ├─ Services / Secrets / PVC
  ├─ Prometheus
  └─ Grafana
```

---

## Tech stack

| Area | Tools |
|------|------|
| Backend | Flask (Python) |
| Testing | pytest |
| Containers | Docker |
| CI/CD | GitHub Actions |
| Orchestration | Kubernetes (kind) |
| Monitoring | Prometheus, Grafana |
| VCS | Git & GitHub |

---

## Repository structure

```text
backend_server/
  ├── app.py
  ├── requirements.txt
  ├── static/
  ├── templates/
  └── tests/

db_server/
  ├── Dockerfile
  ├── db_app.py
  ├── requirements.txt
  ├── books.json
  └── users.json

lib_kube_config/
  └── Kubernetes manifests (*.yaml)

.github/workflows/
  └── ci.yml

Jenkinsfile
create_backend_image.sh
create_db_image.sh
```

---

## Run locally

### Backend only

```bash
cd backend_server
python -m venv venv
```

**Activate venv**

- **Windows (PowerShell)**
```powershell
.
env\Scripts\Activate.ps1
```

- **Windows (CMD)**
```bat
.
env\Scripts ctivate.bat
```

- **Linux/Mac**
```bash
source venv/bin/activate
```

**Install and run**
```bash
pip install -r requirements.txt
python app.py
```

---

## Deploy on kind

### Prerequisites
- Docker
- kubectl
- kind

### Create cluster
```bash
kind create cluster --name library-website
kubectl cluster-info
```

### Deploy manifests
```bash
kubectl apply -f lib_kube_config/
```

### Verify rollout
```bash
kubectl get pods -A
kubectl rollout status deployment/<deployment-name>
```

> If your manifests use namespaces, check resources in that namespace:
> `kubectl get all -n <namespace>`

---

## Monitoring

### Grafana password

```bash
kubectl get secret monitoring-grafana -n monitoring   -o jsonpath="{.data.admin-password}" | base64 --decode
```

- **Username:** `admin`  
- **Password:** output of the command above  

### Access Grafana (port-forward)

```bash
kubectl port-forward -n monitoring svc/monitoring-grafana 3000:80
```

Open:
- `http://localhost:3000`

---

## Rollback

```bash
kubectl rollout undo deployment <deployment-name>
kubectl rollout history deployment <deployment-name>
```

---

## Roadmap

- [ ] Add Helm chart for parameterized deployments
- [ ] Add security scanning (Trivy / Snyk)
- [ ] Add liveness/readiness probes
- [ ] Add Ingress + TLS locally
- [ ] Add centralized logging (Loki / ELK)

---

## Author

**Mohammad Abdelaleem**  
DevOps Engineer  
GitHub: https://github.com/Mohd-Abdelaleem
