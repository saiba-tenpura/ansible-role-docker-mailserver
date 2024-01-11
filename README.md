# Ansible Role: Docker Mailserver
Installs a [docker mailserver](https://github.com/docker-mailserver/docker-mailserver) instance.

## Requirements
Docker and the Compose plugin need to be installed on the server to actually be able to launch the instance

## Role Variables
A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

## Dependencies
None.

## Example Playbook
```
    - hosts: servers
      roles:
         - role: saiba-tenpura.docker-mailserver
```

## License
MIT

## Author Information
This project was created in 2024 by Saiba Tenpura.
