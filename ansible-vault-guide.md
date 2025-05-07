# 🔐 Ansible Vault Guide

Ansible Vault is a feature used to **encrypt sensitive data** such as passwords, API keys, and other secrets within playbooks or variable files.

---

## 📘 1. Create an Encrypted Playbook

```bash
ansible-vault create playbook.yml
```
Prompts for a password.

Opens the file in an editor.

The file is saved in an encrypted format.

Example Content:
```bash
---
- hosts: all
  become: yes
  tasks:
    - name: demo
      debug:
        msg: "This is a protected playbook"
```

🔍 2. View Encrypted File
```
cat playbook.yml
```
Output will look like
$ANSIBLE_VAULT;1.1;AES256
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

✏️ 3. Edit an Encrypted Playbook
```
ansible-vault edit playbook.yml
```
Prompts for password.

Opens the file for editing.

🔑 4. Change Vault Password (Rekey)
```
ansible-vault rekey playbook.yml
```
Prompts for old and new passwords.

🔓 5. Decrypt the File (Remove Encryption)
```
ansible-vault decrypt playbook.yml
```
File becomes plain-text again.


🔐 6. Encrypt an Existing File
```
ansible-vault encrypt anyplaybook.yml
```

▶️ 7. Run an Encrypted Playbook
```
ansible-playbook playbook.yml --ask-vault-pass
```
Prompts for vault password before execution.



💡 Use Ansible Vault when storing secrets like API tokens, SSH keys, or database passwords to ensure secure automation.

