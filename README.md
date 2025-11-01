🌀 Manual Scaling Implementation Guide

This project demonstrates how to implement manual scaling on AWS using EC2 instances and NGINX.
You’ll deploy multiple web servers, each hosting a simple HTML page, and manually distribute traffic among them.

📘 Overview

Manual scaling ensures that your application remains functional and available even when one server is down.

Benefits:

Availability – Keeps your app accessible even if one instance fails

Reliability – Maintains consistent performance

Scalability – You can manually add more servers when traffic increases

Fault Tolerance – Reduces downtime during instance failure

---------

⚙️ Prerequisites

Before starting, ensure you have:

An AWS account

Basic knowledge of EC2, NGINX, and SSH

A configured key pair for SSH access

AWS CLI or AWS Console access with permissions

--------------------

🚀 Step-by-Step Implementation
1. Create the Base EC2 Instance

Go to EC2 Dashboard → Launch Instance

Choose Amazon Linux 2 AMI (Free Tier eligible)

Select t2.micro instance type

Use or create a key pair

Under Network settings:

Enable Auto-assign Public IP

Select or create a Security Group

Launch the instance

2. Connect to the Instance via SSH
ssh -i ./moni.pem ec2-user@<public-ip>

Example:

ssh -i ./moni.pem ec2-user@54.227.90.252

3. Install and Configure NGINX
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx


Navigate to the default web directory:

cd /usr/share/nginx/html


Edit the default HTML file:

sudo nano index.html

Replace with this:

<!DOCTYPE html>
<html>
  <head>
      <title>Welcome</title>
  </head>
  <body>
      <h1> Welcome to Manual scaling </h1>
      <p>This web page is served from your EC2 instance.</p>
  </body>
</html>


Save and exit (Ctrl + O, Enter, Ctrl + X).

----------------------------------

4. Create Additional Servers

Launch 2 more EC2 instances (total 3) — you can copy the AMI from your base instance.

SSH into each and update their HTML page:
2nd Server:
<h1> Welcome to Manual Scaling2 </h1>
3rd Server:
<h1> Welcome to Manual Scaling3 </h1>

Now you have 3 web servers running NGINX, each showing a unique message.

---------------------

5. Test All Servers

Open each instance’s Public IP in your browser:

http://<public-ip>

http://<public-ip>

http://<public-ip>

Each should show its respective “Manual Scaling” message.

-------------------

🧩 Result

You’ve now implemented Manual Scaling — three independent servers that can each handle requests individually.

------------ 

## 🎥 Demo Video

[▶️ Click to Watch the Demo](https://github.com/vinittippanawar/manual-scaling/blob/main/videos/manual-scaling-demo.mp4)


------------------------

🏁 Summary

You have:

3 EC2 instances with NGINX running

Manually scaled architecture

Independent web pages served from each instance

This project demonstrates manual scaling fundamentals before moving to auto-scaling in AWS.

-----------------------------
✍️ Author

Vinit Tippanawar 

Cloud Computing Enthusiast | AWS Learner | Practicing Scalable Architectures
