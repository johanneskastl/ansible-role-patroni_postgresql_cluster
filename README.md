![Ansible Lint](https://github.com/johanneskastl/ansible-role-patroni_postgresql_cluster/workflows/Ansible%20Lint/badge.svg)

patroni_postgresql_cluster
=========

Install and configure Patroni to setup a highly-available PostgreSQL cluster

Requirements
------------

You need to have a working etcd cluster, that Patroni can use to store its status in. See [the johanneskastl.install_and_configure_etcd](https://github.com/johanneskastl/ansible-role-install_and_configure_etcd) role that can be used to setupan etcd cluster.

Role Variables
--------------

The role assumes a inventory group called `patroni_servers_group` that contains all servers that should be used for the Patroni PostgreSQL cluster.

The connect to the etcd cluster, Patroni needs information on it. By default it uses the group `etcd_servers_group` that is being iterated over to get all of the endpoints. Each endpoint's default IPv4 address is added to the Patroni configuration, using port 2379.

## General settings

- `systemd_unit_path`: path to the systemd unit directory (default: `/etc/systemd/system`)
- `systemd_unit_name`: name of the service unit (default: `patroni.service`)

## PostgreSQL specifics

- `postgres_version`: version of postgreSQL to install (default: `14`)

## Patroni settings

To get the complete list of variables that can be used with this role, please have a look at the [defaults file](defaults/main.yml) and the [patroni.yml template](templates/patroni.yml.j2).

The ones most relevant will be highlighted below.

The general settings are made in these three variables:

- `patroni_scope`: The name of the Patroni cluster (default: `postgres`)
- `patroni_namespace`: The namespace within etcd (default: `/service/`). This can be supplied with or without the leading and trailing slash, the role will make sure it is in the right format.
- `patroni_restapi_port`: The port on which the Patroni REST API will listen (default: `8008`)
- `patroni_working_directory`: working directory for patroni. Inside of this directory, the role will create another directory called `patroni`, that will store the actual data. This directory can be renamed by patroni in case of errors.
- `patroni_bootstrap_initdb`: settings for bootstrapping, default is to use `encoding: UTF8` and `data-checksums`

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      roles:
        - role: 'johanneskastl.patroni_postgresql_cluster'

License
-------

BSD-3-Clause

Author Information
------------------

I am Johannes Kastl, reachable via kastl@b1-systems.de.
