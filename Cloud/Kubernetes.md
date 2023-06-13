- K8S -> 8 characters between K and S
- automates deployment, scaling, management of containerized apps
- orchestrator


# Advantages 
- scalable 
- high available
- self healing
- automatic rollouts
- horizontal scaling
- portable: deploy and manage apps in a consistent and reliable way, regardless of the relying infrastructure (GCP, AWS, Azure, Private data center)
- uniform way to deploy and manage apps 

# Disadvantages
- complexity
- upfront cost is high
- requires expertise 
- requires certain minimum level of resources to run 

```
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                                                  │
│ Cluster                                                      ┌─────────────────────────────────────────────────┐ │
│                                                              │                                                 │ │
│                                                              │ Node  ┌──────────────────┐ ┌──────────────────┐ │ │
│                                                              │       │                  │ │                  │ │ │
│                                                              │       │ Pod  ┌─────────┐ │ │ Pod  ┌─────────┐ │ │ │
│                                                              │       │      │         │ │ │      │         │ │ │ │
│                                                              │       │      │Container│ │ │      │Container│ │ │ │
│                                                              │       │      │         │ │ │      │         │ │ │ │
│                                                              │       │      └─────────┘ │ │      └─────────┘ │ │ │
│                                                              │       │                  │ │                  │ │ │
│                                                              │       │      ┌─────────┐ │ │      ┌─────────┐ │ │ │
│                                                              │       │      │         │ │ │      │         │ │ │ │
│                                                              │       │      │Container│ │ │      │Container│ │ │ │
│                                                              │       │      │         │ │ │      │         │ │ │ │
│                                                              │       │      └─────────┘ │ │      └─────────┘ │ │ │
│                                                              │       │                  │ │                  │ │ │
│                                                              │       └──────────────────┘ └──────────────────┘ │ │
│                                                              │                                                 │ │
│                                                              │       ┌───────────────────────────────────────┐ │ │
│                                                              │       │           container runtime           │ │ │
│          ┌────────────────────────────────────┐              │       └───────────────────────────────────────┘ │ │
│          │                                    │              │                                                 │ │
│          │ Control                            │              │       ┌──────────────────┐ ┌──────────────────┐ │ │
│          │ Plane    ┌──────────┐ ┌──────────┐ │              │       │      kubelet     │ │    kube-proxy    │ │ │
│          │          │Controller│ │Scheduler │ │              │       └──────────────────┘ └──────────────────┘ │ │
│          │          │Manager   │ │          │ │              │                                                 │ │
│          │          └──────────┘ └──────────┘ │              └─────────────────────────────────────────────────┘ │
│          │                                    │                                                                  │
│          │          ┌──────────┐ ┌──────────┐ │              ┌─────────────────────────────────────────────────┐ │
│          │          │API       │ │etcd      │ │              │                                                 │ │
│          │          │Server    │ │          │ │              │ Node  ┌──────────────────┐ ┌──────────────────┐ │ │
│          │          └──────────┘ └──────────┘ │              │       │                  │ │                  │ │ │
│          │                                    │              │       │ Pod  ┌─────────┐ │ │ Pod  ┌─────────┐ │ │ │
│          └────────────────────────────────────┘              │       │      │         │ │ │      │         │ │ │ │
│                                                              │       │      │Container│ │ │      │Container│ │ │ │
│                                                              │       │      │         │ │ │      │         │ │ │ │
│                                                              │       │      └─────────┘ │ │      └─────────┘ │ │ │
│                                                              │       │                  │ │                  │ │ │
│                                                              │       │      ┌─────────┐ │ │      ┌─────────┐ │ │ │
│                                                              │       │      │         │ │ │      │         │ │ │ │
│                                                              │       │      │Container│ │ │      │Container│ │ │ │
│                                                              │       │      │         │ │ │      │         │ │ │ │
│                                                              │       │      └─────────┘ │ │      └─────────┘ │ │ │
│                                                              │       │                  │ │                  │ │ │
│                                                              │       └──────────────────┘ └──────────────────┘ │ │
│                                                              │                                                 │ │
│                                                              │       ┌───────────────────────────────────────┐ │ │
│                                                              │       │           container runtime           │ │ │
│                                                              │       └───────────────────────────────────────┘ │ │
│                                                              │                                                 │ │
│                                                              │       ┌──────────────────┐ ┌──────────────────┐ │ │
│                                                              │       │      kubelet     │ │    kube-proxy    │ │ │
│                                                              │       └──────────────────┘ └──────────────────┘ │ │
│                                                              │                                                 │ │
│                                                              └─────────────────────────────────────────────────┘ │
│                                                                                                                  │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

# Control Plane 
- managing state of the cluster

## Controller Manager
- responsible for running controllers that manage the state of the cluster 
	- e.g. ReplicationController: ensures that right amount of replica of a ReplicaSet are running
	- e.g. DeploymentController which manages the rolling update and rollback of deployments

## Scheduler
- responsible for scheduling pods onto the worker nodes in the cluster
- uses information about and the available resources on the worker nodes to make placement decisions  

## etcd
- distributed key value store 
- stores the clusters persistent state 
- other components of the control plane uses etcd to store and retrieve data about the cluster
- 

## API Server
- primary interface between the control plane and the rest of the cluster
- allows clients via REST API to communicate with the control plane and submit requests to manage the cluster


# Worker nodes
- run the containerized workloads

## Pod
- the containers run in Pods
- Pods are the smallest deployable unit in Kubernetes
- provides shared storage and networking for containers

## kubelet
- communicating with the control plane
- it receives instructions from the control plane about which pods to run on the node 
- ensures that the desired state of the pods is maintained 

## kube-proxy
- networking proxy that runs on each worker node 
- routing traffic to the correct pods
- load balancing for the pods and ensures that traffic is distributed evenly across the pods

## container runtime
- runs the container of the worker nodes 
- pulling container images from the registry 
- starting and stopping the containers 
- managing containers ressources




![](attachments/Pasted%20image%2020230609191255.png)