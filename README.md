# Ansible Role: Docker Mailserver (WIP)
Installs a [docker mailserver](https://github.com/docker-mailserver/docker-mailserver) instance.

## Requirements
Docker and the Compose v2 plugin need to be already installed on the server. You could use [this](https://github.com/geerlingguy/ansible-role-docker) ansible role to do so.

## Role Variables
Available variables are listed below, along with default values (see defaults/main.yml):
| Variable        | Description                                                                                                                    | Default value                                                                |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------- |
| dms_hostname    | The FQDN to use for your mailserver in the **compose.yaml**.                                                                   | -                                                                            |
| dms_repo_url    | URL for **compose.yaml** & **mailserver.env** retrieval.                                                                       | https://raw.githubusercontent.com/docker-mailserver/docker-mailserver/master |
| dms_install_dir | Target directory for installation of files, configs, mail data etc.                                                            | /srv/mail                                                                    |
| dms_env_vars    | Dict specifying config for [mailserver.env](https://docker-mailserver.github.io/docker-mailserver/latest/config/environment/). | (see [defaults/main.yml](defaults/main.yml))                                 |

## Dependencies
This role requires the latest release candidate for the community.docker collection.
```
ansible-galaxy collection install community.docker:3.6.0-b2
```

## Example Playbook
```
    - hosts: servers
      vars:
        dms_env_vars:
          ENABLE_FAIL2BAN: 1
          SPOOF_PROTECTION: 1
      roles:
        - role: saiba-tenpura.docker-mailserver
```

## License
MIT

## Author Information
This project was created in 2024 by Saiba Tenpura.
