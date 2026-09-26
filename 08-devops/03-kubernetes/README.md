# Kubernetes

Practical, structured notes for learning, deploying, networking, securing, configuring, scaling, and troubleshooting **Kubernetes clusters and workloads**.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Fundamentals|Kubernetes basics, cluster architecture, control plane, nodes, and `kubectl`|
|02|Workloads|Pods, Deployments, ReplicaSets, StatefulSets, DaemonSets, Jobs, and CronJobs|
|03|Services|Kubernetes Services, ClusterIP, NodePort, LoadBalancer, and service discovery|
|04|Networking|Kubernetes networking, Ingress, NetworkPolicies, and DNS|
|05|Storage|Volumes, PersistentVolumes, PersistentVolumeClaims, and StorageClasses|
|06|Configuration|ConfigMaps, Secrets, and environment configuration|
|07|Security|RBAC, ServiceAccounts, security contexts, and network policies|
|08|Helm|Helm fundamentals, charts, values, and templates|
|09|Troubleshooting|Pod, service, network, and resource debugging|

## Structure

The notes progress from **Kubernetes fundamentals → workloads → services and networking → storage and configuration → security → Helm → troubleshooting**.

The early sections establish Kubernetes architecture and the core workload resources used to deploy applications. Services and networking then explain how workloads communicate with each other and how applications are exposed to clients.

Storage and configuration cover persistent data and application settings, while security focuses on access control, identities, security contexts, and network policies. Helm introduces package management and reusable Kubernetes configurations.

The final section provides practical approaches for diagnosing pod, service, networking, and resource-related problems.

Each topic is separated into focused notes for easier learning, reference, troubleshooting, and expansion.

## Focus

- Kubernetes fundamentals
- Cluster architecture
- Control plane and worker nodes
- `kubectl`
- Pods and workloads
- Deployments and ReplicaSets
- StatefulSets and DaemonSets
- Jobs and CronJobs
- Kubernetes Services
- ClusterIP, NodePort, and LoadBalancer
- Service discovery
- Kubernetes networking
- Ingress
- NetworkPolicies
- Kubernetes DNS
- Volumes and persistent storage
- PersistentVolumes and PersistentVolumeClaims
- StorageClasses
- ConfigMaps and Secrets
- Environment configuration
- RBAC and ServiceAccounts
- Security contexts
- Network security
- Helm and Helm charts
- Helm values and templates
- Pod debugging
- Service and network debugging
- Resource troubleshooting

## Goal

> Build a practical Kubernetes knowledge base that makes it easy to understand cluster architecture, deploy and manage workloads, expose applications, configure networking and storage, secure clusters, package applications with Helm, and troubleshoot Kubernetes problems.
