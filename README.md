# Ansible Role: pass
[![MIT Licensed][badge-license]][link-license]
[![Role gotmax23.pass][badge-role]][link-galaxy]
[![Role Version][badge-version]][link-version]
[![Commits since last version][badge-commits-since]][link-version]
[![Galaxy Role Quality][badge-quality]][link-galaxy]
[![Galaxy Role Downloads][badge-downloads]][link-galaxy]
[![Github Actions Molecule workflow status][badge-molecule-workflow]][link-molecule-workflow]
[![Github Actions Galaxy workflow status][badge-galaxy-workflow]][link-galaxy-workflow]

Ansible role that installs password-store (pass), the standard Unix password manager.

## Requirements

For right now, I only test this role using the latest release of the `ansible` pip package, which includes all the collections that are no longer part of `ansible-core`. This is the supported method. However, if you choose to use `ansible-core` or still use Ansible 2.9, you must manually install the following collections:
- community.general

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
  PREFIX: /usr/local

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
|Debian|buster, bullseye|
|EL|8|
|Fedora|33, 34, 35|
|opensuse|15.2, 15.3, tumbleweed|
|Ubuntu|bionic, focal, hirsute|

## License
[MIT][link-license]

## Author
Maxwell G (@gotmax23)

[badge-license]: https://img.shields.io/github/license/gotmax23/ansible-role-pass.svg
[link-license]: https://github.com/gotmax23/ansible-role-pass/blob/main/LICENSE
[badge-role]: https://img.shields.io/ansible/role/.svg
[link-galaxy]: https://galaxy.ansible.com/gotmax23/pass
[badge-version]: https://img.shields.io/github/release/gotmax23/ansible-role-pass.svg
[link-version]: https://github.com/gotmax23/ansible-role-pass/releases/latest
[badge-commits-since]: https://img.shields.io/github/commits-since/gotmax23/ansible-role-pass/latest.svg
[badge-quality]: https://img.shields.io/ansible/quality/.svg
[badge-downloads]: https://img.shields.io/ansible/role/d/.svg
[badge-molecule-workflow]: https://github.com/gotmax23/ansible-role-pass/actions/workflows/molecule.yml/badge.svg?branch=main
[link-molecule-workflow]: https://github.com/gotmax23/ansible-role-pass/actions/workflows/molecule.yml
[badge-galaxy-workflow]: https://github.com/gotmax23/ansible-role-pass/actions/workflows/galaxy.yml/badge.svg
[link-galaxy-workflow]: https://github.com/gotmax23/ansible-role-pass/actions/workflows/galaxy.yml
[link-defaults]: https://github.com/gotmax23/ansible-role-pass/blob/main/defaults/main.yml
