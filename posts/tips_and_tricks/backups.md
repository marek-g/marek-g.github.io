---
layout: page
title: Backups
parent: Tips & Tricks
---

# Backups

## Archive system files to a file

When creating full system backup it is better to use `bsdtar` than `tar`. `tar` with `--xattrs` will not preserve extended properties.

1. Boot from live USB.

2. Mount source and destination file systems:

    ```sh
    fdisk -l

    mkdir root
    sudo mount </dev/nvme0n1p5> root

    mkdir backup
    sudo mount </dev/sda4> backup
    ```

3. CD to source file system:

    ```sh
    cd root
    ```

4. Create backup:

    ```sh
    sudo bsdtar --acls --xattrs -cpvf ../backup/<archive_name>.tar
    ```

5. Restore backup:

    ```sh
    sudo bsdtar --acls --xattrs -xpvf ../backup/<archive_name>.tar
    ```

## Archive user files

To archive user files and folders on Linux (`tar` stores _owner/group_ of the file):

```sh
tar -cf archive.tar -C <directory> .
```

or

```sh
tar -cf archive.tar <directory>
```

where `-C <path>` (change folder) can be used to not store full path names when whole mount point from some other place, for example:

```sh
tar -cf 2021-08-18_listingi.tar -C /media/truecrypt1 .
```

## Compress archive with password

Compression:

```sh
7z a -p -mhe=on archive.tar.7z archive.tar
```

and `-mhe=on` is for enabling header encryption (cannot list file names).

Decompression:

```sh
7z x archive.tar.7z
```
