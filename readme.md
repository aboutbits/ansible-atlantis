# Ansible Atlantis Role

A role to install and configure Atlantis.

## Requirements

- Docker and Docker Compose

## Role Variables

- `atlantis_config_file`: The name of the Atlantis server side config file (Optional). This file contains all server side config for Atlantis.
- `atlantis_env_file`: The name of the Atlantis environment template file (Optional). This file contains all environment variables for Atlantis and Terraform.

## Environment variables

Some configurations are required by Atlantis to be passed using environment variables. Some of these required variables are `ATLANTIS_ATLANTIS_URL` and `ATLANTIS_REPO_ALLOWLIST`.

In addition, you also have to provide the configuration and credentails for your desired Git host. Please take a look at the documentation for further details:
  - [Git Host Access Credentials](https://www.runatlantis.io/docs/access-credentials.html)
  - [Configuring Webhooks](https://www.runatlantis.io/docs/configuring-webhooks.html)

Last, configure the provider credentails so Atlantis can actually run Terraform commands.

An example of such an environment variable file could be:

```bash
ATLANTIS_ATLANTIS_URL={{ atlantis['url'] }}
ATLANTIS_REPO_ALLOWLIST={{ atlantis['allowed_repos'] }}

ATLANTIS_GH_USER={{ atlantis['github']['user'] }}
ATLANTIS_GH_TOKEN={{ atlantis['github']['token'] }}
ATLANTIS_GH_WEBHOOK_SECRET={{ atlantis['github']['webhook_secret'] }}

DIGITALOCEAN_TOKEN={{ atlantis['digitalocean']['api_key'] }}
AWS_ACCESS_KEY_ID={{ atlantis['terraform']['aws']['access_key_id'] }}
AWS_SECRET_ACCESS_KEY={{ atlantis['terraform']['aws']['secret_access_key'] }}
```

The variables within the brackets are Ansible variables. You could store these secrets for example using Ansible Vault.

## Example Playbook

```yaml
- hosts: all
  tasks:
    - ansible.builtin.include_role:
        name: ansible-atlantis
      vars:
        atlantis_config_file: atlantis_config.yml
        atlantis_env_file: atlantis_env.j2
```

## Versioning

In order to have a verioning in place and working, create leightweight tags that point to the appropriate minor release versions.

Creating a new minor release:

```bash
git tag 1.0
git push --tags
```

Replacing an already existing minor release:

```bash
git tag -d 1.0
git push origin :refs/tags/1.0
git tag 1.0
git push --tags
```
