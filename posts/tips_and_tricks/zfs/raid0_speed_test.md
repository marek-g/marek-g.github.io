---
layout: page
title: ZFS Raid0 Speed Test
parent: ZFS
grand_parent: Tips & Tricks
---

# ZFS Raid0 Speed Test

I have 3 Hitachi 250GB HDD (SATA-II) disks connected.

# Summary

Tested on "Richard Burns Rally" game (2.56 GB, 1831 files)

NTFS,                                       read: 58 MB/s, write:  40 MB/s
NTFS + native compression,                  read: 27 MB/s, write:  77 MB/s, compression ratio: 1.49x
NTFS + LZX compression,                     read: 64 MB/s, write:   ? MB/s, compression ratio: 2.09x, recompression: 27 MB/s

ZFS,                                        read: 42 MB/s, write:  40 MB/s
ZFS + zstd-3 compression,                   read: 75 MB/s, write: 101 MB/s, compression ration: 2.05x
ZFS + RAID0 (2x HDD),                       read: 65 MB/s, write:  97 MB/s
ZFS + RAID0 (2x HDD) + zstd-3 compression,  read: 87 MB/s, write: 164 MB/s, compression ration: 2.05x
ZFS + RAID0 (3x HDD),                       read: 79 MB/s, write:  90 MB/s
ZFS + RAID0 (3x HDD) + zstd-3 compression,  read: 85 MB/s, write: 114 MB/s, compression ration: 2.05x

# Details

## Surface test (file system independent, partition read speed)

Windows, HD Tune - transfer rate - min: 33.3 MB/s, max: 70,5 MB/s, avg: 54,6 MB/s
Windows, HD Tune - access time - 19.7 ms
Windows, HD Tune - burst rate 157,1 MB/s

Linux, gnome-disks - transfer rate - min: 35 MB/s, max: 80 MB/s, avg: 61 MB/s
Linux, gnome-disks - access time - 18,58 ms

## NTFS test

Windows, Crystal Disk Mark - sequential read: 73 MB/s, sequential write: 71 MB/s
Windows, Crystal Disk Mark - random read: 0.74 MB/s, random write: 0.91 MB/s

Linux, kDiskMark - sequential read: 75 MB/s, sequential write: 74 MB/s
Linux, kDiskMark - random read: 0.72 MB/s, random write: 0.84 MB/s

Windows, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 58.3 MB/s, write: 40.3 MB/s
Linux, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 65.5 MB/s, write: 45.0 MB/s

## NTFS test - compression

Windows, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 27.3 MB/s, write: 77.1 MB/s, compression ratio: 1.49x

## NTFS test - compression LZX

Windows, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 63.9 MB/s, *recompress*: 27.3 MB/s, compression ratio: 2.09x

## ext4 test

Linux, kDiskMark - sequential read: 69 MB/s, sequential write: 68 MB/s
Linux, kDiskMark - random read: 0.68 MB/s, random write: 0.78 MB/s

Linux, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 46.8 MB/s, write: 38.5 MB/s

## fat32 test

Windows, Crystal Disk Mark - sequential read: 73 MB/s, sequential write: 71 MB/s
Windows, Crystal Disk Mark - random read: 0.74 MB/s, random write: 0.91 MB/s

Linux, kDiskMark - sequential read: 70 MB/s, sequential write: 70 MB/s
Linux, kDiskMark - random read: 0.75 MB/s, random write: 0.91 MB/s

Windows, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 65.5 MB/s, write: 38.5 MB/s
Linux, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 65.5 MB/s, write: 10.5 MB/s

## zfs - 1 drive, no compression

``` shell
sudo zpool create -f -o ashift=12 -o autotrim=on -O acltype=posixacl -O xattr=sa -O atime=off -O relatime=off -O recordsize=256k -O dnodesize=auto -O normalization=formD -O mountpoint=none -O canmount=off -O devices=off -O compression=off hddtest /dev/sdb1
sudo zfs create -o mountpoint=/tmp/test hddtest/test
sudo chown marek:marek /tmp/test
```

Linux, kDiskMark - sequential read: 6121 MB/s (??), sequential write: 40 MB/s
Linux, kDiskMark - random read: 157 MB/s (??), random write: 27 MB/s (??)

``` shell
cp -r "/tmp/Richard Burns Rally Portable" "/tmp/test" && sudo zpool export hddtest
sudo zpool import hddtest
cp -r "/tmp/test/Richard Burns Rally Portable" "/tmp/2"
```

Linux, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 42.0 MB/s, write: 40.3 MB/s

## zfs - 1 drive, zstd-3 compression

``` shell
sudo zpool destroy hddtest
sudo zpool create -f -o ashift=12 -o autotrim=on -O acltype=posixacl -O xattr=sa -O atime=off -O relatime=off -O recordsize=256k -O dnodesize=auto -O normalization=formD -O mountpoint=none -O canmount=off -O devices=off -O compression=zstd-3 hddtest /dev/sdb1
...
```

Linux, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 74.8 MB/s, write: 100.8 MB/s, compression ration: 2.05x

## zfs - 2 drives RAID0, no compression

``` shell
sudo zpool destroy hddtest
sudo zpool create -f -o ashift=12 -o autotrim=on -O acltype=posixacl -O xattr=sa -O atime=off -O relatime=off -O recordsize=256k -O dnodesize=auto -O normalization=formD -O mountpoint=none -O canmount=off -O devices=off -O compression=off hddtest /dev/sdb1 /dev/sdc1
...
```

Linux, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 64.9 MB/s, write: 97.0 MB/s

## zfs - 2 drives RAID0, zstd-3 compression

``` shell
sudo zpool destroy hddtest
sudo zpool create -f -o ashift=12 -o autotrim=on -O acltype=posixacl -O xattr=sa -O atime=off -O relatime=off -O recordsize=256k -O dnodesize=auto -O normalization=formD -O mountpoint=none -O canmount=off -O devices=off -O compression=zstd-3 hddtest /dev/sdb1 /dev/sdc1
...
```

Linux, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 87.4 MB/s, write: 163.8 MB/s

## zfs - 3 drives RAID0, no compression

``` shell
sudo zpool destroy hddtest
sudo zpool create -f -o ashift=12 -o autotrim=on -O acltype=posixacl -O xattr=sa -O atime=off -O relatime=off -O recordsize=256k -O dnodesize=auto -O normalization=formD -O mountpoint=none -O canmount=off -O devices=off -O compression=off hddtest /dev/sdb1 /dev/sdc1 /dev/sdd1
...
```

Linux, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 79.4 MB/s, write: 90.0 MB/s

## zfs - 3 drives RAID0, zstd-3 compression

``` shell
sudo zpool destroy hddtest
sudo zpool create -f -o ashift=12 -o autotrim=on -O acltype=posixacl -O xattr=sa -O atime=off -O relatime=off -O recordsize=256k -O dnodesize=auto -O normalization=formD -O mountpoint=none -O canmount=off -O devices=off -O compression=zstd-3 hddtest /dev/sdb1 /dev/sdc1 /dev/sdd1
...
```

Linux, copy of "Richard Burns Rally" game (2.56 GB, 1831 files) - read: 84.5 MB/s, write: 113.9 MB/s
