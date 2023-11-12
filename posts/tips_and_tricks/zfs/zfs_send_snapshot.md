---
layout: page
title: ZFS Send Snapshot
parent: ZFS
grand_parent: Tips & Tricks
---

# ZFS Send Snapshot

## Send first copy to laptop

To send first copy of the snapshots to laptop (assuming the drive is connected) - the destination filesystems should not exist:

``` shell
sudo zfs send -w ssd/manjaro/root@2023-06-20_laptop | sudo zfs recv laptop/manjaro/root
sudo zfs send -w ssd/manjaro/home@2023-06-20_laptop | sudo zfs recv laptop/manjaro/home
sudo zfs send -w ssd/encrypted/ext@2023-06-20_laptop | sudo zfs recv laptop/encrypted/ext
```

- `-w` - send raw blocks (do not decompress / decrypt), do not use if you want to recompress / encrypt with different key

Set mount points:

``` shell
sudo zfs set canmount=noauto laptop/manjaro/root
sudo zfs set mountpoint=/ laptop/manjaro/root
sudo zfs set mountpoint=/home laptop/manjaro/hom
sudo zfs set canmount=noauto laptop/encrypted/ext
sudo zfs set mountpoint=/home/marek/Ext laptop/encrypted/ext
```

## Send incremental difference between snapshots

Please note! As of 2023-11-11 I have found a bug in `openzfs` that `dnodesize=auto` makes it impossible to reveive incremental backup, so I changed it form `auto` to `disabled`. If you have it enabled you can still take care and set `-o dnodesize=legacy` during creating datasets. It's enough to set it to the root dataset, as the children datasets will inherit the setting.

To export difference between 2 snaphots to a file:

```shell
sudo zfs send -wI @2023-07-06_laptop laptop/manjaro/home@2023-11-11_desktop >home_snap
```

- `-w` - send raw blocks (do not decompress / decrypt), do not use if you want to recompress / encrypt with different key
- `-I` - include intermediary snapshots

To import the incremental snapshot:

``` shell
sudo zfs recv -F ssd/manjaro/home <home_snap
```

- `-F` - force rollback to the most recent snapshot before performing receive operation
