# Getting started with Kubernetes

## Imperative vs Declarative Systems

Before beginning with Kubernetes, it is important to understand the difference between Imperative and Declarative systems since Kubernetes is a declarative system.

#### Imperative System

- Describe HOW to achieve something
- Has a list of steps listed to achieve a goal
- Provides more control of how things are done
- Involves the process of achieving the desired state

#### Declarative System

- Describe WHAT to achieve
- Just the end goal is defined
- Provides more abstraction about the things that are done
- Involves only the specification of the desired state, the rest is handled by the system.

Usually, a declarative system has an underlying imperative system.

A declarative system has the following components - 

- Machine - The unit performing the actual work (API Server in Kubernetes)
- Desired State - The state of the machine expected (yaml files from the user perspective and data stored in etcd from the system perspective in Kubernetes)
- System State - The state of the machine at any given time (data stored in etcd in Kubernetes)
- Feedback - The continous assessment of the system (Informers(ListWatch) in Kubernetes)

## Some Kubernetes concepts

- Pod - Fundamental unit of k8s, a group of containers
- ReplicaSets - Ensures certain number of pod copies
- Deployment - To create and update Pods and ReplicaSets
- Node - Worker machine
- API Server - Exposes K8s API and acts as a frontend i.e, communicates with the end user
- etcd - Internal database for storing data
- Controllers - The agents responsible for the declarative state. Each controller is independent and responsible for their tasks.
    - scheduler controller - Assigns nodes to pods
    - deployment controller - Watch the deployments defined in etcd for changes. It is not responsible for managing the pods but responsible for managing the replicasets which in turn manage the pods
    - replicaset controller - Watch the replicasets defined in etcd for changes
- kubelet - Manages objects(pods) on a node. kubelet has a local cache like other controllers and makes sure it maintains the desired state in a node using that data. The cache is periodically synced with etcd.

Kubernetes makes sure the desired state is eventually achieved. Kubernetes is also extensible which means the same set of APIs can be used to achieve a desired state of any object.

## Brief overview of the Kubernetes architecture

Kubernetes consists of 2 types of nodes -
- Master node
- Worker node

Each node should contain the following 3 processes-
- kubelet
- kube-proxy (Intelligently manages networking between the pods in a node)
- Container runtime

Master processes - 
- API Server (A cluster gateway and provides authentication for requests)
- Scheduler
- Controller manager (Detect state changes in nodes)
- etcd (Key-value store of the cluster state)

There can be multiple master nodes where the API server is load balanced and the etcd is implemented as a distributed storage across all master nodes.

## References - 

- [Imperative, Declarative and Kubernetes](https://www.youtube.com/watch?v=hB3H4_YRnFc)
- [Kubernetes Architecture explained](https://www.youtube.com/watch?v=umXEmn3cMWY)


> Note: Few of the wordings in this gist are directly from the talks listed in the references and not my
own.