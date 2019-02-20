
## Create a resource group

Create a resource group with the [az group create][az-group-create] command. An Azure resource group is a logical container into which Azure resources are deployed and managed.

The following example creates a resource group named *andyacrdemo* in the *eastus* location.

```azurecli
az group create --name andyacrdemo --location eastus
```

## Create a container registry

In this quickstart you create a *Basic* registry, which is a cost-optimized option for developers learning about Azure Container Registry. For details on available service tiers, see [Container registry SKUs][container-registry-skus].

Create an ACR instance using the [az acr create][az-acr-create] command. The registry name must be unique within Azure, and contain 5-50 alphanumeric characters. In the following example, *andyreg* is used. Update this to a unique value.

```azurecli
az acr create --resource-group andyacrdemo --name andyreg --sku Basic
```

## Log in to registry

Before pushing and pulling container images, you must log in to the registry. To do so, use the [az acr login][az-acr-login] command.

```azurecli
az acr login --name <acrName>
```

### Using Jib to build images in ACR
Please replace the pom.xml for each service using the related pom files under [jib-pom-fils](https://github.com/VSChina/PiggyMetrics/tree/andy-dev15/jib-pom-files)
