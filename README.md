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

## 4. We push the Docker image to the Azure ACR
  

## 5. We create the Azure Container Instance(ACI) and we deploy it



## 6. In Azure Portal we navigate to Azure ACI


