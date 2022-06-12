# Kubernetes API Internals

- APIs in Kubernetes are grouped into various API groups (20+ groups)

    - apps (Deployments, ReplicaSets, StatefulSets)
    - batch
    - core

- APIs are versioned (v1beta1, v1beta2, v1)
- Kinds are objects such as Pod, Deployment, Statefulset
- Each kind exists in a group and the relationship is called Group-Version-Kind(GVK)

For example, `apps.v1/Deployment`
| Group | Version | Kind |
|-------|---------|------|
| apps  |    v1   | Deployment |