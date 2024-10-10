# Install and test argoCD on Azure AKS   

## Creation K8S namespace
```sh
kubectl create namespace argocd
```

## ArgoCD deployment on AKS
```sh
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## check ArgoCD namespace content
```sh
kubectl get all -n argocd
```

## HACK : Expose ArgoCD Dashboard on a public IP :
__WARNING__ : NOT SECURE  (NO SSL AND PUBLIC EXPOSURE) ! FOR TESTING PURPOSE ONLY

### Convert ArgoCD Service to LoadBalancer type
```sh
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

### Get public IP of ArgoCD service
```sh
kubectl get services --namespace argocd argocd-server --output jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

## Get ArgoCD 'admin' user's password 
```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo 
```


## Access ArgoCD dashboard
Open a web browser, and navigate to the ArgoCD Dashboard public IP.
Enter the user 'admin' and  password

## Local proxying of argocd dashboard
This is the prefered method if you maint the dashboard private (service of type 'ClusterIP' )
```sh
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
Open a web browser, and navigate to 'http://localhost:8080'.
Enter the user 'admin' and base64 password .

## Use ArgoCD commandline 
```sh
argocd login 127.0.0.1:8080
```
Enter 'admin' user name and the password


## Creating the argo official demo app ( guestbook)
```sh
argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-server https://kubernetes.default.svc --dest-namespace default
```
The application is configued in argocd, BUT not deployed ('OutOfSync')

## Retrieve list of configured applications :
```sh
argocd app list
```

## Get details of a configured application
```sh
argocd app get guestbook
```

## Force app synchronisation. 

```sh
argocd app sync guestbook
```

Check deployed artifacts
```sh
kubectl get all -n default
```

## Deploying the SQUALE Application using a K8S manifest
```sh
argocd app create squale --repo https://github.com/flyingoverclouds/argodemo.git --path argosquale --dest-server  https://kubernetes.default.svc --dest-namespace default
argocd app sync squale
```

## Deploying the SQUALE Application using Kustomize
Deploy registry config file (update it to match you registry name/secret!!)

```sh
kubectl apply -f ./argocd-imageupdater-registries.yaml -n argocd

kubectl create secret generic argocd-image-updater-acr -n argocd --from-literal=acrcred=ACRUSERNAME:ACRPASSWORD

#kubectl create secret generic argocd-image-updater-acr -n argocd --from-literal=username=admin  --from-literal=password='REPLACE-WITH-YOUR-PWD' 
kubectl delete pod argocd-image-updater-REPLACE-WIT-POD-NAME -n argocd
```


## Deploying the SQUALE Application using Helm chart
```sh
TODO
```