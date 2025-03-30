<div align="center">

# Homelab Documentation 🚀

This GitHub account serves as documentation for everything in my homelab.

The repositories in this account are mirrors to my selfhosted ones, so the code is always updated.

I follow a multi-step setup approach, which is why there are multiple repositories for different stages in this account.

Currently, there are **13 repositories**. While this may be confusing for some, it works well for me so far.

Everything is **subject to change**!

</div>

---

# 📖 Table of Contents
- [Setup ⚙️](#setup)
  - [Why?](#why-did-i-do-it-this-way)
  - [Physical Infrastructure](#physical-infrastructure)
- [Tools 🛠](#tools)
  - [Virtualization](#virtualization)
  - [Containerization](#containerization)
  - [Configuration Management/IaC](#configuration-managementiac)
  - [Monitoring](#monitoring)
  - [CI/CD](#cicd)
- [Stacks 📦](#stacks)
  - [Base](#base)
  - [Infra](#infra)
  - [Docker](#docker)
  - [k3s](#k3s)
  - [Proxmox Workloads](#proxmox-workloads)
- [ToDo ✅](#todo)

## ⚙️ Setup

The setup is divided into **stacks**: one for Docker hosts, one for K3s… you get the idea. The virtual machines for each stack are managed in the [proxmox-workloads](https://github.com/InvalidIdentifier/proxmox-workloads) repository.

Most stacks consist of two separate steps. Due to restrictions in GitLab, each step is in its own repository. Typically, the first step uses **Ansible** to install all prerequisites, while the second step deploys the actual services to the stack.

### ❓ Why did I do it this way?
- GitLab currently allows only one repository per project. If I move to another solution in the future, I will restructure this setup.
- Clear separation of stacks. 🔍
- Separate host management from service deployment. There’s no need to run through all the code to deploy a new service to a stack.
- **GitLab** is used instead of **Gitea** because it is closer to the tools I use at work.
- The **Ansible/.env/docker-compose.yml** setup ensures that all necessary files are on the host in case something breaks or I decide to stop using CI/CD.

### 🏗 Physical Infrastructure

My homelab consists of **four physical servers** with the following roles:

| Server  | OS         | Function                          |
|---------|-----------|----------------------------------|
| node010 | Debian 12 | GitLab/Base Infrastructure       |
| node240 | PBS 3     | Proxmox Backup Server / qDevice |
| node241 | Proxmox 8 | Proxmox Host                     |
| node242 | Proxmox 8 | Proxmox Host                     |

## 🛠 Tools

### Virtualization 🖥
- [Proxmox](https://www.proxmox.com/en/proxmox-virtual-environment)

### Containerization 📦
- [Docker](https://www.docker.com/)
- [K3s](https://k3s.io/)

### Configuration Management/IaC 🔧
- [Ansible](https://github.com/ansible/ansible)
- [Terraform](https://www.terraform.io/)

### Monitoring 📊
- [Prometheus](https://prometheus.io/)
- [Grafana](https://grafana.com/)
- [Loki](https://grafana.com/oss/loki/)

### CI/CD ⚙️
- [GitLab CE](https://about.gitlab.com/)

## 📦 Stacks

### Base
The first layer in the setup. It runs **GitLab**, GitLab runners, and some foundational services like SMB for shares and a registry. It also builds images for use in GitLab pipelines.
#### 📂 Repositories
- [base-ansible](https://github.com/InvalidIdentifier/base-ansible) 
- [base-container](https://github.com/InvalidIdentifier/base-container) 
- [base-images](https://github.com/InvalidIdentifier/base-images)

### Infra
The second layer. It runs **monitoring, log aggregation**, and **notification services**. Additionally, a **Traefik reverse proxy** is deployed in a DMZ subnet.
#### 📂 Repositories
- [infra-ansible](https://github.com/InvalidIdentifier/infra-ansible) 
- [infra-container](https://github.com/InvalidIdentifier/infra-container) 

### Docker
Provides a **runtime environment for Docker containers** and the services deployed to it.
#### 📂 Repositories
- [docker-ansible](https://github.com/InvalidIdentifier/docker-ansible) 
- [docker-container](https://github.com/InvalidIdentifier/docker-container) 

### k3s
Provides a **K3s cluster** and deploys services to it.
#### 📂 Repositories
- [k3s-ansible](https://github.com/InvalidIdentifier/k3s-ansible) 
- [k3s-container](https://github.com/InvalidIdentifier/k3s-container) 

### Proxmox Workloads
Manages the **Proxmox workloads**. Everything is created via **Terraform**, with its state currently stored on a share.
#### 📂 Repositories
- [proxmox-workloads](https://github.com/InvalidIdentifier/proxmox-workloads)

## ✅ ToDo
- Move reverse proxy to DMZ again
- Redo Ansible roles (ongoing)
- Move services to K3s
- Split docker-compose.yml (ongoing)
- Automate Grafana Dashboards and Alerts
- Redo Gitlab Runners
- Cleanup reverseproxies
- Create Readmes for every repository