Here is the single terminal command using a raw heredoc (`cat << 'EOF' > README.md`). Running this directly in your terminal will write the exact Markdown content—including all headings, tables, and code blocks—straight into `README.md` without breaking any formatting:

```bash
cat << 'EOF' > README.md
# ICT171 Cloud Server Project - Documentation

| Parameter | Details |
| :--- | :--- |
| **Student Name** | Sagar Kumar |
| **Student ID** | 36094816 |
| **Unit Code & Name** | ICT171 Introduction to Server Environments and Architectures |
| **Server Global IP** | 13.60.58.95 |
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

```

### 2.2 Security Group & Port Configuration

Inbound security rules were configured on the AWS EC2 management console to allow necessary administrative and web traffic:

* **Port 22 (SSH):** Inbound TCP traffic allowed (`0.0.0.0/0`) for remote administration.
* **Port 80 (HTTP):** Inbound TCP traffic allowed (`0.0.0.0/0`) for web access.
* **Port 443 (HTTPS):** Inbound TCP traffic allowed (`0.0.0.0/0`) for encrypted connections.

---

## 3. Web Server Installation & Site Configuration (Apache2)

### 3.1 Web Server Installation

The system packages were updated and the Apache2 web service was deployed:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2

```

### 3.2 Deploying Application Code with Interactive Scripting

The web application interface was deployed directly into `/var/www/html/index.html`, incorporating an interactive JavaScript feature:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>YTCreatorHub - Project</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 50px; background-color: #f4f4f9; }
        .card { background: white; padding: 30px; border-radius: 10px; max-width: 500px; margin: auto; box-shadow: 0px 4px 10px rgba(0,0,0,0.1); }
        button { padding: 10px 20px; font-size: 16px; cursor: pointer; background-color: #ff0000; color: white; border: none; border-radius: 5px; }
    </style>
</head>
<body>
    <div class="card">
        <h1>Welcome to YTCreatorHub</h1>
        <p>ICT171 Cloud Server Project Deployment</p>
        <p id="timestamp"></p>
        <button onclick="countClicks()">Click Me!</button>
        <p>Clicks: <span id="clickCount">0</span></p>
    </div>

    <script>
        document.getElementById("timestamp").innerText = "Page loaded at: " + new Date().toLocaleTimeString();
        let count = 0;
        function countClicks() {
            count++;
            document.getElementById("clickCount").innerText = count;
        }
    </script>
</body>
</html>

```

---

## 4. DNS Configuration (Porkbun)

### 4.1 Porkbun A Record Mapping

Custom DNS mapping was configured using Porkbun DNS Management to point domain names directly to the AWS EC2 Global IP address (`13.60.58.95`):

| Type | Host | Answer / Value | TTL |
| --- | --- | --- | --- |
| **A** | `ytcreatorhub.xyz` | `13.60.58.95` | `600` |
| **A** | `www.ytcreatorhub.xyz` | `13.60.58.95` | `600` |

---

## 5. SSL/TLS Encryption & HTTPS Setup (Certbot)

### 5.1 SSL Certificate Provisioning

To secure traffic over HTTPS, Let's Encrypt SSL certificates were generated and configured automatically for Apache using Certbot:

```bash
# Install Certbot and Apache plugin
sudo apt install certbot python3-certbot-apache -y

# Issue certificate and enable automatic HTTPS redirection
sudo certbot --apache -d ytcreatorhub.xyz -d www.ytcreatorhub.xyz

```

### 5.2 Verification

Automated SSL certificate renewal mechanisms were verified:

```bash
sudo certbot renew --dry-run

```

---

## 6. Development & Milestone Log

* **Week 1:** Provisioned AWS EC2 Ubuntu instance, configured Inbound Security Group rules (Ports 22, 80, and 443), and established SSH connectivity.
* **Week 2:** Installed Apache2 web server, mapped Porkbun DNS A Records for `ytcreatorhub.xyz`, and configured Let's Encrypt SSL/TLS using Certbot.
* **Week 3:** Integrated dynamic JavaScript counter scripting into HTML, authored project documentation, recorded video walkthrough, and finalized submission PDF.
EOF

```

---

### Push to GitHub

Once executed, push the updated `README.md` to GitHub using these standard commands:

```bash
git add README.md
git commit -m "docs: update README with project architecture and deployment details"
git push origin main

```
