# Replication

Practical, structured notes for understanding and implementing **database replication, read scaling, high availability, and failover**.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Replication Basics|Replication concepts, replication models, use cases, benefits, limitations, and replication architecture|
|02|Primary & Replica|Primary-replica architecture, data flow, replication roles, replica management, and topology|
|03|Synchronous Replication|Synchronous replication, write acknowledgment, consistency, latency, and availability trade-offs|
|04|Asynchronous Replication|Asynchronous replication, replication lag, eventual consistency, and performance characteristics|
|05|Read Replicas|Read replicas, read scaling, routing read traffic, replica lag, and read consistency|
|06|Failover|Failover concepts, automatic and manual failover, leader election, recovery, and failback|

## Structure

The notes progress from **replication fundamentals → primary-replica architecture → synchronous and asynchronous replication → read replicas → failover and recovery**.

The early topics establish how replication works and how database nodes coordinate, while later topics focus on read scaling, consistency, availability, failure handling, and recovery.

Each topic is separated into focused notes for easier learning, reference, and expansion.

## Focus

- Database replication fundamentals
- Replication architectures
- Primary and replica roles
- Replication data flow
- Synchronous replication
- Asynchronous replication
- Replication lag
- Consistency and availability trade-offs
- Read replicas
- Read scaling
- Read routing
- High availability
- Automatic and manual failover
- Leader election
- Recovery and failback
- Replication monitoring and reliability

## Goal

> Understand database replication, choose appropriate replication models, scale read workloads, and build reliable systems that can continue operating through database failures.
