---
layout: page
title: ZFS Encryption
parent: ZFS
grand_parent: Tips & Tricks
---

# ZFS Encryption

## Create

To create new dataset to store encrypted filesystems:

```sh
sudo zfs create -o canmount=off -o mountpoint=none -o encryption=on -o keylocation=prompt -o keyformat=passphrase rpool_ssd/encrypted
```

Create encrypted child filesystem:

```sh
sudo zfs create -o canmount=noauto -o mountpoint=/home/marek/Ext rpool_ssd/encrypted/ext
```

## Mount

```sh
sudo zfs load-key -r rpool_ssd/encrypted
sudo zfs mount rpool_ssd/encrypted/ext
```

## Unmount

```sh
sudo zfs umount rpool_ssd/encrypted/ext
sudo zfs unload-key -r rpool_ssd/encrypted
```

## Set folder permissions after mount

```sh
sudo chmod -R 0700 /home/marek/Ext
sudo chown marek:marek /home/marek/Ext
```
