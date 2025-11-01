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


### 3. Install and Configure NGINX

```bash
sudo yum install nginx -y
sudo service nginx start
sudo systemctl enable nginx.service
```

Navigate to the default web directory:

```bash
cd /usr/share/nginx/html
ls
```

Open the default HTML file:

```bash
sudo nano index.html
```

Replace the content with:

```html
<!doctype html>
<html>
  <body>
    <h1>Welcome to the manual scaling</h1>
  </body>
</html>
```

Save and exit (`Ctrl + S`, `Ctrl + X`).

---

### 4. Test the Web Server

- Copy the **public IP** of your EC2 instance.
- Open it in your browser as:

```
http://<public-ip>
```

If you see **“Welcome to the manual scaling”**, everything works.

If you encounter errors:
- Go to your **Security Group**
- Edit **Inbound Rules**
- Add a new rule:
  - **Type:** HTTP  
  - **Source:** Anywhere (IPv4)
- Save the rule and retry.

---

### 5. Create an AMI (Image) from the Instance

1. Select your base instance → **Actions → Create Image**
2. Give it a name → **Create Image**
3. Go to **AMIs** → Wait for image creation to complete

---

### 6. Launch Additional Instances from the AMI

1. Go to **AMIs** → Select the created image  
2. Click **Launch Instance from AMI**  
3. In **Summary**, choose how many instances to create (e.g., 2)
4. Use the same:
   - **Key pair**
   - **Security group**
   - **Auto-assign Public IP**
5. Launch the instances  
6. Rename them as:
   - `1st Server`
   - `2nd Server`
   - `3rd Server`

Verify each instance by opening their public IPs in a browser.  
You should see **“Welcome to the 1st server”** for all, as they’re replicas.

> 💡 You can delete the AMI later to avoid charges:
> Go to **AMIs → Select → Actions → Deregister.**

---

### 7. Customize Each Server’s HTML Page

SSH into each instance one by one:

```bash
cd /usr/share/nginx/html
sudo nano index.html
```

Modify each page as follows:

- **1st Server:** `Welcome to the manual scaling`
- **2nd Server:** `Welcome to the manual scaling2`
- **3rd Server:** `Welcome to the manual scaling3`

Save and exit.

Test all public IPs again — each should display its respective message.

---

### 8. Create an Application Load Balancer (ALB)

1. Go to **Load Balancers → Create Load Balancer**
2. Choose **Application Load Balancer**
3. Name it (e.g., `my-load-balancer`)
4. Select **Internet-facing**
5. Choose **2 or more subnets**
6. Select your **default security group** (or create one)
7. Scroll to **Target Groups → Create a Target Group**
   - Name it (e.g., `my-target-group`)
   - Register your 3 EC2 instances
   - Click **Create Target Group**
8. Return to ALB creation tab, **refresh target groups**
9. Select your new target group and click **Create Load Balancer**

Wait until the **Status** becomes **Active**.

---

### 9. Update Security Group for the Load Balancer

1. Go to **Load Balancers → Select ALB → Security → Security Group ID**
2. Edit inbound rules → Add:
   - **Type:** HTTP  
   - **Source:** Anywhere (IPv4)
3. Save changes.

---

### 10. Test the Load Balancer

- Copy the **DNS name** of your ALB
- Paste it into your browser as:

```
http://<load-balancer-dns>
```
https://github.com/user-attachments/assets/fdf64482-c9e4-4e32-a849-7f744bf253d7

Each refresh should show a different server (1st, 2nd, or 3rd), proving that the load balancer is distributing traffic evenly.

---


## 🧩 Result

Your setup now includes:

- **3 EC2 instances** running NGINX  
- **1 Application Load Balancer** distributing HTTP traffic  
- **Automatic failover and scalability**  

Each refresh hits a different backend server, confirming that load balancing works as expected.

---

## 💰 Cost Optimization

- **Terminate unused instances**
- **Deregister AMIs** no longer needed
- **Delete unused load balancers** when testing is complete

---
## 📄 Author

**vinit tippanawar**  
Cloud Computing Enthusiast | AWS Learner | Practicing High Availability Architecture  

---

## 🏁 Summary

You’ve successfully deployed a scalable, fault-tolerant, and load-balanced web environment on AWS using EC2 and NGINX.  
Each server responds uniquely, confirming traffic is evenly distributed by the ALB.
