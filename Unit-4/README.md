# Enterprise Smart Library Management Platform
## Unit 4 – Infrastructure Automation Mini Project

### Student Information

| Detail | Information |
|---|---|
| Student Name | Vinay Rajendra Velanjkar |
| PRN | 2125UMLM1107 |
| Module | Unit 4 – Infrastructure Automation |
| Project | Enterprise Smart Library Management Platform |
| Cloud Platform | Amazon Web Services (AWS) |

---

## 1. Project Overview

The Enterprise Smart Library Management Platform is a DevOps-based project that demonstrates how cloud infrastructure and server configuration can be automated using Terraform and Ansible.

The main objective of this project is to reduce manual work by automating infrastructure creation, server configuration, application deployment, and database backups.

Terraform is used to create AWS resources, while Ansible is used to configure the EC2 instance and deploy the application using Docker.

## 2. Technologies Used

- **AWS:** Cloud infrastructure platform.
- **Terraform:** Infrastructure as Code (IaC).
- **Ansible:** Configuration management and automation.
- **Docker:** Application containerization.
- **Nginx:** Web server and reverse proxy.
- **Python Flask:** Backend application framework.
- **SQLite:** Database management.
- **Amazon S3:** Database backup storage.
- **Ubuntu:** Server operating system.

## 3. Project Architecture

The project follows an automated deployment workflow:

1. Terraform provisions AWS infrastructure.
2. AWS EC2 provides the virtual server.
3. Ansible connects to EC2 through SSH.
4. Ansible installs Docker and Nginx.
5. Docker deploys the Flask-based Library Management application.
6. Nginx works as a reverse proxy.
7. Database backups are stored in Amazon S3.

## 4. Project Directory Structure

```text
Unit-4/
│
├── library-terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── providers.tf
│   ├── terraform.tfvars
│   └── backend.tf
│
├── library-ansible/
│   ├── ansible.cfg
│   ├── inventory.ini
│   ├── inventory.ini.example
│   ├── site.yml
│   └── roles/
│
├── library-app/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── scripts/
│   ├── setup_workstation.sh
│   ├── deploy_all.sh
│   ├── test_endpoints.sh
│   ├── backup_to_s3.sh
│   ├── verify_backup.sh
│   └── test_api.py
│
├── docs/
│   ├── ARCHITECTURE.md
│   └── LAB_MANUAL_WALKTHROUGH.md
│
└── README.md
```

## 5. Implementation Steps

### Step 1: Workstation Setup

Install and configure the required tools:

- AWS CLI
- Terraform
- Ansible
- Git
- SSH

Configure AWS credentials:

```bash
aws configure
```

Verify AWS authentication:

```bash
aws sts get-caller-identity
```

### Step 2: Infrastructure Provisioning Using Terraform

Navigate to the Terraform directory:

```bash
cd library-terraform
```

Initialize Terraform:

```bash
terraform init
```

Validate configuration:

```bash
terraform validate
```

Review infrastructure changes:

```bash
terraform plan
```

Create AWS infrastructure after reviewing the plan:

```bash
terraform apply
```

### Step 3: Server Configuration Using Ansible

Update the Ansible inventory file with the EC2 public IP address.

Example:

```ini
[smart_library]
<YOUR_EC2_PUBLIC_IP> ansible_user=ubuntu
```

Check the playbook syntax:

```bash
ansible-playbook -i inventory.ini site.yml --syntax-check
```

Execute the playbook:

```bash
ansible-playbook -i inventory.ini site.yml
```

### Step 4: Application Deployment

Ansible automates the installation and configuration of:

- Docker
- Nginx
- Flask application

The Library Management application runs inside a Docker container.

### Step 5: Application Verification

Check the application endpoints:

```bash
curl http://<YOUR_EC2_PUBLIC_IP>/
```

Check application health:

```bash
curl http://<YOUR_EC2_PUBLIC_IP>/api/health
```

Check the books catalog:

```bash
curl http://<YOUR_EC2_PUBLIC_IP>/api/books
```

### Step 6: Database Backup Using Amazon S3

The project uses Amazon S3 to store database backups.

Check bucket versioning:

```bash
aws s3api get-bucket-versioning --bucket <YOUR_BUCKET_NAME>
```

Check bucket encryption:

```bash
aws s3api get-bucket-encryption --bucket <YOUR_BUCKET_NAME>
```

List backup files:

```bash
aws s3 ls s3://<YOUR_BUCKET_NAME>/backups/
```

## 6. Expected Learning Outcomes

After completing this project, students will understand:

1. Infrastructure as Code using Terraform.
2. AWS resource provisioning and management.
3. Server configuration using Ansible.
4. Docker-based application deployment.
5. Nginx reverse proxy configuration.
6. Cloud database backup using Amazon S3.
7. Automation of DevOps deployment workflows.

## 7. Conclusion

This project demonstrates how Terraform and Ansible can work together to automate cloud infrastructure and application deployment.

Terraform manages the creation of AWS resources, while Ansible handles server configuration and application deployment.

By integrating Docker, Nginx, and Amazon S3, the project provides practical experience with important DevOps and cloud computing concepts.

The project helps develop practical knowledge of Infrastructure as Code, configuration management, cloud automation, and deployment workflows.

---

**Project:** Enterprise Smart Library Management Platform

**Module:** Unit 4 – Infrastructure Automation

**Student:** Vinay Rajendra Velanjkar
