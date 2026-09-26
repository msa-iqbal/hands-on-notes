# Docker

Practical, step-by-step **Docker recipes** for containerizing applications, creating images, managing persistent data, connecting containers, using Docker Compose, building and publishing images, setting up development environments, and cleaning Docker resources.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Setup Node.js Project|Containerize a Node.js application, create the required Docker configuration, install dependencies, expose ports, and run the application in a container|
|02|Setup Python Project|Containerize a Python application, configure the Docker environment, install dependencies, expose ports, and run the application in a container|
|03|Create Dockerfile|Create and configure Dockerfiles, select base images, copy application files, install dependencies, configure environments, expose ports, and define startup commands|
|04|Setup Docker Compose|Create a Compose configuration, define services, networks, volumes, environment variables, dependencies, and manage multi-container applications|
|05|Persist Container Data|Create and use volumes and bind mounts, persist application data, manage storage, and verify data remains available across container lifecycle changes|
|06|Connect Containers|Create Docker networks, connect multiple containers, configure service discovery, expose ports, and enable communication between services|
|07|Build and Push Image|Build Docker images, apply tags, inspect images, authenticate with a container registry, and push images for distribution|
|08|Run Development Environment|Create a containerized development environment, mount source code, enable live development workflows, manage dependencies, and configure development services|
|09|Clean Docker Environment|Identify unused Docker resources, remove stopped containers, unused images, volumes, and networks, and safely reclaim disk space|

## Structure

The recipes progress from **application containerization → Dockerfile creation → multi-container orchestration → data persistence → container networking → image distribution → development environments → Docker cleanup**.

The first recipes focus on containerizing common application types, starting with Node.js and Python projects. The Dockerfile recipe then establishes the reusable process for defining application images, dependencies, runtime configuration, and startup behavior.

Docker Compose introduces multi-container application management through services, networks, volumes, environment variables, and service dependencies. Persistent-data and container-networking recipes then cover two important aspects of running stateful and interconnected applications.

The image-build and publishing recipe covers creating distributable container images and pushing them to a registry. The development-environment recipe focuses on using containers for repeatable local development workflows.

The final recipe addresses Docker environment maintenance by identifying and removing unused resources while preserving resources that are still required.

Each recipe follows an **action-oriented workflow** with commands, configuration, expected results, verification steps, troubleshooting guidance, and important safety considerations.

## Focus

- Node.js and Python application containerization
- Dockerfile creation and image configuration
- Docker Compose and multi-container applications
- Volumes, bind mounts, and persistent data
- Container networking and service communication
- Docker image building, tagging, and publishing
- Containerized development environments
- Docker resource cleanup and disk management

## Goal

> Build a practical Docker recipe knowledge base that makes common containerization, image building, multi-container orchestration, data persistence, networking, development, publishing, and environment-maintenance procedures easy to follow, verify, troubleshoot, and reproduce.
