# Types of Kubernetes Services
                   Kubernetes Service
                          |
    -------------------------------------------------
    |               |             |                |
 ClusterIP      NodePort      LoadBalancer    ExternalName
                          |
                     (Headless Service)

## 1. ClusterIP (Default)
- Exposes the application inside the Kubernetes cluster only
- Not Assecible from the outside the cluster
- Default Service type is none is specified

Client Pod
     |
     v
+-------------------+
| ClusterIP Service |
+-------------------+
     |
     v
+-------------------+
|      Pod 1        |
|      Pod 2        |
|      Pod 3        |
+-------------------+

### Use Cases:

Backend API
Databases
MicroService Communication

## 2. NodePort
- exposes the applicationon every worker node
- accessible using:
NodeIP: Nodeport
- kubernetes assigns a port from 3000-32767(or you specify one).

Browser
   |
NodeIP:30080
   |
+----------------+
| NodePort       |
+----------------+
        |
        v
+----------------+
| ClusterIP      |
+----------------+
        |
        v
 Pods
### Use Cases
- Developemnt
- Testing
- Minikube
- Small Cluster

## 3. LoadBalanacer
- creates an external could load balancer.
- Requires cloude provider such as AWS,AZURE or GCP

Internet
     |
Load Balancer
     |
ClusterIP Service
     |
 Pods

## 4. ExternalName
- Maps a Kubernetes Service to an External DNS Name
- No Pods or Proxing Involve

Application
      |
ExternalName Service
      |
      v
api.example.com

### Use Cases
- External Databases
- ThirtParty API

## 5. Headless Service

- creating by setting clusterIp:None
- Kubernetes doesnot assign Virtual IP
- Client receives the individual PODS ips directly through DNS

pplication
      |
DNS Lookup
      |
-------------------------
|      |        |
Pod1   Pod2    Pod3