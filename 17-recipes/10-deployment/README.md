# Deployment

Practical, step-by-step **deployment recipes** for deploying Node.js, Python, Docker, and static applications, configuring Nginx, systemd, domains, HTTPS, zero-downtime releases, and deployment rollbacks.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Deploy Node.js App|Prepare and build a Node.js application, configure the production environment, deploy the application, start the service, and verify the deployment|
|02|Deploy Python App|Prepare and build a Python application, configure the production environment, install dependencies, deploy the application, start the service, and verify the deployment|
|03|Deploy Docker App|Build a production Docker image, configure runtime settings, deploy the container, manage environment variables and volumes, expose the application, and verify the deployment|
|04|Deploy Static Site|Build a static website, prepare production assets, configure a web server or hosting environment, publish the files, and verify the deployed site|
|05|Deploy with Nginx|Install and configure Nginx, create a server configuration, route requests to an application, configure ports, manage static assets, and verify the deployment|
|06|Deploy with systemd|Create a systemd service for an application, configure startup and restart behavior, manage environment settings, enable the service, and verify application availability|
|07|Configure Domain|Configure DNS records, connect a domain to the deployment server, verify DNS resolution, configure the web server, and validate domain access|
|08|Configure HTTPS|Obtain and configure TLS certificates, enable HTTPS, redirect HTTP traffic, configure secure web-server settings, and verify certificate and HTTPS behavior|
|09|Zero-Downtime Deployment|Prepare multiple application instances or release versions, coordinate traffic switching, perform health checks, deploy new releases, and minimize service interruption|
|10|Rollback Deployment|Identify failed releases, restore a previous application version, revert configuration changes where required, verify service health, and document rollback procedures|

## Structure

The recipes progress from **application deployment → Docker and static-site deployment → Nginx and systemd → domain configuration → HTTPS → zero-downtime deployment → rollback**.

The first four recipes cover deploying common application types, including Node.js, Python, Docker-based, and static applications. These establish the basic workflow of preparing an application, configuring its production environment, starting the service, and verifying availability.

Nginx and systemd then introduce two common components of a production deployment. Nginx handles web-server and reverse-proxy responsibilities, while systemd provides service management, automatic startup, restart behavior, and process supervision.

Domain and HTTPS configuration extend the deployment with public access and encrypted communication. These recipes cover DNS, domain routing, TLS certificates, HTTP-to-HTTPS redirection, and verification of secure access.

The final recipes focus on production reliability through zero-downtime releases and rollback procedures. They provide structured workflows for deploying new versions while minimizing interruption and recovering safely when a release fails.

Each recipe follows an **action-oriented workflow** with commands, configuration, expected results, verification steps, troubleshooting guidance, and important safety considerations.

## Focus

- Node.js and Python application deployment
- Docker application deployment
- Static-site deployment
- Nginx web-server and reverse-proxy configuration
- systemd application service management
- Domain and DNS configuration
- HTTPS and TLS configuration
- Zero-downtime deployment strategies
- Deployment rollback and recovery

## Goal

> Build a practical deployment recipe knowledge base that makes application releases, server configuration, domain and HTTPS setup, production service management, zero-downtime deployments, and rollback procedures easy to follow, verify, troubleshoot, and reproduce.
