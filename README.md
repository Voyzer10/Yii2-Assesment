# 🚀 Yii2 Assessment – DevOps Deployment with Docker Swarm, Ansible & CI/CD

This project demonstrates a full DevOps workflow where a **Yii2 PHP application** is deployed on an **AWS EC2 instance** using **Docker Swarm**, with **NGINX as a host-level reverse proxy**, **Ansible** for automation, and **GitHub Actions** for CI/CD.

---

## 🧰 Tech Stack

- **PHP Framework**: Yii2 (Basic Template)
- **Containerization**: Docker, Docker Swarm
- **CI/CD**: GitHub Actions
- **Provisioning & Automation**: Ansible
- **Web Server**: NGINX (host-based)
- **Monitoring**: Prometheus + Node Exporter (bonus)

---

## 📁 Project Structure
├── ansible/
│ └── playbook.yml # Installs Docker, NGINX, configures Swarm
├── nginx/
│ └── yii2.conf # Host-based reverse proxy config
├── yii2-app-basic/
│ ├── Dockerfile # App container build
│ └── docker-compose.yml # Swarm service config
├── .github/
│ └── workflows/
│ └── deploy.yml # CI/CD workflow
├── prometheus/
│ ├── prometheus.yml # Monitoring config (optional)
├── README.md

🧭 Structured Workflow – Yii2 DevOps 
🧱 Phase 1: Infrastructure Setup
Objective: Provision the target environment (AWS EC2) and make it ready for automation.

🔹 Steps:
Launch EC2 Instance

OS: Ubuntu 20.04+

Allow inbound rules: 22 (SSH), 80 (HTTP), optionally 9100 (Prometheus)

Configure GitHub Secrets

Add SSH_PRIVATE_KEY, HOST, USER, DOCKER_USERNAME, DOCKER_PASSWORD to GitHub repo

🧰 Phase 2: Server Provisioning with Ansible
Objective: Automate environment setup and prepare it for Docker Swarm and reverse proxy.

🔹 Ansible Playbook Tasks (ansible/playbook.yml):
✅ Install Docker & Docker Compose

🔁 Initialize Docker Swarm

🌐 Install NGINX on host (outside containers)

📁 Copy custom NGINX reverse proxy config to /etc/nginx/sites-available/yii2.conf

🔗 Enable the config and reload NGINX

📂 Clone the Yii2 repository (optional if not CI/CD driven)

🐳 Deploy the Yii2 app as a Docker Swarm service

🐳 Phase 3: Docker Containerization
Objective: Build and containerize the Yii2 application using Docker.

🔹 Docker Setup:
Dockerfile: Builds the Yii2 app container

docker-compose.yml: Defines services for Swarm

Ports: Exposes internal ports to NGINX (e.g., 8080)

🔹 NGINX Config (Host-level):
Routes external HTTP requests to the containerized Yii2 app running at localhost:8080.

⚙️ Phase 4: CI/CD Pipeline (GitHub Actions)
Objective: Automate build, push, and deployment using GitHub Actions.

🔹 Trigger:
yaml
Copy
Edit
on:
  push:
    branches: [ main ]
🔹 Pipeline Steps:
🏗️ Build Docker Image from latest code

📦 Push to Docker Hub

🔐 SSH into EC2 using GitHub Secret (SSH_PRIVATE_KEY)

🔄 Pull the latest image

🐳 Update Docker Swarm service with new image

✅ Optional: Add rollback strategy or container health checks

🔍 Phase 5: Monitoring (Bonus)
Objective: Implement lightweight infrastructure monitoring with Prometheus.

🔹 Components:
prometheus/prometheus.yml: Configures scrape jobs

node-exporter: Installed and running on EC2 (port 9100)

🔹 Metrics:
CPU, RAM, Disk I/O, Network usage of the EC2 host
