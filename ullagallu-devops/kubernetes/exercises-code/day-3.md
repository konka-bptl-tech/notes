# kubeadm cluster creation with terraform and ansible
[Repo for kubeadm cluster creation/deletion automation](https://github.com/konka-devops-lab/kubeadm-cluster-terraform-ansible.git)
# Exercise AutoScaling Deployement using HPA to load on pods we would use Job Controller

```yaml
# deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
          resources:
          requests:
            cpu: "10m"
          limits:
            cpu: "50m"

```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
```

```yaml
# hpa
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deployment
  minReplicas: 2
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 10

```

```yaml
# job.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: nginx-load-generator
spec:
  template:
    spec:
      containers:
      - name: loader
        image: busybox
        command:
        - /bin/sh
        - -c
        - >
          while true; do wget -q -O- http://nginx-service; done
      restartPolicy: Never

```


