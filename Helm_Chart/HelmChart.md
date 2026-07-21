# What is Helm Chart ?
- Helm is package manager for Kubernetes, Similar to: - apt for ubuntu - yum for RHEL - npm for Node.js - pip for Python

- A helm Chart is a package collection of Kubernetes manifest(YAML files) that can be installed , upgraded or removed as a single unit

# Without Helm
- suppose you want to deploy a WordPress Application
- you might need to create
Deployment
Service
ConfigMap
Secret
PersistentVolume
PersistentVolumeClaim
Ingress
HorizontalPodAutoscaler

- That's Many YAML files to manage mannually

# With Helm
    helm install wordpress

- helm will automatically create all require resurces of kubernetes togather

# Helm Architecture

            Helm CLI
                |
          Helm Install App
                |
         Kubernetes cluster
                |
            Helm Chart
                |
Deployment | Services | Configmap | secret

