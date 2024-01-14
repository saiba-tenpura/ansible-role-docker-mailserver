# Ansible Role: Docker Mailserver
Installs a [docker mailserver](https://github.com/docker-mailserver/docker-mailserver) instance.

## Requirements
Docker and the Compose plugin need to be already installed on the server. You could use [this](https://github.com/geerlingguy/ansible-role-docker) ansible role to do so.

## Role Variables
Available variables are listed below, along with default values (see defaults/main.yml):
| Variable        | Description                                                         | Default value                                                                |
| --------------- | ------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| dms_repo_url    | URL for **compose.yaml** & **mailserver.env** retrieval.            | https://raw.githubusercontent.com/docker-mailserver/docker-mailserver/master |
| dms_install_dir | Target directory for installation of files, configs, mail data etc. | /srv/mail                                                                    |

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
