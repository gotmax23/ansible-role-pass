```yaml
pass_state: "present"
```

Options:

- `present` ensures that pass is installed

- `absent` ensures that pass is not installed. This does not remove build depdendenies to avoid removing packages that you installed prior to running this role.

``` yaml
pass_install_method: distro_package
```

Please see the `Available Installation Methods` section above

----

These variables only apply when `pass_install_method=source`.

``` yaml
pass_download_path: /opt/pass
```

Where to download and extract the Pass source tarball

``` yaml
pass_version: 1.7.4
```
Which version of Pass to instalt

``` yaml
pass_make_params:
  PREFIX: /usr/local
```

Parameters to pass to make
Please see the [installation instructions](https://git.zx2c4.com/password-store/tree/INSTALL) and [Makefile](https://git.zx2c4.com/password-store/tree/Makefile) in Pass's Git repository for all available parameters.
