# kubernetes-Deploy

## Build Docker Image
```
docker build -t hello-world-nginx -f docker/Dockerfile .
```

## Create a Kind Cluster
```
kind create cluster --name hello-world-cluster
```
**Check if the cluster is running:**

```
kubectl get nodes
```

## Load Docker Image into Kind
```
kind load docker-image hello-world-nginx --name hello-world-cluster
```
## Apply Kubernetes Manifests
```
kubectl apply -f k8s/
```

## Verify Deployment and Service

```
kubectl get pods
kubectl get services
```

## Option 1: Port Forwarding
```
kubectl port-forward service/hello-world-service 8080:80
```
**then access at :**
`http://localhost:8080`

## Option 2: Using Kind Node IP
**Get the Kind container IP:**
```
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' hello-world-cluster-control-plane
```
**access the application using curl with the obtained IP:**
`curl http://<KIND_IP>:30007
`