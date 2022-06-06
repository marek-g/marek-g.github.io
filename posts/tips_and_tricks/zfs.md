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
