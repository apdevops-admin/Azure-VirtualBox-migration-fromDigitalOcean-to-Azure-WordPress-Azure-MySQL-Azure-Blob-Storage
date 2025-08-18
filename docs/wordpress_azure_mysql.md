# WordPress on Azure with Azure Database for MySQL (GUI)

This guide shows how to run WordPress on an Azure VM (or App Service) while using **Azure Database for MySQL – Flexible Server** as the database backend. Steps are Azure Portal–first, with a few minimal commands where needed.

---

## 1. Create Azure Database for MySQL (Flexible Server)

1) In the Azure Portal: **Create a resource** → search **Azure Database for MySQL** → choose **Flexible Server** → **Create**.  
2) **Basics**
   - Resource group: `rg-wp-prod` (or your RG)
   - Server name: `wp-mysql-<unique>`
   - Region: same as your WordPress compute (VM/App Service)
   - Workload: **Development** (start small; scale later)
   - Admin username/password: set and save securely

<img width="839" height="535" alt="image" src="https://github.com/user-attachments/assets/3e24f244-cbbf-4b7e-aaf2-b88f5b0b3155" />
     
4) **Compute + storage**
   - Burstable (e.g., B1ms) for dev/small sites; General Purpose for production
   - Storage: start small; you can autoscale or increase later
5) **Networking**
   - Option A (simple): **Public access** → “Selected networks”, then add your VM’s public IP (or “Allow public access from any Azure service” while testing)
   - Option B (secure): **Private access (VNet Integration)** and place MySQL in the same VNet/subnet as the VM/App Service

<img width="831" height="556" alt="image" src="https://github.com/user-attachments/assets/e516ae71-9525-440d-bbb5-5e388d1481f2" />
<img width="774" height="492" alt="image" src="https://github.com/user-attachments/assets/58cb8d11-733e-41f1-a4bc-fd9da8fc56c3" />

6) **Review + Create** → **Create**
<img width="809" height="394" alt="image" src="https://github.com/user-attachments/assets/ce10addd-aada-48da-8521-4b61fa8ef47a" />




After deployment, open the server → **Overview** to note:
- **Server name/host** (e.g., `wp-mysql-xyz.mysql.database.azure.com`)
- **Admin user** (often `adminuser`)
- **Port** `3306`

---

## 2. Configure Networking & Firewall

If you chose **Public access**:
1) Go to your MySQL server → **Networking** → **Firewall rules**.  
2) Add your **VM’s outbound IP** (or your workstation IP if connecting from local).  
3) Save.

If you chose **Private access**:
1) Ensure the MySQL server is deployed with **VNet Integration** to the same VNet/subnet as the VM/App Service.  
2) Confirm DNS resolution works inside the VNet (default Azure DNS is fine).


---

<img width="862" height="298" alt="image" src="https://github.com/user-attachments/assets/6d76d52b-75c3-43b5-9303-930aa30635db" />
<img width="867" height="370" alt="image" src="https://github.com/user-attachments/assets/dbc6070a-99f2-4003-8af5-e0222b21736e" />

## 3. Create the Database and Application User

You can use **Query editor (preview)** in the Portal or connect from your VM.

**Connect from local system :**
1) MySQL server →  sign in with the admin credentials.  
2) Run:
   
CREATE DATABASE DB_name;
CREATE USER 'wp_user'@'%' IDENTIFIED BY 'REPLACE_WITH_STRONG_PASSWORD';
GRANT ALL PRIVILEGES ON wp_production.* TO 'wp_user'@'%';
FLUSH PRIVILEGES;

<img width="786" height="355" alt="image" src="https://github.com/user-attachments/assets/a733a92e-1f7c-4e31-b2c8-12c90078f1fc" />

4. Point WordPress to Azure MySQL

   On your WordPress host (Azure VM or App Service), edit wp-config.php

   /** Database settings */
define( 'DB_NAME',     'wp_production' );
define( 'DB_USER',     'wp_user' );
define( 'DB_PASSWORD', 'REPLACE_WITH_STRONG_PASSWORD' );
define( 'DB_HOST',     'wp-mysql-xyz.mysql.database.azure.com' ); // your server host
define( 'DB_CHARSET',  'utf8mb4' );
define( 'DB_COLLATE',  '' );

/** Enforce SSL to Azure MySQL (recommended) */
if (!defined('MYSQL_CLIENT_FLAGS')) {
    define('MYSQL_CLIENT_FLAGS', MYSQLI_CLIENT_SSL);
}

<img width="726" height="467" alt="image" src="https://github.com/user-attachments/assets/037dbc1e-7506-4b41-8974-2a35122af3e5" />





