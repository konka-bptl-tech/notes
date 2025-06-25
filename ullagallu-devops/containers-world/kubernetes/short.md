# Introdcution[What is k8s Why k8s]
Problems with docker and containers
-----------------------------------
1. Docker is a tool that used to containerize the application
2. But docker is single host system that is not distributed single point of failure when you run all the container in the docker host any reason if host down all containers also goes down
3. The problem with container is for each and every container comes with an ip by default containers are ephmeral port mapping to expose containers 
4. another problem with containers is storage [reason i don't know please provide me valid problem with containers storage issues]
5. Containers are not capable of auto heal it self if any failure
6. If we say if application have capabilites like loadbalancing and autoscaling but docker does not have capability to do
# k8s architecture
K8s by default brigns with an distributed system it has group of nodes called as cluster has master nodes and worker nodes
Master Nodes are responsible
- Pod placement
- keep the cluster configuration
- ensure high avaialbility, auto healing of workloads
Worker nodes are responsible for
- running the workloads or accomidate the workloads
Master Nodes some components
1. API Server
   - Receiving request , validate the request performs authentication and authorization, entry for to interact with cluster do operations on the cluster this is the only component that can interact with all components in the cluster
2. ETCD
   - is a key-value database responsble for keep the entire cluster configuration[yaml definitions,k8s onjects info...]
3. Scheduler
   - Takes care about pod placment Schedules the pod based on the constraints after gives the Schedulet info the API server
   - It has run algorthm for pod placment
     - algorithm has filtering and scoring
   - Identitfies best fit node for the pod
4. Control Manager
   - It has different controllers to manage k8s workloads
     - ReplicaSet Controller ensure high aviailbity of pods
     - Deployment Controller auotmatic rollout and rollback
     - serviceendpoints  controller responsible for adding and removing of pods from stack where service distributes the requst to the pod
     like wise we have many controller
5. Cloud Control Manager
  - It has multiple controller to manage cloud workloads
    - ALB ingress COntroller
    - DNS Controller.....
Worker Node Conponents
1. kubelet It will take instructions from api server and create pods and update the status to the API server
2. kube-proxy It will create and manage traffic rules of pods and service with in the node route the traffic appropriate service or pod
3. CRI = It will manage the life cycle of containers
worker node components are also there in master node also
# Namespaces
- Namespaces provides isolation for namespaced objects like svc,pod,deployments...
- using namespaces we effectively manage cluster resources and organize k8s objects it also able bring mutlitenancy to the cluster
- by default k8s comes 4 default namespaces
  - kube-system[having system related objects]
  - default[default namespaces where user can deploy objects]
  - kube-public[publicly accessible data like A configmap which contains publicly accessible data]
    - When i type kubectl cluster-info [ I got info from that] or kubectl cluster-info dump
  - kube-node-lease [having heart beat of nodes each node associated with lease object in ns that determines the node availability]
- kubectl create ns siva
- kubectl describe ns siva
- kubectl delete ns siva
apiVersion: v1
kind: Namespace
metadata:
    name: siva
usecases
--------
1. Isolating objects of different projects and it brings multitenacy to the cluster
2. avoids naming conflicts by isolating the objects you can't create 2 objecs with same ns if no ns
3. restrict the resources consumption by applying resouce quotas on a particular ns
4. We can apply RBAC on ns only authorized user can able to access the resources
It is always good idea to grouping resources
# Pod
1. Pod is a k8s Deployable object where it can be placeable rather than running
2. It is a wrapper around one or more containers one pod = one containers is best
3. Irrespective of no of containers in the Pod it has single IP because it contains pause container it holds the network namespace so all containers in the pod connect to that network
4. containers in the pod can reachable via it's localhost and storage of pod can be sharable to all the containers in the Pod
5. Don't place a single pod it does not have capable of restart itself
kubectl run <pod-name> --image=nginx
kubectl get pods
k get pods -o wide
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- bash or sh
# Labels and Selectors
1. Labels are key-value pairs attached k8s objects
2. Selectors are used to filter the k8s objects based on its labels
3. We can integrate 2 objects by using labels and selectors if one object have label app=backend when service have matched labels to filter app=backend so enable connections between 2 different objects
4. Selectors we have 2 types equality based and set based
5. Just Give me small examples about equality based and setbased selectors
# ReplicaSet & Replication Controller
- Both RS and RC can maintain desired no of identitcal pods at any given time
- RS is the newer version to maintain desired no of pods it supports both equality based and set based selectors
- RC is not supports old version to maintain desired no of pods it does not support set based selectors
- It does not supports rolling updates, rollbacks, or advanced deployment strategies.
# Deployment
- Deployment used to rollout and rollback, edit 
- It higher level controller that can handles rs rs maintains pods
- It is easy handle applications rollouts and rollbackes
- Imperative command to create deployment
# Services
In k8s each pod gets it's own IP address which is ephemeral means on every new creation of pod IP get changes so we cannot relay on Pod Ip for communication
Service Object in k8s provides static IP It abstract layers for communication Which is not bound with Pod life cycle
Service also also provides loadbalancing instead of accessing pod IPs service provides single entrypoint to access applications hosted in pod
It enable accessbility inside and outside cluster
What is service discover

`ClusterIP` is a default serivce in k8s it exposes the application with in the cluster
for example you have backend pod and frontend in order connect frontend pod to backendpod we no need to create expose our service outside world just create backend service which is cluster IP frontend pod can communicate via clusterIP
to distribute traffic to pods service uses selector to identity target here endpointscontroller help us maintain stack of pods this stack is modified by controller when new pods/ or deletion of pods this controller update the stack

`NodePort` this open one port in all avialable node in the cluster which so we can access our application via instanceip:nodeport
exposing port is not ideal choice to expose application external traffic access in single static port 

`LoadBalancer` LoadBalcner service does not require to open any port it just spin up cloud providers load balancer

`ExternalName` TO access any CNAME records inside K8s cluster we were using ExternalName Service

LoadBalancer externsion of NodePort
NodePort extension of ClusterIP

loadbalancer endpoint --> nodeip:nodeport --> clusterip:clusterport --> Podip:podport
