# voting-app-k8s
Deploying Voting app on Kubernetes

## Architecture

* A front-end web app in Python which lets you vote between two options.
* A Redis which collects new votes.
* A .NET worker which consumes votes and stores them in.
* A Postgres database backed by a Docker volume.
* A Node.js web app which shows the results of the voting in real time.
<div align="center">
<img  hieght= "300" src="https://user-images.githubusercontent.com/47721226/236636317-edef0f10-d2c3-4535-abf0-c430095a6471.png">
</div>
 
---
## Steps
### 1- Create new namespace 
* `kubectl apply -f app-namespace.yml`

### 2- Create Secret for Postgres Credentials 
* `kubectl apply -f postgres-secret.yml`

### 3- Create Voting App Deployment and Service 
* NodePort service which uses port `30004`
* `kubectl apply -f votingapp-deploy.yml votingapp-service.yml`
* Container image  ` kodekloud/examplevotingapp_vote:v1`


### 4- Create Redis Deployment and Service 
* ClusterIP service which uses port `6379`
* `kubectl apply -f redis-deploy.yml redis-service.yml`
* Container image `redis`

### 5- Create Postgres Deployment and Service 
* ClusterIP service which uses port `5432`
* `kubectl apply -f postgres-deploy.yml postgres-service.yml`
* Container image `postgres`

### 6- Create Worker App Deployment 
* `kubectl apply -f workerapp-deploy.yml`
* Container image ` kodekloud/examplevotingapp_worker:v1`

### 7- Create Result App Deployment and Service
* NodePort service which uses port `30005`
* `kubectl apply -f resultapp-deploy.yml resultapp-service.yml`
* Container image ` kodekloud/examplevotingapp_result:v1`

---
## Validation 
Run `kubectl get pods,svc -n votingapp` to see all created pods and services in votingapp namespace

<div align="center">
<img  hieght= "300" src="https://user-images.githubusercontent.com/47721226/236637427-ce9589c9-4375-4117-aea7-0f988effc7e3.png">
</div>

---
## Demo
![Animation](https://user-images.githubusercontent.com/47721226/236637604-5d3243b9-ac69-43d4-b32d-7f1160850b56.gif)




