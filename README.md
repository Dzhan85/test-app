# test-app on AWS
task for interview 

## Create EKS cluster 

Use manual for deployment AWS EKS https://github.com/terraform-aws-modules/terraform-aws-eks

1. Install ALB controller (https://kubernetes-sigs.github.io/aws-load-balancer-controller/v1.1/guide/controller/setup/)
2. Install External DNS (https://kubernetes-sigs.github.io/aws-load-balancer-controller/v1.1/guide/external-dns/setup/)
3. Configure IAM integration with EKS
4. Configure Ingress (https://kubernetes-sigs.github.io/aws-load-balancer-controller/v1.1/guide/ingress/annotation/)

## Deploy Jenkins in Kubernetes

Used this manual https://www.eksworkshop.com/intermediate/210_jenkins/deploy/
## Configure Jenkins pipeline


1. Add repository token
2. Create pipeline
3. Attach Jenkinsfile to pipeline

## Run Job

After configuting your application run job in Jenkins


In order to check if the helm chart is working properly, we can install it and check if the several components where deployed properly:

```
helm install example ./helm
kubectl get deployment
kubectl get pod
kubectl get service
kubectl get ingress
```
