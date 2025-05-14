# Ansible Atlantis Role

A role to install and configure Atlantis.

## Requirements

- Docker and Docker Compose

## Role Variables

- `atlantis_docker_tag`: The Docker tag that should be used (Optional).
- `atlantis_server_port`: Atlantis server port (Optional).
- `atlantis_env_file`: The name of the Atlantis environment template file (Optional). This file contains all environment variables for Atlantis and Terraform.

## Environment variables

Some configurations are required by Atlantis to be passed using environment variables. Some of these required variables are `ATLANTIS_ATLANTIS_URL` and `ATLANTIS_REPO_ALLOWLIST`.

In addition, you also have to provide the configuration and credentails for your desired Git host. Please take a look at the documentation for further details:
  - [Git Host Access Credentials](https://www.runatlantis.io/docs/access-credentials.html)
  - [Webhook Secrets](https://www.runatlantis.io/docs/webhook-secrets.html)
  - [Configuring Webhooks](https://www.runatlantis.io/docs/configuring-webhooks.html)

Last, configure the provider credentials so Atlantis can actually run Terraform commands.

An example of such an environment variable file could be:

```bash
ATLANTIS_ATLANTIS_URL={{ atlantis['url'] }}
ATLANTIS_REPO_ALLOWLIST={{ atlantis['allowed_repos'] }}
ATLANTIS_REPO_CONFIG_JSON={"repos":[{"id":"/.*/","apply_requirements":["approved","mergeable"],"allowed_overrides":["apply_requirements","workflow","delete_source_branch_on_merge"],"allow_custom_workflows":true,"delete_source_branch_on_merge":true}]}

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
        atlantis_docker_tag: v0.27.1
        atlantis_server_port: 4141
        atlantis_env_file: atlantis_env.j2
```

## Versioning

In order to have a versioning in place and working, create lightweight tags that point to the appropriate minor release versions.

Creating a new minor release:

```bash
git tag v2
git push --tags
```

Replacing an already existing minor release:

```bash
git tag -d v2
git push origin :refs/tags/v2
git tag v2
git push --tags
```
