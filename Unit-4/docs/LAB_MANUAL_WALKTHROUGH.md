# Laboratory Manual Walkthrough & Execution Runbook
## Enterprise Smart Library Management Platform

**Course:** Unit 4 – Infrastructure Automation  
**Student Name:** DHOKANE KRUSHNA BABASAHEB  
**PRN:** 2125UMLM1079  
**Cloud Platform:** Amazon Web Services (AWS)  
**Region:** us-east-1  
**Operating System:** Ubuntu 22.04 LTS  

---

# Chapter 2: Prerequisites & AWS Environment Setup

## 1. Workstation Baseline Utilities

First, update the system packages and install the required utilities.

```bash
sudo apt-get update && sudo apt-get upgrade -y

sudo apt-get install -y curl wget unzip jq git software-properties-common ca-certificates gnupg apt-transport-https python3 python3-pip python3-venv
```

These utilities are required for downloading packages, managing files, running scripts, and configuring the development environment.

## 2. Installing AWS CLI v2

AWS CLI is used to communicate with AWS services through the command line.

Download and install AWS CLI:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip -q awscliv2.zip

sudo ./aws/install --update

rm -rf aws awscliv2.zip
```

Verify installation:

```bash
aws --version
```

## 3. Installing Terraform CLI

Terraform is used to create and manage AWS infrastructure using configuration files.

Add the HashiCorp repository:

```bash
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
```

Add the repository:

```bash
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
```

Install Terraform:

```bash
sudo apt-get update

sudo apt-get install -y terraform
```

Verify installation:

```bash
terraform -version
```

## 4. Installing Ansible Core

Ansible is used to automate server configuration and application deployment.

Add the Ansible repository:

```bash
sudo add-apt-repository --yes --update ppa:ansible/ansible
```

Install Ansible:

```bash
sudo apt-get install -y ansible
```

Install required Ansible collections:

```bash
ansible-galaxy collection install amazon.aws community.general community.docker
```

Verify installation:

```bash
ansible --version
```

## 5. Configuring AWS Credentials

Configure AWS credentials:

```bash
aws configure
```

Enter the following information when prompted:

- AWS Access Key ID
- AWS Secret Access Key
- Default region: `us-east-1`
- Output format: `json`

Verify AWS authentication:

```bash
aws sts get-caller-identity
```

This command displays the AWS account and IAM identity associated with the configured credentials.

**Security Note:** Never upload AWS access keys or secret credentials to GitHub.

## 6. SSH Key Pair Generation

Create the SSH directory:

```bash
mkdir -p ~/.ssh
```

Generate an SSH key pair:

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/library-aws-key -N "" -C "devops-library-project"
```

Set appropriate permissions:

```bash
chmod 400 ~/.ssh/library-aws-key

chmod 644 ~/.ssh/library-aws-key.pub
```

Import the public key into AWS:

```bash
aws ec2 import-key-pair \
  --key-name "library-aws-key" \
  --public-key-material fileb://$HOME/.ssh/library-aws-key.pub \
  --region us-east-1
```

The private key will be used to establish an SSH connection with the EC2 instance.

---

# Chapter 3: Terraform Infrastructure as Code

Terraform is responsible for automatically provisioning the cloud infrastructure required by the Smart Library application.

## 1. Navigate to Terraform Directory

```bash
cd library-terraform
```

## 2. Initialize Terraform

```bash
terraform init
```

This downloads the required providers and initializes the working directory.

## 3. Validate Configuration

```bash
terraform validate
```

This checks whether the Terraform configuration files are syntactically valid.

## 4. Generate Execution Plan

```bash
terraform plan
```

This displays the AWS resources Terraform intends to create or modify.

## 5. Provision AWS Infrastructure

After reviewing the execution plan:

```bash
terraform apply
```

Type `yes` when prompted to approve the deployment.

## 6. Expected Infrastructure Resources

The Terraform configuration is designed to create:

| Resource | Description |
|---|---|
| AWS VPC | Provides an isolated virtual network |
| Public Subnet | Hosts the EC2 instance |
| Internet Gateway | Provides internet connectivity |
| Route Table | Controls network traffic routing |
| Security Group | Controls inbound and outbound traffic |
| EC2 Instance | Hosts the Library application |
| S3 Bucket | Stores database backup files |

The infrastructure uses the following network configuration:

