# Cloud

Practical, step-by-step **cloud recipes** for deploying applications to AWS, Azure, and GCP, configuring cloud storage, databases, networking, IAM, and monitoring.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Deploy Node.js App to AWS|Prepare a Node.js application, configure AWS infrastructure, deploy the application, configure runtime settings, expose the service, and verify the deployment|
|02|Deploy Node.js App to Azure|Prepare a Node.js application, configure Azure resources, deploy the application, configure runtime settings, expose the service, and verify the deployment|
|03|Deploy Node.js App to GCP|Prepare a Node.js application, configure GCP resources, deploy the application, configure runtime settings, expose the service, and verify the deployment|
|04|Setup Cloud Storage|Create cloud storage resources, configure buckets or containers, manage access permissions, upload and retrieve objects, and verify storage configuration|
|05|Setup Cloud Database|Provision a managed cloud database, configure connectivity, users, security, backups, and database settings, and verify application access|
|06|Setup Cloud Network|Configure virtual networks, subnets, routing, security rules, private and public access, and connectivity between cloud resources|
|07|Setup Cloud IAM|Create users, groups, roles, and policies, assign permissions according to required access, configure credentials, and verify authorization|
|08|Configure Cloud Monitoring|Enable cloud monitoring, collect metrics and logs, configure dashboards and alerts, monitor resources, and verify monitoring coverage|

## Structure

The recipes progress from **application deployment → cloud storage → managed databases → cloud networking → identity and access management → cloud monitoring**.

The first three recipes focus on deploying a Node.js application across AWS, Azure, and GCP. Each provides a provider-specific deployment workflow while following the common process of preparing the application, configuring cloud resources, deploying the service, and verifying the result.

Cloud storage and managed databases then introduce commonly required managed services for application data. These recipes cover resource configuration, access control, connectivity, persistence, backups, and operational settings.

Cloud networking focuses on connecting and isolating cloud resources through virtual networks, subnets, routing, and security rules. IAM then addresses identity, roles, policies, credentials, and least-privilege access to cloud resources.

The final recipe covers cloud monitoring through metrics, logs, dashboards, alerts, and resource-health visibility, providing an operational foundation for deployed applications and infrastructure.

Each recipe follows an **action-oriented workflow** with commands, configuration, expected results, verification steps, troubleshooting guidance, and important safety considerations.

## Focus

- Node.js application deployment on AWS, Azure, and GCP
- Cloud storage configuration
- Managed cloud databases
- Cloud networking and connectivity
- IAM users, roles, policies, and permissions
- Cloud security and access control
- Cloud metrics, logs, dashboards, and alerts
- Application and infrastructure monitoring

## Goal

> Build a practical cloud recipe knowledge base that makes application deployment, managed services, networking, identity and access management, monitoring, and common cloud-operations procedures easy to follow, verify, troubleshoot, and reproduce.
