# test-app
task for interview


In order to check if the helm chart is working properly, we can install it and check if the several components where deployed properly:

```
helm install example ./helm
kubectl get deployment
kubectl get pod
kubectl get service
kubectl get ingress
```
