## Namespace Creation
```bash
kubectl create namespace blue
kubectl create namespace red
````
---
## blue-deployment.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue-deployment
  namespace: blue
spec:
  replicas: 2
  selector:
    matchLabels:
      app: blue
  template:
    metadata:
      labels:
        app: blue
    spec:
      containers:
      - name: blue
        image: hashicorp/http-echo
        args:
        - "-text=Hello from BLUE"
        ports:
        - containerPort: 5678
---
apiVersion: v1
kind: Service
metadata:
  name: blue-service
  namespace: blue
spec:
  selector:
    app: blue
  ports:
  - port: 80
    targetPort: 5678
```
---
## red-deployment.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: red-deployment
  namespace: red
spec:
  replicas: 2
  selector:
    matchLabels:
      app: red
  template:
    metadata:
      labels:
        app: red
    spec:
      containers:
      - name: red
        image: hashicorp/http-echo
        args:
        - "-text=Hello from RED"
        ports:
        - containerPort: 5678
---
apiVersion: v1
kind: Service
metadata:
  name: red-service
  namespace: red
spec:
  selector:
    app: red
  ports:
  - port: 80
    targetPort: 5678
```
---
## blue-ingress.yaml
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: blue-ingress
  labels:
    app: blue-ingress
  annotations:
    alb.ingress.kubernetes.io/load-balancer-name: "ullagallu"
    alb.ingress.kubernetes.io/group.name: "blue-red"
    alb.ingress.kubernetes.io/scheme: "internet-facing"
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/tags: "Environment=Dev,Team=test"
    alb.ingress.kubernetes.io/group.order: '10'
    alb.ingress.kubernetes.io/healthcheck-path: '/'
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS": 443}]'
    alb.ingress.kubernetes.io/certificate-arn: "arn:aws:acm:us-east-1:522814728660:certificate/6766b9af-b27a-4f15-9cfc-2038c5f625aa"
    alb.ingress.kubernetes.io/ssl-policy: 'ELBSecurityPolicy-2016-08'
    alb.ingress.kubernetes.io/actions.ssl-redirect: >
      {"Type": "redirect", "RedirectConfig": { "Protocol": "HTTPS", "Port": "443", "StatusCode": "HTTP_301" }}
    external-dns.alpha.kubernetes.io/hostname: "blue.konkas.tech"
    external-dns.alpha.kubernetes.io/ttl: '60'
spec:
  ingressClassName: alb
  rules:
    - host: "blue.konkas.tech"
      http:
        paths:
          - path: "/*"
            pathType: ImplementationSpecific
            backend:
              service:
                name: ssl-redirect
                port:
                  name: use-annotation
          - path: "/"
            pathType: Prefix
            backend:
              service:
                name: blue-service
                port:
                  number: 80
```
---
## red-ingress.yaml
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: red-ingress
  labels:
    app: red-ingress
  annotations:
    alb.ingress.kubernetes.io/load-balancer-name: "ullagallu"
    alb.ingress.kubernetes.io/group.name: "blue-red"
    alb.ingress.kubernetes.io/scheme: "internet-facing"
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/tags: "Environment=Dev,Team=test"
    alb.ingress.kubernetes.io/group.order: '10'
    alb.ingress.kubernetes.io/healthcheck-path: '/'
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS": 443}]'
    alb.ingress.kubernetes.io/certificate-arn: "arn:aws:acm:us-east-1:522814728660:certificate/430d56f7-f634-4023-ae90-3ef6b063ab55"
    alb.ingress.kubernetes.io/ssl-policy: 'ELBSecurityPolicy-2016-08'
    alb.ingress.kubernetes.io/actions.ssl-redirect: >
      {"Type": "redirect", "RedirectConfig": { "Protocol": "HTTPS", "Port": "443", "StatusCode": "HTTP_301" }}
    external-dns.alpha.kubernetes.io/hostname: "red.konkas.tech"
    external-dns.alpha.kubernetes.io/ttl: '60'
spec:
  ingressClassName: alb
  rules:
    - host: "red.konkas.tech"
      http:
        paths:
          - path: "/*"
            pathType: ImplementationSpecific
            backend:
              service:
                name: ssl-redirect
                port:
                  name: use-annotation
          - path: "/"
            pathType: Prefix
            backend:
              service:
                name: red-service
                port:
                  number: 80
```
---
## Optional: Remove Ingress Finalizers (for cleanup)
```bash
kubectl patch ingress blue-ingress -p '{"metadata":{"finalizers":[]}}' --type=merge
```
