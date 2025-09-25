![Alt text](/dynamic_docker-Page-1.jpg)


# 🚀 AWS Containerized Web Application Infrastructure

This project demonstrates how to build and deploy a **containerized web application** on AWS using a secure and scalable **3-tier architecture**. It leverages modern DevOps practices with Docker, GitHub, and AWS services to create a production-ready environment.

---

## 📖 Table of Contents
- [Architecture Overview](#architecture-overview)
- [Services Used](#services-used)
- [Prerequisites](#prerequisites)
- [Setup & Deployment](#setup--deployment)
- [Security Considerations](#security-considerations)
- [Contributing](#contributing)
- [License](#license)

---

## 🏗️ Architecture Overview

The infrastructure follows a **3-tier VPC design**:

1. **Public Tier**
   - Bastion Host for secure SSH access
   - Internet Gateway for external connectivity
   - NAT Gateway to allow private resources outbound internet access

2. **Application Tier**
   - Amazon ECS (Fargate) for running the containerized web app
   - Application Load Balancer (ALB) with SSL termination (Certificate Manager)
   - Auto Scaling Group for horizontal scaling

3. **Database Tier**
   - Amazon RDS (MySQL) in private subnets
   - Database schema and migrations managed by Flyway

Additional services:
- Amazon Route 53 for DNS routing
- Amazon S3 for storage (e.g., static assets, backups)
- IAM Roles and Security Groups for least-privilege access

---

## 🛠️ Services Used

- **Docker** → Build container image for the web app  
- **Git & GitHub** → Track changes, manage Dockerfile and app code  
- **AWS CLI** → Manage AWS resources from the terminal  
- **VS Code** → Development environment  
- **Flyway** → Database migrations into Amazon RDS  
- **Amazon ECR** → Store container images  
- **Amazon ECS (Fargate)** → Deploy and run containers without managing servers  
- **Amazon RDS (MySQL)** → Managed relational database service  
- **Elastic Load Balancer (ALB)** → Distribute traffic across ECS tasks  
- **Auto Scaling Group** → Scale ECS tasks based on demand  
- **Amazon Route 53** → Domain name routing  
- **Amazon S3** → Object storage for assets/backups  
- **IAM Roles** → Secure access management  
- **Certificate Manager** → SSL certificates for HTTPS  
- **Bastion Host** → Secure entry point into the VPC  
- **Security Groups** → Firewall rules to control inbound/outbound traffic  
- **3-tier VPC, NAT Gateway, Internet Gateway** → Networking foundation  

---

## ⚙️ Prerequisites

Before you begin, ensure you have:

- **AWS Account** with appropriate IAM permissions  
- **AWS CLI** installed and configured (`aws configure`)  
- **Docker** installed locally  
- **Git** installed for version control  
- **VS Code** or any preferred IDE  
- A **domain name** managed in Route 53 (optional, but required for HTTPS/SSL setup)  

---

## 🚀 Setup & Deployment

### 1. Clone the Repository
```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
````

### 2. Build and Tag Docker Image

```bash
docker build -t myapp:latest .
docker tag myapp:latest <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/myapp:latest
```

### 3. Push Image to Amazon ECR

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com
docker push <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/myapp:latest
```

### 4. Database Migration with Flyway

```bash
flyway -url=jdbc:mysql://<rds-endpoint>:3306/<db-name> -user=<db-username> -password=<db-password> migrate
```

### 5. Deploy ECS Cluster (Fargate)

* Create ECS cluster with Fargate launch type
* Define a Task Definition using the pushed Docker image
* Attach IAM Role for ECS tasks
* Configure service with ALB and Auto Scaling

### 6. Configure Route 53 & SSL

* Add a record set in Route 53 pointing to the ALB
* Use AWS Certificate Manager (ACM) to issue and attach SSL certificate

---

## 🔒 Security Considerations

* **IAM Roles** follow **least-privilege principle**
* **Secrets & Credentials** should be stored in **AWS Secrets Manager** or **SSM Parameter Store** (never in plain text)
* **Private Subnets** protect RDS and ECS tasks from direct internet exposure
* **Bastion Host** used for controlled SSH access into VPC
* **Security Groups** carefully restrict traffic flows (e.g., ALB → ECS, ECS → RDS)

---

## 🤝 Contributing

Contributions are welcome! Please open an issue or submit a pull request with improvements.

---
