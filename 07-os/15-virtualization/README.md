# Virtualization

Practical, structured notes for understanding and managing **virtualization, hypervisors, virtual machines, containers, virtual networking, virtual storage, and device passthrough**.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Virtualization Basics|Virtualization concepts, hardware abstraction, resource allocation, virtual resources, and common use cases|
|02|Hypervisors|Hypervisor architecture, virtualization layers, CPU and memory virtualization, VM management, and hypervisor responsibilities|
|03|Type 1 vs Type 2|Type 1 and Type 2 hypervisors, architecture differences, deployment models, isolation, and use cases|
|04|Virtual Machines|VM architecture, virtual hardware, guest operating systems, virtual CPUs, memory, disks, snapshots, and VM lifecycle|
|05|Containers|Container concepts, operating-system-level virtualization, namespaces, cgroups, images, isolation, and container workloads|
|06|Networking|Virtual switches, bridges, NAT, virtual network interfaces, routing, network isolation, and VM/container connectivity|
|07|Storage|Virtual disks, storage pools, disk images, snapshots, thin provisioning, storage backends, and virtual storage management|
|08|Device Passthrough|PCI passthrough, USB passthrough, GPU passthrough, IOMMU, device assignment, and hardware access from virtual machines|

## Structure

The notes progress from **virtualization fundamentals → hypervisors → Type 1 and Type 2 architectures → virtual machines → containers → virtual networking → virtual storage → device passthrough**.

The early topics establish how virtualization abstracts physical hardware and how hypervisors provide the execution environment for virtual machines. The VM and container sections explain two major approaches to workload isolation and resource virtualization.

The later sections focus on practical infrastructure concerns, including networking between virtual workloads, virtual storage management, and direct assignment of physical devices through passthrough technologies.

Each topic is separated into focused notes for easier learning, reference, troubleshooting, and expansion.

## Focus

- Virtualization fundamentals
- Hardware abstraction
- Resource allocation and virtualization
- Hypervisor architecture
- CPU and memory virtualization
- Type 1 and Type 2 hypervisors
- Virtual machine architecture
- Virtual hardware
- Guest operating systems
- Virtual CPUs and memory
- Virtual disks and snapshots
- VM lifecycle management
- Containerization
- Namespaces and cgroups
- Container isolation
- Virtual switches and bridges
- NAT and virtual networking
- VM and container network connectivity
- Virtual disks and storage pools
- Disk images and snapshots
- Thin provisioning
- PCI and USB passthrough
- GPU passthrough
- IOMMU and device assignment

## Goal

> Build a practical virtualization knowledge base that makes it easy to understand how workloads are virtualized and isolated, manage virtual machines and containers, configure virtual networking and storage, and use hardware passthrough when direct device access is required.
