# Ansible PostgreSQL
[![CI](https://github.com/supertarto/ansible-postgresql/workflows/CI/badge.svg?event=push)](https://github.com/supertarto/ansible-postgresql/actions?query=workflow%3ACI)

Install and configure Postgresql with Ansible. This role is tested only with Debian.

## Tested plateform
* Debian 10 (Buster)
* Debian 11 (Bulleyes)
* Debian 12 (Bookworm)

## Role variables
A list of package to install.

```yml
postgresql_packages:
  - postgresql
  - postgresql-contrib
  - libpq-dev
```

Informations about PostgreSQL. Wich version to install, the daemon name and directories used.

```yml
postgresql_version: "11"
postgresql_daemon: postgresql@{{ postgresql_version }}-main
postgresql_data_dir: "/var/lib/postgresql/{{ postgresql_version }}/main"
postgresql_bin_path: "/usr/lib/postgresql/{{ postgresql_version }}/bin"
postgresql_config_path: "/etc/postgresql/{{ postgresql_version }}/main"
```

Use the provided dump script

```yml
postgresql_use_dump_script: true
postgresql_dump_path: "/var/local/dump_sql"
postgresql_dump_path_script: "/var/local/scripts"
```

Name of the psycopg2 librairie to install. Used during the users creation.

```yml
postgresql_python_library: python-psycopg2
```

Default Locale

```yml
postgresql_locales:
  - 'fr_FR.UTF-8'
```

Name of the user and group used by Postrgresql

```yml
postgresql_user: postgres
postgresql_group: postgres
```

Informations about the socket.

```yml
postgresql_global_config_options:
  - option: unix_socket_directories
    value: '{{ postgresql_unix_socket_directories | join(",") }}'

postgresql_unix_socket_directories:
  - /var/run/postgresql
```

List of user to create. Only the name is mandatory.

```yml
postgresql_users:
# - name: testuser #required; the rest are optional
#   password: # defaults to not set
#   encrypted: # defaults to not set
#   priv: # defaults to not set
#   role_attr_flags: # defaults to not set
#   db: # defaults to not set
#   login_host: # defaults to 'localhost'
#   login_password: # defaults to not set
#   login_user: # defaults to '{{ postgresql_user }}'
#   login_unix_socket: # defaults to 1st of postgresql_unix_socket_directories
#   port: # defaults to not set
#   state: # defaults to 'present'
```

List of database to create. Only the name is mandatory.

```yml
postgresql_databases:
# - name: exampledb # required; the rest are optional
#   lc_collate: # defaults to 'fr_FR.UTF-8'
#   lc_ctype: # defaults to 'fr_FR.UTF-8'
#   encoding: # defaults to 'UTF-8'
#   template: # defaults to 'template0'
#   login_host: # defaults to 'localhost'
#   login_password: # defaults to not set
#   login_user: # defaults to '{{ postgresql_user }}'
#   login_unix_socket: # defaults to 1st of postgresql_unix_socket_directories
#   port: # defaults to not set
#   owner: # defaults to postgresql_user
#   state: # defaults to 'present'
```

## Examples

```yml
---
- name: Converge
  hosts: all
  roles:
    - role: supertarto.postgresql

  vars:
    postgresql_databases:
      - name: example
    postgresql_users:
      - name: testuse
```

## Installation

```bash
ansible-galaxy install supertarto.postgresql
```

## License
GPL V3.0
