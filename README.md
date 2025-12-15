
# Ansible Automation for Food Delivery Application

A comprehensive Ansible automation solution for deploying a containerized food delivery application with automated Docker environment setup and container orchestration.

## 📋 Overview

This project automates the deployment of a full-stack food delivery application using Ansible. The application consists of a React frontend, Node.js backend, and Nginx reverse proxy, all running in Docker containers. The automation handles everything from Docker installation to container deployment using a role-based Ansible architecture.

## 🏗️ Application Architecture

```
┌─────────────────────────────────────────┐
│         Nginx Reverse Proxy             │
│            (Port 80)                    │
└──────────────┬──────────────────────────┘
               │
       ┌───────┴────────┐
       │                │
┌──────▼─────┐   ┌─────▼──────┐
│  Frontend  │   │  Backend   │
│ (Port 5173)│   │ (Port 4000)│
└────────────┘   └────────────┘
```

### Components

- **Frontend Container**: React application running on port 5173
- **Backend Container**: Node.js API server running on port 4000
- **Nginx Container**: Reverse proxy and load balancer on port 80

## 🎯 Features

- Automated Docker environment setup and installation
- Role-based Ansible architecture for modularity
- Docker Compose orchestration for multi-container deployment
- Nginx reverse proxy configuration
- Idempotent deployment process

## 📁 Project Structure
```
ansible-food-delivery/
├── admin/                      # Admin panel application
├── backend/                    # Backend API source code
├── frontend/                   # Frontend React application
├── nginx/                      # Nginx configuration files
├── inventory/
│   └── hosts                   # Inventory file with target hosts
├── roles/
│   ├── docker/          # Role for Docker installation
│   │   ├── tasks/
│   │   │   └── main.yml
│   └── food-deploy/  # Role for container deployment
│       ├── tasks/
│       │   └── main.yml
├── .gitignore                 # Git ignore file
├── docker-compose.yaml        # Docker Compose configuration
├── playbook.yml               # Main playbook
├── ansible.cfg                # ansible default configuration
└── README.md
```

## 🚀 Prerequisites

- AWS Account with EC2 access
- Basic understanding of Ansible and Docker
- SSH access to target servers
- Minimum 2 EC2 instances (1 master, 1+ workers)

## 📦 Installation & Setup

### Step 1: Launch AWS EC2 Instances

Create EC2 instances for your Ansible infrastructure:

```bash
# Master Node: t2.medium or higher (recommended)
# Worker Node(s): t2.small or higher

# Ensure security groups allow:
# - SSH (Port 22)
# - HTTP (Port 80)
# - Custom ports (4000, 5173) if needed
```

### Step 2: Configure Ansible Master/Worker Architecture

On the **Master Node**:

```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Configure SSH key-based authentication
ssh-keygen -t rsa -b 4096
ssh-copy-id user@worker-node-ip
```

### Step 3: Install Ansible

On the **Master Node**:

```bash
# For Ubuntu/Debian
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y

# Verify installation
ansible --version
```

### Step 4: Clone the Project

```bash
# Clone the repository
git clone <repository-url>
cd ansible-food-delivery

# Update inventory file with your worker node IPs & Private key file path
nano inventory/hosts
```

### Step 5: Run the Playbook

Execute the automation:

```bash
# Run the playbook
ansible-playbook playbook.yml
```

## 🔧 Ansible Roles


### Role 1: docker

Handles Docker environment preparation:

- Installs Docker Engine
- Configures Docker service
- Installs Docker Compose
- Sets up Docker user permissions
- Validates Docker installation

### Role 2: food-deploy

Manages application containers:

- Deploys docker-compose.yml configuration
- Starts frontend, backend, and Nginx containers
- Configures container networking
- Manages container lifecycle

## 🌐 Accessing the Application

After successful deployment:

```
http://<worker-node-ip>        # Main application (via Nginx)
http://<worker-node-ip>:5173   # Frontend (direct access)
http://<worker-node-ip>:4000   # Backend API (direct access)
```

## 🔍 Verification

Check deployment status:

```bash
# On worker node
docker ps                           # List running containers
docker-compose ps                   # Check compose services
docker logs <container-name>        # View container logs
curl http://localhost               # Test Nginx proxy
```

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Create a Pull Request

## 👥 Authors

- Pritam Phadtare - Initial work

## 🙏 Acknowledgments

- Ansible community for excellent documentation
- Docker for containerization platform
- Open source contributors

