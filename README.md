# Ansible Role: pass
[![Role gotmax23.pass][badge-role]][link-galaxy]
[![Github Repo][badge-github-repo]][link-github-repo]
[![SourceHut Repo][badge-srht-repo]][link-srht-repo]
[![MIT Licensed][badge-license]][link-license]
[![Github Open Issues][badge-github-issues]][link-github-issues]
[![Github Open PRs][badge-github-prs]][link-github-prs]
[![Role Version][badge-version]][link-version]
[![Commits since last version][badge-commits-since]][link-version]
[![Galaxy Role Quality][badge-quality]][link-galaxy]
[![Galaxy Role Downloads][badge-downloads]][link-galaxy]
[![Github Actions Molecule workflow status][badge-molecule-workflow]][link-molecule-workflow]
[![Github Actions Galaxy workflow status][badge-galaxy-workflow]][link-galaxy-workflow]

Ansible role that installs password-store (pass), the standard Unix password manager.

## Beta Warning
**This role is currently in beta and is not intended for production use. Breaking changes may occur between releases, so please make sure to read the release notes.**
## Requirements

If you are using Enterprise Linux (CentOS, Rocky Linux, Alamlinux, RHEL, etc.), you need to install the EPEL repository. You can use the [`robertdebock.epel`](https://github.com/robertdebock/ansible-role-epel) role to do so. See the [example playbook](#example-playbook) for a a full example.

This role depends on certain collections that are not included in ansible-core.

This role's [example playbook](#example-playbook) requires another role to prepare the target system.

To install this role's requirements, create a `requirements.yml` file with the contents below and run the following commands:

``` shell
# ansible-base/ansible-core 2.10 and above
ansible-galaxy install -r requirements.yml

# ansible 2.9
ansible-galaxy collection install -r requirements.yml
ansible-galaxy role install -r requirements.yml
```

``` yaml
# requirements.yml
---
roles:
  - name: robertdebock.epel
collections:
  - name: community.general

```

## Role Variables

### Available Installation Methods

This role allows you to choose which source to install Pass from. You may override the default installation method by setting `pass_install_method` to the one of the values outlined below.

#### `pass_install_method=distro_package`

**Description:** This installs Pass from the distribution's repositories. This version may be out of date.

**Supported Distributions:** All distributions supported by this role

**Default:** Yes

#### `pass_install_method=source`
**Description:** This installs Pass from source.

**Supported Distributions:** All distributions supported by this role

**Default:** No

----

Here are this role's variables and their default values, as set in [`defaults/main.yml`][link-defaults]. If you'd like, you may change them to customize this role's behavior.

``` yaml
---
# defaults file for pass
# Options:
# - `present` ensures that pass is installed
# - `absent` ensures that pass is not installed. This does not remove
# depdendenies to avoid removing packages that you installed prior to running
# this role.
pass_state: "present"

# Please see the `Available Installation Methods` section above
pass_install_method: distro_package

# Pass has the ability to copy passwords to your system clipboard using the `-c`
# option of the `pass show` command. By default, this role installs `xclip`, the
# clipbaord helper for Xorg. If you are using Wayland, change this value to
# `wl-clipboard`. If you would like to install both `xclip` and `wl-clipboard`,
# change this value to `both`.
pass_clipboard_helper: xclip

##########
# These Variables only apply when `pass_install_method=source`.

# Where to download and extract the Pass source tarball.
pass_download_path: /opt/pass

# Which version of Pass to install.
# When set to latest, this role will determine the latest version
# and install that.
pass_version: latest
# pass_version: 1.7.4

# Parameters to pass to make. Please see the [installation instructions][1] and
# [Makefile][2] in Pass's Git repository for all available parameters.
pass_make_params:
  # You can remove this value to install Pass to the default location, /usr/bin.
  # You are welcome to change it to any other value, as long as "$PREFIX/bin" is
  # in your system's PATH.
  PREFIX: /usr/local  # Install pass to /usr/local/bin

```

\[1]: https://git.zx2c4.com/password-store/tree/INSTALL

\[2]: https://git.zx2c4.com/password-store/tree/Makefile


## Example Playbook
``` yaml
---
- name: Install Pass
  hosts: all
  become: true

  tasks:
    - name: Update apt cache
      when: ansible_pkg_mgr == "apt"
      ansible.builtin.apt:
        update_cache: true
        cache_valid_time: 3600

    - name: Install EPEL Repo (will only run on EL-based distros)
      ansible.builtin.include_role:
        name: robertdebock.epel

    - name: Install Pass
      ansible.builtin.include_role:
        name: "gotmax23.pass"

```

## Compatibility
This role is compatible with the following distros:

|distro|versions|
|------|--------|
|Archlinux|any|
|Debian|buster, bullseye, bookworm|
|EL|8|
|Fedora|33, 34, 35|
|opensuse|15.2, 15.3, tumbleweed|
|Ubuntu|bionic, focal, hirsute|

## License
[MIT][link-license]

## Author
Maxwell G (@gotmax23)

[badge-role]: https://img.shields.io/ansible/role/56499.svg?logo=ansible
[link-galaxy]: https://galaxy.ansible.com/gotmax23/pass
[badge-github-repo]: https://img.shields.io/static/v1?label=GitHub&message=repo&color=lightblue&logo=github
[link-github-repo]: https://github.com/gotmax23/ansible-role-pass
[badge-srht-repo]: https://img.shields.io/static/v1?label=SourceHut&message=repo&color=lightgreen&logo=data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5vIj8+CjxzdmcKICAgdmlld0JveD0iMCAwIDUxMiA1MTIiCiAgIHZlcnNpb249IjEuMSIKICAgaWQ9InN2ZzUxIgogICBzb2RpcG9kaTpkb2NuYW1lPSJzb3VyY2VodXQtd2hpdGUuc3ZnIgogICBpbmtzY2FwZTp2ZXJzaW9uPSIxLjEgKGM2OGUyMmMzODcsIDIwMjEtMDUtMjMpIgogICB4bWxuczppbmtzY2FwZT0iaHR0cDovL3d3dy5pbmtzY2FwZS5vcmcvbmFtZXNwYWNlcy9pbmtzY2FwZSIKICAgeG1sbnM6c29kaXBvZGk9Imh0dHA6Ly9zb2RpcG9kaS5zb3VyY2Vmb3JnZS5uZXQvRFREL3NvZGlwb2RpLTAuZHRkIgogICB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciCiAgIHhtbG5zOnN2Zz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPgogIDxkZWZzCiAgICAgaWQ9ImRlZnM1NSIgLz4KICA8c29kaXBvZGk6bmFtZWR2aWV3CiAgICAgaWQ9Im5hbWVkdmlldzUzIgogICAgIHBhZ2Vjb2xvcj0iIzUwNTA1MCIKICAgICBib3JkZXJjb2xvcj0iI2ZmZmZmZiIKICAgICBib3JkZXJvcGFjaXR5PSIxIgogICAgIGlua3NjYXBlOnBhZ2VzaGFkb3c9IjAiCiAgICAgaW5rc2NhcGU6cGFnZW9wYWNpdHk9IjAiCiAgICAgaW5rc2NhcGU6cGFnZWNoZWNrZXJib2FyZD0iMSIKICAgICBzaG93Z3JpZD0iZmFsc2UiCiAgICAgaW5rc2NhcGU6em9vbT0iMS42NTQyOTY5IgogICAgIGlua3NjYXBlOmN4PSIyNTYiCiAgICAgaW5rc2NhcGU6Y3k9IjI1NiIKICAgICBpbmtzY2FwZTp3aW5kb3ctd2lkdGg9IjE5MjAiCiAgICAgaW5rc2NhcGU6d2luZG93LWhlaWdodD0iMTA1OSIKICAgICBpbmtzY2FwZTp3aW5kb3cteD0iMCIKICAgICBpbmtzY2FwZTp3aW5kb3cteT0iMCIKICAgICBpbmtzY2FwZTp3aW5kb3ctbWF4aW1pemVkPSIxIgogICAgIGlua3NjYXBlOmN1cnJlbnQtbGF5ZXI9InN2ZzUxIiAvPgogIDxwYXRoCiAgICAgZD0iTTI1NiA4QzExOSA4IDggMTE5IDggMjU2czExMSAyNDggMjQ4IDI0OCAyNDgtMTExIDI0OC0yNDhTMzkzIDggMjU2IDh6bTAgNDQ4Yy0xMTAuNSAwLTIwMC04OS41LTIwMC0yMDBTMTQ1LjUgNTYgMjU2IDU2czIwMCA4OS41IDIwMCAyMDAtODkuNSAyMDAtMjAwIDIwMHoiCiAgICAgaWQ9InBhdGg0OSIKICAgICBzdHlsZT0iZmlsbDojZmZmZmZmIiAvPgo8L3N2Zz4K
[link-srht-repo]: https://git.sr.ht/~gotmax23/ansible-role-pass
[badge-license]: https://img.shields.io/github/license/gotmax23/ansible-role-pass.svg?logo=github
[link-license]: https://github.com/gotmax23/ansible-role-pass/blob/main/LICENSE
[badge-github-issues]: https://img.shields.io/github/issues/gotmax23/ansible-role-pass.svg?logo=github
[link-github-issues]: https://github.com/gotmax23/ansible-role-pass/issues
[badge-github-prs]: https://img.shields.io/github/issues-pr/gotmax23/ansible-role-pass.svg?logo=github
[link-github-prs]: https://github.com/gotmax23/ansible-role-pass/pulls
[badge-version]: https://img.shields.io/github/release/gotmax23/ansible-role-pass.svg?logo=github
[link-version]: https://github.com/gotmax23/ansible-role-pass/releases/latest
[badge-commits-since]: https://img.shields.io/github/commits-since/gotmax23/ansible-role-pass/latest.svg?logo=github
[badge-quality]: https://img.shields.io/ansible/quality/56499.svg?logo=ansible
[badge-downloads]: https://img.shields.io/ansible/role/d/56499.svg?logo=ansible
[badge-molecule-workflow]: https://github.com/gotmax23/ansible-role-pass/actions/workflows/molecule.yml/badge.svg?branch=main
[link-molecule-workflow]: https://github.com/gotmax23/ansible-role-pass/actions/workflows/molecule.yml
[badge-galaxy-workflow]: https://github.com/gotmax23/ansible-role-pass/actions/workflows/galaxy.yml/badge.svg
[link-galaxy-workflow]: https://github.com/gotmax23/ansible-role-pass/actions/workflows/galaxy.yml
[link-defaults]: https://github.com/gotmax23/ansible-role-pass/blob/main/defaults/main.yml
