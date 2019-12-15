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


| Variable                             | Description                                                                 | Default Value             |
|--------------------------------------|-----------------------------------------------------------------------------|---------------------------|
| `controller_ansible_version`         | Use a specific version of Ansible, eg. `2.9.0`. Specify `false` for latest. | `false`                   |
| `controller_ansible_install_dir`     | Installation directory to put Ansible virtual environments.                 | `$HOME/ansible`           |
| `controller_ansible_config_path`     | Path to the default ansible.cfg file to use.                                | `$HOME/.ansible.cfg`      |
| `controller_ansible_inventory_path`  | Path to the default ansible inventory file.                                 | `$HOME/ansible/hosts.yml` |
| `controller_ansible_project_dir`     | Directory to put Ansible projects.                                          | `$HOME/projects`          |
| `controller_ansible_current_dirname` | Name for the currently active Ansible Virtualenv.                           | current                   |
| `controller_install_os_dependencies` | Allow role to install OS dependencies.                                      | `false`                   |
| `controller_python3_path`            | Specify a path to a specific python version to use in virtualenv.           | _NULL_                    |

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
    controller_ansible_project_dir: /opt/ansible/projects
    controller_ansible_current_dirname: current
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

## License

BSD

## Author Information

[Xan Manning](https://xanmanning.co.uk/)
