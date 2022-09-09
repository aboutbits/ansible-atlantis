# Ansible Atlantis Role

A role to install and configure Atlantis.

## Role Variables

- `atlantis_config_file`: The name of the Atlantis server side config file (Optional)
- `atlantis_env_file`: The name of the Atlantis environment template file (Optional)

## Requirements

- Docker and Docker Compose
- A `atlantis_config.yml` file in your files folder. This file contains all server side config for Atlantis
- A `atlantis_env.j2` template in your templates folder. This file contains all environment variables for Atlantis and Terraform.

Some configurations required by Atlantis have to be passed using environment variables. Some required variables are `ATLANTIS_ATLANTIS_URL` and `ATLANTIS_REPO_ALLOWLIST`. 
You also need to specify the required environment variables for your VCS.

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
  roles:
    - role: ansible-atlantis
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