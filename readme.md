Ansible Atlantis Role
========================

Configure Atlantis role.

## Role Variables

- `atlantis_server_config_file`: The name of the atlantis server side config file (Optional)

## Requirements

- Docker and Docker Compose
- A `atlantis_config.yml` file in your files folder. This file contains all server side config for Atlantis
- A `atlantis_env.j2` template in your templates folder. This file contains all environment variables for Atlantis and Terraform.
Atlantis requires `ATLANTIS_ATLANTIS_URL` and `ATLANTIS_REPO_ALLOWLIST` to be set. You also need to specify the all required env variables for your VCS.
This file contains also all secreted needed by Terraform. 

Example: 
```bash
ATLANTIS_ATLANTIS_URL=
ATLANTIS_REPO_ALLOWLIST=

ATLANTIS_GH_USER=
ATLANTIS_GH_TOKEN=
ATLANTIS_GH_WEBHOOK_SECRET=

DIGITALOCEAN_TOKEN=
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
```

## Example Playbook

```yaml
- hosts: all
  roles:
    - role: ansible-atlantis
      vars:
        atlantis_server_config_file: atlantis_config.yml
```

```yaml
- hosts: all
  roles:
    - role: ansible-atlantis
      vars:
        atlantis_server_config_file: atlantis_config.yml
        atlantis_allowed_repos: github.com/aboutbits/infrastructur
        atlantis_url: https://atlantis.aboutbits.cloud
        atlantis_environment_variables: 
          - env_variable_key: ATLANTIS_GH_USER
            env_variable_value: {{ tools01['atlantis']['github']['user'] }}
          - env_variable_key: ATLANTIS_GH_TOKEN
            env_variable_value: {{ tools01['atlantis']['github']['token'] }}
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