ovv.owntrack
============

[![Build Status](https://travis-ci.org/ovv/ansible-role-owntrack.svg?branch=master)](https://travis-ci.org/ovv/ansible-role-owntrack)

Ansible role to install and configure the [owntrack recorder](https://github.com/owntracks/recorder).

Requirements
------------

An Nginx installation with basic auth is required. We recommend using [pyslackers.nginx](https://github.com/pyslackers/ansible-role-nginx).


Installation
------------

To install this roles clone it into your roles directory.

```bash
$ git clone https://github.com/ovv/ansible-role-owntrack.git ovv.owntrack
```

If your playbook already reside inside a git repository you can clone it by using git submodules.

```bash
$ git submodule add -b master https://github.com/ovv/ansible-role-owntrack.git ovv.owntrack
```

Role Variables
--------------

* `owntrack_http_port`: Listening http port (default to `8083`).

Example Playbook
----------------

```yml
- hosts: owntrack
  roles:
    - ovv.owntrack
    - pyslackers.nginx
  vars:
    nginx_sites:
      owntrack:
        locations:
          - location: /ws
            websocket: True
            proxy_pass: http://127.0.0.1:8083
          - location: /view
            custom: |
              proxy_buffering         off;            # Chrome
              proxy_pass              http://127.0.0.1:8083;
          - location: /
            proxy_pass: http://127.0.0.1:8083

    nginx_auth:
      owntrack:
        - {name: "foo", password: "bar"}
```

License
-------

MIT
