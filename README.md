# Jenkins Docker App - CI/CD

A practical CI/CD project using **Jenkins, Docker, Docker Hub, GitHub, and Ansible**.

## Pipeline

```text
GitHub
   ↓
Jenkins
   ↓
Docker Build
   ↓
Docker Hub
   ↓
Ansible
   ↓
Docker Container
   ↓
Application :5000
```

## Technologies

* Git / GitHub
* Jenkins
* Docker
* Docker Hub
* Ansible
* Linux / WSL

## How It Works

1. Jenkins checks out the code from GitHub.
2. Jenkins builds the Docker image.
3. Jenkins pushes the image to Docker Hub.
4. Ansible pulls the latest image.
5. Ansible removes the old container.
6. Ansible starts the new container.

## Docker Image

```text
maximgonik/jenkins-docker-app:latest
```

## Container

```text
jenkins-docker-app
```

Application port:

```text
5000
```

## Ansible

The deployment uses the `community.docker` Ansible collection and currently runs locally using:

```ini
[web]
localhost ansible_connection=local
```

## Git Workflow

Development is done on the `dev` branch.

```bash
git add .
git commit -m "Your message"
git push origin dev
```

After testing, `dev` can be merged into `main`.

## Project Goal

Demonstrate a complete practica
