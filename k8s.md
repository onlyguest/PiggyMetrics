# Run PiggyMetrics in Kubernetes

 ## Overview

 The original PiggyMetrics repository contains 10 services. Each service is dockerized and Docker Compose is used to run all of them in one physical machine. It does not use Kubernetes but you get the idea that orchestration is something we want to further work on.


 
  ## Prerequisites:
 
 1. Install JDK and Maven, make sure the `java` executive is available in `PATH` environment variable and the `JAVA_HOME` is set to the JDK directory.
 2. Start Docker daemon to ensure `docker ps` to be able to executed without any errors.
 3. Login Docker hub using `docker login`, input your Docker hub username and password.
    ```
    docker login    
    ```
 4. Connect to an existing kubernetes cluster to ensure `kubectl get nodes` to be able to executed without any errors.   
    
 ## Essential Steps

 ### Build & Publish Docker Images

 Docker images are needed to be built and published to a registry. In our case, Docker Hub is the place. Basically, you run 'docker build' and 'docker publish' against each service to achieve that.

 A script file is essential to ease the pain. In current implementation, those steps were scripted in `build-images.sh`(`build-image.cmd` in windows). And a personal Docker Hub account was used. Moving forward, we need to replace the personal account with some environment variable, so that user can specify whatever account that is actually working for them.

 Since in kubernetes there are no port conflict issues, all ports are unified to 80 in all dockerfiles.

    
 Build image to docker hub:
  set the `DOCKER_HUB_USER` environment variable to your Docker hub username and run the following scripts in bash shell 
    
    ```
    export DOCKER_HUB_USER=<Your Docker hub username> && ./create-image.sh    
    ```

### Create Azure Cosmos DB
    
 In Piggy Metics, we need a database to store all datas like users, here we choose Cosmos DB because Azure supports Cosmos DB by native, open `vscode` and install `Azure Cosmos DB` plugin, create a new Cosmos database with `MongoDB` as the API and `piggymetrics` as the database name, right click and select `Open in Portal` and then goto the portal to get the connection string in mongodb style, copy the output of the following command in bash shell:
 
    ```
    echo "<Your Cosmos DB Connection String>" | base64
    ```
  
  open `scripts/secret.yaml` file and replace `<your cosmos connection string>` placeholder with output in above command.
  
  ```
     ./create-secret.sh
  ```
     
 ### Deployment to Kubernetes

 To run the services in Kubernetes, we need to model each service using Kubernetes concepts: deployments and services. So you see several manifest yaml files in `scripts` folder.

 With those manifest files, you can run `kubectl create` to get them executed in your active cluster.

 The sequence of deployment should not matter in Kubernetes context because each service should have builtin resilience. However, we do have extra scripts created and it is recommended that you run it in the following fashion:
 
 
 1. Substitute `{{DOCKER_HUB_USER}}` with the Docker hub username in all yaml files under `scripts` folder.        
      
 2. Start `config` service. 
    ```
     ./deploy-config.sh
     ```
 
 3. Start `registry` service. 
    ```
     ./deploy-registry.sh
    ```
 
 4. Start `RabbitMQ` service. 
    ```
     ./create-rabbitmq.sh
    ```
 
 5. Start other services. 
    ```
     ./deploy-services.sh

 ## Code Changes

 To check all the code changes, follow [this link](https://github.com/sqshq/PiggyMetrics/compare/master...VSChina:andy-dev2)
