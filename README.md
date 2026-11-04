# 📚 Smart Library Management System
### A DevOps-Based Cloud Application

## 👨‍🎓 Student Details

- **Name:** Vinay Velanjkar
- **PRN:** 2125UMLM1107
- **Module:** Unit 4 – Infrastructure Automation

---

## 📌 About the Project

The Smart Library Management System is a Python Flask-based web application designed to simplify library operations. It demonstrates how modern DevOps tools can be integrated into the software development lifecycle to improve testing, application packaging and deployment workflows.

## 🎯 Project Goals

- Develop a web-based library management application.
- Implement REST API functionality.
- Automate application testing.
- Maintain code quality using static analysis.
- Containerize the application using Docker.
- Integrate Jenkins for CI/CD automation.
- Explore cloud infrastructure using AWS EC2.

## ⚙️ Tools and Technologies

| Technology | Usage |
|---|---|
| Python | Backend development |
| Flask | Web application framework |
| Pytest | Application testing |
| Flake8 | Code quality analysis |
| Gunicorn | Application server |
| GitHub | Source code management |
| Jenkins | CI/CD automation |
| Docker | Containerization |
| AWS EC2 | Cloud infrastructure |

## 🌟 Main Features

1. **Book Management:** Supports library book-related operations.
2. **User Management:** Maintains library user information.
3. **REST API:** Provides endpoints for application operations.
4. **Health Monitoring:** Includes an application health-check endpoint.
5. **Automated Testing:** Validates functionality through Pytest.
6. **Containerization:** Supports Docker image creation and execution.
7. **Pipeline Automation:** Uses Jenkins to automate development checks.

## 🏗️ Project Architecture

The application follows a modular structure that separates application logic, database operations, testing and deployment configuration.

```text
library-management-platform-aws/
│
├── app/
│   ├── routes/
│   ├── services/
│   ├── models/
│   └── templates/
│
├── database/
├── tests/
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
│
├── Dockerfile
├── Jenkinsfile
├── requirements.txt
├── run.py
└── README.md
```

*Note: Refer to the actual repository files for the implemented structure.*

## 🚀 Installation Guide

### Requirements

- Python 3.11
- Git
- pip
- Docker (optional)

### Step 1: Clone the Repository

```bash
git clone https://github.com/dhokanekrushna467-web/library-management-platform-aws.git
cd library-management-platform-aws
```

### Step 2: Create Virtual Environment

```bash
python3 -m venv venv
```

Activate it:

```bash
source venv/bin/activate
```

### Step 3: Install Packages

```bash
pip install -r requirements.txt
```

### Step 4: Start Application

```bash
python run.py
```

## 🧪 Testing and Code Quality

Execute automated tests:

```bash
pytest -q
```

Check Python code quality:

```bash
flake8 app database tests run.py
```

## 🐳 Docker Implementation

Build the application image:

```bash
docker build -t library-management-platform:latest .
```

Start the container:

```bash
docker run -d --name library-app -p 5000:5000 library-management-platform:latest
```

Verify the container:

```bash
docker ps
```

## 🔄 CI/CD Workflow Using Jenkins

The Jenkins pipeline is designed around these stages:

1. Source code checkout from GitHub.
2. Python environment setup.
3. Dependency installation.
4. Static code analysis using Flake8.
5. Automated testing using Pytest.
6. Docker image building.

## ☁️ Cloud Infrastructure – AWS

AWS EC2 is used as the cloud hosting environment for the project and its DevOps tools.

The EC2 environment can support application execution, Jenkins integration and Docker-based workflows after the required infrastructure and permissions are configured.

## 🔮 Future Scope

- Implement secure user authentication.
- Integrate a persistent database.
- Add automated deployment.
- Introduce application monitoring.
- Improve cloud security and scalability.
- Expand Kubernetes deployment capabilities.

## 🎓 Conclusion

The Smart Library Management System provides practical exposure to application development and DevOps automation.

Working with Python, Flask, GitHub, Jenkins, Docker and AWS helps demonstrate how different technologies contribute to a modern software delivery workflow.

This project provides a foundation for understanding continuous integration, software testing, containerization and cloud-based application management.

## 🙏 Project Acknowledgement

Original project source: **Krushna Dhokane**

This repository contains an educational project adaptation. Original authorship is acknowledged.

---

**Submitted by: Vinay Velanjkar**  
**PRN: 2125UMLM1107**
