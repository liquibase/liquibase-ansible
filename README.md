# Liquibase Ansible Role

![Build Status](https://github.com/liquibase/liquibase/actions/workflows/build.yml/badge.svg)

The Liquibase Ansible role installs [Liquibase](http://www.liquibase.org).

## Requirements

See [meta/requirements.yml](meta/requirements.yml)

## Role Variables

* **liquibase_ver**: This property appears to specify the default version of Liquibase. **Default value** -> `5.0.2`
* **liquibase_mirror**: This property specifies the default mirror or repository from which Liquibase releases can be downloaded. **Default value** -> `https://package.liquibase.com/downloads/oss/ansible`
* **liquibase_parent_install_dir**: This property indicates the default parent installation directory for Liquibase. **Default value** -> `/usr/local`
* **liquibase_checksums**: Checksums for different versions of Liquibase along with their respective download URLs. The checksums are SHA-256 hashes calculated for each Liquibase release file, ensuring the integrity of the downloaded files.

## Example Playbook (installs latest liquibase release)

```yml
- hosts: server
  roles:
    - role: liquibase.liquibase
```

## Example Playbook (installs liquibase 5.0.2)

```yml
- hosts: server
  roles:
    - role: liquibase.liquibase
      liquibase_ver:
        - 5.0.2
```

## Example Playbook installation process

Let's say you would like to have `liquibase` installed in 3 AWS ec2 instances. 

* **Set up your inventory**: Create an inventory file (`inventory.ini`) listing the IP addresses or hostnames of your three EC2 instances.

```bash
[liquibase_hosts]
10.0.0.1
10.0.0.2
10.0.0.3
```

* **Install the Liquibase Ansible role**: The Liquibase Ansible role is available on Ansible Galaxy, you can install it using the ansible-galaxy command:

```bash
ansible-galaxy role install liquibase.liquibase
```

* **Write your playbook**: Create an Ansible playbook (`playbook.yml`) that uses the Liquibase role and targets the hosts specified in your inventory file:

```yml
- name: Apply Liquibase changes
  hosts: liquibase_hosts
  become: yes
  roles:
    - liquibase.liquibase
```

* **Run your playbook**: Execute the playbook against your inventory using the `ansible-playbook` command:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

This command will apply the Liquibase changes using the role to the specified EC2 instances.

* **Verify**: After running the playbook, verify that Liquibase changes were applied successfully on all three EC2 instances. You may need to log in to each instance and check the status of your database schema to ensure that the changes were applied as expected.

## Standalone role install

### Install the role

```bash
ansible-galaxy role install liquibase.liquibase
```

### Uninstall the role

```bash
ansible-galaxy role remove liquibase.liquibase
```

### Upgrade the role

The recommended path to update a role is to use the `--force` option

```bash
ansible-galaxy install --force liquibase.liquibase
```
