# Create kubernetes cluster using AKS and Terraform

### Create Azure account and install azure cli
- Create Azure account from [azure portal](https://azure.microsoft.com/en-us/free/) and create free account.
- Create an Azure service principal: You'll need to create an Azure service principal with contributor access to your subscription. You can follow the instructions [here](https://docs.microsoft.com/en-us/azure/developer/terraform/getting-started-cloud-shell#set-up-a-service-principal-1) to create a new service principal.

### Login to Azure
- Install azure cli using command `brew install azure-cli` (for mac)
- Run `az login` command to login azure account using cli, it will open browser window, and your credentials to login.
- Run `az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/your_subscription_id" --sdk-auth` , make sure to replace `your_subscription_id` with your subscription id.
- It will provide subscription id and other required ids, please make sure to copy those values.

### Manage secrets using GitHub
- Create a new GitHub Repo or Open existing repository which will be used for code
- Add the following variables and use the values which we copied earlier.
	- `AKS_SERVICE_PRINCIPAL_APP_ID`
	- `AKS_SERVICE_PRINCIPAL_CLIENT_SECRET`
	- `AZURE_CREDENTIALS`
- Run the command `ssh-keygen -t rsa -b 4096 -N "passowrd" -C "email" -q -f  ~/.ssh/id_rsa`
- Create github secret with name `SSH_PUBLIC_KEY`, copy the value of `~/.ssh/id_rsa.pub` and paste in value section of secret.
- Clone https://github.com/vipulJain05/azure_terraform and upload the same code into your GitHub repository.
- From GitHub Actions section, select `Terraform creation` workflow and run that manual workflow.
- It will create AKS cluster.

## Upload Docker image on quay.io

Steps:

-  Set up a Quay.io account and repository: If you haven't already, create an account on Red Hat Quay.io and create a new repository to host your Docker image.
- Create Robot account for Github workflow and provide the write permission to the quay repository which we have created.
-  In your GitHub repository, create a Dockerfile that describes the image you want to build. You can find more information about how to create Dockerfiles in the official Docker documentation.
- Add github secrets with name ``QUAY_PASSWORD`` and ``QUAY_USERNAME`` and add values which we copied when creating robot account and put the values.
- Push the code on master branch, it will push the docker image to quay and deploy the code on AKS.
- Get the kubeconfig using `az aks get-credentials --resource-group rg-climbing-sole --name k8stest` command.
- Check the pods using `kubectl get pods -A` commands.