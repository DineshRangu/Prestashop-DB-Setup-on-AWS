# Prestashop-DB-Setup-on-AWS
Complete Setup Guide for Bitnami PrestaShop on AWS EC2 — A clear walkthrough of getting PrestaShop running using the Bitnami AMI on AWS. From launching your EC2 instance and securing your key pair, to connecting over SSH, exploring the file system, and finding your admin credentials — all in one place.
AWS PrestaShop Setup – Full Process

# 1.Launched EC2 Instance
-Service: AWS (Amazon Web Services)

-Instance Type: t3.micro (1 vCPU, 1GB RAM — free tier eligible)

-AMI: Amazon Machine Image for Bitnami PrestaShop 8.1.7

-Key Pair: Downloaded .pem file for SSH authentication.

-Reason: .pem file stores the private key required to securely connect via SSH.

-Set correct permissions using:
``
chmod 400 key-prestashop-server2.pem
``

This ensures the key is only readable by the owner (required by SSH).

# 2.Configured Security Group
-Allowed #HTTP (80) and HTTPS (443) → For web traffic access to PrestaShop site.

-Allowed SSH (22) → For secure terminal access.

-SSH Connection to Instance
Connected using:
ssh -i key-prestashop-server2.pem bitnami@<public-ip>

-Initially got a “UNPROTECTED PRIVATE KEY FILE” error because the .pem file had open permissions (0555).

Fixed by cloning .pem inside WSL’s Linux filesystem and setting chmod 400.

Reason: Files stored on /mnt/c or /mnt/d (Windows drives) don’t properly apply Linux permissions, so SSH rejects them. Cloning moves them to WSL’s native filesystem, where permissions are enforced correctly.

Accessed Bitnami PrestaShop Environment

After SSH login, listed files:

``bitnami_credentials  htdocs  stack``


Viewed bitnami_credentials file to get default admin username & password (hidden here for security).

These credentials allow logging into:

PrestaShop admin panel (web browser)

<img width="1919" height="913" alt="image" src="https://github.com/user-attachments/assets/2c1f52f2-cdb0-40d7-98f9-1659cdd02298" />


MySQL database (included in Bitnami stack)

Logged Into PrestaShop Admin Dashboard

Used public IP + /admin path in browser.

Entered provided credentials to access the PrestaShop dashboard.
