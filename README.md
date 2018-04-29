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
      name: roundcube

  - name: create user
    mysql_user:
      name: roundcube
      password: roundcube
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

  handlers:
    - name: create database
      mysql_db:
        name: roundcube
        state: import
        target: /tmp/roundcube_mysql_files/mysql.initial.sql

  tasks:
    - name: mysql
      include_role:
        name: robertdebock.mysql

    - name: copy database import files
      copy:
        src: mysql.initial.sql
        dest: /tmp/roundcube_mysql_files/
      notify:
        - create database

    - name: create user
      mysql_user:
        name: roundcube
        password: roundcube
        priv: "*.*:ALL"
        host: "{{ hostvars[groups['webservers'][0]]['ansible_eth0']['ipv4']['address'] }}"

- name: configure shared items for webservers
  hosts: webservers
  become: yes
  gather_facts: no

  tasks:
    - name: buildtools
      include_role:
        name: robertdebock.buildtools

    - name: epel
      include_role:
        name: robertdebock.epel

    - name: scl
      include_role:
        name: robertdebock.scl

    - name: python-pip
      include_role:
        name: robertdebock.python-pip

    - name: httpd
      include_role:
        name: robertdebock.httpd

    - name: php
      include_role:
        name: robertdebock.php
      vars:
        php_settings:
          display_errors:
            section: PHP
            value: "On"
          display_startup_errors:
            section: PHP
            value: "On"

    - name: roundcubemail
      include_role:
        name: robertdebock.roundcubemail
      vars:
        roundcubemail_database_url: "mysql://roundcube:roundcube@{{ hostvars[groups['databaseservers'][0]]['ansible_eth0']['ipv4']['address'] }}/roundcube"

```

Install this role using `galaxy install robertdebock.roundcubemail`.

License
-------

Apache License, Version 2.0

Author Information
------------------

Robert de Bock <robert@meinit.nl>
