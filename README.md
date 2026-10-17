# Ansible Configuration Management

Ansible project that automates the configuration of an Ubuntu server using reusable roles. This project is based on the [roadmap.sh Configuration Management challenge](https://roadmap.sh/projects/configuration-management).

## Overview

The playbook configures a Linux server by installing essential utilities, setting up Fail2ban and Nginx, deploying a static website, and managing SSH public keys.

## Tech Stack

- Ansible
- Ubuntu 24.04 LTS
- Nginx
- Fail2ban
- SSH
- YAML

## Project Structure

```text
ansible-server-setup/
├── inventory.ini
├── setup.yml
├── files/
│   └── website.tar.gz
├── group_vars/
│   └── webservers.yml
└── roles/
    ├── base/
    │   └── tasks/main.yml
    ├── nginx/
    │   ├── tasks/main.yml
    │   ├── handlers/main.yml
    │   └── templates/default.j2
    ├── app/
    │   └── tasks/main.yml
    └── ssh/
        └── tasks/main.yml
```

## Roles

- **base** — updates packages, installs basic utilities, and enables Fail2ban.
- **nginx** — installs Nginx and configures the default website.
- **app** — uploads and extracts a static website archive.
- **ssh** — ensures the configured public key is present in the root account's `authorized_keys`.

## Prerequisites

- Ansible installed on the control machine.
- An Ubuntu server accessible through SSH.
- An SSH key configured for server access.
- The `ansible.posix` collection.

Install the required collection:

```bash
ansible-galaxy collection install ansible.posix
```

## Inventory

The `inventory.ini` file defines the target server:

```ini
[webservers]
roadmap-server

[webservers:vars]
ansible_python_interpreter=/usr/bin/python3
```

The `roadmap-server` alias is configured in the local SSH config.

## Usage

Check the playbook syntax:

```bash
ansible-playbook -i inventory.ini setup.yml --syntax-check
```

Run all roles:

```bash
ansible-playbook -i inventory.ini setup.yml
```

Run a specific role:

```bash
ansible-playbook -i inventory.ini setup.yml --tags base
ansible-playbook -i inventory.ini setup.yml --tags nginx
ansible-playbook -i inventory.ini setup.yml --tags app
ansible-playbook -i inventory.ini setup.yml --tags ssh
```

List available tags:

```bash
ansible-playbook -i inventory.ini setup.yml --list-tags
```

Test connectivity:

```bash
ansible -i inventory.ini webservers -m ping
```

## Verification

The deployment was verified by checking:

- Ansible playbook execution with no failed or unreachable hosts.
- Nginx and Fail2ban service status.
- Static website response using `curl`.
- Individual role execution using Ansible tags.

## Learning Outcomes

- Understanding Ansible inventories and playbooks.
- Organizing automation into reusable roles.
- Managing packages and services declaratively.
- Deploying a static website to a remote server.
- Managing SSH public keys with Ansible.
- Using tags to execute selected parts of a playbook.

## Project Reference

[roadmap.sh — Configuration Management](https://roadmap.sh/projects/configuration-management)
