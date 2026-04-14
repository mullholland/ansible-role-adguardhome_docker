# [Ansible role adguardhome_docker](#adguardhome_docker)

Installs and configures adguardhome container based on official adguardhome docker container

|GitHub|Downloads|Version|
|------|---------|-------|
|[![github](https://github.com/mullholland/ansible-role-adguardhome_docker/actions/workflows/molecule.yml/badge.svg)](https://github.com/mullholland/ansible-role-adguardhome_docker/actions/workflows/molecule.yml)|[![downloads](https://img.shields.io/ansible/role/d/mullholland/adguardhome_docker)](https://galaxy.ansible.com/mullholland/adguardhome_docker)|[![Version](https://img.shields.io/github/release/mullholland/ansible-role-adguardhome_docker.svg)](https://github.com/mullholland/ansible-role-adguardhome_docker/releases/)|
## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/mullholland/ansible-role-adguardhome_docker/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true
  vars:
    adguardhome_docker_config:
      tls:
        options:
          modern:
            minVersion: "VersionTLS13"
            sniStrict: true

  roles:
    - role: "{{ lookup('env', 'MOLECULE_PROJECT_DIRECTORY') }}"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/mullholland/ansible-role-adguardhome_docker/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: true

  pre_tasks:
    - name: Install python3-docker (Debian/Ubuntu)
      ansible.builtin.package:
        name: "python3-docker"
      when:
        - ansible_facts['os_family'] == "Debian"

    - name: Install python3-packaging and python3-pip (RedHat)
      ansible.builtin.package:
        name:
          - "python3-packaging"
          - "python3-pip"
        state: present
      when:
        - ansible_facts['os_family'] == "RedHat"

    - name: Install docker via pip (RedHat)
      ansible.builtin.pip:
        name: "docker"
        state: present
      when:
        - ansible_facts['os_family'] == "RedHat"

  roles:
    - role: mullholland.docker
```


## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/mullholland/ansible-role-adguardhome_docker/blob/master/defaults/main.yml):

```yaml
---
# General config
adguardhome_docker_network_name: "web"
adguardhome_docker_base_path: "/opt"
adguardhome_docker_timezone: "Europe/Berlin"

# User/Group of the stack. Everything is mapped to this, instead of root.
adguardhome_docker_user: "homelab"
adguardhome_docker_uid: "900"
adguardhome_docker_group: "homelab"
adguardhome_docker_gid: "900"
adguardhome_docker_user_system: true

# which container version to install
# https://hub.docker.com/_/adguardhome/tags
# can also be latest
adguardhome_docker_version: "adguard/adguardhome:latest"

# additional docker compose environment varaibles
# https://github.com/felddy/foundryvtt-docker?tab=readme-ov-file#optional-variables
adguardhome_docker_environment_variables: []
adguardhome_docker_volumes:
  - "/opt/adguardhome/conf:/opt/adguardhome/conf"
  - "/opt/adguardhome/work:/opt/adguardhome/work"

# which port to expose. can be empty
adguardhome_docker_ports:
  - "53:53/tcp"      # (unencrypted) DNS
  - "53:53/udp"      # (unencrypted) DNS
  - "67:67/udp"      # DHCP
  - "68:68/udp"      # DHCP
  - "80:80/tcp"      # Admin-WebUI & DNS over HTTPS
  - "443:443/tcp"    # Admin-WebUI & DNS over HTTPS
  - "443:443/udp"    # Admin-WebUI & DNS over HTTPS
  - "3000:3000/tcp"  # First
  - "853:853/tcp"    # DNS over TLS
  - "853:853/udp"    # DNS over Quic
  - "784:784/udp"    # DNS over Quic
  - "8853:8853/udp"  # DNS over Quic
  - "5443:5443/tcp"  # DNSCrypt
  - "5443:5443/udp"  # DNSCrypt

adguardhome_docker_labels:
  - "traefik.enable=false"
# - "traefik.enable=true"
# - "traefik.http.routers.adguardhome.entryPoints=https"
# - "traefik.http.routers.adguardhome.rule=Host(`adguardhome.example.com`)"
# - "traefik.http.routers.adguardhome.tls.certResolver=letsEncrypt"
# - "traefik.http.routers.adguardhome.tls=true"
#  - "traefik.http.routers.adguardhome.service=adguardhome"
#  - "traefik.http.routers.adguardhome.loadbalancer.server.port=80"
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/mullholland/ansible-role-adguardhome_docker/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[mullholland.docker](https://galaxy.ansible.com/mullholland/docker)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-docker/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-docker/actions)|[![Build Status GitLab](https://gitlab.com/mullholland-github-mirror/ansible-role-docker/badges/master/pipeline.svg)](https://gitlab.com/mullholland-github-mirror/ansible-role-docker)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://mullholland.net) for further information.

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

|container|tags|
|---------|----|
|[Ubuntu](https://hub.docker.com/r/mullholland/ubuntu)|all|
|[Debian](https://hub.docker.com/r/mullholland/debian)|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The version before the previous version.
- The previous version.
- The current version.

If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-adguardhome_docker/issues).

## [License](#license)

[MIT](https://github.com/mullholland/ansible-role-adguardhome_docker/blob/master/LICENSE).

## [Author Information](#author-information)

[mullholland](https://mullholland.net)
