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

- roundcubemail_database_url - The URL to the database.
- roundcubemail_support_url - A URL to send users to for support.
- roundcubemail_des_key - A 24-character hexadecimal value to encrypt sensitive data.
- roundcubemail_spellcheck_engine - the spellchecker to use.

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
|alpine-3.6|no|yes|yes|
|alpine-3.7|no|yes|yes|
|archlinux|no|yes|yes|
|centos-6|no|no|no|
|centos-7|no|yes|yes|
|debian-buster|no|yes|yes|
|debian-jessie|no|no|no|
|debian-stretch|no|yes|yes|
|fedora-26|no|yes|yes|
|fedora-27|no|yes|yes|
|opensuse-42.2|no|yes|yes|
|opensuse-42.3|no|yes|yes|
|ubuntu-artful|no|yes|yes|
|ubuntu-bionic|no|yes|yes|
|ubuntu-xenial|no|yes|yes|

Example Playbook
----------------

```
- hosts: servers

roles:
  - role: robertdebock.bootstrap
  - role: robertdebock.php
  - role: robertdebock.mysql
  - role: robertdebock.buildtools
  - role: robertdebock.python-pip
  - role: robertdebock.httpd

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

If you don't want to manually setup roundcubemail, you have to import the database schemas. Here is a way to do that:
```
- name: configure shared items for databaseservers
  hosts: databaseservers
  become: yes
  gather_facts: no
  vars:
    roundcube_mysql_files:
      - mysql.initial.sql
      - 2008030300.sql
      - 2008040500.sql
      - 2008060900.sql
      - 2008092100.sql
      - 2009090400.sql
      - 2009103100.sql
      - 2010042300.sql
      - 2010100600.sql
      - 2011011200.sql
      - 2011092800.sql
      - 2011111600.sql
      - 2011121400.sql
      - 2012080700.sql
      - 2013011000.sql
      - 2013042700.sql
      - 2013052500.sql
      - 2013061000.sql
      - 2014042900.sql
      - 2015030800.sql

  handlers:
    - name: create database
      mysql_db:
        name: roundcube
        state: import
        target: "/tmp/roundcube_mysql_files/{{ item }}"
      with_items:
        - "{{ roundcubemail_mysql_files }}"

  tasks:
    - name: mysql
      include_role:
        name: robertdebock.mysql

    - name: copy database import files
      copy:
        src: "{{ item }}"
        dest: /tmp/roundcube_mysql_files/
      with_items:
        - "{{ roundcubemail_mysql_files }}"
      notify:
        - create database

    - name: create user
      mysql_user:
        name: roundcube
        password: roundcube
        priv: "*.*:ALL"
        host: "{{ hostvars[groups['webservers'][0]]['ansible_eth0']['ipv4']['address'] }}"

```

Install this role using `galaxy install robertdebock.roundcubemail`.

License
-------

Apache License, Version 2.0

Author Information
------------------

Robert de Bock <robert@meinit.nl>
