# Installtion
> Installation:-
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

> Permission Related Error
sudo usermod -aG docker $USER
newgrp docker

> Start Your Cluster
minikube start

> if its configured
kubectl cluster-info

kubectl get nodes
> you can see single machine as control-plane which is master node

# Setting up Kubectl in Ubuntu
#Download the latest release with the command:
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

#Validate 
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
#you can see Kubectl:OK
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

kubectl version --client

# Setting up a cluster using minikube
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

miniube start
minikube status
kubectl cluster-info
kubectl get nodes # check Connected Nodes
kubectl dashboard --url # access Minikube IP

## if kubectl is not working re-configure it
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

kubectl version --client

# Create a POD
kubectl run my-pod --image=nginx --port=80
kubectl get pods
kubectl describe pod my-pod

# Create Service
- Expose an application running in your cluster behind a single outward facing endpoint, even when the workload is split accross multiple backends

kubectl expose pod my-pod --type=NodePort --port=80

kubectl get svc

minikbe service my-pod

# Create Pod and Service uing YML
1. create my-pod.yml

apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx # you can use this label to connect pod with any service
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80

2. create my-service.yml

apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: nginx #selecting Pods
  ports:
  - protocol: TCP
    targetPort: 80 # this is docker port
    port: 8084 # exposed port

kubectl apply -f my-pod.yml
kubectl get pods
kubectl describe pod nginx

kubectl apply -f my-service.yml
kubectl get pods
minikube service my-service