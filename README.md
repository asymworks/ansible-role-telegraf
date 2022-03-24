# Ansible Role: Asymworks Telegraf Agent

An Ansible Role that installs the Telegraf agent on Asymworks servers running Debian or Alpine Linux.

## Requirements

None

## Role Variables

Available variables are listed below, along with default values if applicable (see `defaults/main.yml`):

```yaml
telegraf_enabled: true
telegraf_global_tags: {}
telegraf_hostname: "{{ ansible_fqdn }}"
telegraf_collection_interval: "10s"
telegraf_collection_jitter: "0s"
telegraf_flush_interval: "10s"
telegraf_flush_jitter: "5s"
```

Whether the Telegraf agent is installed and enabled, global settings, and global tags to include with each metric.

```yaml
telegraf_influx_enabled: false
telegraf_influx_server: http://metrics.lan
telegraf_influx_port: 8086
telegraf_influx_database: telegraf
telegraf_influx_retention_policy: ''
telegraf_influx_username: ''
telegraf_influx_password: ''
```

Remote InfluxDB 1.x configuration, used for the output plugin.  By default, only InfluxDB 2.0 is used.

```yaml
telegraf_influx_v2_enabled: true
telegraf_influx_v2_server: http://metrics.lan
telegraf_influx_v2_port: 8086
telegraf_influx_v2_bucket: metrics
telegraf_influx_v2_organization: kraussnet
telegraf_influx_v2_token: influxdb-v2-api-token
```

Remote InfluxDB 1.x configuration.  The API token must grant write access to the specified organization and bucket.

```yaml
telegraf_conf_path: /etc
telegraf_conf_d_path: >-
  {{
    '/etc/telegraf.conf.d'
      if ansible_facts['os_family']|lower == 'alpine'
      else '/etc/telegraf/telegraf.d'
  }}

```

Override these variables to control where the Telegraf agent dynamic configuration files are stored.  By default the general configuration is stored in `/etc/telegraf.conf` and plugin configurations go in `/etc/telegraf.conf.d` on Alpine or `/etc/telegraf/telegraf.d` on Debian.

## Role Facts

None

## Dependencies

None

## Example Playbook

```yaml
- hosts: all
  pre_tasks:
    include_role:
      name: asymworks.baseline
      tasks_from: pre-tasks.yml
  roles:
    asymworks.baseline
    asymworks.telegraf
  post_tasks:
    include_role:
      name: asymworks.baseline
      tasks_from: post-tasks.yml
```

## License

MIT / BSD

## Author

This role was created in 2022 by Asymworks, LLC.
