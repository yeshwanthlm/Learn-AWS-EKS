# Learn-AWS-EKS

Step 01: Create EKS Cluster using eksctl

```bash
# Create Cluster
eksctl create cluster --name=eksdemo1 \
                      --region=us-east-1 \
                      --zones=us-east-1a,us-east-1b \
                      --without-nodegroup                  
```

```bash
# Get List of clusters
eksctl get cluster
```
