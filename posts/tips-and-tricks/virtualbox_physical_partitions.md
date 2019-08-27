---
layout: page
title: VirtualBox Physical Partitions
---

# VirtualBox Physical Partitions

```bash
sudo su

fdisk -l # looking for EFI and Windows partition - for me 1 and 5 of sdb 

# create disk image pointing to real partitions
VBoxManage internalcommands createrawvmdk -filename /home/borto/sdb.vmdk -rawdisk /dev/sdb -partitions 1,3 -relative

# give user access to the partitions
setfacl -m u:yourusername:rw /dev/sdb1
setfacl -m u:yourusername:rw /dev/sdb5
```

Enable EFI in VM.

You can also use the same system and disk UUIDs if your Windows Activation needs it:
https://superuser.com/questions/1171524/possible-to-dual-boot-and-virtualize-same-physical-drive-containing-windows-10
