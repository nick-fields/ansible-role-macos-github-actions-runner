# macos-github-actions-runner

This role will install a GitHub Actions self-hosted runner at either the org or repo level on a MacOS/MacOSX. It will perform a clean install, reinstall if desired, reinstall if service is not running, or uninstall. Inspired heavily by [ansible-github_actions_runner](https://github.com/MonolithProjects/ansible-github_actions_runner) but modified to work with MacOS.

## Requirements

This role should only be used on MacOS 10.13 (High Sierra) or newer.

The following environment variables are required:

- `PERSONAL_ACCESS_TOKEN` - GitHub PAT with `repo` scope if adding repo runner or `admin:org` scope if adding org runner

The following variables are required:

- `github_account` - The case-sensitive GitHub account name. This is either the GitHub user name, or the GitHub Organization name.
- `access_token` - This is optional and takes the place of the required `PERSONAL_ACCESS_TOKEN` environment variable.

## Role Variables

Defaults (from `defaults/main.yml`):

```yaml
# Runner user - user under which is the local runner service running
runner_user: "vagrant"

# Directory where the local runner will be installed
runner_dir: ~/actions-runner

# Version of the GitHub Actions Runner
runner_version: "latest"

# If found on the server, delete already existing runner service and install it again
reinstall_runner: no

# Do not show Ansible logs which may contain sensitive data (registration token)
hide_sensitive_logs: yes

# GitHub address
github_url: "https://github.com"

# GitHub API
github_api_url: "https://api.github.com"

# Personal Access Token for your GitHub account
access_token: "{{ lookup('env', 'PERSONAL_ACCESS_TOKEN') }}"

# Is it the runner for organization or not?
runner_org: no

# Name to assign to this runner in GitHub (System hostname as default)
runner_name: "{{ hostname }}"

# Labels to apply to the runner
runner_labels: []

# Labels to apply to the runner
runner_work_dir: "_work"

# Extra arguments to pass to `config.sh`
runner_extra_config_args: ""
```

## Dependencies

None

## Example Playbook

To configure a repo level GitHub self-hosted runner:

```yaml
- name: Install GitHub Actions Runner
  hosts: macs
  user: ansible
  become: yes
  vars:
    - github_account: github-access-user
    - github_repo: my_awesome_repo
  roles:
    - role: nick-invision.macos_github_actions_runner
```

To configure an org level GitHub self-hosted runner:

```yaml
- name: Install GitHub Actions Runner
  hosts: macs
  user: ansible
  become: yes
  vars:
    - github_account: github-access-user
    - runner_org: true
  roles:
    - role: nick-invision.macos_github_actions_runner
```

To configure a GitHub self-hosted runner with various overrides:

```yaml
- name: Install GitHub Actions Runner
  hosts: macs
  user: ansible
  become: yes
  vars:
    - github_account: github-access-user
    - runner_org: true
    - runner_labels:
        - some-label
    - runner_version: 2.277.0
    -
  roles:
    - role: nick-invision.macos_github_actions_runner
```

## License

MIT
