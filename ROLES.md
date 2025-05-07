Ansible-Configuration/
├── roles/
│   └── myrole/
│       ├── tasks/
│       │   └── main.yml
│       ├── vars/
│       │   └── main.yml
│       └── ...
└── site.yml
Let's set up a working example of an Ansible role to:

✅ Update APT
✅ Install Apache
✅ Create a user (with variable)

🗂 1. Create the Role
```
ansible-galaxy init myrole
```
🛠 2. Edit myrole/tasks/main.yml
```
---
- name: Update APT cache
  apt:
    update_cache: yes

- name: Install Apache
  apt:
    name: apache2
    state: present

- name: Create a user
  ansible.builtin.user:
    name: "{{ username }}"
```
📦 3. Set the Variable
Edit myrole/vars/main.yml:
```
---
username: devopsuser
```

▶️ 4. Write the Playbook site.yml
Create a playbook in your project root (same level as the roles/ directory):
```
---
- name: Apply myrole to web servers
  hosts: testgroup
  become: yes
  roles:
    - myrole
```
▶️ 6. Run the Playbook
```
ansible-playbook site.yml -i inventory
```
