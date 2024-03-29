---
layout: page
title: Migrate Linux to ZFS
parent: ZFS
grand_parent: Tips & Tricks
---

# Migrate Linux to ZFS

Some steps I have followed to run Manjaro Linux from ZFS partition.

## Backup system

Make a backup of the system for example with `bsdtar` command as described on Backups pages. One important thing is to make sure that system has installed `linuxNN-zfs` and `zfs-utlis` modules.

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

Created partition with type = 0xBF00 (Solaris Root). To change partition type you can use fdisk. Solaris root partition type in fdisk is 156. You can add it a label, for example "Linux" for easier identification during filesystem import.

## Create ZFS pool on the partition

```sh
sudo zpool create -f -o ashift=12 -o autotrim=on -O acltype=posixacl -O xattr=sa -O atime=off -O relatime=off -O recordsize=256k -O dnodesize=disabled -O normalization=formD -O mountpoint=none -O canmount=off -O devices=off -O compression=zstd-3 ssd /dev/disk/by-partlabel/Linux
```

Please note! As of 2023-11-11 I have found a bug in `openzfs` that `dnodesize=auto` makes it impossible to reveive incremental backup, so I changed it form `auto` to `disabled`. If you have it enabled you can still take care and set `-o dnodesize=legacy` during creating datasets. It's enough to set it to the root dataset, as the children datasets will inherit the setting.

Create containers:

```sh
sudo zfs create -o canmount=off -o mountpoint=none ssd/manjaro
```

Create datasets

```sh
sudo zfs create -o canmount=noauto -o mountpoint=/ ssd/manjaro/root
sudo zfs create -o mountpoint=/home ssd/manjaro/home
```

Verify:

```sh
zfs list
```

## Mount ZFS filesystems

```sh
sudo zfs umount -a
sudo zpool export ssd

sudo zpool import -d /dev/disk/by-partlabel/Linux -R /mnt ssd
sudo zfs mount ssd/manjaro/root
```

## Copy system files

## Update system configuration

Information to boot loaders where to find root file system:

```sh
sudo zpool set bootfs=ssd/manjaro/root ssd
```

Copy the zpool cache file:

```sh
sudo zpool set cachefile=/etc/zfs/zpool.cache ssd

sudo mkdir -p /mnt/etc/zfs
sudo cp /etc/zfs/zpool.cache /mnt/etc/zfs/zpool.cache
```

Chroot to the system:

```sh
sudo chroot /mnt

mount -t proc proc /proc
mount -t sysfs sys /sys
mount -t devtmpfs udev /dev
```

Edit `/etc/mkinitcpio.conf` file and add `zfs` to `HOOKS` before `filesystems` and move `keyboard` hook before `zfs`. If you are not using `ext4` you can remove `fsck`, for example:

```
HOOKS=(base udev autodetect modconf block keyboard zfs filesystems)
```

Regenerate initramfs:

```sh
mkinitcpio -P
```

Comment out old root and home partitions from `/etc/fstab`.

Enable necessary services for automatically mounting zfs datasets:

```sh
sudo systemctl enable zfs.target
sudo systemctl enable zfs-import-cache
sudo systemctl enable zfs-mount
sudo systemctl enable zfs-import.target
```

Exit from chroot:

```sh
umount /dev
umount /sys
umount /proc
exit
```

## Unexport ZFS pool

```sh
sudo zfs umount ssd/manjaro/root
sudo zpool export ssd
```

## Reboot

Now the system should be bootable from `ZFSBootMenu`.

If after boot the filesystem is readonly, you can load it by adding `rw` to the kernel command line (`CTRL+E` in `ZFSBootMenu`). After the system is loaded you can set it as persistent with:

- call `cat /proc/cmdline` to see what's the current command line is, for example: `zfs=ssd/manjaro/root nvidia-drm.modeset=1 spl.spl_hostid=0x00bab10c`
- call `sudo zfs set org.zfsbootmenu:commandline="<OLD_COMMAND_LINE_WITHOUT_ZFS_PART> rw" ssd/manjaro/root`, for example: `sudo zfs set org.zfsbootmenu:commandline="nvidia-drm.modeset=1 spl.spl_hostid=0x00bab10c rw" ssd/manjaro/root`

You can set this property on the parent dataset (`ssd/manjaro`) and `inherit` it for `ssd/manjaro/root` (please verify if the property is set for it, if it is just unset it with `inherit` command).

