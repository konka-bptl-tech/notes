kubectl create namespace blue
kubectl create namespace red

blue-deployment.yaml

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


red-deployment.yaml

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


blue-ingress.yaml

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
    alb.ingress.kubernetes.io/tags: '[{"Key":"Environment","Value":"Dev"},{"Key":"Team","Value":"test"}]'
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
          # HTTPS redirect rule
          - path: "/*"
            pathType: ImplementationSpecific
            backend:
              service:
                name: ssl-redirect
                port:
                  name: use-annotation

          # Actual service route
          - path: "/"
            pathType: Prefix
            backend:
              service:
                name: blue-service
                port:
                  number: 80


red-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: red-ingress
  namespace: red
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: YOUR_DOMAIN_OR_LB
    http:
      paths:
      - path: /red
        pathType: Prefix
        backend:
          service:
            name: red-service
            port:
              number: 80
