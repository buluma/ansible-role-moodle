# [Ansible role moodle](#ansible-role-moodle)

Install and configure moodle on your system.

|GitHub|GitLab|Downloads|Version|
|------|------|---------|-------|
|[![github](https://github.com/buluma/ansible-role-moodle/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-moodle/actions)|[![gitlab](https://gitlab.com/shadowwalker/ansible-role-moodle/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-moodle)|[![downloads](https://img.shields.io/ansible/role/d/buluma/moodle)](https://galaxy.ansible.com/buluma/moodle)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-moodle.svg)](https://github.com/buluma/ansible-role-moodle/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-moodle/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
- become: true
  gather_facts: true
  hosts: all
  name: converge
  pre_tasks:
  - apt: update_cache=yes cache_valid_time=600
    changed_when: false
    name: Update apt cache.
    when: ansible_os_family == 'Debian'
  - ansible.builtin.stat:
      path: /usr/lib/python3.11/EXTERNALLY-MANAGED
    name: Check if python3.11 EXTERNALLY-MANAGED file exists
    register: externally_managed_file_py311
  - ansible.builtin.command:
      cmd: mv /usr/lib/python3.11/EXTERNALLY-MANAGED /usr/lib/python3.11/EXTERNALLY-MANAGED.old
    args:
      creates: /usr/lib/python3.11/EXTERNALLY-MANAGED.old
    name: Rename python3.11 EXTERNALLY-MANAGED file if it exists
    when: externally_managed_file_py311.stat.exists
  - ansible.builtin.stat:
      path: /usr/lib/python3.12/EXTERNALLY-MANAGED
    name: Check if python3.12 EXTERNALLY-MANAGED file exists
    register: externally_managed_file_py312
  - ansible.builtin.command:
      cmd: mv /usr/lib/python3.12/EXTERNALLY-MANAGED /usr/lib/python3.12/EXTERNALLY-MANAGED.old
    args:
      creates: /usr/lib/python3.12/EXTERNALLY-MANAGED.old
    name: Rename python3.12 EXTERNALLY-MANAGED file if it exists
    when: externally_managed_file_py312.stat.exists
  roles:
  - role: buluma.moodle
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-moodle/blob/master/molecule/default/prepare.yml):

```yaml
- become: true
  gather_facts: false
  hosts: all
  name: prepare
  roles:
  - role: buluma.bootstrap
  - role: buluma.buildtools
  - role: buluma.epel
  - mysql_databases:
    - collation: utf8mb4_unicode_ci
      encoding: utf8mb4
      name: moodle
    mysql_users:
    - name: moodle
      password: moodle
      priv: moodle.*:ALL
    role: buluma.mysql
  - role: buluma.python_pip
  - openssl_items:
    - common_name: '{{ ansible_fqdn }}'
      name: apache-httpd
    role: buluma.openssl
  - role: buluma.php
  - role: buluma.selinux
  - httpd_vhosts:
    - name: moodle
      servername: moodle.example.com
    role: buluma.httpd
  - role: buluma.cron
  - role: buluma.core_dependencies
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-moodle/blob/master/defaults/main.yml):

```yaml
moodle_data_directory: /opt/moodledata
moodle_database_hostname: localhost
moodle_database_name: moodle
moodle_database_password: moodle
moodle_database_prefix: ''
moodle_database_type: mysqli
moodle_database_username: moodle
moodle_directory_mode: '0750'
moodle_version: 401
moodle_wwwroot: https://{{ ansible_default_ipv4.address }}/moodle
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-moodle/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.buildtools](https://galaxy.ansible.com/buluma/buildtools)|[![Build Status GitHub](https://github.com/buluma/ansible-role-buildtools/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-buildtools/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-buildtools/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-buildtools)|
|[buluma.cron](https://galaxy.ansible.com/buluma/cron)|[![Build Status GitHub](https://github.com/buluma/ansible-role-cron/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-cron/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-cron/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-cron)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Build Status GitHub](https://github.com/buluma/ansible-role-core_dependencies/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-core_dependencies/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-core_dependencies)|
|[buluma.epel](https://galaxy.ansible.com/buluma/epel)|[![Build Status GitHub](https://github.com/buluma/ansible-role-epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-epel/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-epel/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-epel)|
|[buluma.httpd](https://galaxy.ansible.com/buluma/httpd)|[![Build Status GitHub](https://github.com/buluma/ansible-role-httpd/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-httpd/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-httpd/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-httpd)|
|[buluma.mysql](https://galaxy.ansible.com/buluma/mysql)|[![Build Status GitHub](https://github.com/buluma/ansible-role-mysql/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-mysql/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-mysql/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-mysql)|
|[buluma.openssl](https://galaxy.ansible.com/buluma/openssl)|[![Build Status GitHub](https://github.com/buluma/ansible-role-openssl/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-openssl/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-openssl/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-openssl)|
|[buluma.php](https://galaxy.ansible.com/buluma/php)|[![Build Status GitHub](https://github.com/buluma/ansible-role-php/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-php/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-php/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-php)|
|[buluma.python_pip](https://galaxy.ansible.com/buluma/python_pip)|[![Build Status GitHub](https://github.com/buluma/ansible-role-python_pip/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-python_pip/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-python_pip/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-python_pip)|
|[buluma.selinux](https://galaxy.ansible.com/buluma/selinux)|[![Build Status GitHub](https://github.com/buluma/ansible-role-selinux/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-selinux/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-selinux/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-selinux)|

## [Context](#context)

This role is part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-moodle/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/buluma/enterpriselinux)|all|
|[Debian](https://hub.docker.com/r/buluma/debian)|all|
|[Fedora](https://hub.docker.com/r/buluma/fedora)|all|
|[opensuse](https://hub.docker.com/r/buluma/opensuse)|all|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|all|

The minimum version of Ansible required is 2.12, tests have been done on:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them on [GitHub](https://github.com/buluma/ansible-role-moodle/issues).

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-moodle/blob/master/LICENSE).

## [Author Information](#author-information)

[buluma](https://buluma.github.io/)

