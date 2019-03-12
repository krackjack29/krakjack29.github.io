---
title: "Deploying Sonarqube Container in Azure"
layout: single
comments: true
date: 2019/03/13
categories:
    - Azure
tags:
    - azure
    - cloud
    - appservice
    - sonarqube
    - deployment
    - containers
    - docker
    - postgres
---

Sonarqube is an open source tool which provides continuous inspection of the code quality. It integrates seamlessly with all the CI/CD tools such as Jenkins, Azure DevOps, TeamCity etc. 

Sonarqube can be deployed as a window service, web application and also as a container. In this article, we will explore how to deploy Sonarqube container in Azure using PaaS services.

## Sonaqube container
Sonarqube has an official docker container in the [dockerhub](https://hub.docker.com/_/sonarqube), it has the following properties.

* Runs on port 9000
* Supports different databases such as 
  * PostGres
  * MySql
  * Sql Server
  * H2 (in-memory database, should be used for non-production scenarios)
* For data persistence outside the container, volumes needs to be mounted 
  * /opt/sonarqube/extensions - Sonarqube extensions and plugins are stored.
  * /opt/sonarqube/conf - Sonarqube configurations
  * /opt/sonarqube/logs - Sonarqube logs

## Deploying Sonarqube to Azure WebApp for Containers and Azure PostGres
![Deployment Diagram Webapp Container](/assets/images/sonarqube/1.PNG)

To persist the data we would have to mount the volumes to the container, unfortunately, Azure AppService doesn't allow external storage to be added. /opt is stored in a "D:/temp" directory of the app service, which will be wiped off once the app restarts. Only contents on "D:/home" will be persisted between app restarts. You could verify the same using kudu tools in the app service.

Thanks to [NATHANAEL MARCHAND](https://www.natmarchand.fr/sonarqube-azure-webapp-containers/) !!, we created a wrapper around the vanilla sonarqube container to create a soft link of /opt to /home. 

```docker
FROM sonarqube:7.0-alpine
COPY entrypoint.sh ./bin/
RUN chmod +x ./bin/entrypoint.sh
EXPOSE 9000
ENTRYPOINT ["./bin/entrypoint.sh"]
```

["entrypoint.sh"](https://raw.githubusercontent.com/pratap-dotnet/sonarqube-azure-webapp-container/master/sonarqube-webapp-postgres/entrypoint.sh) contains the code to create soft link to /opt directory to /home. 

There were some issues downloading Nathaneal's docker image, because of the last lines of entrypoint script. I have fixed the same and uploaded the container to [dockerhub](https://hub.docker.com/r/pratapgowda/azuresonarqube).

Next step is to create Azure Database for PostrgreSql to connect to the Sonarqube by updating the environment variable **SONARQUBE_JDBC_URL**. 

Note: PostGreSql should enable communication to Azure IPs.

*Environment variables for the container are set by the application setting of the web app*. 

Once the postgres is created, run the following azure cli commands to pull the image and run sonarqube.

```bash
# Create app service plan
az appservice plan create --name sonarqubedc-asp \
 --resource-group sonarqubedc --sku S1 --is-linux
# Set the configuration files
az webapp create --resource-group sonarqubec \
    --plan sonarqubec-asp --name sonarqubec-container \
    --deployment-container-image-name "alpine"
az webapp config appsettings set \
    --resource-group sonarqubec \
    --name sonarqubec-container \
    --settings SONARQUBE_JDBC_URL="{connectionstring}" \
    SONARQUBE_JDBC_USERNAME="sonar" SONARQUBE_JDBC_PASSWORD="{password}"
az webapp config container set \
    --resource-group sonarqubec \
    --name sonarqubec-container \
    --enable-app-service-storage true \
    --docker-custom-image-name "pratapgowda/azuresonarqube:latest"
```

### Pros:
* Runs as a managed service.
* Connect to Azure Postgres SQL over SSL.
* Auto-scaling works in same lines as any other webapp.
* Add SSL and custom domains. 

### Cons
* Container is built on 7.0-alpine version, but the current version of sonarqube is 7.6. 
* Updating/upgrading the container is an overhead. 

Code is hosted [here](https://github.com/pratap-dotnet/sonarqube-azure-webapp-container/tree/master/sonarqube-webapp-postgres) in github, if you want to take it to spin. 

## Deploying Sonarqube to Azure Container Instance and SQL server
![Deployment Diagram Azure Container Instance](/assets/images/sonarqube/2.PNG)

Azure container instances is a managed service which allows running containers is an easiest and fastest way. Unlike, app service there will be no storage provided by default but you can use Azure file share as persistence layer. 

The diagram above shows the relationship between azure file share and the container instance deployed. In this example I am using Azure SQL database. For brevity, wanted to to try a different database.

Azure container instance can be deployed using Azure CLI, but it's limited only to a single mount. To mount more than one mount, one should be using ARM template. 

### Pros
* Supports running general available Docker image for Sonarqube and we can use Azure Managed Postgres SQL service or Azure SQL Database service for database.
* Simpler container management.
* Data persistence addressed by mounting Azure File Shares.

### Cons
* No Scale-out options. You can scale up the instance on CPU.
* Would need another service like Application gateway for custom domain and SSL offloading.

Code is hosted [here](https://github.com/pratap-dotnet/sonarqube-azure-webapp-container/tree/master/sonarqube-aci-sql-external) in Github. 

Happy Coding!!!