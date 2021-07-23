## What is this?
Cloudformation that builds a Kubernetes Cluster on a high availability
AWS cloud infrastructure, something like this:


## Requirements
- aws account


## Installation
1. Set up the VPC and the subnets in a stack.
```bash 
aws cloudformation create-stack --stack-name eks-vpc --region us-east-1 --template-body file://network.yml --parameters file://network-params.json
```
Pick stack names and a region of your liking.

2. Wait until for the stack to complet and set up the 
Kubernetes cluster
```
aws cloudformation create-stack --stack-name eks --region us-east-1 --template-body file://cluster.yml --parameters file://cluster-params.json --capabilities CAPABILITY_NAMED_IAM
```
This second stack will take approx 15 minutes to create.  
## Usage
Use it as any other kubernetes cluster. A refereence deployment is provided here.
This deployment will create additonal resources in AWS.
- EC2 instances (automatic creation and removal)
- S3 bucket for the template (to be deleted manually when not needed)
- A Load balanceer (to be deleted manually when not needed)

### Update the resources
```bash
aws cloudformation update-stack --stack-name eks --region us-east-1 --template-body file://cluster.yml --parameters file://cluster-params.json --capabilities CAPABILITY_NAMED_IAM
```

## Clean-up
Tear down your resources like this
```bash
aws cloudformation delete-stack --stack-name eks --region us-east-1
```
Wait for CloudFormation to delete your stack, otherwise the next command will eventually fail when trying to delete dependencies.
```bash
aws cloudformation delete-stack --stack-name eks-vpc --region us-east-1
```
---
**NOTE**  
You have to delete the load balancer and the S3 bucket manually.  
---


