# [timezone](#timezone)

Configure timezine settings

|GitHub|GitLab|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![github](https://github.com/mullholland/ansible-role-timezone/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-timezone/actions)|[![gitlab](https://gitlab.com/opensourceunicorn/ansible-role-timezone/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-timezone)|[![quality](https://img.shields.io/ansible/quality/57676)](https://galaxy.ansible.com/mullholland/timezone)|[![downloads](https://img.shields.io/ansible/role/d/57676)](https://galaxy.ansible.com/mullholland/timezone)|[![Version](https://img.shields.io/github/release/mullholland/ansible-role-timezone.svg)](https://github.com/mullholland/ansible-role-timezone/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/mullholland/ansible-role-timezone/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true
  roles:
    - role: "mullholland.timezone"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/mullholland/ansible-role-timezone/blob/master/molecule/default/prepare.yml):

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
          - "cronie"  # default timezone dependent services
          - "rsyslog"  # default timezone dependent services
        state: present
      when:
        - ansible_distribution in [ "RedHat", "CentOS", "Amazon", "Rocky", "AlmaLinux", "Fedora" ]

    - name: Install dependencies
      ansible.builtin.apt:
        name:
          - "cron"  # default timezone dependent services
          - "rsyslog"  # default timezone dependent services
        state: present
        update_cache: true
      when:
        - ansible_os_family == "Debian"
```


## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/mullholland/ansible-role-timezone/blob/master/defaults/main.yml):

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

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/mullholland/ansible-role-timezone/blob/master/requirements.txt).


## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://mullholland.net) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/mullholland/ansible-role-timezone/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/repository/docker/mullholland/docker-centos-systemd/general)|all|
|[Amazon](https://hub.docker.com/repository/docker/mullholland/docker-amazonlinux-systemd/general)|Candidate|
|[Fedora](https://hub.docker.com/repository/docker/mullholland/docker-fedora-systemd/general)|all|
|[Ubuntu](https://hub.docker.com/repository/docker/mullholland/docker-ubuntu-systemd/general)|all|
|[Debian](https://hub.docker.com/repository/docker/mullholland/docker-debian-systemd/general)|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-timezone/issues)

## [License](#license)

[MIT](https://github.com/mullholland/ansible-role-timezone/blob/master/LICENSE).

## [Author Information](#author-information)

[Mullholland](https://mullholland.net)

Please consider [sponsoring me](https://github.com/sponsors/mullholland).
