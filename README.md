<div align="center">

# Homelab Documentation 🚀

This GitHub account serves as documentation for everything in my homelab.

The repositories in this account are mirrors to my selfhosted ones, so the code is always up-to-date.

Currently, there are **13 repositories**. While this may be confusing for some, it works well for me so far.

Everything is **subject to change**! I am in the process of using as much Ansible as possible in order to learn.

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
  - [Docker](#docker)
  - [k3s](#k3s)
  - [Proxmox Workloads](#proxmox-workloads)
- [ToDo ✅](#todo)

## ⚙️ Setup

The setup is divided into **stacks**: one for Docker hosts, one for K3s… you get the idea. The virtual machines for each stack are managed in the [proxmox-workloads](https://github.com/InvalidIdentifier/proxmox-workloads) repository.

### ❓ Why did I do it this way?
- GitLab currently allows only one repository per project. If I move to another solution in the future, I will restructure this setup.
- Clear separation of stacks. 🔍
- **GitLab** is used instead of **Gitea** because it is closer to the tools I use at work.
- The **Ansible/.env/docker-compose.yml** setup ensures that all necessary files are on the host in case something breaks or I decide to stop using CI/CD.

### 🏗 Physical Infrastructure

My homelab consists of **four physical servers** with the following roles:

| Server  | OS        | Function                         |
|---------|-----------|----------------------------------|
| node010 | Debian 12 | GitLab/Base Infrastructure       |
| node240 | PBS 3     | Proxmox Backup Server / qDevice  |
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
The Stack needs an overhaul wich will happen in the future. Currently the 'base_node' role just sits in the repository, wich i want to change sometime. Altough the stack is technically just another Dockernode its seperated because it needs to be there before everything else because its used to deploy everything else.
#### 📂 Repositories
- [base-ansible](https://github.com/InvalidIdentifier/base-ansible) 
- [base-images](https://github.com/InvalidIdentifier/base-images)

#### Used Ansible Roles
- [ansible-role-docker-common-setup](https://github.com/InvalidIdentifier/ansible-role-docker-common-setup)

### Docker
Provides a **runtime environment for Docker containers** and the services deployed to it. Also currently deploys the monitoring-stack.
#### 📂 Repositories
- [ansible-docker](https://github.com/InvalidIdentifier/ansible-docker) 

#### Used Ansible Roles
- [ansible-role-docker-common-setup](https://github.com/InvalidIdentifier/ansible-role-docker-common-setup)
- [ansible-role-docker-common-services](https://github.com/InvalidIdentifier/ansible-role-docker-common-services)
- [ansible-role-docker-infra-node](https://github.com/InvalidIdentifier/ansible-role-docker-infra-node)
- [ansible-role-docker-service-node](https://github.com/InvalidIdentifier/ansible-role-docker-service-node)

### k3s
Provides a **K3s cluster** and deploys services to it.
#### 📂 Repositories
- [k3s-ansible](https://github.com/InvalidIdentifier/k3s-ansible) 
- [k3s-container](https://github.com/InvalidIdentifier/k3s-container) 

#### Used Ansible Roles
- [the ones i stole from k3s.io](https://github.com/k3s-io/k3s-ansible)

### Proxmox Workloads
Manages the **Proxmox workloads**. Everything is created via **Terraform**, with its state currently stored on a share.
#### 📂 Repositories
- [proxmox-workloads](https://github.com/InvalidIdentifier/proxmox-workloads)

## ✅ ToDo
- Move reverse proxy to DMZ again
- Move services to K3s
- Automate Grafana Dashboards and Alerts
- Redo Gitlab Runners
- Cleanup reverseproxies
- Create Readmes for every repository
- Add Repository for maintenance
- Add Repository for backups
- Add move molecule verifications
- Add automatic testing before deployments via molecule
