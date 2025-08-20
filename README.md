<h1 align="center" id="title">Prestashop-DB Setup on AWS</h1>

<p id="description">This project is a complete guide to getting PrestaShop up and running on AWS EC2 using the Bitnami AMI. It walks you through every step — from launching your EC2 instance and setting up security groups to connecting over SSH grabbing your admin credentials and logging into the PrestaShop dashboard.</p>
<img src="https://github.com/user-attachments/assets/f3196318-f6bf-403d-bb28-e4eb316bbf42" alt="Logo 1" width="250"/>  
<img src="https://github.com/user-attachments/assets/49917104-866f-4619-a097-5ef034704450" alt="Logo 2" width="180"/>  
<img src="https://github.com/user-attachments/assets/910cd89c-12da-460f-a37c-ff544f6c07ca" alt="Logo 3" width="500"/>  




<img width="1530" height="728" alt="image" src="https://github.com/user-attachments/assets/c9db09fa-7813-4da6-997f-7f3d26182a3c" />


<img src="https://ibb.co/Nn2mnLrM" alt="project-screenshot" width="400" height="400/"> 

<h2>🛠️ Installation Steps:</h2>

<p> 1. Launch EC2 Instance Open AWS Management Console → EC2 → Launch Instance Choose Bitnami PrestaShop 8.1.7 AMI Instance Type: t3.micro (Free Tier eligible) Download the .pem key pair (e.g. key-prestashop-server2.pem)</p>

<p>2. Set Key File Permissions</p>

```
chmod 400 key-prestashop-server2.pem
```

<p>3. Configure Security Group Allow inbound rules: HTTP (80) → Access store frontend HTTPS (443) → Secure access SSH (22) → Terminal login</p>

<p>4. Connect to EC2 Instance via SSH</p>

```
ssh -i key-prestashop-server2.pem bitnami@<public-ip>
```

<p>5. If you see UNPROTECTED PRIVATE KEY FILE! → Move the .pem into WSL/Linux home folder and reapply permissions:</p>

```
cp /mnt/c/Users//Downloads/key-prestashop-server2.pem ~/ chmod 400 ~/key-prestashop-server2.pem ssh -i ~/key-prestashop-server2.pem bitnami@
```

<p>6. Locate PrestaShop Credentials</p>

```
cat ~/bitnami_credentials
```

<p>7. Access PrestaShop in Browser:-Admin Dashboard:</p>

```
http://<public-ip>/admin
```
