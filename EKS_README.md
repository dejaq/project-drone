### Steps to configure EKS (Amazon Elastic Kubernetes Engine)
Free tier offers only 500MB/month of ECR (Elastic Container Registry) storage. You can apply for a 300USD AWS credit by 
by submitting an application
#### Steps to create cluster 
- eksctl utility provide a way to configure and manage EKS clusters (https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)
- prerequisites 
    - install awscli (https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
    - generate and configure aws credentials  (https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)
    - install kubectl (https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- create cluster with command 
```
eksctl create cluster \
--name prod \
--version 1.16 \
--region us-west-2 \
--nodegroup-name standard-workers \
--node-type t3.medium \
--nodes 3 \
--nodes-min 1 \
--nodes-max 4 \
--ssh-access \
--ssh-public-key my-public-key.pub \
--managed
```

- get kube config with command `aws eks --region region-code update-kubeconfig --name cluster_name`
- in case you have multiple contexts you need switch context with `kubectl config use-context cluster-name`
- check kube config is functional by running `kubectl cluster-info`
- rest of the configs are done using kubectl