# Ansible Role: Docker Mailserver
Installs a [docker mailserver](https://github.com/docker-mailserver/docker-mailserver) instance based on the official documentation.
Adjustments to the compose.yaml are handled by generating a custom composer.override.yaml.

## Requirements
Docker and the Compose v2 plugin need to be already installed on the server. You could use [this](https://github.com/geerlingguy/ansible-role-docker) ansible role to do so.

## Role Variables
Available variables are listed below, along with default values (see defaults/main.yml):
| Variable          | Description                                                                                                                    | Default value                                                                |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------- |
| dms_hostname      | The FQDN to use for your mailserver in the **compose.yaml**.                                                                   | -                                                                            |
| dms_accounts      | List of email accounts and their aliases to add to the mailserver.                                                             | []                                                                           |
| dms_env_vars      | Dict specifying config for [mailserver.env](https://docker-mailserver.github.io/docker-mailserver/latest/config/environment/). | {}                                                                           |
| dms_install_dir   | Target directory for installation of files, configs, mail data etc.                                                            | /srv/mail                                                                    |
| dms_ipv6_enable   | When enabled it adds an IPv6 network to the mailserver container.                                                              | false                                                                        |
| dms_ipv6_subnet   | The IPv6 subnet used for the mailserver container.                                                                             | fd00:cafe:face:feed::/64                                                     |
| dms_ssl_provider  | Adds additional services for provisioning certificates a list of options can be found [here](#ssl-providers).                  |                                                                              |
| dms_ssl_email     | Email used for certificate provisioning.                                                                                       |                                                                              |
| dms_ssl_path      | Storage path for the certificate data.                                                                                         |                                                                              |
| dms_compose_state | Target state of the docker compose services after the rollout has been completed.                                              | restarted                                                                    |
| dms_dkim_keysize  | Key size used for generating the DKIM keys.                                                                                    | 2048                                                                         |
| dms_repo_url      | URL for **compose.yaml** & **mailserver.env** retrieval.                                                                       | https://raw.githubusercontent.com/docker-mailserver/docker-mailserver/master |

### IPv6 Support
Before enabling IPv6 support you should first read the [official documentation](https://docker-mailserver.github.io/docker-mailserver/edge/config/advanced/ipv6/) on the topic in order to understand the consequences of an improper configuration and why you may or may not want to enable it.

Please also make sure you made the necessary adjustments in your Docker daemon.json to enable IPv6 support.

### SSL Providers
| Option           | Description                                                                                                                                                             |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| traefik          | Adds additional Traefik and Whoami services to the setup. Based on [this](https://docker-mailserver.github.io/docker-mailserver/latest/config/security/ssl/#traefik-v2) |
| traefik-external | Adds an Whoami service with additional labels to utilize a separate Traefik instance via a shared network.                                                              |

### Additional Remarks
- Setting **ENABLE_FAIL2BAN** to 1 in the **dms_env_vars** will automatically add the necessary NET_ADMIN capabilities to the compose.override.yaml.

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
    # BEWARE: Accounts and aliases are managed by ansible and are not be manually modified.
    # You should ideally store the credentials via ansible-vault.
    dms_accounts:
      - email: user@example.com
        password: eX@mPl3_p4$5w0rd
        aliases:
          - dmarc.report@example.com
    dms_env_vars:
      ENABLE_FAIL2BAN: 1
      SPOOF_PROTECTION: 1
    dms_ssl_provider: traefik
    dms_ssl_email: admin@example.com
  roles:
    - role: saiba-tenpura.docker-mailserver
```

## Post-Installation Steps
After your installation has finished make sure to verify the following:
* Add the [baseline DNS entries](https://docker-mailserver.github.io/docker-mailserver/latest/usage/#minimal-dns-setup) (MX, A, PTR) which are necessary for your mailserver to function.
* Based on your chosen configuration/setup add [additional DNS entries](https://docker-mailserver.github.io/docker-mailserver/latest/config/best-practices/dkim_dmarc_spf/) (DKIM, DMARC, SPF) to increase security.
* Adjust your firewall configuration to allow access via the [necessary ports](https://docker-mailserver.github.io/docker-mailserver/latest/config/security/understanding-the-ports/#overview-of-email-ports).
* Ensure everything is working correctly by using the testing tools found [here](https://docker-mailserver.github.io/docker-mailserver/latest/usage/#testing).

## License
MIT

## Author Information
This project was created in 2024 by Saiba Tenpura.
