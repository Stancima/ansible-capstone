# Ansible Capstone (AAP 2.4) — Chef Client + Web Page Demo

This repo is designed for an **Ansible Automation Platform (AAP) 2.4** demo. It contains:

- A role + playbook to **install Chef Client** on:
  - Linux nodes (SSH)
  - Windows nodes (WinRM over HTTPS / 5986)
- A role + playbook to **deploy a simple HTML homepage** on Linux nodes
- Pre-flight connectivity playbooks for:
  - Linux (`ping`)
  - Windows (`win_ping`)

## Repo Structure

collections/
requirements.yml
inventories/
hosts.yml
playbooks/
install_chef.yml
deploy_web.yml
test_linux_ssh.yml
test_windows_winrm.yml
roles/


## Inventory Groups

- `linux_nodes` — Linux instances reachable over SSH
- `windows_nodes` — Windows instances reachable over WinRM (HTTPS 5986)

> **No secrets are stored in Git.**  
> SSH keys and Windows passwords are managed with **AAP Credentials**.

## Playbooks

### 1) Test Linux connectivity
- **File:** `playbooks/test_linux_ssh.yml`
- **Expected:** `pong`

### 2) Test Windows connectivity
- **File:** `playbooks/test_windows_winrm.yml`
- **Expected:** `pong`

### 3) Install Chef Client (all nodes)
- **File:** `playbooks/install_chef.yml`
- Installs Chef on both Linux + Windows using OS-specific tasks in the role.

### 4) Deploy Web Page (Linux nodes only)
- **File:** `playbooks/deploy_web.yml`
- Installs NGINX and deploys a homepage displaying:

`welcome to my ansible capstone --Stanley Ibe`

## AAP 2.4 Setup (high level)

1. Create a **Project** pointing to this GitHub repo and Sync it.
2. Create an **Inventory** (or use the YAML inventory in `inventories/hosts.yml`).
3. Add Credentials:
   - Linux: Machine credential with SSH private key
   - Windows: Machine credential with decrypted Administrator password
4. Create Job Templates for each playbook.
5. (Recommended) Create a Workflow:
   - Test Linux
   - Test Windows
   - Install Chef
   - Deploy Web Page (Linux)

## WinRM Notes (Windows)
- Ensure **TCP 5986 inbound** is allowed to the Windows instance from the AAP controller.
- Inventory uses:
  - `ansible_winrm_transport: ntlm`
  - `ansible_winrm_server_cert_validation: ignore`

## Demo Flow
1. Launch **Test Linux SSH**
2. Launch **Test Windows WinRM**
3. Launch **Install Chef Client**
4. Launch **Deploy Web Page**