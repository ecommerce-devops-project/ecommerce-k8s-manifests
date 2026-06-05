# ecommerce-k8s-manifests
“Kubernetes manifests and deployment configurations for the e-commerce application, managed through GitOps with ArgoCD.”

# 🚀 Ecommerce DevOps Project

## 📌 Project Overview

Built and deployed a production-style Ecommerce application using Angular, Node.js, Docker, Kubernetes, Helm, GitHub Actions CI/CD, Prometheus, and Grafana.

The project demonstrates containerization, orchestration, automated deployment, auto-scaling, monitoring, and observability.

---

# 🏗️ Architecture

```text
Angular Frontend
        ↓
Node.js Backend
        ↓
Docker
        ↓
GitHub Actions CI/CD
        ↓
Docker Hub
        ↓
Kubernetes
        ↓
Helm
        ↓
Ingress
        ↓
PV / PVC
        ↓
Readiness Probe
        ↓
Liveness Probe
        ↓
HPA
        ↓
Prometheus
        ↓
Grafana
```

---

# ☸️ Kubernetes Concepts Covered

## 1. Pods

### Understanding
- Pod lifecycle
- Pod creation and deletion
- Pod troubleshooting
- Viewing logs

### Commands

```bash
kubectl get pods -n ecommerce

kubectl describe pod <pod-name> -n ecommerce

kubectl logs <pod-name> -n ecommerce

kubectl delete pod <pod-name> -n ecommerce
```

---

## 2. Deployments

### Understanding
- Rolling updates
- Replica management
- Deployment strategies
- Deployment rollout

### Commands

```bash
kubectl get deployments -n ecommerce

kubectl describe deployment ecommerce-backend -n ecommerce

kubectl rollout restart deployment ecommerce-backend -n ecommerce

kubectl rollout status deployment ecommerce-backend -n ecommerce
```

---

## 3. Services

### Understanding
- NodePort Services
- ClusterIP Services
- Service Discovery
- Internal and External communication

### Commands

```bash
kubectl get svc -n ecommerce

kubectl describe svc ecommerce-backend-service -n ecommerce
```

---

## 4. ConfigMaps

### Understanding
- Externalized configuration
- Environment-specific configuration
- Configuration management

### Commands

```bash
kubectl get configmaps -n ecommerce

kubectl describe configmap <configmap-name> -n ecommerce
```

---

## 5. Secrets

### Understanding
- Secure credential storage
- Managing sensitive data
- Injecting secrets into containers

### Commands

```bash
kubectl get secrets -n ecommerce

kubectl describe secret <secret-name> -n ecommerce
```

---

## 6. Persistent Volumes (PV)

### Understanding
- Persistent storage
- Data persistence
- Storage abstraction

### Commands

```bash
kubectl get pv

kubectl describe pv
```

---

## 7. Persistent Volume Claims (PVC)

### Understanding
- Storage requests
- Volume binding
- Persistent application data

### Commands

```bash
kubectl get pvc -n ecommerce

kubectl describe pvc ecommerce-backend-pvc -n ecommerce
```

---

## 8. Ingress

### Understanding
- Path-based routing
- Domain-based routing
- NGINX Ingress Controller

### Commands

```bash
minikube addons enable ingress

kubectl get ingress -n ecommerce

kubectl describe ingress ecommerce-ingress -n ecommerce
```

---

## 9. Helm

### Understanding
- Kubernetes package management
- Reusable templates
- Release management
- Environment-based deployments

### Commands

```bash
helm create ecommerce-chart

helm template ecommerce .

helm install ecommerce . -n ecommerce

helm upgrade ecommerce . -n ecommerce

helm list -n ecommerce

helm history ecommerce -n ecommerce

helm rollback ecommerce <revision> -n ecommerce
```

---

## 10. Readiness Probe

### Understanding

Readiness Probe checks whether the application is ready to receive traffic.

If the application is not ready:

- Kubernetes will not send traffic to the Pod
- Pod remains running
- Traffic starts only after successful health checks

### Verification

```bash
kubectl describe deployment ecommerce-backend -n ecommerce
```

Example:

```text
Readiness:
http-get http://:3000/health
```

---

## 11. Liveness Probe

### Understanding

Liveness Probe checks whether the application is alive.

If the application becomes unhealthy:

- Kubernetes automatically restarts the container
- Improves self-healing capability

### Verification

```bash
kubectl describe deployment ecommerce-backend -n ecommerce
```

