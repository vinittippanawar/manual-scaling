# 🌀 AWS Manual Scaling Implementation Guide

This project demonstrates how to implement **manual scaling** on AWS using **EC2 instances**, **NGINX**, and **an Application Load Balancer (ALB)**.  
You’ll deploy multiple web servers, each hosting a simple HTML page, and manually scale them for better performance and fault tolerance.

---

## 📘 Overview

Manual scaling ensures that no single server bears too much traffic, improving:

- **Availability** – Keeps your app accessible even if one server fails  
- **Reliability** – Maintains consistent performance  
- **Scalability** – Handles increased traffic by adding more servers manually  
- **Fault Tolerance** – Minimizes downtime during outages  

---

## ⚙️ Prerequisites

Before starting, ensure you have:

- An **AWS account**
- Basic knowledge of **EC2**, **NGINX**, and **SSH**
- A configured **key pair** for SSH access
- AWS CLI or Console access with sufficient permissions

---

## 🚀 Step-by-Step Implementation

### 1. Create the Base EC2 Instance

1. Go to **EC2 Dashboard → Launch Instance**
2. Select **Amazon Linux 2 AMI**
3. Choose instance type (e.g., `t2.micro`)
4. Create/select a **key pair**
5. Under **Network settings**:
   - Enable **Auto-assign Public IP**
   - Select existing **security group**
6. Click **Launch Instance**

---

### 2. Connect to the Instance via SSH

```bash
ssh -i ./path/to/keypair.pem ec2-user@<public-ip>
