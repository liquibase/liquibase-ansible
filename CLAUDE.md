# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Common Commands

### Development and Testing
```bash
# Install role dependencies
ansible-galaxy install -r meta/requirements.yml

# Run the test playbook locally
ansible-playbook --connection=local --inventory 127.0.0.1, playbook.yml

# Validate role syntax
ansible-lint .
yamllint .
```

### Role Generation
This repository uses a template-based system to generate Ansible roles for different Liquibase distributions. The generation process is handled by GitHub Actions but can be understood through the template structure.

## Architecture Overview

### Dual Distribution System
This repository maintains two distinct Liquibase distributions:

- **liquibase.liquibase**: Community edition from GitHub releases
- **liquibase.liquibase-secure**: Enterprise/secure edition from repo.liquibase.com

Both distributions are generated from the same templates but with different configurations.

### Template-Driven Role Generation
The role files are not manually maintained but generated from templates in the `templates/` directory:

- `templates/defaults/main.yml.tmpl` → `defaults/main.yml`
- `templates/vars/main.yml.tmpl` → `vars/main.yml` 
- `templates/tasks/main.yml.tmpl` → `tasks/main.yml`
- `templates/meta/main.yml.tmpl` → `meta/main.yml`
- `templates/handlers/main.yml.tmpl` → `handlers/main.yml`
- `templates/templates/liquibase.sh.j2.tmpl` → `templates/{distribution}.sh.j2`

### Conditional Template Logic
Templates use Go-style conditional blocks to handle distribution-specific content:
- `{{- if eq .secure "true" }}` for secure-only content
- `{{- if eq .secure "false" }}` for OSS-only content  
- `{{- if eq .distribution_name "liquibase" }}` for OSS distribution
- `{{- if eq .distribution_name "liquibase-secure" }}` for secure distribution

### GitHub Actions Workflows
- `.github/workflows/test.yml`: Tests the role by running the playbook locally
- `.github/workflows/deploy-role.yml`: Generates role files from templates and commits changes

### Deployment Process
The deploy workflow:
1. Downloads the specified Liquibase distribution package
2. Calculates SHA256 checksum
3. Processes all templates with distribution-specific variables
4. Validates generated files with ansible-lint and yamllint
5. Commits generated files to the repository

### Key Variables
- `liquibase_ver`: Version to install (e.g., "4.26.0", "5-secure-release-test")
- `liquibase_distribution`: Distribution type ("liquibase" or "liquibase-secure")
- `liquibase_mirror`: Base URL for downloads
- `liquibase_parent_install_dir`: Installation directory (default: `/usr/local`)
- `liquibase_checksums`: Version-to-checksum mapping

### Installation Structure
Both distributions install to:
- Installation directory: `/usr/local/liquibase-{distribution}-{version}`
- Symlink: `/usr/local/liquibase` or `/usr/local/liquibase-secure`
- Shell environment: `/etc/profile.d/{distribution}.sh`

### Dependencies
The role depends on several andrewrothstein roles:
- `andrewrothstein.unarchivedeps`: Archive extraction utilities
- `andrewrothstein.bash`: Bash shell setup
- `andrewrothstein.temurin`: OpenJDK JRE installation

### Dry Run Support
The deployment workflow supports dry run mode for testing releases before publication, with special handling for pre-release artifacts and checksum validation.