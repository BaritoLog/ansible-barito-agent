[![Build Status](https://travis-ci.org/BaritoLog/ansible-barito-agent.svg?branch=master)](https://travis-ci.org/BaritoLog/ansible-barito-agent)

Ansible Role: ansible-barito-agent
=========

Ansible role to install barito agent (td-agent)

Requirements
------------

- Ansible >= 2.9 (It might work on previous versions, but we cannot guarantee it)

Role Variables
--------------

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.

| Name                       | Default Value                                    | Description                      |
| -------------------------- | ------------------------------------------------ | -------------------------------- |
| `barito_log_directory`     | "/var/log/td-agent"                              | log directory location           |
| `tdagent_version`          | "3"                                              | td-agent version to install      |
| `tdagent_groups`           | ["root"]                                         | td-agent user groups             |
| `tdagent_plugins`          | ["fluent-plugin-systemd", fluent-plugin-barito"] | install td-agent plugin          |
| `tdagent_plugins_versions` |                                                  | install specific td-agent plugin |
| `tdagent_sources`          | []                                               | td-agent source configuration    |
| `tdagent_matches`          | []                                               | td-agent match configuration     |
| `tdagent_filters`          | []                                               | td-agent filter configuration    |


Dependencies
------------

None

Example Playbook
----------------

Use it in a playbook as follows:
```yaml
- name: Converge
  hosts: all
  roles:
    - role: ansible-barito-agent
      tdagent_sources:
        - "@type": "tail"
          type: "tail"
          name: "barito-test"
          tag: "barito-test"
          path: "/var/log/dpkg.log"
          pos_file: "/etc/td-agent/dpkg.pos"
          parse:
            "@type": "none"
      tdagent_matches:
        - "@type": barito_vm
          type: barito_vm
          name: "barito-test"
          application_secret: "123456"
          produce_url: "https://barito-router.local/produce"
          buffer:
            flush_mode: immediate
      tdagent_plugins:
        - fluent-plugin-systemd
        - fluent-plugin-barito
      tdagent_plugins_versions:
        barito:
          name: fluent-plugin-barito
          version: "0.3.4"
```

License
-------

This project is licensed under MIT License.

Author Information
------------------

BaritoLog
