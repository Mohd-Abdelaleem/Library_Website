Library Website — DevOps CI/CD on Kubernetes (kind) 🚀

























A production-style DevOps project that delivers a Flask web app + database through a fully automated CI/CD pipeline using GitHub Actions, deployed on Kubernetes (kind), and observed with Prometheus + Grafana.



✅ Built to demonstrate real DevOps skills: testing, container builds, image publishing, Kubernetes deployment, rollout verification, monitoring, and rollback.



✨ Highlights



End-to-end CI/CD triggered on every push to main



Automated testing using pytest before any build/deploy



Dockerized services (backend + DB)



Kubernetes deployment (kind) to simulate production locally



Observability built-in: Prometheus scrapes metrics, Grafana dashboards visualize them



Rollback-ready: Kubernetes rolling updates + quick rollback commands



🧭 What This Pipeline Does



On every push to main, GitHub Actions automatically runs:



Checkout repository



Run unit tests (pytest)



Build Docker images (backend + db)



Push images to Docker Hub (or your registry)



Create a kind cluster



Apply Kubernetes manifests and deploy workloads



Deploy monitoring stack (Prometheus + Grafana)



Validate rollout (kubectl rollout status)



🏗 Architecture (High Level)



GitHub Actions (CI/CD)

|

|-- Unit Tests (pytest)

|-- Docker Build (backend + db)

|-- Push Images (registry)

|-- Create kind cluster

|-- Apply K8s manifests

|-- Deploy Monitoring



Kubernetes (kind)

|

|-- Flask Backend Deployment

|-- Database Deployment

|-- Services / ConfigMaps / Secrets / PVC

|-- Prometheus

|-- Grafana



🧰 Tech Stack



Backend: Flask (Python)



Testing: pytest



Containers: Docker



CI/CD: GitHub Actions



Orchestration: Kubernetes (kind)



Monitoring: Prometheus + Grafana



Version Control: Git \& GitHub



📁 Repository Layout



backend\_server/

├── app.py

├── requirements.txt

├── static/

├── templates/

└── tests/



db\_server/

├── Dockerfile

├── db\_app.py

├── requirements.txt

├── books.json

└── users.json



lib\_kube\_config/

└── Kubernetes manifests (\*.yaml)



.github/workflows/

└── ci.yml



Jenkinsfile

create\_backend\_image.sh

create\_db\_image.sh



⚡ Quick Start (Run Locally)

Backend only (local Python)



cd backend\_server

python -m venv venv



Linux/Mac:

source venv/bin/activate



Windows PowerShell:

.\\venv\\Scripts\\Activate.ps1



pip install -r requirements.txt

python app.py



Tip (Windows + CMD): use PowerShell for activation, or run:

.\\venv\\Scripts\\activate.bat



☸️ Deploy Locally on Kubernetes (kind)

Prerequisites



Docker



kubectl



kind



Create cluster



kind create cluster --name library-website

kubectl cluster-info



Deploy application manifests



kubectl apply -f lib\_kube\_config/



Verify rollout



kubectl get pods -A

kubectl rollout status deployment/<deployment-name>



If your manifests include namespaces, make sure you also check them:

kubectl get all -n <namespace>



📊 Monitoring (Prometheus + Grafana)

Get Grafana admin password



kubectl get secret monitoring-grafana -n monitoring

-o jsonpath="{.data.admin-password}" | base64 --decode



Username: admin



Password: output of the command above



If you expose Grafana via port-forward:



kubectl port-forward -n monitoring svc/monitoring-grafana 3000:80



Then open: http://localhost:3000



🔄 Rollback \& Recovery



Kubernetes makes rollbacks simple:



kubectl rollout undo deployment <deployment-name>

kubectl rollout history deployment <deployment-name>



This is useful if a new image tag introduces regression or a bad configuration.



🛡️ CI/CD Notes (For Recruiters / Reviewers)



Tests run before building and pushing images → prevents broken deployments.



Docker images are produced for both backend and DB services.



Kubernetes manifests represent a realistic deployment model (services, volumes, secrets).



Monitoring stack demonstrates operational readiness and observability principles.



🗺 Roadmap (Optional Improvements)



Add Helm chart for parameterized deployments



Add security scanning (Trivy / Snyk)



Add liveness/readiness probes for backend



Add Ingress controller + TLS locally



Add structured logging (ELK / Loki)



🧯 Troubleshooting

“LF will be replaced by CRLF”



This is normal on Windows. If you want to standardize:



git config --global core.autocrlf true



Push rejected (remote has commits)



Pull then push:



git pull origin main --allow-unrelated-histories

git push -u origin main



👤 Author



Mohammad Abdelaleem

DevOps Engineer

GitHub: https://github.com/Mohd-Abdelaleem

