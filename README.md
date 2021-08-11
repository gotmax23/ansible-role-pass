# Ansible Role: pass
[![MIT Licensed][badge-license]][link-license]
[![Role gotmax23.pass][badge-role]][link-galaxy]
[![Role Version][badge-version]][link-version]
[![Commits since last version][badge-commits-since]][link-commits-since]
[![Galaxy Role Quality][badge-quality]][link-galaxy]
[![Galaxy Role Downloads][badge-downloads]][link-galaxy]
[![Github Actions Molecule workflow status][badge-molecule-workflow]][link-molecule-workflow]
[![Github Actions Galaxy workflow status][badge-galaxy-workflow]][link-galaxy-workflow]

Ansible role to install password-store (pass), the standard Unix password manager.

## Requirements

For right now, I only test this role using the latest release of the `ansible` pip package, which includes all the collections that are no longer part of `ansible-core`. This is the supported method. However, if you choose to use `ansible-core` or still use Ansible 2.9, you must manually install the following collections:
- community.general

## Role Variables

Here are this role's variables and their default values, as set in [`defaults/main.yml`][link-defaults]. If you'd like, you may change them to customize this role's behavior.

``` yaml
---
# defaults file for pass
# Options:
# - `present` ensures that pass is installed
# - `absent` ensures that pass is not installed.
pass_state: "present"

```

## Example Playbook
``` yaml
---
- name: Install Pass
  hosts: all
  become: true

  tasks:
    - name: Install EPEL Repo
      when:
        - ansible_os_family == "RedHat"
        - ansible_distribution_major_version in [ "7", "8" ]
      ansible.builtin.include_role:
        name: geerlingguy.repo-epel

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
[link-version]: https://github.com/gotmax23/ansible-role-pass/releases
[badge-commits-since]: https://img.shields.io/github/commits-since/gotmax23/ansible-role-pass/latest.svg
[link-commits-since]: https://github.com/gotmax23/ansible-role-pass/commits/main
[badge-quality]: https://img.shields.io/ansible/quality/.svg
[badge-downloads]: https://img.shields.io/ansible/role/d/.svg
[badge-molecule-workflow]: https://github.com/gotmax23/ansible-role-pass/actions/workflows/molecule.yml/badge.svg?branch=main
[link-molecule-workflow]: https://github.com/gotmax23/ansible-role-pass/actions/workflows/molecule.yml
[badge-galaxy-workflow]: https://github.com/gotmax23/ansible-role-pass/actions/workflows/galaxy.yml/badge.svg
[link-galaxy-workflow]: https://github.com/gotmax23/ansible-role-pass/actions/workflows/galaxy.yml
[link-defaults]: https://github.com/gotmax23/ansible-role-pass/blob/main/defaults/main.yml
