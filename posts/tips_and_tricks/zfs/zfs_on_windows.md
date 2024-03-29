---
layout: page
title: ZFS on Windows
parent: ZFS
grand_parent: Tips & Tricks
---

# Enable Test Mode

Enable Test Mode before installing unsigned driver:

```shell
bcdedit.exe -set testsigning on
```

and reboot.

# Install driver

Binary
https://github.com/openzfsonwindows/openzfs/releases

# Test the driver

```shell
zpool.exe status
```

Failure would be: `Unable to open \\.\ZFS: No error.`

Success would be: `No pools available`

# Locate disk name

```shell
wmic diskdrive list brief
```

# Create pool

```shell
zpool create -O casesensitivity=insensitive -o ashift=12 -O atime=off -O recordsize=256k -o feature@zilsaxattr=disabled -o feature@head_errlog=disabled -o feature@blake3=disabled -o feature@block_cloning=disabled -O compression=zstd-3 zfswin disk

zpool create -O casesensitivity=insensitive -o ashift=12 -O acltype=posixacl -O xattr=sa -O atime=off -O relatime=off -O recordsize=256k -O dnodesize=auto -O normalization=formD -o feature@zilsaxattr=disabled -o feature@head_errlog=disabled -o feature@blake3=disabled -o feature@block_cloning=disabled -O compression=zstd-3 zfswin disk
```

where disk is for example `PHYSICALDRIVE2`.

# Set drive letter

```shell
zfs set driveletter=Z zfswin
```

# Create dataset

Not needed! Default dataset zfswin, mountpoint=/zfswin is created

```shell
zfs create -o mountpoint=/ zfswin/root
```

# Importing pool

```shell
zpool import zfswin
```

# Exporting pool

```shell
zpool export zfswin
```


# ZFS created on Windows - features

zfswin  feature@async_destroy          enabled                        local
zfswin  feature@empty_bpobj            active                         local
zfswin  feature@lz4_compress           active                         local
zfswin  feature@multi_vdev_crash_dump  enabled                        local
zfswin  feature@spacemap_histogram     active                         local
zfswin  feature@enabled_txg            active                         local
zfswin  feature@hole_birth             active                         local
zfswin  feature@extensible_dataset     active                         local
zfswin  feature@embedded_data          active                         local
zfswin  feature@bookmarks              enabled                        local
zfswin  feature@filesystem_limits      enabled                        local
zfswin  feature@large_blocks           active                         local
zfswin  feature@large_dnode            active                         local
zfswin  feature@sha512                 enabled                        local
zfswin  feature@skein                  enabled                        local
zfswin  feature@edonr                  enabled                        local
zfswin  feature@userobj_accounting     active                         local
zfswin  feature@encryption             enabled                        local
zfswin  feature@project_quota          active                         local
zfswin  feature@device_removal         enabled                        local
zfswin  feature@obsolete_counts        enabled                        local
zfswin  feature@zpool_checkpoint       enabled                        local
zfswin  feature@spacemap_v2            active                         local
zfswin  feature@allocation_classes     enabled                        local
zfswin  feature@resilver_defer         enabled                        local
zfswin  feature@bookmark_v2            enabled                        local
zfswin  feature@redaction_bookmarks    enabled                        local
zfswin  feature@redacted_datasets      enabled                        local
zfswin  feature@bookmark_written       enabled                        local
zfswin  feature@log_spacemap           active                         local
zfswin  feature@livelist               enabled                        local
zfswin  feature@device_rebuild         enabled                        local
zfswin  feature@zstd_compress          active                         local
zfswin  feature@draid                  enabled                        local

zfswin  feature@zilsaxattr             enabled                        local    <- not compatible with Linux or from newer version
zfswin  feature@head_errlog            disabled                       local    <- not compatible with Linux or from newer version
zfswin  feature@blake3                 enabled                        local    <- not compatible with Linux or from newer version
zfswin  feature@block_cloning          enabled                        local    <- not compatible with Linux or from newer version

# Found problems

- `TrackMania` game throws file access error
