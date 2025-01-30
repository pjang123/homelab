# Minecraft Server Deployment

## Project Overview
This project demonstrates how to:
1. Partition a drive to support dual-boot functionality with Windows 10 Pro and Ubuntu.
2. Deploy a modded Minecraft servers using Pterodactyl and Docker.
3. Automate daily backups of server directories using a Bash script.

---

## Table of Contents
1. [Dual-Boot Setup](#dual-boot-setup)
2. [Pterodactyl Installation](#pterodactyl-installation)
3. [Minecraft Server Deployment](#minecraft-server-deployment)
4. [Custom Domain Name](#custom-domain-name)
5. [Backup Automation](#backup-automation)
6. [Tools & Technologies](#tools--technologies)
7. [Conclusion](#conclusion)

---

## Dual-Boot Setup
### Objective
Enable dual-boot functionality on a Windows 10 Pro machine.

### Steps
1. **Create a Bootable USB**:
   - Download Ubuntu 24.04 from the [official website](https://ubuntu.com/download).
   - Use [Rufus](https://rufus.ie/) to create a bootable USB drive.
2. **Partition the Drive**:
   - Disable BitLocker encryption on the Windows drive.
   - Shrink the Windows partition to free up space for Ubuntu.
   - During Ubuntu installation, select the free space and allocate it for Ubuntu (e.g., 50 GB for root, 8 GB for swap).
3. **Install Ubuntu**:
   - Follow the installation prompts and install Ubuntu alongside Windows.
   - Install the GRUB bootloader to manage dual-boot functionality.

---

## Pterodactyl Installation
Follow the installation guide on the [Official Pterodactyl website](https://pterodactyl.io/wings/1.0/installing.html).

### Summary of Steps
1. Install dependencies (Docker, MySQL, PHP, etc.).
2. Set up the Pterodactyl panel and configure the database.
3. Install Pterodactyl Wings (the server management daemon).

---

## Minecraft Server Deployment
### Objective
Deploy Minecraft servers using Pterodactyl and Docker while optimizing resource allocation and ensuring high availability.

### Steps
1. **Initialization**:
   - Configure locations, databases, nodes, and servers in the Pterodactyl panel.
2. **Deploy Minecraft Servers**:
   - Use SFTP to transfer the necessary server files to the configured servers.
   - Install mods and configure server settings.
3. **Resource Allocation Optimization**:
   - Allocate CPU and memory limits in Docker to prevent resource exhaustion.
   - Monitor server performance using Pterodactyl's built-in tools.

---

## Custom Domain Name
### Objective
Create a domain name for users to connect to the Minecraft servers instead of using the router's public IP.

### Steps
1. **Find a Domain Registrar**:
   - Choose a registrar (e.g., Namecheap, Google Domains) and purchase a domain name.
2. **Configure DNS Records**:
   - Create an **A record** to point the domain to your router's public IP.
   - Optionally, create an **SRV record** to specify the Minecraft server port (default: 25565).
3. **Port Forwarding**:
   - Configure your router to forward traffic from the Minecraft port (25565) to the server's local IP.

---

## Backup Automation
### Objective
Automate daily backups of server directories, ensuring data integrity and reducing manual intervention.

### Tools Used
- **Bash**: To create the backup script.
- **Cron**: To schedule the script to run daily at 12 PM.
- **msmtp**: To send email notifications for backup success or failure.

### Backup Script
```bash
#!/bin/bash

# Define variables
TIMESTAMP=$(date +"%Y%m%d_%H%M%S") # Timestamp (format: YYYYMMDD_HHMMSS)
SOURCE_FILE="/var/lib/pterodactyl/volumes/filenamef" # Path to file to be backed up
BACKUP_DIR="/var/lib/pterodactyl/backups" # Where backup files will be stored
BACKUP_FILE="$BACKUP_DIR/filename_$TIMESTAMP.bak" # Backup file name
EMAIL="email@example.com" # Recipient email

# Check if the backups directory exists
if [ ! -d "$BACKUP_DIR" ]; then
    echo "Backup directory does not exist. Creating it now..."
    mkdir -p "$BACKUP_DIR"
fi

# Remove any existing backup file
if [ -f "$BACKUP_FILE" ]; then
    echo "Existing backup found. Removing it..."
    rm "$BACKUP_FILE"
fi

# Copy the file to the backups directory as a new backup
if cp -r "$SOURCE_FILE" "$BACKUP_FILE"; then
    echo "Backup of $SOURCE_FILE created at $BACKUP_FILE"
    # Send success email
    echo -e "Subject: Backup Successful\n\nThe backup of $SOURCE_FILE was successfully created at $BACKUP_FILE." | msmtp "$EMAIL"
else
    echo "Failed to backup $SOURCE_FILE"
    # Send failure email
    echo -e "Subject: Backup Failed\n\nThe backup of $SOURCE_FILE to $BACKUP_FILE failed." | msmtp "$EMAIL"
    exit 1
fi
```

---

## Tools & Technologies
- Operating Systems: Windows 10 Pro, Ubuntu 24.04
- Virtualization: Docker
- Server Management: Pterodactyl
- Automation: Bash, Cron
- Monitoring: msmtp (email notifications)

---

## Conclusion
This project successfully demonstrates:

- Setting up a dual-boot system with Windows 10 Pro and Ubuntu.

- Deploying and managing modded Minecraft servers using Pterodactyl and Docker.

- Automating daily backups with a Bash script and monitoring via email notifications.

---

Made by: Paul Jang
