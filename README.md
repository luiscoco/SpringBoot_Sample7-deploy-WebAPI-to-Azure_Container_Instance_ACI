# How to deploy SpringBoot WebAPI to Azure Container Instance(ACI)

## 1. Create an Azure Container Registry

az acr create --resource-group myRG --name myregistryluiscoco1974 --sku Basic --location westeurope

## 2. Create a Spring Boot Web API in VSCode

https://github.com/luiscoco/SpringBoot_Sample2-created-WebAPI-with-VSCode

## 3. Create the Docker image and run it

Execute the following command to build the Docker image:

```
docker build -t springbootapi:latest .
```

Tag the Image for Azure Container Registry (ACR). Once the image is built, you'll need to tag it with the Azure Container Registry's address.

Assume your ACR login server is myregistryluiscoco1974.azurecr.io, type this command:

```
docker tag springbootapi:latest myregistryluiscoco1974.azurecr.io/springbootapi:latest
```

Run the Docker container, to start a container from your image, use the docker run command:

```
docker run -p 8080:8080 myregistryluiscoco1974.azurecr.io/springbootapi:latest
```

## 4. Log in to an Azure Container Registry (ACR) using Docker

### 4.1. Using Azure CLI

Install Azure CLI: Make sure you have the Azure CLI installed on your machine.

Log in to Azure: Open a terminal or command prompt and log in to your Azure account:

```
az login
```

Follow the instructions to complete the login process.

Get ACR Login Server Name: Retrieve your ACR's login server name. Replace myregistryluiscoco1974 with your actual ACR name.

```
az acr show --name myregistryluiscoco1974 --query loginServer --output table
```

It will return something like myregistryluiscoco1974.azurecr.io.

Login to ACR: Use the Azure CLI to log in to your ACR. 

This command automatically uses the credentials stored from your Azure login to authenticate with ACR.

```
az acr login --name myregistryluiscoco1974
```

This command will handle the authentication with Docker for you.

### 4.2. Using Docker Login Command

Get ACR Credentials: First, retrieve your ACR's username and password. Replace myregistryluiscoco1974 with your actual ACR name.

```
az acr credential show --name myregistryluiscoco1974
```

Note the username and one of the passwords from the output:

```
PS C:\SpringBoot WebAPI> az acr credential show --name myregistryluiscoco1974
{
  "passwords": [
    {
      "name": "password",
      "value": "m+QYxC4Y6xCJWTLI/huNzIjOvhM65xlVBWzihklezR+ACRDK1LbO"
    },
    {
      "name": "password2",
      "value": "PgT2yZrhXD5fKHKi/RCYwB9F9y8vF70Ttpxvn2yi/d+ACRCB+AYZ"
    }
  ],
  "username": "myregistryluiscoco1974"
}
```

Docker Login: Use the docker login command with your ACR's login server URL and the credentials you just retrieved.

```
docker login myregistryluiscoco1974.azurecr.io -u <username> -p <password>
```

```
docker login myregistryluiscoco1974.azurecr.io -u myregistryluiscoco1974 -p m+QYxC4Y6xCJWTLI/huNzIjOvhM65xlVBWzihklezR+ACRDK1LbO
```

Replace <username> and <password> with the credentials from the previous step.

## 5. We push the Docker image to the Azure ACR

```
docker push myregistryluiscoco1974.azurecr.io/springbootapi:latest
```  

## 6. We create the Azure Container Instance(ACI) and we deploy it

```
az container create --resource-group myRG ^
  --name mycontainerinstance ^
  --image myregistryluiscoco1974.azurecr.io/springbootapi:latest ^
  --cpu 1 ^
  --memory 1.5 ^
  --registry-login-server myregistryluiscoco1974.azurecr.io ^
  --registry-username myregistryluiscoco1974 ^
  --registry-password m+QYxC4Y6xCJWTLI/huNzIjOvhM65xlVBWzihklezR+ACRDK1LbO ^
  --dns-name-label myspringbootwebapidns ^
  --ports 8080 ^
  --location westeurope
```

## 7. In Azure Portal verify the Azure ACI


