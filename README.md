# [timezone](#timezone)

|GitHub|GitLab|
|------|------|
|[![github](https://github.com/mullholland/ansible-role-timezone/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-timezone/actions)|[![gitlab](https://gitlab.com/mullholland/ansible-role-timezone/badges/master/pipeline.svg)](https://gitlab.com/mullholland/ansible-role-timezone)|[![quality](https://img.shields.io/ansible/quality/unset)](https://galaxy.ansible.com/mullholland/timezone)|

description

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
```yaml
---
# Default timezone
timezone: "Etc/UTC"

timezone_dependent_services_map:
  RedHat:
    - "crond"
    - "rsyslog"
  Rocky:
    - "crond"
    - "rsyslog"
  Debian:
    - "cron"
    - "rsyslog"

timezone_dependent_services: "{{ timezone_dependent_services_map[ansible_distribution] | default(timezone_dependent_services_map[ansible_os_family] | default(timezone_dependent_services_map['default'] )) }}"
```


## [Example Playbook](#example-playbook)

This example is taken from `molecule/default/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true
  # vars:
  #   example_var: "value"
  roles:
    - role: "mullholland.timezone"
```

The machine needs to be prepared in CI this is done using `molecule/default/prepare.yml`:
```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: true

  tasks:
    # dependend packages can be configured via `timezone_dependent_services`
    - name: Install dependencies
      ansible.builtin.package:
        name:
          - "cronie"  # For the cron script
          - "rsyslog"  # For the cron script
        state: present
      when:
        - ansible_distribution in [ "RedHat", "CentOS", "Amazon", "Rocky", "AlmaLinux", "Fedora" ]

    - name: Install dependencies
      ansible.builtin.package:
        name:
          - "cron"  # default timezone dependent services
          - "rsyslog"  # default timezone dependent services
        state: present
      when:
        - ansible_os_family == "Debian"
```





## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

-   [debian9](https://hub.docker.com/r/mullholland/docker-molecule-debian9)
-   [debian10](https://hub.docker.com/r/mullholland/docker-molecule-debian10)
-   [debian11](https://hub.docker.com/r/mullholland/docker-molecule-debian11)
-   [ubuntu1804](https://hub.docker.com/r/mullholland/docker-molecule-ubuntu1804)
-   [ubuntu2004](https://hub.docker.com/r/mullholland/docker-molecule-ubuntu2004)
-   [centos7](https://hub.docker.com/r/mullholland/docker-molecule-centos7)
-   [centos-stream8](https://hub.docker.com/r/mullholland/docker-molecule-centos-stream8)
-   [ubi8](https://hub.docker.com/r/mullholland/docker-molecule-ubi8)
-   [fedora34](https://hub.docker.com/r/mullholland/docker-molecule-fedora34)
-   [fedora35](https://hub.docker.com/r/mullholland/docker-molecule-fedora35)
-   [amazonlinux](https://hub.docker.com/r/mullholland/docker-molecule-amazonlinux)
-   [rockylinux8](https://hub.docker.com/r/mullholland/docker-molecule-rockylinux8)
-   [almalinux8](https://hub.docker.com/r/mullholland/docker-molecule-almalinux8)

The minimum version of Ansible required is 2.10, tests have been done to:

-   The last 2 versions.
-   The current version.



## [Exceptions](#exceptions)

Some variations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| centos-stream9 | Testing problems ATM |


If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-timezone/issues)

## [License](#license)

MIT


## [Author Information](#author-information)

[Mullholland](https://github.com/mullholland)

## [Special Thanks](#special-thanks)

Template inspired by [Robert de Bock](https://github.com/robertdebock)
