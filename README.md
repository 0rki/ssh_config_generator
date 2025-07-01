# SSH Config Generator Ansible Role

Automatically generates SSH client configuration files with support for bastion hosts, custom options, and multi-environment setups.

I used it mostly for infrastructure pipelines and deploys in Gitlab.

## Features

- Generate SSH config file
- ProxyJump support for bastion host configurations
- Custom SSH options per host

## Requirements

- Ansible 2.9+
- Python 3.6+
- OpenSSH 7.3+ for multi-hop ProxyJump support

## Role Variables

### Core Configuration
| Variable | Default | Description |
|----------|---------|-------------|
| `ssh_config_file` | `~/.ssh/config1` | Output file path |
| `ssh_config_mode` | `0600` | File permissions |
| `ssh_config_owner` | `{{ ansible_user }}` | File owner |
| `ssh_config_group` | `{{ ansible_user }}` | File group |

### Host Configuration (`ssh_hosts` list)
| Parameter | Required | Example | Description |
|-----------|----------|---------|-------------|
| `name` | Yes | `web-server` | Host alias |
| `hostname` | Yes | `app.example.com` | Server hostname/IP |
| `user` | No | `deploy` | SSH username |
| `port` | No | `2222` | SSH port |
| `identity_file` | No | `~/.ssh/prod_key` | Private key path |
| `proxy_jump` | No | `bastion-host` | ProxyJump host reference |
| `proxy_command` | No | `ssh -W %h:%p jump` | Custom proxy command |
| `options` | No | `['StrictHostKeyChecking no']` | List of SSH options |

> **Version Note**: Multi-hop bastion support (comma-separated hosts in `proxy_jump`) requires **OpenSSH 7.3 or later**.

## Usage
Install the role.

Customize `example/playbook.yml` and then run it
```bash
ansible-playbook -i inventory.yml playbook.yml
```
By default it saves config as "~/.ssh/config1" so it won't rewrite your current config.
