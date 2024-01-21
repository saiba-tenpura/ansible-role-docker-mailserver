# Ansible Role: Docker Mailserver (WIP)
Installs a [docker mailserver](https://github.com/docker-mailserver/docker-mailserver) instance.

## Requirements
Docker and the Compose v2 plugin need to be already installed on the server. You could use [this](https://github.com/geerlingguy/ansible-role-docker) ansible role to do so.

## Role Variables
Available variables are listed below, along with default values (see defaults/main.yml):
| Variable         | Description                                                                                                                    | Default value                                                                |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------- |
| dms_hostname     | The FQDN to use for your mailserver in the **compose.yaml**.                                                                   | -                                                                            |
| dms_accounts     | List of email accounts of the mailserver. New ones are added and already existing ones are updated.                            | []                                                                           |
| dms_env_vars     | Dict specifying config for [mailserver.env](https://docker-mailserver.github.io/docker-mailserver/latest/config/environment/). | (see [defaults/main.yml](defaults/main.yml))                                 |
| dms_install_dir  | Target directory for installation of files, configs, mail data etc.                                                            | /srv/mail                                                                    |
| dms_dkim_keysize | (Only executed when dms_accounts is not empty) Keysize for the DKIM key generation.                                            | 2048                                                                         |
| dms_repo_url     | URL for **compose.yaml** & **mailserver.env** retrieval.                                                                       | https://raw.githubusercontent.com/docker-mailserver/docker-mailserver/master |

## Dependencies
This role requires the installation of the community.docker (>=3.6.0) collection to work.
```
ansible-galaxy collection install community.docker
```

## Example Playbook
```
    - hosts: servers
      vars:
        dms_hostname: mail.example.com
        dms_accounts:
          - email: user@example.com
            password: eX@mPl3_p4$5w0rd
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
