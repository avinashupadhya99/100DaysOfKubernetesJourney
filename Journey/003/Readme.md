# Kubernetes Design Principles

### 1. Declarative API

Primary benefit of declarative over imperative is Automatic Recovery.
In a traditional setup, the master tells the clients what to do and when to do something. In this case, if one of the nodes die, the master becomes very complicated trying to store the state and making sure the commands are sent to the node when it comes up.

### 2. No hidden internal APIs

- Same API exposed to end user and used within components.
- Master defines a desired state
- Nodes work independently to drive itself towards that state. This pattern is called level triggered instead of edge triggered. 
- In edge triggered, the master is responsible for sending the events and sending them again until a node has received it. 
- In level triggered, the nodes are responsible for maintaining the system state by looking at the information in the master.
- In case of level triggered, there are no cases of missing events since the nodes are responsible for collecting the state.
- No single point of failure with the master.
- The master component is much simpler.
- Since Kubernetes uses the same APIs for internal functionality, it is extensible by replacing any component with a custom implementation.

### 3. Meet the user where they are

> #### Kube API Data
>
> - Secrets - Sensitive data stored in KubeAPI
>    - eg. passwords, certificates
> - ConfigMap - Configuration info stored in KubeAPI
>    - eg. application startup parameters
> - DownwardAPI - Pod information in KubeAPI
>    - eg. name/namespace/uid of my current pod


- Make it easy to transition existing applications to Kubernetes
- Secrets can be loaded into pods as volumes/environment variables without the need for modification of how the code reads the secrets.

### 4. Workload Portability
> #### Remote Storage
> 
> - Could directly reference a remote volume (GCE PD, AWS EBS, NFS, etc) in pod definition using plugins.
> 
> #### Attach/Detach controller
> 
> - Watches for pods scheduled with volumes and checks if the node scheduled has the volume available.
> - Ensures the volume is attached to the required node by communicating with the storage backend of the volume and updates the API server that the volume is attached.
> - If the pod is rescheduled to another node, the A/D controller ensures the volume is attached to the new node.
> > Never reference a particular type of storage in a pod. This makes the pod no longer portable.
> 
> This is solved using a Persistent Volume Claim and Persistent Volume. The pods can reference a PVC inside the definition instead of referencing the storage directly. The PV references the storage. 

- Allows applications to be moved from cluster to cluster.
- Make Kubernetes a true abstraction layer for the underlying components like storage, like an OS.
- Cloud agnostic and cluster agnostic.

## References

- [Kubernetes Design Principles: Understand the Why](https://www.youtube.com/watch?v=ZuIQurh_kDk)

> Note: Few of the wordings in this gist are directly from the talks listed in the references and not my
own.