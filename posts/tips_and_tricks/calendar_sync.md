---
layout: page
title: Calendar Synchronization
parent: Tips & Tricks
---

# Calendar Synchronization

How to setup CalDev server on PC to store calendars & contacts.

## Install Radicale

[Radicale](https://radicale.org/) is CalDAV (calendars, to-do lists) and CalDAV (contacts) server. Instructions for ArchLinux: [ArchLinux wiki](https://wiki.archlinux.org/title/Radicale).

On ArchLinux the data is stored by default at `/var/lib/radicale` and the configuration file is `/etc/radicale/config`. The service is created system wide. I changed it to run it as a user, so I can keep the data in the home folder. For configurations with multiple users it's better to leave it system wide.

### Enable and run the service

If you want to keep it system wide, enable and run the service:

``` shell
sudo systemctl enable radicale
sudo systemctl start radicale
```

If you want to run it as a user, follow the instructions from Radicale page: [Linux with systemd as a user](https://radicale.org/v3.html#linux-with-systemd-as-a-user). Copy and modify `config`, `rights`, `user` files from `/etc/radicale`. To manage service:

``` shell
# Enable the service
systemctl --user enable radicale
# Start the service
systemctl --user start radicale
# Check the status of the service
systemctl --user status radicale
# View all log messages
journalctl --user --unit radicale.service
```

### Restrict access

Install `apache-tools` from AUR. Run `htpasswd -c <usersfile> <username>`. Provide password. Append generated line from `usersfile` to `users` file. 

In the `config` file configure:

``` ini
[auth]
type = htpasswd
htpasswd_filename = <path to users file>
# encryption method used in the htpasswd file
htpasswd_encryption = md5
```

### Enable SSL

Generate certificate: `openssl req -x509 -newkey rsa:4096 -keyout radicale.key.pem -out radicale.cert.pem -nodes -days 99999`. Use host name (eg. `192.168.0.20` as `Common Name`. Copy files to `/etc/ssl` folder in the case of system wide installation (in the case of user installation it can be `~/.config/radicale` folder). In the case of system wide installation restrict key file access to `radicale` user (cert can be read by all).

Such generated certificate is still not perfect - Firefox complains, but we can live with it (DAVx5 on Android and Akonadi on Linux both work with it).

Add certificate to trusted. On ArchLinux:

``` shell
sudo trust anchor /etc/ssl/radicale.cert.pem
sudo update-ca-trust
```

In the config file configure:

``` ini
[server]
hosts = 0.0.0.0:5232
ssl = True
certificate = /etc/ssl/radicale.cert.pem
key = /etc/ssl/radicale.key.pem
```

### Access radicale URL and add some calendars

Default URL: https://localhost:5232
