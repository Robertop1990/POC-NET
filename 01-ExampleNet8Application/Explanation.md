# Implementation and deploy of a Web Api .NET Core 8 in Azure Kubernetes Services (AKS)

### Steps

[1. NET Core 8 Web API](#net-core-8-web-api)

[2. Create Resource Group in Azure](#create-resource-group-in-azure)

[3. Create Container Registry in Azure](#create-container-registry-in-azure)

[4. Create Azure Kubernetes Service(AKS) in Azure](#create-azure-kubernetes-serviceaks-in-azure)

[5. Publish NET Core 8 Web API in Container Registry](#publish-net-core-8-web-api-in-container-registry)

[6. Deploy image examplenet8application in AKS](#deploy-image-examplenet8application-in-aks)

### Prerequisites
- Visual Studio 2022
- Net Core 8 SDK
- Docker Desktop
- Azure Portal Subscription

In the world of software development, we find ourselves at the stage in which we have to deploy our application. Whether on premise or on cloud, there will always be some small challenges to overcome, but there is nothing better than being prepared when that time comes. Next, we will explain how to deploy a NET Core 8 web API in AKS.(The deployment was approached under a manual approach. In an automatic approach deployment are usually handled under automated pipelines (CI/CD). In a next post we will address the automatic approach)


### NET Core 8 Web API

Create a .NET Core 8 Web API
![image info](./Images/1_Step.png)

Configure your new project
![image info](./Images/2_Step.png)

Additional information
![image info](./Images/3_Step.png)

Project created
![image info](./Images/3.1_Step.png)

Add Support Docker to the application **ExampleNet8Application**
![image info](./Images/4_Step.png)

**Target OS**: Linux and **Container build type**: Dockerfile 
![image info](./Images/4.1_Step.png)

![image info](./Images/4.2_Step.png)

In a few seconds we should be able to visualize the image of our **ExampleNet8Application** application in docker.
![image info](./Images/4.3_Step.png)

Run application with docker
![image info](./Images/4.4_Step.png)

Execute GET /WeatherForecast
![image info](./Images/4.5_Step.png)

### Create Resource Group in Azure

Login on Azure Portal and look for the resource group service.
![image info](./Images/1_Step_Resource_Group.png)

Select create
![image info](./Images/1.1_Step_Resource_Group.png)

Write a name for our resource group "**RG-ExampleNet8-Aks**". Select Review + create.
![image info](./Images/1.2_Step_Resource_Group.png)

Select Create
![image info](./Images/1.3_Step_Resource_Group.png)

In an instance we will see our resource group "**RG-ExampleNet8-Aks**".
![image info](./Images/1.4_Step_Resource_Group.png)


### Create Container Registry in Azure

We are looking for the Container Registries service
![image info](./Images/1_Step_Container_Registry.png)

Select create
![image info](./Images/1.1_Step_Container_Registry.png)

Select our resource group **RG-ExampleNet8-Aks** and name the registry container **ExampleNet8Registry**. Select Review + create.
![image info](./Images/1.2_Step_Container_Registry.png)

Select Create
![image info](./Images/1.3_Step_Container_Registry.png)

In a instace we will see our container registry "**ExampleNet8Registry**"
![image info](./Images/1.4_Step_Container_Registry.png)


### Create Azure Kubernetes Service(AKS) in Azure

We are looking for the Kubernetesservices service
![image info](./Images/1_Step_Aks.png)

Select Create a Kubernetes cluster
![image info](./Images/1.1_Step_Aks.png)

Select our resource group **RG-ExampleNet8-Aks** and name the Kubernetes cluster **ExampleNet8KubernetesCluster**. 
![image info](./Images/1.2_Step_Aks.png)

In the Integrations tab we select our container registry **ExampleNet8Registry**. Select Review + create
![image info](./Images/1.3_Step_Aks.png)

Select Create
![image info](./Images/1.4_Step_Aks.png)

We waited a few minutes and the headed to our resource group **RG-ExampleNet8-Aks**. We must find our aks **ExampleNet8KubernetesCluster**
![image info](./Images/1.5_Step_Aks.png)


### Publish NET Core 8 Web API in Container Registry
Right click on our **ExampleNet8Application** project and select Publish
![image info](./Images/1_Step_Publish_Container_Registry.png)

Select Docker Container Registry
![image info](./Images/1.1_Step_Publish_Container_Registry.png)

Select Azure Container Registry
![image info](./Images/1.2_Step_Publish_Container_Registry.png)

Select the ExampleNet8Registry container registry that we created earlier
![image info](./Images/1.3_Step_Publish_Container_Registry.png)

Select Docker Desktop
![image info](./Images/1.4_Step_Publish_Container_Registry.png)
![image info](./Images/1.5_Step_Publish_Container_Registry.png)

Select Publish
![image info](./Images/1.6_Step_Publish_Container_Registry.png)
![image info](./Images/1.7_Step_Publish_Container_Registry.png)

Publish Succeeded
![image info](./Images/1.8_Step_Publish_Container_Registry.png)

In the Azure portal, within our resource group we select the ExampleNet8Registry container registry
![image info](./Images/1.9_Step_Publish_Container_Registry.png)

Select the **Repositories** option and we can see our published imagen **examplenet8application**
![image info](./Images/1.10_Step_Publish_Container_Registry.png)

Select tag latest
![image info](./Images/1.11_Step_Publish_Container_Registry.png)

We see the location of our image.
![image info](./Images/1.12_Step_Publish_Container_Registry.png)


### Deploy image examplenet8application in AKS
Create two yml files and place them in the root of our project

pod.yml

---
![image info](./Images/Image_Pod_Yml.png)
---

service.yml

---
![image info](./Images/Image_Service_Yml.png)
---

Open the powershell and we are located at the root of the project. Enter az login and a tab will open in the browser to log with our Azure credentials
![image info](./Images/1_Step_Deploy_In_Aks.png)
![image info](./Images/1.1_Step_Deploy_In_Aks.png)
![image info](./Images/1.2_Step_Deploy_In_Aks.png)


We go to our aks **ExampleNet8KubernetesCluster** and select connect. We enter the two commands in powershell
![image info](./Images/1.4_Step_Deploy_In_Aks.png)

![image info](./Images/1.5_Step_Deploy_In_Aks.png)

Enter the following command **kubectl apply -f pod.yml** to create the pod and then enter the following command **kubectl get pods** to see the status of the pod(**Status Running**)
![image info](./Images/1.6_Step_Deploy_In_Aks.png)

Enter the following command **kubectl get pods** to create the service and then enter the following command **kubectl get services** to see the status of the service
![image info](./Images/1.7_Step_Deploy_In_Aks.png)

In the option Services and Ingresses of ExampleNet8KubernetesCluster we can see the service **examplenet8-aks** (**Status Ok**). We click on the external ip **4.156.57.235** 
![image info](./Images/1.8_Step_Deploy_In_Aks.png)

A tab will open and we add /weatherforecast to the url. We can see the result of the service
![image info](./Images/1.9_Step_Deploy_In_Aks.png)

Diagram of components participating in flow
![image info](./Images/Architect.png)

With this we have completed this deployment tutorial in Azure Aks. In a next publication we will discuss this deployment automatically and focused on a business enviroment.

Tranks and regards









