---
layout: page
title: ZFS
parent: Tips & Tricks
---

# ZFS

Some steps I have followed to run Manjaro Linux from ZFS partition.

## Create Manjaro Live CD with zfs support

More info: https://wiki.manjaro.org/index.php/Build_Manjaro_ISOs_with_buildiso

1. Clone iso-profiles:

   ```sh
   git clone https://gitlab.manjaro.org/profiles-and-settings/iso-profiles.git ~/iso-profiles
   ```

2. Add required components to `manjaro/kde/Packages-Live` (for me it was not enough to add it to `Packages-Desktop`, but to be sure add it there, too):

   ```
   KERNEL-zfs
   zfs-utils
   gparted
   ```

3. Create iso with:

   ```sh
   buildiso -p -f kde
   ```

4. Burn ISO to USB with:

   ```sh
   sudo dd bs=4M if=/var/cache/manjaro-tools/iso/manjaro/kde/<version</manjaro-kde-<version>.iso of=/dev/<usb-drive> status=progress oflag=sync
   ```

## Install EFI boot loaders

Useful EFI boot loaders: rEFInd (to choose between Windows & Linux), ZFSBootMenu (to easily boot Linux from ZFS pool).

## Partitions

Created partition with type = 0xBF00 (Solaris Root). To change partition type you can use fdisk. Solaris root partition type in fdisk is 156.

## Create ZFS pool on the partition

```sh
sudo zpool create -f -o ashift=12 -o autotrim=on -O acltype=posixacl -O xattr=sa -O atime=off -O relatime=off -O recordsize=256k -O dnodesize=auto -O normalization=formD -O mountpoint=none -O canmount=off -O devices=off -O compression=zstd-3 rpool_ssd /dev/disk/by-id/nvme-WDC_WDS100T2B0C-00PXH0_2041D4801869-part5
```
Create containers:

```sh
sudo zfs create -o canmount=off -o mountpoint=none rpool_ssd/manjaro
```

Create datasets

```sh
sudo zfs create -o canmount=noauto -o mountpoint=/ rpool_ssd/manjaro/root
sudo zfs create -o mountpoint=/home rpool_ssd/manjaro/home
```

Verify:

```sh
zfs list
```

## Mount ZFS filesystems

```sh
sudo zfs umount -a
sudo zpool export rpool_ssd

sudo zpool import -d /dev/disk/by-id/nvme-WDC_WDS100T2B0C-00PXH0_2041D4801869-part5 -R /mnt rpool_ssd
sudo zfs mount rpool_ssd/manjaro/root
sudo zfs mount rpool_ssd/manjaro/home
```

## Copy system files


