# kargo demo
Manifests for example application composed of 3 different microservices, to demo kargo and its features.

- argo reconciliation repository.
- uses kustomize with base and overlays.
- uses ingress.

# Getting Started
We have 3 microservices
- auth
- products, and
- notifications

We have an ingress.
```
export $ARGOCD_PROJ = "kargo-demo"

$REPO_URL = https://github.com/agumbe-ai/kargo-demo-manifests.git
```

## Pre Requisites
- kubernetes cluster with kubcetl access.
- argo cd installed.
- kargo installed.

1. Git clone manifests repository, and change directory
   ```
   git clone $REPO_URL
   cd kargo-demo-manifests
   ```
2. Create Argo CD Project: $ARGOCD_PROJ
   ```
   kubectl apply -n argocd -f infra/k8s/templates/argo-project.yaml
   ```
3. Add $REPO_URL to $ARGOCD_PROJ
   ```
   argocd repo add https://github.com/agumbe-ai/kargo-demo-manifests.git --project kargo-demo
   ```
4. Create branch in the repo for each overlay (manual step)
   
5. Create Argo applications
   ```
   # Get argocd credentials
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo;
   
   # port forward argo cd
   kubectl port-forward svc/argocd-server -n argocd 8080:443
   
   # login
   argocd login localhost:8080 --username admin --grpc-web-root-path /kustomize build infra/k8s/auth/argo/overlays/test | argocd app create --file -
   ```