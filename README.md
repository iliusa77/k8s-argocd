### Install ArgoCD from manifest

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Install ArgoCD cli
```
brew install argocd
```

### Reset password
```
argocd admin initial-password -n argocd
633ZW09owd948xE7
```

### Port-forward
```
kubectl port-forward svc/argocd-server 8080:443 -n argocd
```

### Deploy application (guestbook) via ArgoCD cli
#### login
```
argocd login 127.0.0.1:8080
WARNING: server certificate had error: tls: failed to verify certificate: x509: certificate signed by unknown authority. Proceed insecurely (y/n)? y
Username: admin
Password: 633ZW09owd948xE7

'admin:login' logged in successfully
Context '127.0.0.1:8080' updated
```
#### install application
```
kubectl config set-context --current --namespace=argocd
argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-server https://kubernetes.default.svc --dest-namespace default
```
#### check application
```
argocd app list  
NAME              CLUSTER                         NAMESPACE  PROJECT  STATUS     HEALTH   SYNCPOLICY  CONDITIONS  REPO                                                 PATH       TARGET
argocd/guestbook  https://kubernetes.default.svc  default    default  OutOfSync  Missing  <none>      <none>      https://github.com/argoproj/argocd-example-apps.git  guestbook
```
#### sync application
```
argocd app sync guestbook
```
#### re-check application
```
argocd app list          
NAME              CLUSTER                         NAMESPACE  PROJECT  STATUS  HEALTH   SYNCPOLICY  CONDITIONS  REPO                                                 PATH       TARGET
argocd/guestbook  https://kubernetes.default.svc  default    default  Synced  Healthy  <none>      <none>      https://github.com/argoproj/argocd-example-apps.git  guestbook
```

### Open in browser https://127.0.0.1:8080
- Username: admin
- Password: 633ZW09owd948xE7

### Create application (guestbook) in ArgoCD UI
- New app -> Create
- Application Name: guestbook
- Project Name: default
- SYNC POLICY: Automatic
  PRUNE RESOURCES +
  SELF HEAL +
  AUTO-CREATE NAMESPACE +
- Repository URL: https://github.com/iliusa77/k8s-argocd.git
- Path: applications/guestbook/manifests
- Cluster URL: https://kubernetes.default.svc
- Namespace: default

