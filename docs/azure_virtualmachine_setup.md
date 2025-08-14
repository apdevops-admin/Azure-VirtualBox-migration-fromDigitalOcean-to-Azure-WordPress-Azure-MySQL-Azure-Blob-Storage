# Azure Virtual Machine Setup 

This guide walks through creating an Azure Virtual Machine using the Azure Portal.  

---

## 1. Sign in to the Azure Portal
1. Open your browser and go to [https://portal.azure.com](https://portal.azure.com).
2. Log in with your Azure account (Free Tier account works fine).

You will land on the Azure Portal dashboard.

---

## 2. Create a Resource Group
A resource group is a container for your Azure resources.

1. In the top search bar, type **Resource groups** and click **Create**.
2. Make sure your **Subscription** is set to the Free Tier subscription.
3. Name it `rg-free-vm`.
4. Choose a **Region** close to your location.
5. Click **Review + create**, then **Create**.

![Create Resource Group](https://raw.githubusercontent.com/apdevops-admin/Azure-VirtualBox-migration-fromDigitalOcean-to-Azure-WordPress-Azure-MySQL-Azure-Blob-Storage/main/screenshots/create_resource_group.png)



---

## 3. Create the Virtual Machine
1. On the Azure Portal home page, click **Create a resource**.
2. Search for **Virtual Machine** and click **Create**.
3. In the **Basics** tab:
   - Resource group: `rg-free-vm`
   - Virtual machine name: `free-linux-vm`
   - Region: same as resource group
   - Image: **Ubuntu Server 24.04 LTS**
   - Size: **B1s** (Free Tier eligible)
   - Authentication type: **SSH public key** or **Password**
   - Username: `azureuser`
   - Inbound ports: Allow **SSH (22)** and optionally **HTTP (80)** if hosting a site

![VM Basics](screenshots/vm_basics1.png)
![VM Basics](screenshots/vm_basics2.png)
![VM Basics](screenshots/vm_basics3.png)

---

## 4. Configure the Disk
1. OS disk type: **Standard HDD** (30 GB )
2. Keep the default encryption settings.

![VM Disks](screenshots/vm_disks.png)

---

## 5. Networking
1. Use the automatically created **Virtual Network** and **Subnet**.
2. Public IP: **Enabled** (Dynamic).
3. Network Security Group: Select **Basic** and ensure SSH is allowed.

![VM Networking](screenshots/vm_networking.png)

---

## 6. Management Settings
1. Enable **Auto-shutdown** to save costs when the VM is not in use.
2. Disable boot diagnostics unless you specifically need them.

![VM Management](screenshots/vm_management.png)

---

## 7. Review and Create
1. Ensure you see the **Free Tier** eligibility message at the top.
2. Click **Create**.
3. Wait for deployment to complete.

![VM Review](screenshots/vm_review.png)

---

## 8. Connect to the VM
1. Open your VM resource in the Azure Portal.
2. Click **Connect** → **SSH**.
3. Copy the provided SSH command.
4. Paste it into your local terminal:
   ```bash
   ssh azureuser@<PUBLIC_IP>
   ssh -i /home/user_name/keys/login.pem user@ip
   
