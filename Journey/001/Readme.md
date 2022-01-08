# Day - 1 (100DaysOfKubernetes)

## Containers

I have worked with containers for a while now and I am capable of working with Docker containers using the CLI and [compose](https://docs.docker.com/compose/). I also have a fundamental understanding how containers work using namespaces, cgroups and chroot. Here are some of the resources that I went through recently to learn about these concepts - 

- [Containers From Scratch](https://www.youtube.com/watch?v=8fi7uSYlOdc)
- [Intro to namespaces](https://www.youtube.com/watch?v=-YnMr1lj4Z8)

If you want to follow along, I would suggest learning Docker and the above concepts first before diving into the next set of content. I learnt Docker through hands-on at an internship and a lot of online resources. I would suggest the following resources although I have not gone through them myself but are taught by excellent instructors - 

- [Docker Tutorial for Beginners by Tech with Nana](https://www.youtube.com/watch?v=3c-iBn73dDE)
- [Docker Tutorial for Beginners by KodeKloud](https://www.youtube.com/watch?v=fqMOX6JJhGo)

### Container runtimes

Before I dive into Kubernetes with my current knowledge of containers, I will explore container runtimes for my Day 1.

Here is a gist of what I learnt on Day 1

- Docker standardized the container process and and solved many of the problems that developers had running containers end-to-end. Docker along with other companies created **Open Container Initiative (OCI)** and donated the underlying component that _runs_ containers to serve as a standard.
- There are two kinds of container runtimes -
    - High-level container runtime (image transport, image management, image unpacking, and APIs)
        - containerd (Docker)
        - cri-o
    - Low-level container runtime (running the actual container)
        - runc
        - rkt
        - lmctfy
- Low-level containers are responsible for creating the namespaces, cgroups, filesystem and running the cgroup in the new namespaces with the cgroups and the new filesystem. It is also responsible for destroying the cgroups at the end.
- Building a sample runtime using linux commands like _cgcreate_(create cgroup), _cgset_(set parameters of cgroup), _cgexec_(run task in given cgroup), _chroot_(change root) and _unshare_(run process in given new namespaces)
- Some low level containers -
    - **lmctfy** allows creation of path based sub containers(cgroups) and the child containers are limited by the parent cgroup.
    - **runc** follows OCI, which means it runs from specific [OCI bundle](https://github.com/opencontainers/runtime-spec)
    - **rkt** provides both high-level and low-level features. Orginally used appc container but later discontinued to use OCI.
- High-level runtimes are responsible for transport and management of container images, unpacking the image, and passing off to the low-level runtime to run the container.
- **containerd** is focused solely on running containers, so it does not provide a mechanism for building containers.
- **rkt** provides most of Docker's functionalities but is no longer actively developed. This means that Docker is the only popular tool that provides all of the features for running a container.
- **Container Runtime Intereface(CRI)** was introduced by Kubernetes to implement a standard for high level runtimes for Kubernetes and acts as a bridge between the kubelet and the container runtime.
- containerd, Docker(using a shim, which is now set to be deprecated) and cri-o implement CRI.
- CRI defines several RPCs(remote procedure calls) for actions like "pull image", "create pod", "start container", "stop container", etc.
- **crictl** can be used to interact with CRI without having the need for kubelet or a k8s cluster. crictl can be configured with the runtime's gRPC endpoint.

I would highly suggest going through the articles in the [References](#References) section for a complete understanding.

## References

- https://www.ianlewis.org/en/container-runtimes-part-1-introduction-container-r
- https://www.ianlewis.org/en/container-runtimes-part-2-anatomy-low-level-contai
- https://www.ianlewis.org/en/container-runtimes-part-3-high-level-runtimes
- https://www.ianlewis.org/en/container-runtimes-part-4-kubernetes-container-run

> Note: Few of the wordings in this gist are directly from the articles listed in the references and not my
own.

Huge thanks to [Anais](https://github.com/AnaisUrlichs) and [Rishab](https://github.com/rishabkumar7 ) for setting up the challenge and [Ian Lewis](https://twitter.com/IanMLewis) for the well written articles.