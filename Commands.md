Some useful Azure commands

Create a resource group

```
az group create -n jgp-SplitAuthDemoRG -l "UK South"
```

Create an app service plan on a basic S1 tier

```
az appservice plan create --name SplitAuthDemoPlan --resource-group jgp-SplitAuthDemoRG --is-linux --sku S1
```

Create a Web App supporting docker-compose
```
az webapp create --name SplitAuthDemoApp --resource-group jgp-SplitAuthDemoRG --plan SplitAuthDemoPlan --multicontainer-config-type compose --multicontainer-config-file docker-compose.yml
```

# Create an AzureAD App Registration

```
az ad app create --display-name jgp-SplitAuthApp \
  --available-to-other-tenants false \
  --reply-urls https://SplitAuthDemoApp.azurewebsites.net https://localhost:8080
  --
```

Will respond with a JSON document containing some crucial information;
- `appId`
- ...?