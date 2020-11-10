# [roundcubemail](#roundcubemail)

Install and configure roundcubemail on your system.

|Travis|GitHub|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-roundcubemail.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-roundcubemail)|[![github](https://github.com/robertdebock/ansible-role-roundcubemail/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-roundcubemail/actions)|[![quality](https://img.shields.io/ansible/quality/24815)](https://galaxy.ansible.com/robertdebock/roundcubemail)|[![downloads](https://img.shields.io/ansible/role/d/24815)](https://galaxy.ansible.com/robertdebock/roundcubemail)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-roundcubemail.svg)](https://github.com/robertdebock/ansible-role-roundcubemail/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.httpd
    - role: robertdebock.roundcubemail
```

The machine needs to be prepared in CI this is done using `molecule/resources/prepare.yml`:
```yaml
---
- name: Prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
    - role: robertdebock.buildtools
    - role: robertdebock.python_pip
    - role: robertdebock.openssl
      openssl_items:
        - name: apache-httpd
          common_name: "{{ ansible_fqdn }}"
    - role: robertdebock.httpd
    - role: robertdebock.php
      php_upload_max_filesize: 5M
      php_post_max_size: 6M
      php_date_timezone: Europe/Amsterdam
      php_extension:
        - mcrypt.so
    - role: robertdebock.mysql
      mysql_databases:
        - name: roundcube
      mysql_users:
        - name: roundcube
          password: roundcube
          priv: "roundcube.*:ALL"
    - role: robertdebock.selinux
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for roundcubemail

roundcubemail_database_host: localhost
roundcubemail_database_user: roundcube
roundcubemail_database_password: roundcube
roundcubemail_database_name: roundcube

# A URL to get support.
roundcubemail_support_url: "{{ ansible_fqdn }}/support"

# A key to encrypt sensitive data.
roundcubemail_des_key: 964af56991531a805bd55085

# The spellchecker to use. Either: 'google', 'pspell', 'enchant' or 'atd'.
roundcubemail_spellcheck_engine: pspell
```

## [Requirements](#requirements)

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

## [Status of requirements](#status-of-requirements)

| Requirement | Travis | GitHub |
|-------------|--------|--------|
| [robertdebock.bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-bootstrap.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-bootstrap) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-bootstrap/actions) |
| [robertdebock.buildtools](https://galaxy.ansible.com/robertdebock/buildtools) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-buildtools.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-buildtools) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-buildtools/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-buildtools/actions) |
| [robertdebock.epel](https://galaxy.ansible.com/robertdebock/epel) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-epel.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-epel) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-epel/actions) |
| [robertdebock.httpd](https://galaxy.ansible.com/robertdebock/httpd) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-httpd.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-httpd) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-httpd/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-httpd/actions) |
| [robertdebock.mysql](https://galaxy.ansible.com/robertdebock/mysql) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-mysql.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-mysql) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-mysql/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-mysql/actions) |
| [robertdebock.openssl](https://galaxy.ansible.com/robertdebock/openssl) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-openssl.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-openssl) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-openssl/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-openssl/actions) |
| [robertdebock.php](https://galaxy.ansible.com/robertdebock/php) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-php.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-php) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-php/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-php/actions) |
| [robertdebock.python_pip](https://galaxy.ansible.com/robertdebock/python_pip) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-python_pip.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-python_pip) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-python_pip/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-python_pip/actions) |
| [robertdebock.reboot](https://galaxy.ansible.com/robertdebock/reboot) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-reboot.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-reboot) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-reboot/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-reboot/actions) |
| [robertdebock.selinux](https://galaxy.ansible.com/robertdebock/selinux) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-selinux.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-selinux) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-selinux/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-selinux/actions) |

## [Dependencies](#dependencies)

Most roles require some kind of preparation, this is done in `molecule/default/prepare.yml`. This role has a "hard" dependency on the following roles:

- robertdebock.httpd
## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/roundcubemail.png "Dependency")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|el|7|
|fedora|all|
|opensuse|all|

The minimum version of Ansible required is 2.9, tests have been done to:

- The previous version.
- The current version.
- The development version.

## [Exceptions](#exceptions)

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| centos:8 | no package roundcubemail. |


## [Testing](#testing)

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-roundcubemail) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-roundcubemail/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## [License](#license)

Apache-2.0

## [Contributors](#contributors)

I'd like to thank everybody that made contributions to this repository. It motivates me, improves the code and is just fun to collaborate.

- [TravisWhitehead](https://github.com/TravisWhitehead)

## [Author Information](#author-information)

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
