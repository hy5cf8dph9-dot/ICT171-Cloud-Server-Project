# ICT171 Cloud Server Project - Documentation

| Parameter | Details |
| :--- | :--- |
| **Student Name** | Sagar Kumar |
| **Student ID** | 36094816 |
| **Unit Code & Name** | ICT171 Introduction to Server Environments and Architectures |
| **Server Global IP** | http://13.60.58.95 |
| **Domain Name** | https://ytcreatorhub.xyz |
| **GitHub Repository** | https://github.com/hy5cf8dph9-dot/ICT171-Cloud-Server-Project |
| **Video Explainer Link**| [Insert Video Link Here] |

---

## 1. Project Architecture & Overview
This project documents the deployment, security hardening, and configuration of an Infrastructure as a Service (IaaS) cloud server running on Ubuntu Server 26.04 LTS on AWS EC2.

The server hosts **YTCreatorHub**, a web platform integrated with custom client-side interactive scripting and dynamic page metadata.

### Multi-Purpose Server Architecture:
1. **Infrastructure Layer (IaaS):** Amazon Web Services (AWS) EC2 `t3.micro` instance configured via SSH terminal.
2. **Web Service Engine:** Apache2 web server managing web delivery.
3. **Domain Name System:** Custom DNS A Records configured via Porkbun (`ytcreatorhub.xyz` and `www.ytcreatorhub.xyz`).
4. **Transport Layer Security:** SSL/TLS certificate issued via Certbot (Let's Encrypt) with enforced HTTP-to-HTTPS redirection.
5. **Scripting Subsystem:** Client-side JavaScript embedded in `/var/www/html/index.html` providing dynamic timestamp rendering and an interactive click counter.

---

## 2. Infrastructure Provisioning & Security Setup (AWS EC2)

### 2.1 SSH Access
Access to the remote cloud server was established via SSH terminal:
```bash
ssh -i "your-key.pem" ubuntu@13.60.58.95