Example:

```text
Liveness:
http-get http://:3000/health
```

---

## 12. Metrics Server

### Understanding
- Resource monitoring
- CPU and memory metrics
- Required for HPA

### Commands

```bash
minikube addons enable metrics-server

kubectl top pods -n ecommerce

kubectl top nodes
```

---

## 13. Horizontal Pod Autoscaler (HPA)

### Understanding

Implemented CPU-based auto-scaling.

Features:

- Automatic scale-up during high load
- Automatic scale-down during low load
- Dynamic pod management

### Commands

```bash
kubectl get hpa -n ecommerce

kubectl describe hpa ecommerce-backend-hpa -n ecommerce

kubectl top pods -n ecommerce
```

### Observed

- Pods automatically increased when CPU utilization exceeded threshold.
- Pods automatically reduced when traffic decreased.

---

## 14. Prometheus

### Understanding

Prometheus is used for collecting and storing metrics from Kubernetes resources.

Monitored:

- Pods
- Deployments
- Nodes
- Services
- Resource utilization

### Installation

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo update

kubectl create namespace monitoring

helm install prometheus prometheus-community/prometheus \
  --namespace monitoring
```

### Commands

```bash
kubectl get pods -n monitoring

kubectl get svc -n monitoring

kubectl port-forward -n monitoring svc/prometheus-server 9090:80
```

### Sample PromQL

```promql
up
```

Result:

```text
1 = Target is healthy
0 = Target is down
```

---

## 15. Grafana

### Understanding

Grafana is used for visualizing metrics collected by Prometheus.

Monitored:

- CPU Usage
- Memory Usage
- Pod Health
- Node Health
- Kubernetes Metrics

### Installation

```bash
helm repo add grafana https://grafana.github.io/helm-charts

helm repo update

helm install grafana grafana/grafana \
  --namespace monitoring
```

### Commands

```bash
kubectl get pods -n monitoring

kubectl get secret grafana -n monitoring \
-o jsonpath="{.data.admin-password}" | base64 --decode

kubectl port-forward -n monitoring svc/grafana 3000:80
```

### Access

```text
http://localhost:3000
```

---

# 🔄 CI/CD Pipeline

## GitHub Actions

Implemented CI/CD using GitHub Actions and Self-Hosted Runner.

### CI Process

```text
Code Push
    ↓
GitHub Actions
    ↓
Build Application
    ↓
Build Docker Image
    ↓
Push Image to Docker Hub
```

### CD Process

```text
Docker Image Updated
    ↓
Helm Upgrade
    ↓
Kubernetes Deployment Update
    ↓
Pods Restart Automatically
```

### Commands

```bash
cd ~/actions-runner

./run.sh
```

Trigger Pipeline:

```bash
git commit --allow-empty -m "Trigger CI/CD"

git push origin main
```

---

# 🔍 Frequently Used Commands

```bash
kubectl get all -n ecommerce

kubectl get pods -o wide -n ecommerce

kubectl get deployments -n ecommerce

kubectl get svc -n ecommerce

kubectl get ingress -n ecommerce

kubectl get pvc -n ecommerce

kubectl get hpa -n ecommerce

kubectl top pods -n ecommerce

kubectl logs <pod-name> -n ecommerce

kubectl describe deployment ecommerce-backend -n ecommerce

kubectl rollout restart deployment ecommerce-backend -n ecommerce

kubectl rollout restart deployment ecommers-frontend -n ecommerce
```

---

# 🎯 Key Learnings

- Kubernetes Architecture
- Pods and Deployments
- ReplicaSets
- Services
- ConfigMaps
- Secrets
- Persistent Storage
- Ingress Controller
- Helm Charts
- Readiness Probes
- Liveness Probes
- Auto Scaling (HPA)
- Metrics Server
- Prometheus Monitoring
- Grafana Dashboards
- GitHub Actions CI/CD
- Docker Hub Integration
- Self-Hosted Runner Setup
- Rolling Updates
- Self-Healing Applications
- Observability and Monitoring

---

# ✅ Project Outcome

Successfully built a production-style Ecommerce DevOps platform demonstrating:

- Containerization with Docker
- Kubernetes Orchestration
- Helm-based Deployments
- Automated CI/CD
- Health Monitoring
- Auto Scaling
- Persistent Storage
- Ingress Routing
- Metrics Collection
- Dashboard Visualization

This project simulates real-world DevOps practices used in modern cloud-native applications.
