# Liquibase Ansible Roles

![Build Status](https://github.com/liquibase/liquibase/actions/workflows/build.yml/badge.svg)

This repository provides Ansible roles for installing different distributions of [Liquibase](http://www.liquibase.org):

- **liquibase.liquibase**: Liquibase distribution from GitHub releases
- **liquibase.liquibase-secure**: Secure Liquibase distribution from repo.liquibase.com

## Available Distributions

### Liquibase (`liquibase.liquibase`)

- Downloads from GitHub releases
- Community edition
- Installs to `/usr/local/liquibase`

### Liquibase Secure (`liquibase.liquibase-secure`)

- Downloads from repo.liquibase.com
- Enhanced security features and enterprise capabilities
- Installs to `/usr/local/liquibase-secure`

## Requirements

See [meta/requirements.yml](meta/requirements.yml)

## Role Variables

- **liquibase_ver**: This property appears to specify the default version of Liquibase. **Default value** -> `4.26.0`
- **liquibase_mirror**: This property specifies the default mirror or repository from which Liquibase releases can be downloaded. **Default value** -> `https://github.com/liquibase/liquibase/releases/download`
- **liquibase_parent_install_dir**: This property indicates the default parent installation directory for Liquibase. **Default value** -> `/usr/local`
- **liquibase_checksums**: Checksums for different versions of Liquibase along with their respective download URLs. The checksums are SHA-256 hashes calculated for each Liquibase release file, ensuring the integrity of the downloaded files.

## Example Playbooks

### Liquibase Distribution

Install latest version:

```yml
- hosts: server
  roles:
    - role: liquibase.liquibase
```

Install specific version:

```yml
- hosts: server
  roles:
    - role: liquibase.liquibase
      vars:
        liquibase_ver: "4.33.0"
```

### Liquibase Secure Distribution

Install latest secure version:

```yml
- hosts: server
  roles:
    - role: liquibase.liquibase-secure
```

Install specific secure version:

```yml
- hosts: server
  roles:
    - role: liquibase.liquibase-secure
      vars:
        liquibase_ver: "DAT-20703"
```

### Installing Both Distributions

You can install both distributions on the same system:

```yml
- hosts: server
  roles:
    - role: liquibase.liquibase
      vars:
        liquibase_ver: "4.33.0"
    - role: liquibase.liquibase-secure
      vars:
        liquibase_ver: "DAT-20703"
```

## Example Playbook installation process

Let's say you would like to have `liquibase` installed in 3 AWS ec2 instances.

- **Set up your inventory**: Create an inventory file (`inventory.ini`) listing the IP addresses or hostnames of your three EC2 instances.

```bash
[liquibase_hosts]
10.0.0.1
10.0.0.2
10.0.0.3
```

- **Install the Liquibase Ansible role**: Both Liquibase roles are available on Ansible Galaxy. Install the one you need:

For Liquibase:

```bash
ansible-galaxy role install liquibase.liquibase
```

For Liquibase Secure:

```bash
ansible-galaxy role install liquibase.liquibase-secure
```

Or install both:

```bash
ansible-galaxy role install liquibase.liquibase liquibase.liquibase-secure
```

- **Write your playbook**: Create an Ansible playbook (`playbook.yml`) that uses the Liquibase role and targets the hosts specified in your inventory file:

```yml
- name: Apply Liquibase changes
  hosts: liquibase_hosts
  become: yes
  roles:
    - liquibase.liquibase
```

- **Run your playbook**: Execute the playbook against your inventory using the `ansible-playbook` command:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

This command will apply the Liquibase changes using the role to the specified EC2 instances.

- **Verify**: After running the playbook, verify that Liquibase changes were applied successfully on all three EC2 instances. You may need to log in to each instance and check the status of your database schema to ensure that the changes were applied as expected.

## Standalone Role Installation

### Install Roles

Liquibase:

```bash
ansible-galaxy role install liquibase.liquibase
```

Liquibase Secure:

```bash
ansible-galaxy role install liquibase.liquibase-secure
```

### Uninstall Roles

Liquibase:

```bash
ansible-galaxy role remove liquibase.liquibase
```

Liquibase Secure:

```bash
ansible-galaxy role remove liquibase.liquibase-secure
```

### Upgrade Roles

The recommended path to update roles is to use the `--force` option:

Liquibase:

```bash
ansible-galaxy install --force liquibase.liquibase
```

Liquibase Secure:

```bash
ansible-galaxy install --force liquibase.liquibase-secure
```

## Distribution Differences

| Feature           | OSS (`liquibase.liquibase`) | Secure (`liquibase.liquibase-secure`)     |
| ----------------- | --------------------------- | ----------------------------------------- |
| Source            | GitHub releases             | repo.liquibase.com                        |
| Installation Path | `/usr/local/liquibase`      | `/usr/local/liquibase`                    |
| Command Access    | `liquibase`                 | `liquibase` (via `/usr/local/liquibase`)  |
| Version Format    | Semantic (e.g., `4.33.0`)   | Build ID (e.g., `4.33.0`)                 |
| Features          | Community edition           | Enhanced security and enterprise features |

Both distributions can coexist on the same system without conflicts.
