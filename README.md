haxwithaxe.certdeploy_client
============================

Install and configure the CertDeploy client as a systemd service.

The plan is to make this work with sysvinit as well to support Alpine.


Requirements
------------

Requires pipx to already be installed. This also depends on systemd for the moment.


Role Variables
--------------

- `certdeploy_client_config` - The CertDeploy client config as described [here](https://certdeploy.readthedocs.io/en/latest/readme.html#client-settings).
  - The following are required keys.
    - `destination` - The place where the CertDeploy client stores it's certs for sharing with other applications.
    - `update_services` - This is a list of [service definitions](https://certdeploy.readthedocs.io/en/latest/readme.html#service-definitions).
  - The following are set by default or as part of the process of assembling the config and can be overridden by providing them in this `dict`.
    - `source` is set to `certdeploy_client_config_source`.
    - `sftpd.server_pubkey` is set to `certdeploy_client_config_sftpd_server_pubkey` or `certdeploy_client_server_pubkey`.

    - `sftpd.listen_port` is set to `certdeploy_client_config_sftpd_listen_port` or `certdeploy_client_listen_port`.
    - `privkey_filename` is set to `{{ certdeploy_config_path }}/client_key`.
- `certdeploy_client_config_sftpd_server_pubkey` - The server's pubkey. Alias `certdeploy_client_server_pubkey`.
- `certdeploy_client_privkey` - The private key text. It should be encrypted or read from some other source at runtime.
- `certdeploy_client_config_dir` (optional) - The CertDeploy config path. Defaults to ``/etc/certdeploy``.
- `certdeploy_client_config_source` (optional) - The staging directory where certdeploy-client puts incoming certs. Defaults to ``/var/cache/certdeploy``.
- `certdeploy_client_config_sftpd_listen_port` - The port the SFTP server listens on. Defaults to ``33774``. Alias `certdeploy_client_listen_port`.
- `certdeploy_client_directory_mode` (optional) - The directory permissions for CertDeploy related directories. Defaults to ``u=rwX,g=,o=``.
- `certdeploy_client_uid` (optional) - The user to run certdeploy-client as and own all the certdeploy related directories. Defaults to ``root``.
- `certdeploy_client_gid` (optional) - The group to run certdeploy-client as and own all the certdeploy related directories. Defaults to ``root``.
- `certdeploy_client_install_lib_systemd_system` (optional) - The directory where the systemd service file will be installed. Any directory where systemd will discover the ``certdeploy-client.service`` file will do. Defaults to ``/usr/local/lib/systemd/system``.
- `certdeploy_client_install_pipx_bin_dir` (optional) - The directory in which pipx will install package entrypoints. Corresponds to the `PIPX_BIN_DIR` environment variable. Defaults to ``/usr/local/bin``.
- `certdeploy_client_install_pipx_home` (optional) - The home directory for pipx to operate in. Corresponds to the `PIPX_HOME` environment variable. Defaults to ``/opt/pipx``.
- `certdeploy_client_mode` (optional) - The permissions for the files in certdeploy related directories. Defaults to ``0600``. Currently this must be given in octal.
- `certdeploy_client_service_file_group` (optional) - The owner of ``certdeploy-client.service``. Defaults to ``root``.
- `certdeploy_client_service_file_owner` (optional) - The group of ``certdeploy-client.service``. Defaults to ``root``.


Dependencies
------------

None.


Example Playbook
----------------

Using all defaults and reloading the nginx server install on the system:

    - hosts: servers
      roles:
         - role: haxwithaxe.certdeploy_client
           vars:
              certdeploy_client_config:
                update_services:
                  - type: systemd
                    name: nginx.service
                    action: reload
              certdeploy_client_server_pubkey: <the CertDeploy server's public key>
              certdeploy_client_privkey: !vault |
                <the vault encrypted CertDeploy client's private key>


License
-------

GPLv3


Author Information
------------------

Created by [haxwithaxe](https://github.com/haxwithaxe)
