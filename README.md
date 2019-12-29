# Ansible Role: controller

Ansible role for installing an Ansible Controller into a Python3 VirtualEnv.

[![Build Status](https://www.travis-ci.org/PyratLabs/ansible-role-controller.svg?branch=master)](https://www.travis-ci.org/PyratLabs/ansible-role-controller)

## Requirements

This role requires an existing installation of Ansible. A temporary installation
can be created by running the following [bootstrap script](bootstrap.sh).

This role has been tested on Ansible 2.6.0+ against the following Linux Distributions:

  - Amazon Linux 2
  - CentOS 8
  - CentOS 7
  - Debian 10
  - Fedora 29
  - Fedora 30
  - Fedora 31
  - Ubuntu 18.04 LTS

## Disclaimer

If you have any problems please create a GitHub issue, I maintain this role in
my spare time so I cannot promise a speedy fix delivery.

## Role Variables


| Variable                                    | Description                                                                 | Default Value              |
|---------------------------------------------|-----------------------------------------------------------------------------|----------------------------|
| `controller_ansible_version`                | Use a specific version of Ansible, eg. `2.9.0`. Specify `false` for latest. | `false`                    |
| `controller_ansible_install_dir`            | Installation directory to put Ansible virtual environments.                 | `$HOME/.virtualenvs`       |
| `controller_ansible_current_dirname`        | Name for the currently active Ansible Virtualenv.                           | ansible                    |
| `controller_ansible_config_path`            | Path to the default ansible.cfg file to use.                                | `$HOME/.ansible.cfg`       |
| `controller_ansible_inventory_path`         | Path to the default ansible inventory file.                                 | `$HOME/.ansible/hosts.yml` |
| `controller_ansible_projects_dir`           | Directory to put Ansible projects.                                          | `$HOME/projects`           |
| `controller_ansible_roles_dir`              | Directory to install Ansible Galaxy roles to.                               | `$HOME/.ansible/roles`     |
| `controller_install_os_dependencies`        | Allow role to install OS dependencies.                                      | `false`                    |
| `controller_python3_path`                   | Specify a path to a specific python version to use in virtualenv.           | _NULL_                     |
| `controller_ansible_galaxy_roles`           | List of Ansible roles to be installed with `ansible-galaxy`. See notes.     | _NULL_                     |
| `controller_ansible_projects`               | List of Ansible projects to be cloned with `git`. See notes.                | _NULL_                     |
| `controller_ansible_projects_install_roles` | Install Ansible roles defined in project requirements.yml file. (boolean)   | `false`                    |

## Dependencies

No dependencies on other roles.

## Example Playbook

Example playbook for installing to single user:

```yaml
- hosts: control_hosts
  roles:
     - { role: xanmanning.controller, controller_ansible_version: 2.6.5 }
```

Example playbook for installing the latest Ansible version globally:

```yaml
---
- hosts: control_hosts
  become: true
  vars:
    controller_install_os_dependencies: true
    controller_ansible_install_dir: /opt/ansible/bin
    controller_ansible_config_path: /etc/ansible/ansible.cfg
    controller_ansible_inventory_path: /etc/ansible/hosts
    controller_ansible_projects_dir: /opt/ansible/projects
    controller_ansible_roles_dir: /opt/ansible/roles
    controller_ansible_current_dirname: current
    controller_ansible_galaxy_roles:
      - name: controller
        src: xanmanning.controller
      - name: git
        src: https://github.com/geerlingguy/ansible-role-git
        scm: git
        version: 2.1.0
  roles:
    - role: xanmanning.controller
```

### Activating the Ansible venv

Once logged into the controller, you need to activate the python3 virtual
environment to be able to access Ansible. This is done as per the below:

```bash
source {{ controller_ansible_install_dir }}/{{ controller_ansible_current_dirname }}/bin/activate
```

In the above example global installation playbook, this would look like the
following:

```bash
source /opt/ansible/bin/current/bin/activate
```

### Note about `controller_ansible_galaxy_roles`

This is a list of Ansible roles to be installed using `ansible-galaxy`. This
variable needs to be structured as per the below example:

```yaml
controller_ansible_galaxy_roles:
  # Install the latest xanmanning.k3s role
  - src: xanmanning.k3s

  # Install the latest xanmanning.controller role as "controller"
  - name: controller
    src: xanmanning.controller

  # Install geerlingguy.jenkins version 3.8.1 from github as "jenkins"
  - name: jenkins
    src: https://github.com/geerlingguy/ansible-role-jenkins
    scm: git
    version: 3.8.1
```

### Note about `controller_ansible_projects`

This is a list of git repositories to be cloned into the projects directory.
If this is empty, no projects will be cloned.

Below is an example of a project:

```yaml
controller_ansible_projects:
    - name: mac-dev-workstation                           # Directory name to clone into
      repo: git@github.com:geerlingguy/mac-dev-playbook   # Repository to clone
      update_repo: true                                   # Always update local copy of repo
      version:  master                                    # Check out this version of the repo
      force: false                                        # Discard any existing working copy of the repo
      key_file: "{{ ansible_user_dir }}/.ssh/id_rsa"      # Key file to use to clone the repo
      recursive: true                                     # Include submodules in clone
```

## License

BSD

## Author Information

[Xan Manning](https://xanmanning.co.uk/)
