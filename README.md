Ansible Role Fluentbit
=========

[![Ansible Version](https://img.shields.io/badge/ansible-v2.8+-blue.svg)](https://github.com/eagleusb/ansible-fluentbit)

This role installs and configure [fluentbit](https://fluentbit.io/) log collector.

Installation
------------

```sh
ansible-galaxy install eagleusb.fluentbit
```

Requirements
------------

None

Role Variables
--------------

| Variables                               | Required | Default value | Description                                            |
|-----------------------------------------|----------|---------------|--------------------------------------------------------|
| fluentbit_service_flush_seconds         | no       | *5.0*         | flush input data to output every seconds.nanoseconds   |
| fluentbit_service_log_file              | no       | ""            | output td-agent daemon logs to this file               |
| fluentbit_service_enable_metrics        | no       | *false*       | enable metrics http endpoint                           |
| fluentbit_service_metrics_listen_ip     | no       | "0.0.0.0"     | metrics HTTP endpoint listening addr                   |
| fluentbit_service_metrics_listen_port   | no       | *2020*        | metrics HTTP endpoint listening port                   |
| fluentbit_parser_files                  | no       | *[]*          | array of custom parser templates in jinja2             |
| fluentbit_plugins                       | no       | *[]*          | array of path(s) to your plugins                       |
| fluentbit_filters                       | no       | *[]*          | array of fluentbit filters(s) in JSON                  |
| fluentbit_inputs                        | no       | *[]*          | array of fluentbit inputs(s) in JSON                   |
| fluentbit_outputs                       | no       | *[]*          | array of fluentbit ouputs in JSON                      |
| fluentbit_include_files                 | no       | *[]*          | array of extra configuration files in jinja2           |

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: all
  roles:
    - role: ansible-fluentbit
      vars:
        fluentbit_service_log_level: "trace"
        fluentbit_service_log_file: "/tmp/fluentbit.log"
        fluentbit_service_flush_seconds: 5
        fluentbit_service_enable_metrics: true
        fluentbit_service_metrics_listen_port: 2020
        fluentbit_service_storage_path: "/tmp/fluentbit"
        fluentbit_service_storage_sync: "normal"
        fluentbit_service_storage_checksum: "off"
        fluentbit_service_storage_mem_limit: "100M"
        fluentbit_inputs:
          - Name: "tail"
            Path: "/var/log/syslog"
            Storage.type: "memory"
            Mem_Buf_Limit: "20M"
            Tag: "syslog"
        fluentbit_outputs:
          - Name: "stdout"
            Match: "*"
        fluentbit_filters:
          - Name: "record_modifier"
            Match: "*"
            Record: "hostname ${HOSTNAME}"
        fluentbit_parser_files:
          - name: "myparser.conf"
            template: "{{ playbook_dir }}/templates/myparser.conf.j2"
        fluentbit_include_files:
          - name: "cpu.conf"
            template: "{{ playbook_dir }}/templates/cpu.conf.j2"
```

License
-------

BSD

Author Information
------------------

Heavily modified, this role was originally created by Rachide Ouattara.
