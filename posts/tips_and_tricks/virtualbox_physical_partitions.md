---
layout: page
title: VirtualBox Physical Partitions
parent: Tips & Tricks
---

# VirtualBox Physical Partitions

```bash
sudo su

fdisk -l # looking for EFI and Windows partition - for me 1 and 5 of sdb 

# create disk image pointing to real partitions
VBoxManage internalcommands createrawvmdk -filename /home/marek/sdb.vmdk -rawdisk /dev/sdb -partitions 1,5 -relative

# give user access to the partitions
setfacl -m u:marek:rw /dev/sdb1
setfacl -m u:marek:rw /dev/sdb5
```

## To run `setfacl` command during boot with systemd:

Create script `/usr/local/bin/virtualbox-partitions.sh`:

```bash
#!/bin/bash

setfacl -m u:marek:rw /dev/sdb1
setfacl -m u:marek:rw /dev/sdb5
```

Set executable attribute:

```bash
chmod 744 /usr/local/bin/virtualbox-partitions.sh
```

Create service file: /etc/systemd/system/virtualbox-partitions.service:

```systemd
[Service]
ExecStart=/usr/local/bin/virtualbox-partitions.sh

[Install]
WantedBy=default.target
```

Register and run service:

```bash
systemctl enable virtualbox-partitions.service
systemctl start virtualbox-partitions.service
```

## Other

Enable EFI in VM.

You can also use the same system and disk UUIDs if your Windows Activation needs it (https://superuser.com/questions/1171524/possible-to-dual-boot-and-virtualize-same-physical-drive-containing-windows-10):

```
dmidecode -s system-uuid
vboxmanage modifyvm your-vm-name --hardwareuuid <your-hardware-uuid>
```
