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

## Pausing and resuming traffic

One of the most useful things about PGBouncer is the ability to pause and resume database connections.

This makes it easier for you to do DB maintenance and other work while minimising interruption to clients of the databases.

This playbook includes some ad-hoc commands that can be run to pause and resume database connectivity as part of this process.

Here's how we are using them, create a play that gets prompts the user for the db to pause or resume and then execute the command

Create an `ops.yml` somewhere:

```yaml
- hosts: pgbouncers
  sudo: yes
  vars_files:
    - ./defaults/main.yml
  handlers:
    - include: ./handlers/main.yml
  vars_prompt:
    pgbouncer_command_db: "which database do you want to operate on [blank for all]?"
    pgbouncer_command: "which command would you like to run [stats|pause|resume]?"
  tasks:
    - include: ./tasks/ops.yml pgbouncer_command=pause
```

You can now invoke this across your bouncer cluster like so:

```
ansible-playbook -i inventory ops.yml
```

## License

This playbook is distributed under the
[Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0.html).
