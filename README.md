# Deploy an app to ACR and push to AKS
Attach ACR to AKS
```
az aks update -n <cluster name> -g <cluster resource group> --attach-acr <acr name without .azurecr.io>
```


The GitHub action depends on this secrets to created before running (more details [here](https://docs.microsoft.com/en-us/azure/aks/kubernetes-action#create-secrets)):

```
az ad sp create-for-rbac \
    --name "ghActionWeatherApp" \
    --scope /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP> \
    --role Contributor \
    --sdk-auth
```

| Secret name |	Secret value |
| ----------- | ------------ |
| AZURE_CREDENTIALS	| The entire JSON output from the az ad sp create-for-rbac command |
| service_principal	| The value of &lt;clientId&gt; |
| service_principal_password	| The value of &lt;clientSecret&gt; |
| subscription	| The value of &lt;subscriptionId&gt; |
| tenant	| The value of &lt;tenantId&gt; |
| registry	| The name of your registry |
| repository	| weather-app
| resource_group	| The name of your resource group |
| cluster_name	| The name of your cluster |

Update environment variables in main.yml
```
env:
  AZURE_CONTAINER_REGISTRY: <acr name>
  CONTAINER_NAME: "weather-app"
  RESOURCE_GROUP: <AKS cluster resource group>
  CLUSTER_NAME: <AKS cluster name>
  DEPLOYMENT_MANIFEST_PATH: 'deployment-weather-app.yaml'
```