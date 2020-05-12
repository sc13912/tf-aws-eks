# tf-aws-eks
Terraform Example for building a Multi-AZ EKS Cluster on AWS

# RUN AWS Config & Terraform plan
aws configure  
terraform init && terraform apply  
aws eks --region us-east-1 update-kubeconfig --name xxx  

# Deploy K8s Dashboard
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended.yaml  
kubectl apply -f ./kube-dashboard/  

# Retrieve Dashboard User Token
SA_NAME=admin-user  
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep ${SA_NAME} | awk '{print $1}')  

# Deploy K8s Metrics-Server
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.6/components.yaml  

# Deploy Storage Classes (Persistent Storage)
kubectl apply -f ./storage/storageclass/  

# Optional - Deploy CSI Driver (follow below guide)
https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html

# Deploy Demo-App: Guestbook
kubectl create ns guestbook-app
kubectl apply -f ./demo-apps/guestbook/  

# Deploy Demo-App: GCP HispterShop
kubectl apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/microservices-demo/master/release/kubernetes-manifests.yaml  





 
