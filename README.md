[![build status](https://github.com/alecunsolo/ansible-role-postfix/actions/workflows/ci.yml/badge.svg)](https://github.com/alecunsolo/ansible-role-postfix/actions/workflows/ci.yml)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)

Ansible Role: postfix
=========

This role install `postfix` and configure it to use a remote mail relay.

Requirements
------------

To send mails you need access to a remote mail relay (such as Gmail).

Role Variables
--------------
```yaml
postfix_remote_host: a.remote.host  # smtp.gmail.com
postfix_remote_port: 587
postfix_remote_user: username  # your.username@gmail.com
postfix_remote_password: password  # application password if 2FA is enabled
```
These are the variables used to establish a connection with the remote relay server and must be provided in the playbook.

```yaml
postfix_config:
  - name: relayhost
    value: '[{{ postfix_remote_host }}]:{{ postfix_remote_port }}'
  - name: smtp_use_tls
    value: 'yes'
  - name: smtp_sasl_auth_enable
    value: 'yes'
  - name: smtp_sasl_security_options
    value:
  - name: smtp_sasl_password_maps
    value: hash:/etc/postfix/sasl/sasl_passwd
  - name: inet_interfaces
    value: localhost
  - name: inet_protocols
    value: ipv4
```
This is an array with the needed configuration options to add to the `/etc/postfix/main.cf` file.

```yaml
mail_packages:
  # Debian
  - postfix
  - libsasl2-modules
  - mailutils
  # RedHat
  - postfix
  - cyrus-sasl
  - cyrus-sasl-plain
  - s-nail
```
List of the needed packages.

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- name: Postfix
  hosts: all
  vars:
    postfix_remote_host: a.remote.host
    postfix_remote_port: 587
    postfix_remote_user: username
    postfix_remote_password: password
  roles:
    - alecunsolo.postfix
```

License
-------

MIT

Notes
-----

Testing with molecule (including the docker images used) is ~~stolen from~~ heavily inspired by [Jeff Geerling](https://www.jeffgeerling.com/). Watch [his video](https://youtu.be/FaXVZ60o8L8) (and the other ones as well).
