📖 ## Overview ##
ansible-setup/
├── ansible.cfg
├── inventory.ini
├── README.md
└── playbooks/
    └── sample-playbook.yml
This guide explains how to install, configure, and run Ansible for managing remote Linux servers.

You will learn:

How to install Ansible

How to configure inventory and SSH access

How to run ad-hoc Ansible commands

How to fix common permission issues


🛠️## Prerequisites ##
A control machine (your laptop or an EC2 instance) with:

Linux (Ubuntu, RHEL, etc.)

Python 3.x installed

Remote Linux servers (Ubuntu/RHEL/CentOS)

SSH access to remote servers (via key or password)

1. Install Ansible on Control Node
```
sudo apt update
sudo apt install ansible -y
Verify installation:
ansible --version
```
2. Setup SSH Access to Remote Servers
Ensure you can SSH into the remote server manually:
```
ssh ubuntu@<remote-server-ip>
```

If needed, copy your SSH key to remote servers:
```
ssh-copy-id ubuntu@<remote-server-ip>
```
This avoids password prompts during Ansible runs.

3. Create Ansible Inventory
Create (or edit) the file /etc/ansible/hosts or create your own custom inventory file.
```
Example Inventory:
[devgroup]
172.31.45.134 ansible_user=ubuntu
172.31.45.84 ansible_user=ubuntu
```
[devgroup] is a group name
You can reference this group in Ansible commands

4. Basic Ansible Commands
4.1 Ping Servers (Check SSH connectivity)
```
ansible devgroup -m ping
```
✅ Output should show "pong" for each host.

4.2 Run Commands on Remote Servers
Example: Create a file:
```
ansible devgroup -a "touch /tmp/demo.txt"
```
❗ For commands requiring root permissions (like apt, yum), add -b:

Example: Update packages:
```
ansible devgroup -b -a "apt update -y"
```
-u to specify the SSH user (optional if inventory has ansible_user)

-b to become root (sudo)


6. Example: Full Ad-Hoc Command Flow
Ping:
ansible devgroup -u ubuntu -m ping
Create a file:
ansible devgroup -u ubuntu -a "touch /tmp/ansible.txt"
Update system packages:
ansible devgroup -u ubuntu -b -a "apt update -y"

6️⃣ ansible.cfg Setup
To avoid repeatedly typing -i inventory.ini or -u ubuntu, create an ansible.cfg file:
[defaults]
inventory = ./inventory.ini
remote_user = ubuntu
host_key_checking = False
✅ Now you can run simpler commands like:
ansible devgroup -m ping


7️⃣ Example Playbook
You can automate multiple tasks using playbooks.

Create a file: playbooks/sample-playbook.yml
```
---
- name: Install and start nginx
  hosts: devgroup
  become: yes

  tasks:
    - name: Update apt repositories
      apt:
        update_cache: yes

    - name: Install nginx
      apt:
        name: nginx
        state: present

    - name: Start nginx service
      service:
        name: nginx
        state: started
        enabled: yes
```
Run the Playbook:
ansible-playbook playbooks/sample-playbook.yml
✅ This will install and start Nginx on all servers in devgroup.


## Useful Ansible Commands Summary ##

Task	Command
Ping all servers	ansible devgroup -m ping
Create a file	ansible devgroup -a "touch /tmp/hello.txt"
Update packages (sudo)	ansible devgroup -b -a "apt update -y"
Run a playbook	ansible-playbook playbooks/sample-playbook.yml
