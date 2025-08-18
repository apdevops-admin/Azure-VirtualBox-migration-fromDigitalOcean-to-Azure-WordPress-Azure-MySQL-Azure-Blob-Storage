Migrating WordPress from DigitalOcean to Azure

1. Prerequisites

A running WordPress site on DigitalOcean Droplet with MySQL database.

A GitHub account and repository.

An Azure account with:

App Service

Azure Database for MySQL

Azure Blob Storage

2. Step 1: Backup WordPress Files and Database from DigitalOcean

Backup Files

# SSH into your DigitalOcean Droplet
ssh root@your_droplet_ip

# Navigate to WordPress root
cd /var/www/html

# Create a compressed backup
tar -czvf wordpress_files.tar.gz .


Backup Database

mysqldump -u db_user -p db_name > wordpress_db.sql

Copy both backups to your local machine:

scp root@your_droplet_ip:/var/www/html/wordpress_files.tar.gz .
scp root@your_droplet_ip:~/wordpress_db.sql .

Step 2: Push WordPress to GitHub

Initialize Git repo:

mkdir wordpress-migration
cd wordpress-migration
tar -xzvf wordpress_files.tar.gz
git init
git remote add origin https://github.com/yourusername/wordpress-azure-migration.git
git add .
git commit -m "Initial WordPress migration from DigitalOcean"
git push -u origin main

<img width="1430" height="882" alt="image" src="https://github.com/user-attachments/assets/56c925f4-3985-4235-95b1-3a2fc2e205d8" />


Setup Azure Environment
Reference Guide: 
[Azure Virtual Machine Setup Guide →](https://github.com/apdevops-admin/Azure-VirtualBox-migration-fromDigitalOcean-to-Azure-WordPress-Azure-MySQL-Azure-Blob-Storage/blob/main/docs/azure_virtualmachine_setup.md)

[Azure Blob Storage Integration Guide](https://github.com/apdevops-admin/Azure-VirtualBox-migration-fromDigitalOcean-to-Azure-WordPress-Azure-MySQL-Azure-Blob-Storage/blob/main/docs/azure_blob_storage_integration.md)



