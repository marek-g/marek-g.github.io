---
layout: page
title: Calendar Synchronization
parent: Tips & Tricks
---

# Calendar Synchronization

How to setup CalDev server on PC to store calendars & contacts.

## Install Radicale

[Radicale](https://radicale.org/) is CalDAV (calendars, to-do lists) and CalDAV (contacts) server. Instructions for ArchLinux: [ArchLinux wiki](https://wiki.archlinux.org/title/Radicale).


On ArchLinux the data is stored by default at `/var/lib/radicale` and the configuration file is `/etc/radicale/config`. To change it, please read ArchLinux wiki.

Enable and run the service:

``` shell
systemctl enable radicale
systemctl start radicale
```

Default URL: http://localhost:5232

### Restrict access

Install `apache-tools` from AUR. Run `htpasswd -c <usersfile> <username>`. Provide password. Append generated line from `usersfile` to `/etc/radicale/users` file. 

In the config file configure:

``` ini
[auth]
type = htpasswd
htpasswd_filename = /etc/radicale/users
# encryption method used in the htpasswd file
htpasswd_encryption = md5
```

### Enable SSL

Generate certificate: `openssl req -x509 -newkey rsa:4096 -keyout radicale.key.pem -out radicale.cert.pem -nodes -days 99999`. Use host name (eg. `192.168.0.20` as `Common Name`. Copy files to `/etc/ssl` folder. Restrict key file access to `radicale` user (cert can be read by all).

Add certificate to trusted. On ArchLinux: `sudo trust anchor /etc/ssl/radicale.cert.pem`.

In the config file configure:

``` ini
[server]
hosts = 0.0.0.0:5232
ssl = True
certificate = /etc/ssl/radicale.cert.pem
key = /etc/ssl/radicale.key.pem
```
