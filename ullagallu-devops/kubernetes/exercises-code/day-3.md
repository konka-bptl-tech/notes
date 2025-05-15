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
  minReplicas: 1
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
  name: cpu-load-generator
spec:
  template:
    spec:
      containers:
      - name: loader
        image: busybox
        command: ["sh", "-c", "while true; do :; done"]
        resources:
          requests:
            cpu: "500m"
      restartPolicy: Never
  backoffLimit: 1
```


