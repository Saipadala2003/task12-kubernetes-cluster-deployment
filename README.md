# Task 12: Kubernetes Cluster Setup and Deployment

**Course:** MSc Artificial Intelligence  
**Practical:** Kubernetes cluster setup and deployment using Minikube

## Overview

This repository contains starter manifests and command notes for deploying a sample web application to a local Kubernetes cluster with Minikube. The example uses the official NGINX container image and exposes it through a Kubernetes Service.

> **Important:** Run the commands and capture your own terminal/browser screenshots before submitting. The manifests are provided as a reproducible example; do not report a step as completed until you have run and verified it on your machine.

## Objectives

- Understand Kubernetes clusters, nodes, Pods, Deployments, and Services.
- Start a local Kubernetes cluster using Minikube.
- Deploy a container image using a declarative YAML manifest.
- Expose the Deployment through a Service.
- Inspect resources and verify application access.

## Repository structure

```text
task12-kubernetes-cluster-deployment/
├── README.md
├── commands.md
└── k8s/
    ├── deployment.yaml
    └── service.yaml
```

## Prerequisites

Install and verify:
- Docker Desktop (or another supported container runtime)
- kubectl
- Minikube

Check versions:

```powershell
docker --version
kubectl version --client
minikube version
```

## Deployment steps

### 1. Start Minikube

```powershell
minikube start --driver=docker
kubectl cluster-info
kubectl get nodes
```

If you use a different supported driver, replace `docker` with your configured driver.

### 2. Apply the Kubernetes manifests

From the repository root:

```powershell
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

### 3. Verify resources

```powershell
kubectl get deployments
kubectl get pods -o wide
kubectl get services
kubectl describe deployment task12-nginx
```

Wait until the Pod is in the `Running` state and the Deployment shows an available replica.

### 4. Open the application

```powershell
minikube service task12-nginx-service
```

Minikube should open the Service URL in a browser. Alternatively, use port forwarding:

```powershell
kubectl port-forward service/task12-nginx-service 8080:80
```

Then visit http://localhost:8080 while the port-forward command is running.

### 5. Clean up

```powershell
kubectl delete -f k8s/service.yaml
kubectl delete -f k8s/deployment.yaml
minikube stop
```

## Manifests

- `k8s/deployment.yaml`: defines one NGINX replica and its container port.
- `k8s/service.yaml`: exposes the Deployment using a NodePort Service.

## Evidence checklist

Add your own screenshots to the repository (for example, a `screenshots/` folder):
- [ ] Minikube running and `kubectl get nodes`
- [ ] Deployment and Pod status
- [ ] Service details
- [ ] NGINX page opened through the Minikube Service or port-forward URL
- [ ] Any required task-specific output

## Troubleshooting

- **Minikube will not start:** confirm Docker Desktop is running and try `minikube status`.
- **Pod is pending or failing:** inspect `kubectl describe pod <pod-name>` and `kubectl logs <pod-name>`.
- **Service is not reachable:** check `kubectl get endpoints`, confirm the Pod is ready, and use `minikube service task12-nginx-service` or port forwarding.
- **kubectl cannot connect:** run `kubectl config current-context` and `kubectl cluster-info`.

## Notes

This repository is a starter practical implementation. Add your actual command output, screenshots, and final report after validating the steps in your environment.
