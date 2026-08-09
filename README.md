# Kubernetes & Ansible Automation Setup

A containerized Python Flask application deployed on a local Kubernetes cluster,
with server configuration automated using Ansible.

## Tools Used
- Docker — containerization
- Kubernetes (Docker Desktop / kind) — container orchestration
- Ansible — configuration management and automation
- Python (Flask) — sample application

## Project Structure
```
k8s-ansible-project/
├── app/
│   ├── app.py
│   └── requirements.txt
├── Dockerfile
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
├── ansible/
│   ├── inventory.ini
│   └── playbook.yml
└── README.md
```

## What This Project Demonstrates
1. **Containerization** — the Flask app is packaged into a Docker image.
2. **Kubernetes Deployment** — a Deployment manifest runs 2 replicas of the app,
   with liveness and readiness probes configured for health checking.
3. **Kubernetes Service** — a NodePort Service exposes the app so it can be
   accessed outside the cluster.
4. **Rolling Updates & Scaling** — demonstrated using `kubectl rollout` and
   `kubectl scale` commands.
5. **Ansible Automation** — a playbook automates server checks (Docker
   installation verification) and stages application files for deployment,
   simulating how configuration management would work on a real server.

## How to Run

### 1. Build the Docker image
```bash
docker build -t k8s-ansible-app:latest .
```

### 2. Deploy to Kubernetes
```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

### 3. Verify the deployment
```bash
kubectl get pods
kubectl get service k8s-ansible-app-service
```

### 4. Access the application
```
http://localhost:30080
```

### 5. Perform a rolling update
```bash
kubectl set image deployment/k8s-ansible-app k8s-ansible-app=k8s-ansible-app:v2
kubectl rollout status deployment/k8s-ansible-app
```

### 6. Scale the deployment
```bash
kubectl scale deployment/k8s-ansible-app --replicas=4
```

### 7. Run the Ansible playbook
```bash
ansible-playbook -i ansible/inventory.ini ansible/playbook.yml
```

## Health Checks
The application exposes a `/health` endpoint used by Kubernetes liveness and
readiness probes to automatically detect and recover from failures.

## Live Proof
- Docker Hub Image: https://hub.docker.com/r/subash309206/k8s-ansible-app