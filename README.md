# [mount](#mount)

Configure mounts

|Travis|GitHub|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-mount.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-mount)|[![github](https://github.com/robertdebock/ansible-role-mount/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-mount/actions)|[![quality](https://img.shields.io/ansible/quality/51485)](https://galaxy.ansible.com/robertdebock/mount)|[![downloads](https://img.shields.io/ansible/role/d/51485)](https://galaxy.ansible.com/robertdebock/mount)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-mount.svg)](https://github.com/robertdebock/ansible-role-mount/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.mount
      mount_requests:
        - path: /mnt/tmp
          src: /tmp
          opts: bind
          state: mounted
          fstype: none
```

The machine may need to be prepared using `molecule/resources/prepare.yml`:
```yaml
---
- name: prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.bootstrap
```

For verification `molecule/resources/verify.yml` runs after the role has been applied.
```yaml
---
- name: Verify
  hosts: all
  become: yes
  gather_facts: no

  tasks:
    - name: check directory
      stat:
        path: /mnt/tmp
      register: mount_check_directory
      failed_when:
        - not mount_check_directory.stat.exists

    - name: place some file
      file:
        path: /mnt/tmp/some_file.txt
        state: touch

    - name: unmount
      mount:
        path: /mnt/tmp
        state: unmounted

    - name: check if some file is gone
      stat:
        path: /mnt/tmp/some_file.txt
      register: mount_check_some_file
      failed_when:
        - mount_check_some_file.stat.exists

    - name: revert to mounted state before verify
      mount:
        path: /mnt/tmp
        src: /tmp
        opts: bind
        fstype: none
        state: mounted
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for mount

# You can define mounts as variables. All parameters for the `mount` module are
# supported.

# mount_requests:
#   - path: /mnt/tmp
#     src: /tmp
#     opts: bind
#     state: mounted
#     fstype: none
```

## [Requirements](#requirements)

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap

```

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/mount.png "Dependency")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|alpine|all|
|debian|all|
|el|7, 8|
|fedora|all|
|opensuse|all|
|ubuntu|all|

The minimum version of Ansible required is 2.8, tests have been done to:

- The previous version.
- The current version.
- The development version.



## [Testing](#testing)

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-mount) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-mount/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## [License](#license)

Apache-2.0


## [Author Information](#author-information)

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
