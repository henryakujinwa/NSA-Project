# NSA-Project
# GCP Web Server Setup and Configuration

## Introduction
This document provides detailed instructions for setting up a web server on Google Cloud Platform (GCP). The setup includes:
- Configuring a virtual network
- Provisioning virtual machines
- Installing and configuring a web server (Apache)
- Securing the VM
- Ensuring all security patches are up-to-date
- Configuring SSH access
- Setting up monitoring and logging

## Prerequisites
Before setting up the web server, ensure the following prerequisites are met:
- A Google Cloud Platform (GCP) account with access to free-tier services
- Basic understanding of GCP networking and VM configuration
- SSH key pair for secure access to VMs
- Administrative access to a Linux-based VM (Ubuntu 22.04)

## Creating a Virtual Network (VPC)
To ensure secure communication, create a Virtual Private Cloud (VPC):
1. Navigate to the Google Cloud Console.
2. Go to **VPC network** > **VPC networks**.
3. Click **Create VPC network**.
4. Provide a name for the VPC, select the subnet mode (Auto or Custom), and define subnets.
5. Configure firewall rules to allow SSH (port 22) and HTTP/HTTPS (ports 80/443).
6. Create the VPC network.

![image](https://github.com/user-attachments/assets/db55090a-1a5e-4496-9595-a66ad7c75c68)


## Creating Virtual Machines
Create two virtual machines (VMs): one for the web server and another for monitoring/logging.
1. Navigate to **Compute Engine** > **VM instances** in the Google Cloud Console.
2. Click **Create Instance**.
3. Configure the web server VM:
   - Choose Ubuntu as the operating system (e.g., `e2-micro`).
   - Set networking with the VPC network and apply correct firewall rules.
   - Set the external IP to `Ephemeral`.
   - Use SSH keys for authentication.
4. Create another VM for monitoring/logging using similar steps.

![image](https://github.com/user-attachments/assets/0bbceaae-8fe3-4c97-8117-5760e4672bf8)


## Configuring the Web Server (Apache)
1. SSH into the web server VM.
2. ![image](https://github.com/user-attachments/assets/749b1545-b0fb-4cef-bb05-6ab7561e9388)

3. Install Apache:
   ```bash
   sudo apt install apache2
   ```
   ![image](https://github.com/user-attachments/assets/a3f0f3ff-970c-4e9a-937c-9623a01631c2)

5. Start and enable Apache on boot:
   ```bash
   sudo systemctl start apache2
   sudo systemctl enable apache2
   ```
6. Upload a simple HTML page to `/var/www/html`.
7. Test by navigating to the public IP address of the VM in a browser.

![image](https://github.com/user-attachments/assets/809b83a7-33c8-4bee-9fc4-0de609982f05)


## Security Considerations
1. Disable password authentication in SSH:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
   - Set `PasswordAuthentication no`
   - ![image](https://github.com/user-attachments/assets/141dba24-93a1-45ba-8985-dccef6a7773a)

   - Restart SSH:
     ```bash
     sudo systemctl restart ssh
     ```
2. Regularly update the system:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

![image](https://github.com/user-attachments/assets/96df17d3-9c78-4fd2-8d7e-0c524b8e4c15)
![image](https://github.com/user-attachments/assets/c1c653db-0b49-4fc3-92d4-d03d1d2df662)
![image](https://github.com/user-attachments/assets/bde4c3cb-c6b2-475f-ba78-ffed7e141ca7)




## SSH Configuration
1. Generate an SSH key pair:
   ```bash
   ssh-keygen -t rsa -b 2048
   ```
2. Upload the public key to the VM:
   ```bash
   cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
   ```
3. Set correct permissions:
   ```bash
   chmod 600 ~/.ssh/authorized_keys
   ```
4. Log in using SSH keys.

![image](https://github.com/user-attachments/assets/d42f4095-e5ce-4b67-8135-fa23f3b2eca4)


## Monitoring and Logging Setup
Use a second VM to monitor the web server and store logs:
1. Install a lightweight logging service, such as `rsyslog`, on the monitoring VM:
   ```bash
   sudo apt install rsyslog
   ```
   ![image](https://github.com/user-attachments/assets/bba349e6-2b47-48b3-8260-7614816b7191)

2. Configure the web server to send logs to the monitoring VM.


## Testing
Ensure all components are functioning correctly:
- Verify Apache is running and accessible via the public IP.
- Confirm firewall rules allow necessary traffic.
- Test SSH access with keys.
- Check logging service on the monitoring VM.

![image](https://github.com/user-attachments/assets/0455034b-7905-4275-9908-3c9777bed174)


---
This guide provides a structured approach to setting up and securing a web server on GCP with basic monitoring capabilities.
