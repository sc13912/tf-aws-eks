# tf-aws-eks
Terraform Example for building a Multi-AZ EKS Cluster on AWS

## Prerequisites
* Install Git, Kubectl, AWSCLI, AWS-IAM-Authenticator & Terraform
* Check NTP clock sync status on your client!
* Clone the Repo
```
git clone https://github.com/sc13912/tf-aws-eks.git
```

## Prepare the AWS Envrionment 
```
aws configure 
```

## Deploy a AWS EKS Cluster and add-on services
### Run the terraform script to deploy a EKS cluster
```
terraform init
terraform apply  
```

### Register cluster and update kubeconfig file (in order to use kubectl)
```
aws eks --region us-east-1 update-kubeconfig --name "replace-with-cluster-name"  
```

### Deploy K8s Dashboard
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended.yaml  
kubectl apply -f ./kube-dashboard/  
```

### Retrieve Dashboard User Token
```
SA_NAME=admin-user  
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep ${SA_NAME} | awk '{print $1}')  
```

### Deploy K8s Metrics-Server
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.6/components.yaml  
```

### Deploy Storage Classes (Persistent Storage)
```
kubectl apply -f ./storage/storageclass/  
```

### Deploy NGINX Ingress Controller
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-0.32.0/deploy/static/provider/aws/deploy.yaml   
```

### Optional - Deploy CSI Driver (follow the below guide)
```
https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html  
```

## Deploy demo apps for testing
### Deploy Demo-App1: GCP HispterShop
```
kubectl apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/microservices-demo/master/release/kubernetes-manifests.yaml  
```

### Deploy Demo-App2: Guestbook (verify Storage Classes)
```
kubectl create ns guestbook-app  
kubectl apply -f ./demo-apps/guestbook/    
```

### Deploy Demo-App3: Yelb (verify Ingress Controller)
```
kubectl create ns yelb  
kubectl apply -f ./demo-apps/yelb/  
```






 
