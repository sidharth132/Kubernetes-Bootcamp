## Create image
` docker build --no-cache --platform=linux/amd64 -t ttl.sh/kubesimplify/app:10h .
`
kubectl get nodes -o json | grep architecture # for architecture check 

docker buildx build --platform linux/arm64 -t sidharthkr175/app:latest --push .

docker buildx imagetools inspect sidharthkr175/app:latest | Select-String Platform  # check Platform for powershell 

docker buildx imagetools inspect sidharthkr175/app:latest | grep Platform # check platform for bash

## Install cloudnative PG
```
kubectl apply --server-side -f \
  https://raw.githubusercontent.com/cloudnative-pg/cloudnative-pg/release-1.29/releases/cnpg-1.29.0.yaml   # install custom resource 
```
## create Database cluster
`kubectl apply -f postgres-cluster.yaml`
## Create secret 
```
kubectl create secret generic my-postgresql-credentials --from-literal=password='new_password'  --from-literal=username='goals_user'  --dry-run=client -o yaml | kubectl apply -f -
```

## Exec into pod to create table

```
kubectl exec -it my-postgresql-1 -- psql -U postgres -c "ALTER USER goals_user WITH PASSWORD 'new_password';"
kubectl port-forward my-postgresql-1 5432:5432
PGPASSWORD='new_password' psql -h 127.0.0.1 -U goals_user -d goals_database -c "

CREATE TABLE goals (
    id SERIAL PRIMARY KEY,
    goal_name VARCHAR(255) NOT NULL
);
"
```


===============================================
## Install CERT MANAGER
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.14.5/cert-manager.yaml

## Install nginx ingress controller 
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.4/deploy/static/provider/cloud/deploy.yaml

## Install Metrics server
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

===============================================

# GithubActions and ArgoCD

## Steps 
Create .github/workflows folder 
Create a file build-push-image.yaml 
Create a jinja template app/tmpl/deploy.j2
Create deployment file - /app/deploy/deploy.yaml
Create GitHub Actions secret - DOCKERHUB_USERNAME and DOCKERHUB_PASSWORD
Make sure your actions have push access as well. 


## Install ArgoCd
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
kubectl get secret -n argocd argocd-initial-admin-secret -oyaml

```
ip.nip.io || ip.xip.io # wildcard domain
