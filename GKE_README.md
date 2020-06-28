### Steps to configure GKE (Google Kubernetes Engine)
You get 300 USD  for 365 days for free trial account (need to provide CC info). You have to change from free trial to 
full account, switch is not done automatically.
#### Steps to create cluster
- create a project (24 max projects available on the free trial). GCP organizes resources into projects and this allows you 
to collect all of the related resources for a single application in one place. 
- create a kubernetes cluster (select a name and a zone) or use the my-first-cluster template (an affordable cluster to experiment with)
  - Cluster name: my-first-cluster-1
  - Cluster zone: us-central1-c
  - Version: Rapid release channel instead of default version
  - Machine type: g1-small instead of n1-standard-1
  - Boot disk size: 32GB instead of 100GB boot disk size
  - Autoscaling: Disabled
  - Kubernetes Engine Monitoring: Disabled
- accessing cluster 
    - from GKE web interface console: activate cloud shell on cluster (Cloud Shell is a built-in command-line tool for the console in browser)
    - locally using Google Cloud SDK: 
        - install kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl/
        - install Google Cloud SDK: https://cloud.google.com/sdk/docs/quickstarts
        - run `gcloud init`
            - authenticate to your account using google sign-in (oauth 2.0)
            - select project to use from interactive menu
            - configure a default zone (same as project)
- get kube config by running: `gcloud container clusters get-credentials [cluster-name] --zone [cluster-zone]`.
- check kube config is functional by running `kubectl cluster-info`
- rest of the configs are done using kubectl
 
