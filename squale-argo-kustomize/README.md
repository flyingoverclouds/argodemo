# Deploy Squale using ArgoCD using kustomize'd manifest

TODO

## ArgoCD Image Updater
Add annotation on app manifest : 

Update to the higher version (semantive ersion is defaut)
```yaml
argocd-image-updater.argoproj.io/image-list: acrargonc.azurecr.io/vintagecomputer/squale
```


Update to the higher 1.* version (semantive version is defaut)
```yaml
argocd-image-updater.argoproj.io/image-list: acrargonc.azurecr.io/vintagecomputer/squale:~1
```



