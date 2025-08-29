<h1 align="center" id="title">PrestaShop on AWS: A Comprehensive Setup Guide</h1>

<p id="description">This guide provides a complete walkthrough for deploying a PrestaShop e-commerce site on AWS using the Bitnami-certified AMI. Follow these steps to launch an EC2 instance, configure security, retrieve your credentials, and get your online store up and running.</p>

<div align="center">
<img src="https://github.com/user-attachments/assets/f3196318-f6bf-403d-bb28-e4eb316bbf42" alt="PrestaShop Logo" width="250"/>&nbsp;&nbsp;
<img src="https://github.com/user-attachments/assets/49917104-866f-4619-a097-5ef034704450" alt="Bitnami Logo" width="180"/>&nbsp;&nbsp;
<img src="https://github.com/user-attachments/assets/910cd89c-12da-460f-a37c-ff544f6c07ca" alt="AWS Logo" width="500"/>
</div>

<br>

![PrestaShop Dashboard Screenshot](https://github.com/user-attachments/assets/c9db09fa-7813-4da6-997f-7f3d26182a3c)

---

## 📋 Prerequisites

Before you begin, ensure you have the following:
* An active **AWS Account**.
* An **SSH client** installed on your computer (e.g., Terminal on macOS/Linux, or WSL/PuTTY on Windows).

---

## 🚀 Installation Steps

### 1. Launch the EC2 Instance

First, we will launch a virtual server (EC2 instance) from the AWS console.

1.  Navigate to the **AWS Management Console** → **EC2** → **Launch Instance**.
2.  In the "Application and OS Images" search bar, find **"Bitnami PrestaShop"** and select the latest available version.
3.  For **Instance Type**, choose `t3.micro`. This is eligible for the AWS Free Tier and is sufficient for a small store.
4.  In the **Key pair (login)** section, create a new key pair. Give it a memorable name (e.g., `prestashop-key`) and download the `.pem` file.
    > **⚠️ Important:** Store this `.pem` file securely. You cannot download it again, and it is the only way to access your server's command line.

### 2. Configure the Security Group

A security group acts as a virtual firewall for your instance. You need to open specific ports to allow access.

1.  While launching the instance, create a new security group.
2.  Add the following **inbound rules**:

| Type | Port | Source | Description |
| :--- | :--- | :--- | :--- |
| **SSH** | 22 | My IP | Allows secure terminal access from your IP only. |
| **HTTP** | 80 | Anywhere | Allows standard web traffic to your store frontend. |
| **HTTPS**| 443 | Anywhere | Allows secure, encrypted web traffic. |

3.  Finish launching the instance and wait for it to initialize.

### 3. Connect to Your Instance via SSH

Now, you'll connect to the server's terminal to retrieve your password.

1.  Open your terminal and navigate to the directory where you saved your `.pem` key file.
2.  Set the correct file permissions. This is a security measure required by SSH.
    ```bash
    chmod 400 your-key-name.pem
    ```
    > **What does `chmod 400` do?** This command makes your key file read-only *exclusively for you*, preventing unauthorized access.

3.  Connect to the instance using its **Public IPv4 address** (you can find this on your EC2 instance summary page).
    ```bash
    ssh -i your-key-name.pem bitnami@<your-public-ip-address>
    ```

    > **Troubleshooting: "UNPROTECTED PRIVATE KEY FILE!"**
    > If you are using Windows Subsystem for Linux (WSL) and your key is in your Windows Downloads folder (`/mnt/c/...`), you may see this error.
    >
    > **Solution:** Move the key file into your WSL home directory (`~`) and set the permissions again.
    > ```bash
    > # Copy the key from Windows Downloads to your WSL home folder
    > cp /mnt/c/Users/YourWindowsUser/Downloads/your-key-name.pem ~/
    >
    > # Set permissions again
    > chmod 400 ~/your-key-name.pem
    >
    > # Connect using the new path
    > ssh -i ~/your-key-name.pem bitnami@<your-public-ip-address>
    > ```

### 4. Retrieve Your PrestaShop Credentials 🔑

Once you are connected, Bitnami provides the initial application credentials in a text file.

1.  Run the following command to display the contents of the file:
    ```bash
    cat ~/bitnami_credentials
    ```
2.  This will show the default **username** (usually `user`) and a randomly generated **password**. Copy the password to your clipboard.

### 5. Log in to Your PrestaShop Dashboard

Your store is now live!

1.  **Access your Store Front** by navigating to your instance's public IP in a browser:
    `http://<your-public-ip-address>`
2.  **Access the Admin Dashboard** by adding `/admin` to the URL:
    `http://<your-public-ip-address>/admin`
3.  Log in using the username and password you retrieved in the previous step.

---

## ✅ Next Steps & Best Practices

Your store is running, but for a production environment, you should complete these final steps:

1.  **Change the Default Password:** Inside the PrestaShop admin dashboard, immediately change the default password to something strong and unique.
2.  **Assign an Elastic IP:** By default, the public IP of your instance will change if it's stopped and restarted. To get a permanent, static IP address, go to the **EC2 Console → Elastic IPs**, allocate a new IP, and associate it with your instance.
3.  **Configure a Domain Name:** Point your custom domain (e.g., `www.yourstore.com`) to your new Elastic IP using your domain registrar's DNS settings.
4.  **Enable SSL (HTTPS):** Bitnami includes a simple tool to install a free Let's Encrypt SSL certificate. Run the following command in your server's SSH terminal and follow the prompts:
    ```bash
    sudo /opt/bitnami/bncert-tool
    ```
