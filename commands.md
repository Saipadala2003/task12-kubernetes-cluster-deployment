# Kubernetes Practical Command Log

Use this as a command reference and record the actual output/date from your own system. Commands listed here are not proof of execution.

## Environment checks

```powershell
docker --version
kubectl version --client
minikube version
```

## Cluster setup and status

```powershell
minikube start --driver=docker
minikube status
kubectl config current-context
kubectl cluster-info
kubectl get nodes -o wide
```

## Deploy and inspect

```powershell
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl get deployments
kubectl get pods -o wide
kubectl get services
kubectl describe deployment task12-nginx
kubectl describe service task12-nginx-service
kubectl logs deployment/task12-nginx
```

## Access the service

```powershell
minikube service task12-nginx-service
```

Or in a separate terminal:

```powershell
kubectl port-forward service/task12-nginx-service 8080:80
```

Open http://localhost:8080 while port forwarding is active.

## Cleanup

```powershell
kubectl delete -f k8s/service.yaml
kubectl delete -f k8s/deployment.yaml
minikube stop
```

## Record your results

- Date/time executed:
- Minikube driver:
- Node status:
- Deployment ready replicas:
- Pod status:
- Service type / port:
- Browser verification:
- Issues and resolution:
