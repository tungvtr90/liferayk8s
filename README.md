# liferayk8s
Desplegando Liferay en Kubernetes

Repositorio de código con el fin de servir los ficheros necesarios para realizar el how-to explicado en el blog: https://liferay.dev/blogs/-/blogs/desplegando-liferay-7-3-en-kubernetes

# Step:

## setup minikube
```
    minikube config set cpus 4
    minikube config set memory 10384
    minikube delete
    minikube start
```
## run elasticsearch, postgres
Run postgres
```
docker run -d -p 5432:5432 -v /home/tungvtr90/docker/pgdata:/var/lib/postgresql/data --name postgres -e POSTGRES_PASSWORD=postgres -d postgres:12 
```
Run elastic
```
kubectl apply -f search-deployment.yaml -n=liferay-prod
```
## Run deployment
```
    kubectl create namespace liferay-prod
    kubectl config set-context --current --namespace=liferay-prod
    kubectl apply -f liferay-deployment.yaml -n=liferay-prod
```
## copy config to pod
```
    kubectl cp ./files liferay-79d6bf4c75-p24fs:/mnt/liferay -n=liferay-prod -c liferay
```
restart pod

## Run ingress
enable ingress
```
minikube addons enable ingress
kubectl get pods -n ingress-nginx
```
start ingress:
```
    kubectl apply -f nginx-ingress.yaml -n=liferay-prod
    kubectl get ingress -n=liferay-prod
```
edit hosts file to expose port:
```
    sudo nano /etc/hosts
    add:
    127.0.0.1 liferay.kubernetes.com (do not use minikube IP)
```
Run:
```
minikube tunnel
```

## scale to 2 pod
## Result
Open chrome and hit: liferay.kubernetes.com
