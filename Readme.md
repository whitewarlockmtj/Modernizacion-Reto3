#ArgoCD#
El Proyecto muestra un proceso de CI/CD utilizando ArgoCD, Kustomize y GitHub. 


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

4. Crear el repositorio

5. En el repositorio ingresar las credenciales del docker hub en Settings / Secrets And Variables / Actions, luego New Repository Secret.

6. En argocd, crear la conexión al repositorio, se dirigen a Settings / Repository, luego Connect Repo.
Seleccionar Via HTTPS
Selecctionar Type Git
Colocar un nombre 
Copiar la ruta del repositorio, Ejemplo: https://github.com/whitewarlockmtj/Modernizacion-Reto3.git
Seleccionar Connect

7. Ejecutar el despliegue
kubectl apply -f argocd-application.yaml

8. Ver en que puerto esta el servicio
kubectl get all -n default

9. Realizar el port forward para ver el servicio
kubectl port-forward svc/product-service -n default 3000:80

Con eso se puede validar el servicio


