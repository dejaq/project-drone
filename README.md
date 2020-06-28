## Project Drone

#### SW Requirements
- Programmatic setup and teardown of different broker, producers, consumers configurations 
- Support of major k8s cloud native providers
- Flow like way of designing and running tests
- Simulate network problems 

#### HW Requirements
- MVP broker configuration: 3 x broker nodes behind an Application LB
- fast SSD HDD and higher number of CPUs

#### Proposed tools
- **Dagster** with **Dagit** DAG -  programmatic flows design framework with a powerful web UI development environment
    - project: https://github.com/dagster-io/dagster
    - docs: https://docs.dagster.io/
- **Ray** - framework for building and running distributed applications.
    - project: https://github.com/ray-project/ray
    - docs:  https://docs.ray.io/en/latest/
- **cdk8s** - framework for defining Kubernetes applications and reusable abstractions programmatically
    - https://cdk8s.io/
    - getting started - https://github.com/awslabs/cdk8s/blob/master/docs/getting-started/python.md
- **Octant** - k8s cluster visualisation tool
    - https://octant.dev/
    - project: https://github.com/vmware-tanzu/octant
- **TektonCD** - k8s native CI/CD pipeline 
    - project: https://github.com/tektoncd/pipeline
    - docs: https://github.com/tektoncd/pipeline/blob/master/docs/tutorial.md
- **tcconfig** - linux traffic shaping tool (wrapper to tc command)
    - project: https://pypi.org/project/tcconfig/
    - docs: https://tcconfig.rtfd.io/

#### Arhitecture Diagram
![Diagram](project_drone.png)


#### Tools k8s setup
##### Dagster
###### Installation
- Installation details using helm chart - https://docs.dagster.io/docs/deploying/k8s
- Core packages
    - *dagster* - Contains the core programming model, which you'll use to write solids, pipelines, and all the other components 
    of a data application, as well as the dagster CLI tool for executing and managing pipelines. 
    - *dagit* - GUI tool for visualizing, testing, scheduling, running, and monitoring Dagster pipelines, written against the GraphQL API.
    - *dagster-graphql* -  GraphQL API for executing pipelines and includes a CLI tool for executing queries against the API
###### Concepts
- **Solid** -  a solid is a functional unit of computation with defined inputs and outputs
- **Pipeline** - in Dagster, pipelines are directed acyclic graphs (DAGs) of solids – that is, they are made up of a number 
of solids which have data dependencies on each other (but no circular dependencies).
- **Output** - an output is how a solid’s compute function communicates the name and value of an output to Dagster.
- **Dependencies** - solids are linked together into pipelines by defining the dependencies between their inputs and outputs. 
An important difference between Dagster and other workflow systems is that in Dagster, dependencies are expressed as data 
dependencies ("A requires a particular output from B"), not only ordering dependencies ("A runs after B").

More information on the concepts: https://docs.dagster.io/docs/learn/concepts
##### Ray
###### Installation
- Installation details - https://docs.ray.io/en/master/deploy-on-kubernetes.html
- k8s yml files used for setup - https://github.com/ray-project/ray/tree/master/doc/kubernetes 
- Installation will create:
    - a **ray-head Kubernetes Service** that enables the worker nodes to discover the location of the head node on start up.
    - a **ray-head Kubernetes Deployment** that backs the ray-head Service with a single head node pod (replica).
    - a **ray-worker Kubernetes Deployment** with multiple worker node pods (replicas) that connect to the ray-head pod using the ray-head Service.
###### Running Ray Programs
- There are three options to run a ray program (https://docs.ray.io/en/master/deploy-on-kubernetes.html#running-ray-programs):
    - using kubectl exec to run a Python script.
    - using kubectl exec -it bash to work interactively in a remote shell.
    - submitting a Kubernetes Job (https://docs.ray.io/en/master/deploy-on-kubernetes.html#submitting-a-job). The Job will 
    run a single pod running the Ray driver program to completion, then terminate the pod but allow you to access the logs.
##### cdk8s
###### Installation
- Local installation - https://github.com/awslabs/cdk8s/blob/master/docs/getting-started/python.md
###### Concepts
Apps are structured as a tree of constructs, which are composable units of abstraction. The initial code created by _cdk8s init_
 defines an app with a single, empty, chart. When you run _cdk8s synth_, a Kubernetes manifest YAML will be synthesized for each Chart 
 in your app and will write it to the dist directory.Similarly to charts and apps, Kubernetes API Objects are also represented in cdk8s as constructs. 
 The manifest synthesized by your app is ready to be applied to any Kubernetes cluster using standard tools like kubectl apply.
Eg.
`$ kubectl apply -f dist/hello.k8s.yaml`
##### Octant
###### Installation
- Local installation - https://github.com/vmware-tanzu/octant#installation
###### Running
- First check cluster access with `kubectl cluster-info`
- Start running Octant with `octant` coommand. This should open your default browser to `127.0.0.1:7777`
##### TektonCD
###### Installation
- Installation walkthrough - https://github.com/tektoncd/pipeline/blob/master/docs/install.md#installing-tekton-pipelines-on-kubernetes
- Artifact storage - https://github.com/tektoncd/pipeline/blob/master/docs/install.md#configuring-a-cloud-storage-bucket
###### Management
- tkn CLI - https://github.com/tektoncd/cli
###### Concepts
- A **Task** defines a series of steps that run in a desired order and complete a set amount of build work. Every Task runs 
as a Pod on your Kubernetes cluster with each step as its own container 
- A **Pipeline** defines an ordered series of Tasks that you want to execute along with the corresponding inputs and outputs 
for each Task. You can specify whether the output of one Task is used as an input for the next Task using the from property
##### tcconfig
###### Installation
- `pip install tcconfig` will install the package
- requirements
     - python >=3.5
     - `iproute2` or `iproute-tc` linux package depending on platform
- more info: https://pypi.org/project/tcconfig/#installation-
###### Running 
- tcconfig should be installed on the image deployed on the pods. 
- container needs to be started with `--cap-add NET_ADMIN`
- available parameters:
    - Network bandwidth rate [G/M/K bps]
    - Network latency [microseconds/milliseconds/seconds/minutes]
    - Packet loss rate [%]
    - Packet corruption rate [%]
    - Packet duplicate rate [%]
    - Packet reordering rate [%]