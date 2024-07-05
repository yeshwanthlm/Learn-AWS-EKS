# Learn-AWS-EKS

## Step 01: Create EKS Cluster using eksctl

```bash
# Create Cluster
eksctl create cluster --name=amcdemo \
                      --region=us-east-1 \
                      --zones=us-east-1a,us-east-1b \
                      --without-nodegroup                  
```

```bash
# Get List of clusters
eksctl get cluster
```

## Step 02: Create & Associate IAM OIDC Provider for our EKS Cluster
* To enable and use AWS IAM roles for Kubernetes service accounts on our EKS cluster, we must create & associate OIDC identity provider.
* To do so using eksctl we can use the below command.

```bash
# Template
eksctl utils associate-iam-oidc-provider \
    --region region-code \
    --cluster <cluter-name> \
    --approve

# Replace with region & cluster name
eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster amcdemo \
    --approve
```

## Step 03: Create EC2 Keypair
* Create a new EC2 Keypair with name as amc-demo
* This keypair we will use it when creating the EKS NodeGroup.
* This will help us to login to the EKS Worker Nodes using Terminal.

## Step 04: Create Node Group with additional Add-Ons in Public Subnets
These add-ons will create the respective IAM policies for us automatically within our Node Group role.

```bash
# Create Public Node Group   
eksctl create nodegroup --cluster=amcdemo \
                       --region=us-east-1 \
                       --name=amcdemo-ng-public1 \
                       --node-type=t3.medium \
                       --nodes=2 \
                       --nodes-min=2 \
                       --nodes-max=4 \
                       --node-volume-size=20 \
                       --ssh-access \
                       --ssh-public-key=amc-demo \
                       --managed \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --alb-ingress-access
```

## Step 05: Verify Cluster & Nodes

```bash
# List EKS clusters
eksctl get cluster

# List NodeGroups in a cluster
eksctl get nodegroup --cluster=<clusterName>

# List Nodes in current kubernetes cluster
kubectl get nodes -o wide

# Our kubectl context should be automatically changed to new cluster
kubectl config view --minify
```

## Step 06: Delete Node Group

```bash
# List EKS Clusters
eksctl get clusters

# Capture Node Group name
eksctl get nodegroup --cluster=<clusterName>
eksctl get nodegroup --cluster=amcdemo

# Delete Node Group
eksctl delete nodegroup --cluster=<clusterName> --name=<nodegroupName>
eksctl delete nodegroup --cluster=amcdemo --name=amcdemo-ng-public
```

## Step 07: Delete Cluster
```bash
# Delete Cluster
eksctl delete cluster <clusterName>
eksctl delete cluster amcdemo
```

