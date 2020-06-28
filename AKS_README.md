### Steps to configure AKS (Microsoft Azure Kubernetes Engine)
Free account provides 170 EURO for 30 days. You will need to upgrade after period to continue with account.
#### Steps to create cluster
- interacting with azure cloud can be done through:
    - Azure Cloud Shell https://shell.azure.com/ (selection of bash or powershell)
    - Azure CLI (az) - https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest. Need to run `az login` after installation
- create kubernetes cluster
    - create resource group - `az group create --name myResourceGroup --location eastus`
    - create aks cluster - `az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys`
- accesing cluster is done through kubectl. Installing kubectl locally can be done with `az aks install-cli`
- get kube config by running `az aks get-credentials --resource-group myResourceGroup --name myAKSCluster`
- in case you have multiple contexts you need switch context with `kubectl config use-context myAKSCluster`
- check kube config is functional by running `kubectl cluster-info`
- rest of the configs are done using kubectl
