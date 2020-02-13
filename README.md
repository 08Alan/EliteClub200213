# App Service

> Demo 0 : App Service Plan & Scale

> Demo 1 : Upload app file
> Demo 2 : Auto Swap
> Demo 2 : Visual Studio deploy app
    - Config
    - Extension

> Demo 3 : Kudo

> Demo 4 : API Management
![apim with apigateway](CheatSheet/2020-02-13-03-21-07.png)

> Demo 5 : Azure DevOps with App Service
    - Version Control System(VCS)
    - Continuous integration
    - Continuous Deployment

> Demo 6 : App Service Best Practices

# Container Service

> Demo 0 : Azure Kubernetes Engine
# Demo AKS_Engine

## Create Service Principal
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/6500a4b3-d5f9-4ef4-a764-5d6360780b0b/resourceGroups/AKS_Engine_Demo"

##Generate ARM Template
./aks-engine generate --api-model Kubernetes.json

## Deployment Azure Resource by ARM Template
az group deployment create --name=aksengine --resource-group=AKS_Engine_Demo --template-file=azuredeploy.json --parameters=azuredeploy.parameters.json

<!--
kubectl run nginx --image nginx
kubectl expose deployment nginx --port=80
kubectl get pods
kubectl edit svc/nginx
kubectl get services
-->

> Demo 1 : ACR + AKS
## Create Resource Group
`az group create -l eastus --name ACSDemo`
## Clone App
`git clone https://github.com/Azure-Samples/azure-voting-app-redis.git`
`cd azure-voting-app-redis`
## Docker compose for defining and running multi-container
`Docker-compose up -d`
`Docker ps`
[http://localhost:8080/](http://localhost:8080/)

## Create ACR (Azure Container Registry)
`az acr create --resource-group ACSDemo --name ACSDemoACR --sku Basic`
## ACR login
`az acr login --name ACSDemoACR`
`docker login -u ACSDemoACR -p lwk/OjRd1w2pY4D3SlLvgxxxxxxxxxx acsdemoacr.azurecr.io`
`az acr list --resource-group ACSDemo --query "[].{acrLoginServer:loginServer}" --output table`
`docker tag azure-vote-front acsdemoacr.azurecr.io/azure-vote-front:v1`
`docker push acsdemoacr.azurecr.io/azure-vote-front:v1`
`az acr repository list --name ACSDemoACR --output table`

`az aks create --resource-group ACSDemo --name AKSCluster --node-count 1 --generate-ssh-keys`
`az aks get-credentials --resource-group ACSDemo --name AKSCluster`
`kubectl get nodes`

`CLIENT_ID=$(az aks show --resource-group ACSDemo --name AKSCluster --query "servicePrincipalProfile.clientId" --output tsv)`
`ACR_ID=$(az acr show --name ACSDemoACR --resource-group ACSDemo --query "id" --output tsv)`
`az role assignment create --assignee $CLIENT_ID --role Reader --scope $ACR_ID`
## 修改azure-vote-all-in-one-redis.yaml
`acsdemoacr.azurecr.io/azure-vote-front:v1`

`kubectl apply -f azure-vote-all-in-one-redis.yaml`
`kubectl get service`

> Demo 3  : AKS Best Practices
