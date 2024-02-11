# To run the Flask API inside of a container, the Docker image must exist inside of Azure Container Registry

## To build the Docker image
`docker build -t name_of_docker_image_tag .`

## To create the Azuure Container Registry

`az acr create --name name_of_acr --resource-group name_of_rg --sku Standard`

## To log into the ACR from the terminal

`az acr login --name name_of_acr`

## To tag and push the Docker image and prepare it for a push to ACR

`docker tag name_of_docker_image_tag name_of_acr.azurecr.io/azureapi/flask`
`docker push name_of_acr.azurecr.io/azureapi/flask`

----------

AzureAPI pipeline (azure.yml) make use of AzurCLI step. It should  set first / beforehand . Can be found in GitHub marketplace . There is example of code to add into pipeline

AzureAPI pipeline  runs within virtualenv that uses to run the flow, in our case ubuntu :
builds: 
  runs-on: ubuntu-latest

  
AzureAPI pipeline uses credentials to login into azure. 

   - uses: azure/login@v1.1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }} 

have to create RBAC role first :

#Create AAD App and Service Principal and assign RBAC Role

_az ad sp create-for-rbac -n "azureapiflask" --role contributor --scopes /subscriptions/22-284d2-6a19-4781-87f8-5c564ec4fec9 --json-auth

"scopes" - is Azure subscription ID. The command returns JSON doc . Copy it to GitHub 

https://learn.microsoft.com/en-us/azure/container-instances/container-instances-github-action?tabs=userlevel#configure-github-workflow 




   
