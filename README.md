# 🏗️ Secure and Highly Available 3-Tier Web Architecture on AWS

A production-ready, fault-tolerant 3-tier web architecture deployed on AWS, featuring a custom VPC, public/private subnets across multiple Availability Zones, Bastion Host, Apache + PHP app servers, and a managed MySQL RDS database.

---

## 📐 Architecture Overview

![Three Tier App Architecture](Screenshots/architecture.png)

**Flow:**
- **Route 53** → **Application Load Balancer** (across 2 Availability Zones)
- ALB routes to **EC2 instances** running PHP + Apache (App tier)
- App servers connect to **RDS MySQL** (Multi-AZ DB tier)

---

## ☁️ AWS Services Used

| Service | Purpose |
|---|---|
| **VPC** | Custom isolated network (CIDR: 20.0.0.0/16) |
| **EC2 (t3.micro)** | App servers + Bastion/Jump server |
| **Application Load Balancer** | Distributes traffic across app servers |
| **Amazon RDS (MySQL)** | Managed database in private subnet |
| **IAM** | Access management and permissions |
| **CloudWatch** | Monitoring and alarm configuration |
| **NAT Gateway** | Outbound internet for private instances |
| **Security Groups** | Firewall rules for each tier |

---

## 🌐 Network Design

### Subnets Created

| Subnet Name | Type | CIDR |
|---|---|---|
| web-pub-sub1 | Public | 20.0.1.0/24 |
| web-pub-sub2 | Public | 20.0.2.0/24 |
| app-pvt-sub1 | Private | 20.0.3.0/24 |
| app-pvt-sub2 | Private | 20.0.4.0/24 |
| db-pvt-sub1 | Private | 20.0.5.0/24 |
| db-pvt-sub2 | Private | 20.0.6.0/24 |

- Internet Gateway attached to VPC for public subnet traffic
- NAT Gateway in public subnet for private instance outbound access
- Route tables configured per subnet type

---

## 🖥️ EC2 Instances

| Instance | Role | Subnet | AZ |
|---|---|---|---|
| jump server | Bastion Host | web-pub-sub1 | us-east-1a |
| app-server1 | Application Server | app-pvt-sub1 | us-east-1a |
| app-server2 | Application Server | app-pvt-sub2 | us-east-1b |

- App servers run **Apache HTTP Server** and **PHP**
- Access to app servers only via **Bastion Host (SSH jump)**
- phpMyAdmin installed for database management

---

## 🗄️ Database

- **Engine:** MySQL (Amazon RDS)
- **Instance:** db.t3.micro
- **Identifier:** mydb-project
- **Region:** us-east-1a
- Deployed in **private subnet** (no public access)
- Security Group restricts DB access to app servers only

---

## 🔐 Security Configuration

- **Bastion Host** is the only publicly accessible instance
- App servers have **no public IP** — accessible only via jump server
- RDS is in a **private subnet** with access restricted to app-tier Security Group
- IAM roles and least-privilege policies applied
- Security Groups configured per tier (web, app, DB)

---

## 📸 Screenshots

### Architecture Diagram
![Architecture](Screenshots/architecture.png)

### Subnet Configuration
![Subnets 1](Screenshots/subnets1.png)
![Subnets 2](Screenshots/subnets2.png)

### Route Tables
![Route Tables](Screenshots/routetables.png)

### Internet Gateway
![Internet Gateway](Screenshots/igw.png)

### NAT Gateway
![NAT Gateway](Screenshots/nat.png)

### EC2 Instances Running
![EC2 Instances](Screenshots/ec2-instances.png)

### SSH via Bastion Host
![SSH Bastion](Screenshots/ssh1.png)

### Apache (httpd) Setup
![httpd](Screenshots/httpd.png)

### PHP Installation
![PHP Install](Screenshots/phpinstall.png)

### PHP Server Test
![PHP 1](Screenshots/php1.png)
![PHP 2](Screenshots/php2.png)
![PHP 3](Screenshots/php3.png)

### Load Balancer
![Load Balancer](Screenshots/lb.png)

### RDS MySQL Database
![RDS](Screenshots/rds.png)

---

## 🚀 Deployment Steps

1. **Create custom VPC** with CIDR block `20.0.0.0/16`
2. **Create 6 subnets** (2 public, 2 app-private, 2 db-private) across 2 AZs
3. **Attach Internet Gateway** and configure route tables
4. **Deploy NAT Gateway** in public subnet for private instance internet access
5. **Launch EC2 instances** — Bastion in public, app servers in private subnets
6. **Install Apache and PHP** on app servers via Bastion Host SSH
7. **Deploy Application Load Balancer** with target group pointing to app servers
8. **Launch RDS MySQL** in db-private subnets with multi-AZ setup
9. **Configure Security Groups** to enforce tier-level access control
10. **Set up CloudWatch alarms** for CPU and instance health monitoring

---

## 🛠️ Tech Stack

- **Cloud:** AWS
- **OS:** Amazon Linux 2023
- **Web Server:** Apache (httpd)
- **Backend:** PHP, phpMyAdmin
- **Database:** MySQL (Amazon RDS)
- **Scripting:** Bash / Shell

---

## 👤 Author

**Sameer Ali Khan**  
Cloud & DevOps Enthusiast  
📧 sameeralikhan2160@gmail.com  
🔗 [LinkedIn](https://linkedin.com/in/sameer-khan-9a6603249)
