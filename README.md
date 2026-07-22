# ICT171 Cloud Server Project Documentation

* **Student Name:** Sagar Kumar
* **Student ID:** 36094816
* **Server Global IP Address:** http://13.60.58.95
* **Domain Name:** https://ytcreatorhub.xyz
* **Video Explainer Link:** [Insert Video Link Here]

---

## 1. Executive Summary & Overview
This project documents the deployment, configuration, and security setup of a cloud-based web server using Infrastructure as a Service (IaaS) on Amazon Web Services (AWS) EC2. The server hosts the website `ytcreatorhub.xyz`, configured with custom DNS records via Porkbun, secured with SSL/TLS encryption, and features an embedded interactive JavaScript application.

---

## 2. Infrastructure Setup (AWS EC2)
* **Cloud Provider:** Amazon Web Services (AWS)
* **Instance Type:** EC2 (`t3.micro`)
* **Operating System:** Ubuntu Server 26.04 LTS
* **Security Group Configuration:**
  * **Port 22 (SSH):** Inbound traffic allowed for administrative management.
  * **Port 80 (HTTP):** Inbound traffic allowed (`0.0.0.0/0`) for web browsing.
  * **Port 443 (HTTPS):** Inbound traffic allowed (`0.0.0.0/0`) for secure web connections.

---

## 3. Web Server & DNS Configuration
### Step 1: Web Server Installation (Apache)
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
