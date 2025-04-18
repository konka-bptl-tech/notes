# Ingress
- Ingress is a API object that provides external access to services in the cluster
- It acts as layers 7 loadbalancer external traffic routed different services based on the rules and routed defined in the ingress resource

Layer 4 loadbalancers works basic functionality like just receive request distribute to backend targets as it is no modifications any kind of changes apply to the forwarded traffic
- No intelligence while routing the traffic like any kind of data manipulation no decision making no advance routing 

Layer 7 Loadbalcners some extracapabilities
- supports host,path based routing
www.google.com	        Main Google Search
drive.google.com	    Google Drive (cloud storage)
mail.google.com	        Gmail
calendar.google.com	    Google Calendar
meet.google.com	        Google Meet
photos.google.com	    Google Photos
maps.google.com	        Google Maps

Generally LoadBalncers used for highavialability

We have services to expose application outside the k8s cluster using NodePort and Loadbalancer
NodePort opens one port in all nodes in cluster exposing ports is not a security best practice
Loadbalancer creates loadbalncer it is not ideal choice to use for example if i want expose multiple applications need to create those many loadblancer this is costly solution why because it creates layer 4 loadbalancer

When you type google.com
- DNS Resoultions happend
- 3 way handshake happend
- Loadbalncer forwards traffic to one fo the webserver
- webserver process the request and gives the response to the client

Browser → DNS → TCP Handshake → (TLS Handshake) → Load Balancer → Web Server → Response → Browser

Ingress Resource  = Defines the routing rules
Ingress Controller = Implements the rules and handles traffic

# RBAC
Authentication = check the identity
Authroization = what access have

Every Company implements Single-Sign-On After joinnning company they will create username and passwd for you use anywhere 

AWS IAM:
Roles,Users = Authentications
Policies = Permissions to acces services in AWS 

Kubernetes:
ServiceAccount : provides identity
RBAC: Policies : define policies

For auth we are depending on the AWS
For authorization k8s gives it self

Role
RoleBinding
ClusterRole
ClusterRoleBinding

harish = trainee
suresh senior 

Roles = Permissions
apiGroups = All Objects in k8s grouped into multiple groups
Resources = Objects
Verbs  = Actions[get,watch,list]

Add the users in aws auth configmap

admin will email project team that ns and access is given

ServiceAccount:



Scheduler decides pods placement on nodes

NodeSeletor = add the constraint based on constraint scheduler place pod on desired node if constrint does not meet pod was in pending state

Affinity = advnace selector options for pod placement compare to node selector

1. requiredDuringSchedulingIgnoredDuringExecution   =   same as node selector if constraint meets conditions pod was placed Schduler check the condtion during the pod plaement not running state,if contstrint not matches pod was in pending state uring running of pod event label was changed pod was not get effected

2. preferredDuringSchedulingIgnoredDuringExecution = scheduler to tries find node for pod if contstrint not matches still pod was placed in another nodes during running of pod event label was changed pod was not get effected

Anti-Affinity
- opposite of Affinity

Pod Affinity and Anti-Affinity

for example I have backend pod requires cache pod at same node it will improve performance of application

Taints = used to apply on nodes to avoid pod not place on certain nodes

NoExecute = If you apply taint on NoExecute it will evict the pods already running
NoSchedule = If you apply taint on node only tolerated pods can be placed [This is hard] pods running on node not evicted
PreferNoSchedule = this is soft version of NoSchedule avoids pod placement it is not guaranted

Tolerations = add the property to pod so pods can be placed in desired node








