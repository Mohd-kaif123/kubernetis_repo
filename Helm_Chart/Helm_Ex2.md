# Exercise:2 Scale the application using Helm

- Change: value.yml
replicaCount: 5

- upgrade the release
helm upgrade myapp ./nginx-chart

- Verify:
kubectl get pods

Output:
NAME                     READY   STATUS    RESTARTS      AGE
my-pod                   1/1     Running   1 (23h ago)   25h
myapp-7c5c65b47b-jsgf4   1/1     Running   0             8m8s
myapp-7c5c65b47b-t9wwr   1/1     Running   0             8m8s
myapp-7c5c65b47b-t9xyz   1/1     Running   0             8m8s
myapp-7c5c65b47b-t9pwr   1/1     Running   0             8m8s
myapp-7c5c65b47b-t9wqr   1/1     Running   0             8m8s

# Change the Image Version
- Update values.yml:

image:
  repository: nginx
  tag: 1.27

- Upgrade:

helm upgrade myapp ./nginx-chart

# View Release history
helm history myapp

# Roll Back
- If the latest upgrade causes issues:

helm rollback myapp 2

- Helm restores the application to revision 2.

# Uninstall
helm unistall myapp