#!/bin/bash
# ==============================================================================
# ICT171 Cloud Server Master Deployment Script - YTCreatorHub
# Target Domains: ytcreatorhub.xyz & www.ytcreatorhub.xyz
# Global IP: 13.60.58.95
# ==============================================================================

set -e # Exit immediately if any command fails

# ------------------------------------------------------------------------------
# CONFIGURATION VARIABLES
# ------------------------------------------------------------------------------
DOMAIN="ytcreatorhub.xyz"
WWW_DOMAIN="www.ytcreatorhub.xyz"
WEB_ROOT="/var/www/html"
SSL_EMAIL="your-email@example.com" # Replace with your email for SSL alerts

echo "=================================================="
echo "Starting Full Deployment for $DOMAIN"
echo "=================================================="

# ==============================================================================
# 2. INFRASTRUCTURE PROVISIONING & SECURITY SETUP
# ==============================================================================
echo "[1/4] Updating System Repositories..."

sudo apt update && sudo apt upgrade -y

# Install DNS utilities for domain verification
sudo apt install dnsutils -y

# ==============================================================================
# 3. WEB SERVER INSTALLATION & SITE CONFIGURATION (APACHE2)
# ==============================================================================
echo "[2/4] Installing Apache2 & Deploying Application Code..."

sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2

# Enable Apache rewrite module for SSL redirects
sudo a2enmod rewrite

# Deploy application code with interactive JavaScript counter to /var/www/html/index.html
sudo bash -c "cat << 'EOF' > $WEB_ROOT/index.html
<!DOCTYPE html>
<html lang=\"en\">
<head>
    <meta charset=\"UTF-8\">
    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">
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
EOF"

# Adjust directory permissions
sudo chown -R www-data:www-data $WEB_ROOT
sudo chmod -R 755 $WEB_ROOT

# Restart Apache to apply baseline configuration
sudo systemctl restart apache2

# ==============================================================================
# 4. DNS CONFIGURATION (PORKBUN)
# ==============================================================================
echo "[3/4] Verifying Porkbun DNS Propagation..."

echo "Checking DNS resolution for $DOMAIN..."
dig +short $DOMAIN || true

echo "Checking DNS resolution for $WWW_DOMAIN..."
dig +short $WWW_DOMAIN || true

# ==============================================================================
# 5. SSL/TLS ENCRYPTION & HTTPS SETUP (CERTBOT)
# ==============================================================================
echo "[4/4] Installing Certbot and Provisioning SSL/TLS Certificate..."

sudo apt install certbot python3-certbot-apache -y

# Issue certificate and enable HTTP-to-HTTPS redirection
if [ "$SSL_EMAIL" != "your-email@example.com" ]; then
    sudo certbot --apache -d $DOMAIN -d $WWW_DOMAIN --non-interactive --agree-tos -m $SSL_EMAIL --redirect
else
    echo "Running Certbot interactively..."
    sudo certbot --apache -d $DOMAIN -d $WWW_DOMAIN
fi

# Verify Certificate Auto-Renewal
echo "Testing automated certificate renewal..."
sudo certbot renew --dry-run

echo "=================================================="
echo "DEPLOYMENT COMPLETED SUCCESSFULLY!"
echo "Live Website: https://$DOMAIN"
echo "=================================================="
