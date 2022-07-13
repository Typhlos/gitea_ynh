<!--
N.B.: This README was automatically generated by https://github.com/YunoHost/apps/tree/master/tools/README-generator
It shall NOT be edited by hand.
-->

# Gitea for YunoHost

[![Integration level](https://dash.yunohost.org/integration/gitea.svg)](https://dash.yunohost.org/appci/app/gitea) ![Working status](https://ci-apps.yunohost.org/ci/badges/gitea.status.svg) ![Maintenance status](https://ci-apps.yunohost.org/ci/badges/gitea.maintain.svg)  
[![Install Gitea with YunoHost](https://install-app.yunohost.org/install-with-yunohost.svg)](https://install-app.yunohost.org/?app=gitea)

*[Lire ce readme en français.](./README_fr.md)*

> *This package allows you to install Gitea quickly and simply on a YunoHost server.
If you don't have YunoHost, please consult [the guide](https://yunohost.org/#/install) to learn how to install it.*

## Overview

Gitea is a fork of Gogs a self-hosted Git service written in Go. Alternative to GitHub.


**Shipped version:** 1.16.9~ynh1

## Screenshots

![Screenshot of Gitea](./doc/screenshots/screenshot.png)

## Disclaimers / important information

## Additional informations

### Notes on SSH usage

If you want to use Gitea with SSH and be able to pull/push with your SSH key, your SSH daemon must be properly configured to use private/public keys. Here is a sample configuration `/etc/ssh/sshd_config` that works with Gitea:

```bash
PubkeyAuthentication yes
AuthorizedKeysFile /home/yunohost.app/%u/.ssh/authorized_keys
ChallengeResponseAuthentication no
PasswordAuthentication no
UsePAM no
```

You must also add your public key to your Gitea profile.

When using SSH on any port other than 22, you need to add these lines to your SSH configuration `~/.ssh/config`:

```bash
Host domain.tld
    port 2222 # change this with the port you use
```

### Upgrade

By default, a backup is performed before upgrading. To avoid this, you have the following options:
- Pass the `NO_BACKUP_UPGRADE` env variable with `1` at each upgrade. For example `NO_BACKUP_UPGRADE=1 yunohost app upgrade gitea`.
- Set `disable_backup_before_upgrade` to `1`. You can set it with this command:

`yunohost app setting gitea disable_backup_before_upgrade -v 1`

After that, the settings will be applied for **all** the next updates.

From command line:

`yunohost app upgrade gitea`

### Backup

This application now uses the core-only feature of the backup. To keep the integrity of the data and to have a better guarantee of the restoration it is recommended to proceed as follows:

- Stop Gitea service with this command:

`systemctl stop gitea.service`

- Launch Gitea backup with this command:

`yunohost backup create --app gitea`

- Backup your data with your specific strategy (could be with rsync, borg backup or just cp). The data is generally stored in `/home/yunohost.app/gitea`.
- Restart Gitea service with theses command:

`systemctl start gitea.service`

### Remove

Due of the backup core only feature the data directory in `/home/yunohost.app/gitea` **is not removed**. It must be manually deleted to purge user data from the app.

### LFS setup
To use a repository with an `LFS` setup, you need to activate it on `/opt/gitea/custom/conf/app.ini`

```ini
[server]
LFS_START_SERVER = true
LFS_HTTP_AUTH_EXPIRY = 20m
```
By default, NGINX is configured with a maximum value for uploading files at 200 MB. It's possible to change this value on `/etc/nginx/conf.d/my.domain.tld.d/gitea.conf`.
```
client_max_body_size 200M;
```
Don't forget to restart Gitea `sudo systemctl restart gitea.service`.

> These settings are restored to the default configuration when updating Gitea. Remember to restore your configuration after all updates.

### Git command access with HTTPS

If you want to use the Git command (like `git clone`, `git pull`, `git push`), you need to set this app as **public**.

## Documentation and resources

* Official app website: <https://gitea.io/>
* Official admin documentation: <https://docs.gitea.io/>
* Upstream app code repository: <https://github.com/go-gitea/gitea>
* YunoHost documentation for this app: <https://yunohost.org/app_gitea>
* Report a bug: <https://github.com/YunoHost-Apps/gitea_ynh/issues>

## Developer info

Please send your pull request to the [testing branch](https://github.com/YunoHost-Apps/gitea_ynh/tree/testing).

To try the testing branch, please proceed like that.

``` bash
sudo yunohost app install https://github.com/YunoHost-Apps/gitea_ynh/tree/testing --debug
or
sudo yunohost app upgrade gitea -u https://github.com/YunoHost-Apps/gitea_ynh/tree/testing --debug
```

**More info regarding app packaging:** <https://yunohost.org/packaging_apps>
