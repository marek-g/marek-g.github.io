---
layout: page
title: Backups
parent: Tips & Tricks
---

# Backups

## Compressing files and folders

To compress and encrypt files and folders on Linux (`tar` stores _owner/group_ of the file):

```
tar -cf archive.tar -C <directory> .
```

or

```
tar -cf archive.tar <directory>
```

where `-C <path>` (change folder) can be used to not store full path names when whole mount point from some other place, for example:

```
tar -cf 2021-08-18_listingi.tar -C /media/truecrypt1 .
```

and then

```
7z a -p -mhe=on archive.tar.7z archive.tar
```

and `-mhe=on` is for enabling header encryption (cannot list file names).

Decompression:

```
7z x archive.tar.7z
```
