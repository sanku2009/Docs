# Setup Kubernetes on Amazon EKS

You can follow same procedure in the official  AWS document [Getting started with Amazon EKS – eksctl](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)   

#### Pre-requisites: 
  - an EC2 Instance (SAN_EKS_MANAGEMENT_SERVER) -- can use t2.micro, create new key pair

Once server is started login to server using SSH tool MobeXtrem, use the newly created and downloaded .pem file

execute -- [ sudo su -  ]

# https://docs.aws.amazon.com/eks/latest/userguide/clusters.html -- check the prerequisite
1. Install and setup kubectl on Management host  ---  https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html#eksctl-install-update
   a. Download kubectl version 1.31.2 
   b. Grant execution permissions to kubectl executable   
   c. Move kubectl onto /usr/local/bin   
   d. Test that your kubectl installation was successful    
   ```sh 
   curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.31.2/2024-11-15/bin/linux/amd64/kubectl

    ls -ltr

   chmod +x ./kubectl 

   mv ./kubectl /usr/local/bin -- move in this location, so that execute the command from anywhere inside the srver

   kubectl version --- to check the version
   ```
2. Install and setup eksctl on Management Host   
   a. Download and extract the latest release   
   b. Move the extracted binary to /usr/local/bin   
   c. Test that your eksclt installation was successful   
   ```sh
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv /tmp/eksctl /usr/local/bin
   eksctl version
   ```
  
3. Create an IAM Role and attache it to EC2 instance Management Host. this is required, so that SAN_EKS_MANAGEMENT_SERVER EC2 instance can access and set up EKS cluster. While creating the IAM role select AWS service and EC2 as the EC2 server SAN_EKS_MANAGEMENT_SERVER needs this roles
SAN_EKS_MANAGEMENT_SERVER_IAM_ROLES
   `Note: create IAM user with programmatic access if your bootstrap system is outside of AWS`   
   IAM user should have access to   
   IAM   ----  IAMFullAccess
   EC2   ---- AmazonEC2FullAccess
   VPC   ---- AmazonVPCFullAccess
   CloudFormation  ----- AWSCloudFormationFullAccess
   AdministratorAccess  --- better to have that also as just to avoide any access issue while creating the EKS cluster

4. Create EKS cluster and nodes from EC2 Management Host
   ```sh
   eksctl create cluster --name cluster-name  \
   --region region-name \
   --node-type instance-type \
   --nodes-min 2 \
   --nodes-max 2 \ 
   --zones <AZ-1>,<AZ-2>
   
   example:
eksctl create cluster --name san-eks-cluster1 --region us-east-1 --node-type t2.medium
    ```

5. To delete the EKS clsuter 
   ```sh 
   eksctl delete cluster san-eks-cluster1 --region us-east-1
   ```
   
6. Validate your cluster using by creating by checking nodes and by creating a pod 
some command to execute to validate
kubectl get nodes
kubectl get ns -- show default namespaces
kubectl get pods -n kube-system -- show default pods on kube-system namespace



   ```sh 
   kubectl get nodes
   kubectl run pod tomcat --image=tomcat 
   ```
