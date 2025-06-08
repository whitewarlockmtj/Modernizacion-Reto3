#ArgoCD#

1. Iniciar postgres
helm install postgresql bitnami/postgresql --set auth.postgresPassword=postgres --set auth.database=products --set persistence.size=20Gi

2. Activar ArgoCD
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update 

kubectl create namespace argocd
helm install argocd argo/argo-cd --namespace argocd --values argocd-application.yaml
kubectl port-forward svc/argocd-server -n argocd 8080:443

3. Obtener el password de argocd
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d



