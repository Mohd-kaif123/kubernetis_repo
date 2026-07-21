# Exercise:1 Deploy nginx Using Helm Chart
Run:
helm create nginx-chart

Output:

nginx-chart/

├── Chart.yaml
├── values.yaml
├── charts/
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── serviceaccount.yaml
│   ├── ingress.yaml
│   ├── hpa.yaml
│   ├── tests/
│   ├── NOTES.txt
│   └── _helpers.tpl
└── .helmignore

# Understand Folder Structure

chart.yml      -> Chart Information
values.yml     -> User Configurable Values
templates/     -> Kubernetes menifest templates
charts/        -> Dependency charts(Option)

# Edit values.yml
replicaCount: 2

image:
  repository: nginx
  tag: latest
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80

# Edit templates/deployment.yml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}

spec:
  replicas: {{ .Values.replicaCount }}

  selector:
    matchLabels:
      app: {{ .Release.Name }}

  template:
    metadata:
      labels:
        app: {{ .Release.Name }}

    spec:
      containers:
        - name: nginx
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          ports:
            - containerPort: 80

# Edit templates/service.yml

apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}
 
spec:
  type: {{ .Values.service.type }}
  ports:
    - port: {{ .Values.service.port }}
      targetPort: 80
  selector:
    app: {{.Release.Name }}
  type: {{ .Values.service.type }}

# Check your Generated YAML
helm template myapp ./nginx-chart

Output:
kind: Deployment

metadata:
  name: myapp

spec:
  replicas: 2

# Install Chart
helm install myapp ./nginx-chart

- note: if myapp is already running remove it and re install

# Verify the Deployment
kubectl get deployments

Output:
NAME    READY   UP-TO-DATE   AVAILABLE
myapp     2          2            2

# Chekc the Pods
kubectl get pods

Output:
NAME                     READY   STATUS    RESTARTS      AGE
my-pod                   1/1     Running   1 (23h ago)   25h
myapp-7c5c65b47b-jsgf4   1/1     Running   0             8m8s
myapp-7c5c65b47b-t9wwr   1/1     Running   0             8m8s
nginx-rs-2c9qv           1/1     Running   0             114m
nginx-rs-89pvr           1/1     Running   0             114m
nginx-rs-pfz9q           1/1     Running   0             117m

# Check the Service
kubectl get svc