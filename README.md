# [moodle](#moodle)

Install and configure moodle on your system.

|GitHub|GitLab|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![github](https://github.com/buluma/ansible-role-moodle/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-moodle/actions)|[![gitlab](https://gitlab.com/buluma/ansible-role-moodle/badges/master/pipeline.svg)](https://gitlab.com/buluma/ansible-role-moodle)|[![quality](https://img.shields.io/ansible/quality/55981)](https://galaxy.ansible.com/buluma/moodle)|[![downloads](https://img.shields.io/ansible/role/d/55981)](https://galaxy.ansible.com/buluma/moodle)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-moodle.svg)](https://github.com/buluma/ansible-role-moodle/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/default/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: buluma.moodle
```

The machine needs to be prepared. In CI this is done using `molecule/default/prepare.yml`:
```yaml
---
- name: prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: buluma.bootstrap
    - role: robertdebock.buildtools
    - role: robertdebock.epel
    - role: buluma.mysql
      mysql_databases:
        - name: moodle
          encoding: utf8mb4
          collation: utf8mb4_unicode_ci
      mysql_users:
        - name: moodle
          password: moodle
          priv: "moodle.*:ALL"
    - role: robertdebock.python_pip
    - role: robertdebock.openssl
      openssl_items:
        - name: apache-httpd
          common_name: "{{ ansible_fqdn }}"
    - role: buluma.php
    - role: robertdebock.selinux
    - role: robertdebock.httpd
      httpd_vhosts:
        - name: moodle
          servername: moodle.example.com
    - role: buluma.cron
    - role: robertdebock.core_dependencies
```


## [Role Variables](#role-variables)

The default values for the variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for moodle

# The version of moodle to install.
moodle_version: 310

# A path where to save the data.
moodle_data_directory: /opt/moodledata

# The permissions of the created directories.
moodle_directory_mode: "0750"

# Details to connect to the database.
moodle_database_type: mysqli
moodle_database_hostname: localhost
moodle_database_name: moodle
moodle_database_username: moodle
moodle_database_password: moodle
moodle_database_prefix: ""

# The URL where to serve content.
moodle_wwwroot: "https://{{ ansible_default_ipv4.address }}/moodle"
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-moodle/blob/master/requirements.txt).

## [Status of used roles](#status-of-requirements)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[robertdebock.bootstrap](https://galaxy.ansible.com/buluma/robertdebock.bootstrap)|[![Build Status GitHub](https://github.com/buluma/robertdebock.bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/robertdebock.bootstrap/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/robertdebock.bootstrap/badges/master/pipeline.svg)](https://gitlab.com/buluma/robertdebock.bootstrap)|
|[robertdebock.buildtools](https://galaxy.ansible.com/buluma/robertdebock.buildtools)|[![Build Status GitHub](https://github.com/buluma/robertdebock.buildtools/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/robertdebock.buildtools/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/robertdebock.buildtools/badges/master/pipeline.svg)](https://gitlab.com/buluma/robertdebock.buildtools)|
|[robertdebock.cron](https://galaxy.ansible.com/buluma/robertdebock.cron)|[![Build Status GitHub](https://github.com/buluma/robertdebock.cron/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/robertdebock.cron/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/robertdebock.cron/badges/master/pipeline.svg)](https://gitlab.com/buluma/robertdebock.cron)|
|[robertdebock.core_dependencies](https://galaxy.ansible.com/buluma/robertdebock.core_dependencies)|[![Build Status GitHub](https://github.com/buluma/robertdebock.core_dependencies/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/robertdebock.core_dependencies/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/robertdebock.core_dependencies/badges/master/pipeline.svg)](https://gitlab.com/buluma/robertdebock.core_dependencies)|
|[robertdebock.epel](https://galaxy.ansible.com/buluma/robertdebock.epel)|[![Build Status GitHub](https://github.com/buluma/robertdebock.epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/robertdebock.epel/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/robertdebock.epel/badges/master/pipeline.svg)](https://gitlab.com/buluma/robertdebock.epel)|
|[robertdebock.httpd](https://galaxy.ansible.com/buluma/robertdebock.httpd)|[![Build Status GitHub](https://github.com/buluma/robertdebock.httpd/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/robertdebock.httpd/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/robertdebock.httpd/badges/master/pipeline.svg)](https://gitlab.com/buluma/robertdebock.httpd)|
|[robertdebock.mysql](https://galaxy.ansible.com/buluma/robertdebock.mysql)|[![Build Status GitHub](https://github.com/buluma/robertdebock.mysql/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/robertdebock.mysql/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/robertdebock.mysql/badges/master/pipeline.svg)](https://gitlab.com/buluma/robertdebock.mysql)|
|[robertdebock.openssl](https://galaxy.ansible.com/buluma/robertdebock.openssl)|[![Build Status GitHub](https://github.com/buluma/robertdebock.openssl/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/robertdebock.openssl/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/robertdebock.openssl/badges/master/pipeline.svg)](https://gitlab.com/buluma/robertdebock.openssl)|
|[robertdebock.php](https://galaxy.ansible.com/buluma/robertdebock.php)|[![Build Status GitHub](https://github.com/buluma/robertdebock.php/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/robertdebock.php/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/robertdebock.php/badges/master/pipeline.svg)](https://gitlab.com/buluma/robertdebock.php)|
|[robertdebock.python_pip](https://galaxy.ansible.com/buluma/robertdebock.python_pip)|[![Build Status GitHub](https://github.com/buluma/robertdebock.python_pip/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/robertdebock.python_pip/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/robertdebock.python_pip/badges/master/pipeline.svg)](https://gitlab.com/buluma/robertdebock.python_pip)|
|[robertdebock.selinux](https://galaxy.ansible.com/buluma/robertdebock.selinux)|[![Build Status GitHub](https://github.com/buluma/robertdebock.selinux/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/robertdebock.selinux/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/robertdebock.selinux/badges/master/pipeline.svg)](https://gitlab.com/buluma/robertdebock.selinux)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-moodle/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|el|8|
|debian|all|
|fedora|all|
|opensuse|all|
|ubuntu|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

## [Exceptions](#exceptions)

Some roles can't run on a specific distribution or version. Here are some exceptions.

| variation                 | reason                 |
|---------------------------|------------------------|
| alpine | mysql role does not work on alpine. |


If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-moodle/issues)

## [License](#license)

Apache-2.0

## [Author Information](#author-information)

[Michael Buluma](https://buluma.co.ke/)
