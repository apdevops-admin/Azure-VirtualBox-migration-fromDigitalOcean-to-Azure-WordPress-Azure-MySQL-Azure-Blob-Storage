# Azure Blob Storage Integration with WordPress

This document explains how to offload WordPress media to Azure Blob Storage for scalability and lower disk usage on your VM.

---

## 1. Create a Storage Account (GUI)
1. In the Azure Portal → Create a resource → Storage account.
2. Provide:
   - Resource group: `ur_resourcegrp` (or chosen RG)
   - Name: `wpstorage<unique>`
   - Region: same as WordPress VM/App Service
   - Performance: Standard
   - Account kind: StorageV2
3. Create a container (e.g., `wp-media`) and set access level to private.

<img width="912" height="434" alt="image" src="https://github.com/user-attachments/assets/92a8e919-c2da-409c-8b22-cddd6e7c46f1" />
<img width="1064" height="802" alt="image" src="https://github.com/user-attachments/assets/743eb749-4c32-4a68-8327-22cc4f536efe" />
<img width="946" height="756" alt="image" src="https://github.com/user-attachments/assets/82630315-c05c-4bf6-a4e3-6d80f184b23e" />
<img width="878" height="373" alt="image" src="https://github.com/user-attachments/assets/5b809997-45c2-4bfa-91b1-2f52e9bf398f" />
<img width="875" height="260" alt="image" src="https://github.com/user-attachments/assets/1ba46f76-61a0-4952-982a-b167ce3be44a" />
<img width="899" height="453" alt="image" src="https://github.com/user-attachments/assets/b945d450-119d-4660-87e9-14c4432241eb" />
<img width="867" height="317" alt="image" src="https://github.com/user-attachments/assets/a97640c0-d1a5-42bb-a5dd-5715eb69232f" />



---

## 2. Obtain Access Credentials
1. In the storage account → Access keys → copy `Account name` and `Key` (or use SAS token).
2. Alternatively, configure a Managed Identity for the VM/App Service and grant it `Storage Blob Data Contributor` role.

<img width="830" height="569" alt="image" src="https://github.com/user-attachments/assets/abc08b89-33ed-47fa-b7c8-24ed45bc7b25" />

---

## 3. Install WordPress Plugin (GUI)
1. In WordPress Dashboard → Plugins → Add New.
2. Search and install a plugin that supports Azure Blob Storage (e.g., "Microsoft Azure Storage" or compatible third-party plugin).
3. Configure plugin with:
   - Storage account name
   - Access key or SAS
   - Container name (e.g., `ur-containername`)
4. Set upload rewrite rules (so future uploads go to Blob Storage).

<img width="866" height="598" alt="image" src="https://github.com/user-attachments/assets/8509cedf-5cff-4830-93f3-12ac2b0d0dc9" />


---

## 4. Migrate existing media (plugin-dependent)
1. Many plugins provide a migration tool to copy existing `wp-content/uploads` to Blob Storage.
2. Run migration and verify files appear in the Azure Storage container.

<img width="875" height="694" alt="image" src="https://github.com/user-attachments/assets/b8099089-194b-4287-8dd4-0aa6434e7937" />

---

## 5. Serve media securely and efficiently
- Use Azure CDN in front of Blob Storage for global delivery and caching.
- If using private blobs, generate SAS URLs via plugin or custom code for secure access.

---

## 6. Troubleshooting
- If images don't show: check plugin configuration, container permissions, and the generated URLs.
- If uploads fail: verify account key/SAS and container name; check CORS settings if needed.

---

## 7. Optional: Backups and lifecycle rules
- Configure lifecycle management on the storage account to move old media to cool/archival tiers.
- Back up critical blobs if you need an additional restore layer.

