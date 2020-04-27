# Create Cluster Kubernetes Digital Ocean

## configuration nginx-ingress
- kubectl create namespace nginx-ingress
- helm repo add stable https://kubernetes-charts.storage.googleapis.com
- helm install nginx-ingress stable/nginx-ingress --namespace nginx-ingress --set controller.autoscaling.enabled=true --set controller.autoscaling.minReplicas=1 --set controller.autoscaling.maxReplicas=10 --set controller.autoscaling.targetCPUUtilizationPercentage=65 --set controller.autoscaling.targetMemoryUtilizationPercentage=65 --set controller.publishService.enabled=true

## Configuration Jenkins
- kubectl create namespace devops
- kubectl apply -f ./jenkins/pv-pvc.yaml
- helm upgrade jenkins stable/jenkins -n devops --set master.numExecutors=5 --set persistence.existingClaim=jenkins --set master.ingress.enabled=true --set master.ingress.hostName=devops.clubedaposta.com --set master.ingress.apiVersion=extensions/v1beta1

printf $(kubectl get secret --namespace devops jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo

user: admin
pass: xxxx

- kubectl create clusterrolebinding sa-devops-role-clusteradmin --clusterrole=cluster-admin --serviceaccount=devops:default --namespace=devops
- kubectl create clusterrolebinding sa-devops-role-clusteradmin-kubesystem --clusterrole=cluster-admin --serviceaccount=devops:default --namespace=kube-system

## configuration cert-manager
- kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v0.14.2/cert-manager.yaml
- kubectl create namespace cert-manager
- helm repo add jetstack https://charts.jetstack.io
- helm repo update
- helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v0.14.2


# docker registry
- kubectl create secret docker-registry oauth-docker --docker-server=<host> --docker-username=<username> --docker-password=<password> --docker-email=<email>
