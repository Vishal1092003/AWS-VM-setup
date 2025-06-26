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
