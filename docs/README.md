# UdaConnect Project for Udacity Data Engineer for AI application Nano-degree
## Overview


### Description
UdaConnect uses location data from mobile devices. UdaConnect has been built an application to ingest location data named UdaTracker. This POC was built with the core functionality of ingesting location and identifying individuals who have shared a close geographic proximity.


### What you need:
* [Flask](https://flask.palletsprojects.com/en/1.1.x/) - API webserver
* [SQLAlchemy](https://www.sqlalchemy.org/) - Database ORM
* [PostgreSQL](https://www.postgresql.org/) - Relational database
* [PostGIS](https://postgis.net/) - Spatial plug-in for PostgreSQL enabling geographic queries]
* [Vagrant](https://www.vagrantup.com/) - Tool for managing virtual deployed environments
* [VirtualBox](https://www.virtualbox.org/) - Hypervisor allowing you to run multiple operating systems
* [K3s](https://k3s.io/) - Lightweight distribution of K8s to easily develop against a local cluster
* [Helm](https://helm.sh/docs/intro/install/) - manager Kubernetes applications — Helm Charts helps define, install, and upgrade even the most complex Kubernetes application.

### Architecture Schetch

![Architecture](docs/architecture_design.png)


### Prerequisites
First, the following applications should be installed. 
1. [Install Docker](https://docs.docker.com/get-docker/)
2. [Set up a DockerHub account](https://hub.docker.com/)
3. [Set up `kubectl`](https://rancher.com/docs/rancher/v2.x/en/cluster-admin/cluster-access/kubectl/)
4. [Install VirtualBox](https://www.virtualbox.org/wiki/Downloads) with at least version 6.0 but version 7.0 did not work for me!!
5. [Install Vagrant](https://www.vagrantup.com/docs/installation) with at least version 2.0

### VM Setup
To run the application, a VM is needed. There is a vagrant file in the project to set up a VM. 
I used windows power shell(run as administrator). Then cd into directory and then:
vagrant up
It may take a few seconds/minutes to set up a VM.
I also used a synch folder which contains my whole project to make my procedure more simple!
It also installs k3s.
If you did this one time and want to use your VM again, just use the command vagrant reload.
To exit from this environment use: exit
when you want to stop it : vagrant halt
and if you don't need it any more and want to delete it: vagrant destroy

after that, SSH into vagrant env using: vagrant ssh
Then, switch into super user using: sudo su
#### `kubectl` installation
After all, use the command:
`sudo cat /etc/rancher/k3s/k3s.yaml` 
to see its content. Then, you can copy and paste it to save in a file.
The output could be sth like this:
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJkekNDQVIyZ0F3SUJBZ0lCQURBS0JnZ3Foa2pPUFFRREFqQWpNU0V3SHdZRFZRUUREQmhyTTNNdGMyVnkKZG1WeUxXTmhRREUyTmpneU1EWTJPRFV3SGhjTk1qSXhNVEV4TWpJME5EUTFXaGNOTXpJeE1UQTRNakkwTkRRMQpXakFqTVNFd0h3WURWUVFEREJock0zTXRjMlZ5ZG1WeUxXTmhRREUyTmpneU1EWTJPRFV3V1RBVEJnY3Foa2pPClBRSUJCZ2dxaGtqT1BRTUJCd05DQUFRdEdCTzg2UWlidXpBUUVma3JhckhaaWRQM2tkelNRSnhVMU5BNGhURFMKNzI2eTV4RWgvb0lXbENpaExOVGg3RWxTSmtpUjlmQU8wS0VScXRWT2hQRTFvMEl3UURBT0JnTlZIUThCQWY4RQpCQU1DQXFRd0R3WURWUjBUQVFIL0JBVXdBd0VCL3pBZEJnTlZIUTRFRmdRVUpVc2IzWWNGWmZJYTdnd2pBVWpzClcrZVlTOE13Q2dZSUtvWkl6ajBFQXdJRFNBQXdSUUlnTFhlbUh0dUxqNk9VR0l5S0R6aE9TRFM2TFNJckpMY2oKcTFYdWNPZDZWbGNDSVFER3dnVlpzUGhHZGw4ZlRKNDdMcHp0bCthcVNJY09wSDVIMG5xYjZIeExOUT09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    server: https://127.0.0.1:6443
  name: default
contexts:
- context:
    cluster: default
    user: default
  name: default
current-context: default
kind: Config
preferences: {}
users:
- name: default
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJrRENDQVRlZ0F3SUJBZ0lJWjV6NW5QVHRkL3d3Q2dZSUtvWkl6ajBFQXdJd0l6RWhNQjhHQTFVRUF3d1kKYXpOekxXTnNhV1Z1ZEMxallVQXhOalk0TWpBMk5qZzFNQjRYRFRJeU1URXhNVEl5TkRRME5Wb1hEVEl6TVRFeApNVEl5TkRRME5Wb3dNREVYTUJVR0ExVUVDaE1PYzNsemRHVnRPbTFoYzNSbGNuTXhGVEFUQmdOVkJBTVRESE41CmMzUmxiVHBoWkcxcGJqQlpNQk1HQnlxR1NNNDlBZ0VHQ0NxR1NNNDlBd0VIQTBJQUJMOWZiSXY0YWtLOUxyZy8KY2hPY1JHUG8xSXRSNEJKR0lIcS94NkpGOTFPVEpkT1B3U2dkM0VtYk9XVitaYXgvb2gxZnhFQUNXUGE4aC9ObQpnQmtJWXdTalNEQkdNQTRHQTFVZER3RUIvd1FFQXdJRm9EQVRCZ05WSFNVRUREQUtCZ2dyQmdFRkJRY0RBakFmCkJnTlZIU01FR0RBV2dCVEZmcTA3VU83YnRLSHZJVFpyMEFQNWZiNWsrREFLQmdncWhrak9QUVFEQWdOSEFEQkUKQWlCK0hKdW0rWGxIb0tsVk16U2Vhc1pQN2dnV09Xc05jL3dBRTdTbFVqcVJXQUlnUHp5RlVXSmQ5dFpqV243VgpzZU9HV29hOTlibmJKTW1qOWxzc0dpVnlFY2c9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0KLS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJkakNDQVIyZ0F3SUJBZ0lCQURBS0JnZ3Foa2pPUFFRREFqQWpNU0V3SHdZRFZRUUREQmhyTTNNdFkyeHAKWlc1MExXTmhRREUyTmpneU1EWTJPRFV3SGhjTk1qSXhNVEV4TWpJME5EUTFXaGNOTXpJeE1UQTRNakkwTkRRMQpXakFqTVNFd0h3WURWUVFEREJock0zTXRZMnhwWlc1MExXTmhRREUyTmpneU1EWTJPRFV3V1RBVEJnY3Foa2pPClBRSUJCZ2dxaGtqT1BRTUJCd05DQUFURENKUHJoZEtsSDg0bCthTGMxeDRhTHN1R1JZbUVlUjcrS2dMR1V3YjMKOWZPM29UWTNkRnF3VkZKMHFLMDVEbmFXd2FCdXRoTHk5cTIvanJOb0xLVUJvMEl3UURBT0JnTlZIUThCQWY4RQpCQU1DQXFRd0R3WURWUjBUQVFIL0JBVXdBd0VCL3pBZEJnTlZIUTRFRmdRVXhYNnRPMUR1MjdTaDd5RTJhOUFECitYMitaUGd3Q2dZSUtvWkl6ajBFQXdJRFJ3QXdSQUlnZER6QVg1S0Z6SUtXa1lSVi9iNEpVMkZJUkJRRk5yVmQKMmdvTU9ZS3dSem9DSURlSjkyZDd0KzNnd1BaRyt6VXFzdkxMTWNHMHhXSFlRSlJzU3dhVThUMzEKLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
    client-key-data: LS0tLS1CRUdJTiBFQyBQUklWQVRFIEtFWS0tLS0tCk1IY0NBUUVFSU9Tb0d3R3hMWVNDT1AyRlJwbS9XNkQwTEVlS2xaa3Y0bkhuWkNEanRkcVdvQW9HQ0NxR1NNNDkKQXdFSG9VUURRZ0FFdjE5c2kvaHFRcjB1dUQ5eUU1eEVZK2pVaTFIZ0VrWWdlci9Ib2tYM1U1TWwwNC9CS0IzYwpTWnM1Wlg1bHJIK2lIVi9FUUFKWTlyeUg4MmFBR1FoakJBPT0KLS0tLS1FTkQgRUMgUFJJVkFURSBLRVktLS0tLQo=
	
	master:/home/project7 # helm install udaconnect-kafka bitnami/kafka  --kubeconfig /etc/rancher/k3s/k3s.yaml

To check whether the kubectl is installed properly you can use: `kubectl describe services`. 
It should not return any errors.

Now we should deploy .yaml files. The following commands will create pods using .yaml files in the deployment folder.   cd ..  cd project7

1. `kubectl apply -f deployment/db-configmap.yaml` - Set up environment variables for the pods
2. `kubectl apply -f deployment/db-secret.yaml` - Set up secrets for the pods
3. `kubectl apply -f deployment/postgres.yaml` - Set up a Postgres database running PostGIS
4. `kubectl apply -f deployment/udaconnect-api.yaml` - Set up the service and deployment for the API v1
6. `kubectl apply -f deployment/udaconnect-persons-api.yaml` - Set up the service and deployment for the Person API
7. `kubectl apply -f deployment/udaconnect-connections-api.yaml` - Set up the service and deployment for the Connection API
8. `kubectl apply -f deployment/udaconnect-app.yaml` - Set up the service and deployment for the web app
9. Then run kubectl get pods to see the up and running pods. Copy the complete pod name of postgres pod and use it in the next step instead of POD_NAME.
10. `sh scripts/run_db_command.sh <POD_NAME>` - Seed your database against the `postgres` pod. 
11. Install and configure helm in Kubernetes using following commands:

  ```
  curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3

  chmod 700 get_helm.sh

  ./get_helm.sh
  ```
    
12. Install and configure kafka in Kubernetes using following commands:
 ```
  helm repo add bitnami https://charts.bitnami.com/bitnami

  helm install udaconnect-kafka bitnami/kafka  --kubeconfig /etc/rancher/k3s/k3s.yaml 
  ```
  
13. Save the the output issued from your own command in a file by copy and paste:
  ```

NAME: udaconnect-kafka
LAST DEPLOYED: Fri Nov 11 22:50:48 2022
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: kafka
CHART VERSION: 19.1.3
APP VERSION: 3.3.1

** Please be patient while the chart is being deployed **

Kafka can be accessed by consumers via port 9092 on the following DNS name from within your cluster:

    udaconnect-kafka.default.svc.cluster.local

Each Kafka broker can be accessed by producers via port 9092 on the following DNS name(s) from within your cluster:

    udaconnect-kafka-0.udaconnect-kafka-headless.default.svc.cluster.local:9092

To create a pod that you can use as a Kafka client run the following commands:

    kubectl run udaconnect-kafka-client --restart='Never' --image docker.io/bitnami/kafka:3.3.1-debian-11-r11 --namespace default --command -- sleep infinity
    kubectl exec --tty -i udaconnect-kafka-client --namespace default -- bash

    PRODUCER:
        kafka-console-producer.sh \

            --broker-list udaconnect-kafka-0.udaconnect-kafka-headless.default.svc.cluster.local:9092 \
            --topic test

    CONSUMER:
        kafka-console-consumer.sh \

            --bootstrap-server udaconnect-kafka.default.svc.cluster.local:9092 \
            --topic test \
            --from-beginning
  ```
14. Check if all the pods are created without errors: kubectl get pods
All the pods should be in running state. It may take a few minutes to 'kafka-0' get running.
15. Create the kafka topic "location-data" using the following commands:
  ```
  kubectl exec -it udaconnect-kafka-0 -- kafka-topics.sh \ --create --bootstrap-server udaconnect-kafka-headless:9092 \ --replication-factor 1 --partitions 1 \ --topic 'location-data'
  ```
Now, create the other pods:
15. `kubectl apply -f deployment/kafka-configmap.yaml` - environment variables setting up for the pods
16. `kubectl apply -f deployment/udaconnect-location-microservice.yaml`
17. `kubectl apply -f deployment/udaconnect-location-generator.yaml`
18. Test gRPC pipeline using kafka
  ##### `kubectl exec -it <location-generator-pod-name> sh`
  kubectl exec -it udaconnect-location-generator-service-85f6f79bb-6vp2t sh
  
  Then, when we are in sh mode, we can run the following command to create random data for different people.
  ( It can be executed several times!)
  python grpc_client.py
  
  #####  `python app/grpc_location_generator.py` Execute the grpc location generator with the command below 
  ##### 14.3 Check logs
  ```
  kubectl logs -f <location-generator-pod-name>

  kubectl logs -f <location-microservice-pod-name>
  ```


Manually applying each of the individual `yaml` files is cumbersome but going through each step provides some context on the content of the starter project. In practice, we would have reduced the number of steps by running the command against a directory to apply of the contents: `kubectl apply -f deployment/`.

Note: The first time you run this project, you will need to seed the database with dummy data. Use the command `sh scripts/run_db_command.sh <POD_NAME>` against the `postgres` pod. (`kubectl get pods` will give you the `POD_NAME`). Subsequent runs of `kubectl apply` for making changes to deployments or services shouldn't require you to seed the database again!

### Is everything working properly?
Once the project is up and running, you should be able to see three deployments and three services in Kubernetes. To verify, run the following commands:

`kubectl get pods`
`kubectl get services`


These pages should also load on your web browser:
http://localhost:30000/ - Frontend ReactJS Application
http://localhost:30001/ - OpenAPI Documentation for legacy API
http://localhost:30001/api/ - Base path for legacy API
http://localhost:30002/ - OpenAPI Documentation for Persons API
http://localhost:30002/api/ - Base path for Persons API
http://localhost:30003/ - OpenAPI Documentation for Connections API
http://localhost:30003/api/ - Base path for Connections API