- VPC CIDR: `10.0.0.0/16`
- Subnet CIDR: `10.0.1.0/24`
- AWS Region: `us-east-1`
- Operating System: Ubuntu 22.04 LTS

Terraform outputs should provide the EC2 public IP and S3 bucket name.

---

# Chapter 4: Ansible Configuration Management

Ansible automates the configuration of the EC2 instance and deployment of the Library Management application.

## 1. Navigate to Ansible Directory

```bash
cd ../library-ansible
```

## 2. Configure Inventory

Open `inventory.ini` and enter your EC2 public IP address.

Example:

```ini
[smart_library]
<YOUR_EC2_PUBLIC_IP> ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/library-aws-key
```

## 3. Verify Playbook Syntax

```bash
ansible-playbook -i inventory.ini site.yml --syntax-check
```

## 4. Execute Ansible Playbook

```bash
ansible-playbook -i inventory.ini site.yml
```

Ansible performs the following configuration tasks.

### Common Role

- Updates the package cache.
- Installs system utilities.
- Configures firewall rules.
- Allows required network traffic.

### Docker Role

- Installs Docker.
- Starts the Docker service.
- Enables Docker at system startup.

### Nginx Role

- Installs Nginx.
- Configures reverse proxy settings.
- Enables the Library application site.
- Removes the default Nginx configuration.
- Ensures Nginx is running.

### Library Application Role

- Creates the application directory.
- Copies application files.
- Creates persistent database storage.
- Builds the Docker image.
- Runs the application container.
- Verifies application health.

## 5. Expected Ansible Execution Result

A successful execution should show a play recap similar to:

```text
PLAY RECAP *********************************************************************

<EC2_IP> : ok=20 changed=<VALUE> unreachable=0 failed=0
```

The actual task count and changed value depend on the playbook execution.

---

# Chapter 5: API and Amazon S3 Verification

## 1. Root Endpoint Verification

Check whether the application is running:

```bash
curl http://<EC2_IP>/
```

Expected response:

```json
{
  "application": "Smart Library Management Platform",
  "status": "running"
}
```

## 2. Application Health Check

```bash
curl http://<EC2_IP>/api/health
```

Expected response:

```json
{
  "status": "healthy"
}
```

## 3. Books Catalog Verification

```bash
curl http://<EC2_IP>/api/books
```

Expected response:

```json
[
  {
    "author": "F. Scott Fitzgerald",
    "id": 1,
    "title": "The Great Gatsby"
  }
]
```

These API requests help verify whether the application is responding correctly.

## 4. Amazon S3 Backup Verification

Amazon S3 is used to store database backups.

### Check Bucket Versioning

```bash
aws s3api get-bucket-versioning \
  --bucket <YOUR_BUCKET_NAME> \
  --region us-east-1
```

Expected response:

```json
{
  "Status": "Enabled"
}
```

### Check Server-Side Encryption

```bash
aws s3api get-bucket-encryption \
  --bucket <YOUR_BUCKET_NAME> \
  --region us-east-1
```

Expected encryption configuration:

```json
{
  "SSEAlgorithm": "AES256"
}
```

### Verify Database Backup

```bash
aws s3 ls s3://<YOUR_BUCKET_NAME>/backups/ \
  --region us-east-1
```

This command lists the available backup files stored in the S3 bucket.

---

# Chapter 6: Final Learning Outcomes

After completing this laboratory exercise, the following concepts will be understood:

1. Infrastructure provisioning using Terraform.
2. AWS cloud resource management.
3. Server configuration using Ansible.
4. Docker container creation and deployment.
5. Nginx reverse proxy configuration.
6. Flask application deployment.
7. Amazon S3 database backup.
8. Cloud infrastructure verification and management.

# Conclusion

This laboratory project demonstrates the integration of Terraform and Ansible for automating cloud infrastructure and server configuration.

Terraform simplifies the provisioning of AWS resources, while Ansible automates software installation, server configuration, and application deployment.

The integration of Docker, Nginx, Flask, SQLite, and Amazon S3 provides practical experience with modern cloud computing and DevOps technologies.

The project helps develop practical skills in Infrastructure as Code, configuration management, containerization, cloud deployment, and automated backup management.

---

**Student Name:** DHOKANE KRUSHNA BABASAHEB  
**PRN:** 2125UMLM1079  
**Course:** Unit 4 – Infrastructure Automation
