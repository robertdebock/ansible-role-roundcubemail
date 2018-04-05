roundcubemail
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-roundcubemail.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-roundcubemail)

Provides roundcubemail for your system.

Context
--------
This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/robertdebock.github.io/artifacts/roundcubemail.png "Dependency")

Requirements
------------

Access to a repository containing packages, likely on the internet.

Role Variables
--------------

None known

Dependencies
------------

You can use this role to prepare your system:

- robertdebock.bootstrap

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.3|ansible 2.4|ansible 2.5|
|------------|-----------|-----------|-----------|
|alpine-3.6|yes|yes|yes|
|alpine-3.7|yes|yes|yes|
|archlinux|yes|yes|yes|
|centos-6|yes|yes|yes|
|centos-7|yes|yes|yes|
|debian-buster|yes|yes|yes|
|debian-jessie|yes|yes|yes|
|debian-stretch|yes|yes|yes|
|debian-wheezy|yes|yes|yes|
|fedora-26|yes|yes|yes|
|fedora-27|yes|yes|yes|
|opensuse-42.2|yes|yes|yes|
|opensuse-42.3|yes|yes|yes|
|ubuntu-artful|yes|yes|yes|
|ubuntu-trusty|yes|yes|yes|
|ubuntu-xenial|yes|yes|yes|

Example Playbook
----------------

```
- hosts: servers

  roles:
    - robertdebock.bootstrap
    - robertdebock.mysql

  tasks:
  - name: create database
    mysql_db:
      name: bobdata

  - name: create user
    mysql_user:
      name: bob
      password: 12345
      priv: '*.*:ALL'

  - name: include roundcubemail role
    include_role:    
      name: robertdebock.roundcubemail
```

Install this role using `galaxy install robertdebock.roundcubemail`.

License
-------

Apache License, Version 2.0

Author Information
------------------

Robert de Bock <robert@meinit.nl>
