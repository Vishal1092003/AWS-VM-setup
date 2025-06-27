## SSH into AWS EC2 Instance

This README describes how to securely connect from your local machine to an AWS EC2 instance using SSH.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Certificate (Private Key) Permissions](#certificate-private-key-permissions)

   * [On Linux / WSL](#on-linux--wsl)
3. [Connecting to the EC2 Instance](#connecting-to-the-ec2-instance)
4. [Example](#example)

---

## Prerequisites

* An AWS EC2 instance running and accessible over SSH.
* A PEM-format private key file (e.g., `vishal-Suarsapv@123.pem`).
* The public IP address or DNS name of the EC2 instance (e.g., `16.171.169.70`).
* SSH client installed on your machine:

  * **Linux/macOS/WSL**: built-in `ssh`
  * **Windows**: use WSL, Git Bash, or built-in OpenSSH (Windows 10+)

## Certificate (Private Key) Permissions

SSH requires that your private key file be readable **only** by you. Below are instructions for setting the correct permissions.

### On Linux / WSL

```bash
# Navigate to the directory containing your PEM file
cd /mnt/d/Deploy/aws/aws\ account/PemFile

# Set file permissions to read-only for your user
chmod 400 vishal-Suarsapv@123.pem
```

## Connecting to the EC2 Instance

Once your private key has correct permissions, initiate an SSH session:

```bash
ssh -i vishal-Suarsapv@123.pem ubuntu@16.171.169.70
```

* `-i` specifies the identity (private key) file.
* `ubuntu` is the default user for Ubuntu AMIs; use the appropriate user for your AMI (e.g., `ec2-user`, `centos`).
* Replace `16.171.169.70` with your instance's public IP or DNS name.

## Example

1. **Set permissions** (Linux):

   ```bash
   chmod 400 vishal-Suarsapv@123.pem
   ```
2. **SSH into server**:

   ```bash
   ssh -i vishal-Suarsapv@123.pem ubuntu@16.171.169.70
   ```

You should now be securely connected to your AWS EC2 instance.














## nginx (engine-x)
## note whenever you change the configuration of ngnix we need to restart the ngnix :   sudo nginx -s reload
## to run the backend always : npm i -g pm2
       ## then  : pm2 start index.js
       ## to see the logs : pm2 logs 0
     
  ## SSH into AWS EC2 Instance

```bash
chmod 400 vishal-Suarsapv@123.pem  # Restrict key file permissions securely
ssh -i vishal-Suarsapv@123.pem ubuntu@16.171.169.70  # Connect via SSH using private key
```

## Install and Configure MongoDB

```bash
curl -fsSL https://www.mongodb.org/static/pgp/server-6.0.asc | gpg --dearmor \
  | sudo tee /usr/share/keyrings/mongodb-org-archive-keyring.gpg > /dev/null  # Import MongoDB signing key

echo "deb [arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-org-archive-keyring.gpg] \
  https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" \
  | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list  # Add MongoDB repository

sudo apt-get update  # Refresh package lists
sudo apt-get install -y mongodb-org  # Install MongoDB server and tools
sudo systemctl start mongod  # Start MongoDB service
sudo systemctl enable mongod  # Enable MongoDB on boot
```

## Install and Configure Nginx

```bash
sudo apt update  # Update package database
sudo apt install nginx  # Install Nginx web server
```

### Configure Reverse Proxy (Map port 80 → 8080)

```bash
sudo rm /etc/nginx/nginx.conf  # Remove default configuration
sudo tee /etc/nginx/nginx.conf > /dev/null << 'EOF'
events {}

http {
  server {
    listen 80;  # Public HTTP port
    server_name backedn;  # Your domain name

    location / {
      proxy_pass http://localhost:8080;  # Forward requests to backend
      proxy_http_version 1.1;  # Use HTTP/1.1 for WebSocket support
      proxy_set_header Upgrade $http_upgrade;  # Handle WebSocket upgrades
      proxy_set_header Connection 'upgrade';
      proxy_set_header Host $host;  # Pass original Host header
      proxy_cache_bypass $http_upgrade;  # Bypass cache on WebSocket
    }
  }
}
EOF

sudo nginx -s reload  # Reload Nginx with new settings
```

## Run Backend with PM2

```bash
npm i -g pm2  # Install PM2 globally
pm2 start index.js  # Start Node.js app as daemon
pm2 logs 0  # View application logs
pm2 startup  # Generate startup script for PM2
```

---

*Each command above is paired with its concise purpose.*









