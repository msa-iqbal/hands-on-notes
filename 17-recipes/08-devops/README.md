# DevOps

Practical, step-by-step **DevOps recipes** for setting up CI/CD pipelines, GitHub Actions, container registries, Nginx reverse proxies, SSL, Terraform, Ansible, and application monitoring.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Setup CI Pipeline|Create continuous-integration pipelines, configure triggers, install dependencies, run automated tests, build applications, and publish pipeline results|
|02|Setup CD Pipeline|Create continuous-delivery pipelines, configure deployment stages, manage environments, deploy applications, handle failures, and verify releases|
|03|Setup GitHub Actions|Create workflows, configure triggers, jobs, steps, runners, secrets, environment variables, caching, artifacts, and reusable automation|
|04|Setup Docker Registry|Set up a container registry, configure authentication, create repositories, tag images, push and pull images, and manage registry access|
|05|Setup Nginx Reverse Proxy|Install and configure Nginx as a reverse proxy, route requests to backend services, configure virtual hosts, manage ports, and verify proxy behavior|
|06|Setup SSL with Nginx|Configure HTTPS with Nginx, obtain and install TLS certificates, redirect HTTP to HTTPS, configure secure settings, and verify SSL configuration|
|07|Setup Terraform Project|Create a Terraform project, configure providers, define infrastructure resources, manage variables and state, plan changes, and apply infrastructure configuration|
|08|Setup Ansible Project|Create an Ansible project, configure inventories, define variables, write playbooks, manage hosts, execute tasks, and verify configuration changes|
|09|Setup Monitoring|Set up application and infrastructure monitoring, collect metrics, configure dashboards and alerts, monitor system health, and verify monitoring coverage|

## Structure

The recipes progress from **CI pipelines → CD pipelines → GitHub Actions → container registries → Nginx reverse proxy → SSL → Terraform → Ansible → monitoring**.

The first sections establish automated software delivery through continuous integration and continuous delivery. GitHub Actions then provides a concrete workflow for implementing these automation pipelines with repositories, runners, secrets, artifacts, and reusable workflows.

Container registries and Nginx introduce important deployment infrastructure. The registry recipe covers storing and distributing container images, while Nginx provides reverse-proxy routing between clients and backend services.

SSL configuration then extends the reverse-proxy setup with HTTPS, TLS certificates, HTTP-to-HTTPS redirection, and secure server configuration. Terraform and Ansible introduce infrastructure provisioning and configuration management through infrastructure-as-code and automation.

The final recipe focuses on monitoring applications and infrastructure through metrics, dashboards, alerts, and health checks, providing visibility into deployed systems and operational behavior.

Each recipe follows an **action-oriented workflow** with commands, configuration, expected results, verification steps, troubleshooting guidance, and important safety considerations.

## Focus

- Continuous integration and delivery pipelines
- GitHub Actions automation
- Docker registry management
- Nginx reverse-proxy configuration
- SSL/TLS and HTTPS configuration
- Terraform infrastructure management
- Ansible configuration management
- Application and infrastructure monitoring

## Goal

> Build a practical DevOps recipe knowledge base that makes CI/CD, automation, container distribution, reverse proxies, HTTPS, infrastructure provisioning, configuration management, and monitoring procedures easy to follow, verify, troubleshoot, and reproduce.
