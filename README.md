# Ansible PGBouncer playbook

[![Build Status](https://travis-ci.org/balanced-ops/ansible-pgbouncer.svg)](https://travis-ci.org/balanced-ops/ansible-pgbouncer)

This is an Ansible playbook for deploying PGBouncer

This playbook was tested on Ubuntu 12.04 x86_64.

## Features

* Install PGBouncer
* Installs newrelic monitoring for PGBouncer
* Installs monit monitoring and alerting for PGBouncer

## Requirements

This playbook requires Ansible 1.2 or higher.

## Running playbook

```bash
ansible-playbook -i ansible.host ./tasks/main.yml
```

## Notes

* We recommend using a proper newrelic plugin like https://github.com/sivel/ansible-newrelic.git as well if you go the newrelic route
* Monit needs to be configured fully in order for you to receive email alerts etc.
* Configure databases like this
  ```yaml
  pgbouncer_aliases:
    - name: balanced_integration
      host: db-integration-prod-mppbhl-p01.us-west-1.vandelay.io
      port: 5432
      db_name: balanced
    - name: balanced_live
      host: balanced-db-03
      port: 5432
  ```
* Connect to the stats interface like this `psql -p 6432 -h host pgbouncer -U pgbouncer` 

## License

This playbook is distributed under the
[Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0.html).
