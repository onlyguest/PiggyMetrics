# Java Microservices in Kubernetes

 ## Overview
A microservice is a single self-contained unit which, together with many others, makes up a large application. By splitting your app into small units every part of it is independently deployable and scalable, can be written by different teams and in different programming languages and can be tested individually. 


###Microservices Code Layer Considerations

#### Service Discovery
Options:  
 1. Consul (ca) - support spring cloud
 2. Eureka (ap) - support spring cloud
 3. Zookeeper (cp)
 4. etcd (cp)
 5. service metsh Istio
 
#### Service
 easy annotation for exposing a service = RestAPI
 Open API Options: 
 * http/json 
 * gRPC


#### Gateway/Router
Options:  
 * Zuul
 * Ingress (k8s)
  

#### Configuration

* spring-cloud-config-server

File System/Git/redis/database/Consul

k8s:
ConfigMap - Secret:
    configMapKeyRef - secretKeyRef 
 

#### Security
OAuth2/AAD

#### Persistence 
* MongoDB
* Mysql
* azure service: cosmos db

#### Message  
* RabbitMQ
* azure service: Azure Service Bus

#### Load Balancing
* Spring cloud round robin.
* K8s service load balancing

#### Monitoring
Distributed Tracing 
* ELK 
* Zipkin
* sleuth-turbine(spring cloud) 


### Batch Jobs
* Spring Batch

###Microservices OPS

#### Docker Image Mgr

* ACR(chance)

Jib(?) for quick building and publish images(one step to build image collections)

#### CI/CD
* Jenkins
 
#### Deployment 
Options: 
1. Kubernetes Manifest files
2. Helm/chart (compose to convert existing docker-compose.yml to helm charts) 

Practise to add liveness and readiness for the right startup sequence

Deploy tool(chance) for Rolling Update Deployment:
1. Pause 
2. Rollback 
3. Quick verification

<img src="http://cdn.pic1.54php.cn/20180410/f4341ad71ad9de554c7bdc578de7048a.jpg?imageView/2/w/600" width="300" alt="600">

#### Scale & Monitoring
Management tool(chance) for monitoring traffic/errors and circuiting break service.
* hystrix dashboard (no tools to )



#### Other
##### Migration/Backup
