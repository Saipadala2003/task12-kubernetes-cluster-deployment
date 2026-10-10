# Task 12 - Kubernetes Cluster Setup and Deployment

## Objective
Understand Kubernetes orchestration and deploy the containerized Task 11 application using a local Minikube cluster.

## Application
The deployment uses the existing Task 11 container images:
- `task11-flask-api:latest`
- `task11-streamlit-ui:latest`

The Flask deployment loads `deep_learning_model.h5` inside the API image. The Streamlit deployment calls the Flask service through Kubernetes DNS using `http://flask-api:5000`.

## Architecture
```text
Browser
   |
   v
Minikube NodePort Service :30080
   |
   v
Streamlit Deployment / Pods :8501
   |
   | http://flask-api:5000
   v
Flask ClusterIP Service :5000
   |
   v
Flask Deployment / Pods
   |
   v
deep_learning_model.h5
```

## Prerequisites
- Docker Desktop running
- Minikube installed
- kubectl installed or `minikube kubectl --` available
- Task 11 images already built locally

## Setup
```powershell
minikube version
kubectl version --client
minikube start --driver=docker
kubectl get nodes
```

## Load Task 11 images into Minikube
```powershell
minikube image load task11-flask-api:latest
minikube image load task11-streamlit-ui:latest
minikube image ls | Select-String "task11-"
```

## Deploy
```powershell
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/flask-deployment.yaml
kubectl apply -f k8s/flask-service.yaml
kubectl apply -f k8s/streamlit-deployment.yaml
kubectl apply -f k8s/streamlit-service.yaml
```

## Verify
```powershell
kubectl get namespaces
kubectl get deployments -n task12
kubectl get pods -n task12 -o wide
kubectl get services -n task12
kubectl get endpoints -n task12
kubectl rollout status deployment/flask-api -n task12
kubectl rollout status deployment/streamlit-ui -n task12
```

## Access Streamlit
```powershell
minikube service streamlit-ui -n task12 --url
```

Open the returned URL in a browser. Upload the existing `7.png` test image and click **Predict Digit**.

## Test API
```powershell
kubectl exec -n task12 deployment/flask-api -- python -c "import urllib.request; print(urllib.request.urlopen('http://127.0.0.1:5000/health').read().decode())"
```

Test the API service from the Streamlit pod:
```powershell
kubectl exec -n task12 deployment/streamlit-ui -- python -c "import urllib.request; print(urllib.request.urlopen('http://flask-api:5000/health').read().decode())"
```

## Monitoring
```powershell
kubectl get pods -n task12 -w
kubectl describe deployment flask-api -n task12
kubectl describe pod -n task12 -l app=flask-api
kubectl logs -n task12 deployment/flask-api --tail=100
kubectl logs -n task12 deployment/streamlit-ui --tail=100
kubectl top pods -n task12
```

If `kubectl top` reports that Metrics Server is unavailable, this is expected until Minikube metrics are enabled. The core deployment can still be verified using `kubectl get`, `describe`, and logs.

## Cleanup
```powershell
kubectl delete namespace task12
minikube stop
```

## Evidence Sequence
1. `01_minikube_version.png`
2. `02_minikube_start.png`
3. `03_kubectl_nodes.png`
4. `04_images_loaded.png`
5. `05_manifests_applied.png`
6. `06_pods_services.png`
7. `07_streamlit_browser.png`
8. `08_prediction_result.png`
9. `09_api_health.png`
10. `10_service_dns_test.png`
11. `11_logs.png`
12. `12_rollout_status.png`

Author: Saikumar Padala  
Course: MSc Artificial Intelligence
