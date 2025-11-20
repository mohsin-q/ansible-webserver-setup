# Ansible Web Server Setup

This project demonstrates how to use **Ansible** to automate the installation and configuration of Apache web servers on **CentOS 7 VMs**, including deploying a **custom HTML page**.  

It can be scaled to multiple VMs and includes firewall configuration and repo fixes for CentOS 7.

Tested on **Windows** using **PowerShell** for Vagrant and **WSL (Windows Subsystem for Linux)** for Ansible.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Step 1 : Start VMS](#step-1-start-vms)
- [Step 2: SSH Host Key Scanning](#step-2-ssh-host-key-scanning)
- [Step 3: Ansible Inventory](#step-3-ansible-inventory)
- [Step 4: Run the Playbook](#step-4-run-the-playbook)
- [Step 5: Verify the Deployment](#step-5-verify-the-deployment)
- [Scaling to Multiple VMs](#scaling-to-multiple-vms)

---

## Prerequisites

- **Vagrant** and **VirtualBox** (or another provider) to run your CentOS VMs  
- **Ansible** installed on your host machine (WSL or Linux recommended)  

---

## Getting Started

Clone this repository to your local machine using **PowerShell** :

```powershell
[git clone https://github.com/your-username/ansible_webserver_setup.git](https://github.com/mohsin-q/ansible-webserver-setup.git)
cd ansible_webserver_setup
```
This will give you the full project structure:

```
ansible_webserver_setup/
├─ ansible_configurations/
│  ├─ files/
│  │  └─ index.html        # Custom HTML page
│  ├─ hosts.ini            # Ansible inventory with VM IPs
│  └─ webserver.yml        # Ansible playbook
├─ vagrant_configurations/
│  └─ Vagrantfile      
```

Follow the following steps after cloning 

### Step 1: Start VMs

In PowerShell (Windows):
```
cd vagrant_configurations
vagrant up
```

Check VM status:
```
vagrant status
```
### Step 2: SSH Host Key Scanning

To prevent Ansible from prompting for SSH confirmation for each VM, scan and add host keys from in WSL:

```
ssh-keyscan 192.168.56.10 >> ~/.ssh/known_hosts
ssh-keyscan 192.168.56.11 >> ~/.ssh/known_hosts
```

This is specially useful when managing many VMs.

### Step 3: Ansible Inventory (hosts.ini)

List all VM IPs under a group, for example:

```
[webservers]
192.168.56.10
192.168.56.11
```

### Step 4: Run the Playbook
In WSL, navigate to the Ansible configuration folder and run:
```
cd ansible_configurations
ansible-playbook -i hosts.ini webserver.yml
```

All tasks should complete successfully.

It will do the follwoing things:

Fix CentOS 7 repos for older images

Install Apache (httpd)

Open firewall ports for HTTP and HTTPS

Deploy custom HTML page from files/index.html

Reload Apache and firewall as needed

### Step 5: Verify the Deployment

You can verify that the web server and custom page are working in two ways:

Using curl:
```
curl http://192.168.56.10
curl http://192.168.56.11
```

Using a web browser:

Open your browser and navigate to:
```
http://192.168.56.10
http://192.168.56.11
```

You should see custom HTML page from files/index.html.

## Scaling to Multiple VMs

Add more VM definitions in Vagrantfile

Add their IPs to hosts.ini

Ansible will automatically apply the configuration to all VMs under [webservers]

The SSH host key scan ensures no prompts for each new VM.

## Notes

Run Vagrant commands in PowerShell (or Command Prompt)

Run Ansible commands in WSL

Ensure VMs are up and reachable before running Ansible

The playbook uses become: yes for tasks requiring root privileges.

Firewall configuration is done via firewalld to allow HTTP/HTTPS.


