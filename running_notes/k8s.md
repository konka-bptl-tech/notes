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


