# kubernetes-Deploy

## Build Docker Image
```
docker build -t hello-world-nginx -f Docker/Dockerfile .
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

![Screenshot From 2025-04-25 22-33-13](https://github.com/user-attachments/assets/5a83be7d-ab3b-4238-bca1-84424959ba56)

## Option 2: Using Kind Node IP
**Get the Kind container IP:**
```
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' hello-world-cluster-control-plane
```
**access the application using curl with the obtained IP:**
`curl http://<KIND_IP>:30007
`

![Screenshot From 2025-04-25 22-37-40](https://github.com/user-attachments/assets/03852c51-1229-4d80-9194-124ec000f183)

![Screenshot From 2025-04-25 22-37-53](https://github.com/user-attachments/assets/facc2412-b77a-479e-b7f8-23c664fc6418)


## Setup & Install Argo CD

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods -n argocd
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
**then open** `https://localhost:8080`



## Login credentials:
```
# Get initial password for admin user
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d && echo
```

### Username: admin

### Password: (from above command)

## Apply Argo CD App to the Cluster

```
kubectl apply -f argocd/application.yaml -n argocd
```

![Screenshot From 2025-04-25 23-34-38](https://github.com/user-attachments/assets/1d2af469-8769-4dea-a33c-4fdd26712359)

