## BUILD 'Squale' image container
```sh
docker build -t squale:latest .
```

## RUN 'squale' container locally
```sh
docker run -p 8080:8007 --name aSqualeIsRunning squale
```

Test locally by browsing to http://localhost:8080 ( you should see a nice picture of the squale computer )

## PUSH image to Azure Container Registry
*Replace **YOUR_ACR_NAME_HERE** with your Azure Container Registry name*

Add a tag to the image that target your acr
```sh
docker tag squale:latest YOUR_ACR_NAME_HERE.azurecr.io/vintagecomputer/squale:latest
```

Login to your ACR 
```sh
az acr login --name YOUR_ACR_NAME_HERE
```

then push the image to your ACR
```sh
docker push YOUR_ACR_NAME_HERE.azurecr.io/vintagecomputer/squale:latest
```

You can know use your locally built container image in AKS or ACA.




