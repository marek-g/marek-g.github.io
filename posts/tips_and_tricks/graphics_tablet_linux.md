---
layout: page
title: Graphics Tablet in Linux
parent: Tips & Tricks
---

# Graphics Tablet in Linux

Source: https://www.reddit.com/r/XPpen/comments/gnw8tu/deco_01_v2_for_manjaro/frf17ye/

## XP-Pen Deco 01 v2 with DIGImend driver

1. Install `kcm-wacomtablet`, `libwacom` and `xf86-input-wacom` packages.
2. Install required kernel headers, for example `linux511-headers` package.
3. Clone git repository from `https://github.com/ihewitt/digimend-kernel-drivers`
4. Run: `sudo make dkms_install` in the source repository.

Now that's done, there are two files that need to be added for our tablet. Download these two text files - once downloaded, rename them to remove the .txt extension. After that copy the files into the directories indicated:

- [10-xppen.hwdb.txt](https://github.com/DIGImend/digimend-kernel-drivers/files/4464206/10-xppen.hwdb.txt) in /etc/udev/hwdb.d
- [xp-pen-dec01.tablet.txt](https://github.com/DIGImend/digimend-kernel-drivers/files/4464207/xp-pen-dec01.tablet.txt) in /usr/share/libwacom

Change `[Buttons]` section in the `xp-pen-dec01.tablet.txt` to:

```ini
[Buttons]
Left=A;B;C;D;E;F;G;H;
EvdevCodes=0x100;0x101;0x102;0x103;0x104;0x105;0x106;0x107
```

### Uninstall

To uninstall, run `sudo make dkms_uninstall` in the source repository (`/usr/src/digimend-10/`) and remove above files.

### Other info

USB VID/PID: 28bd:0905

Button numbers are 1, 2, 3, 8, 9, 10, 11, 12.
Setting them with `xsetwacom`:

```sh
xsetwacom set "UGTABLET 10 inch PenTablet Keyboard pad" button 1 key ctrl z
xsetwacom set "UGTABLET 10 inch PenTablet Keyboard pad" button 2 key ctrl z
xsetwacom set "UGTABLET 10 inch PenTablet Keyboard pad" button 3 key ctrl z
xsetwacom set "UGTABLET 10 inch PenTablet Keyboard pad" button 8 key ctrl z
xsetwacom set "UGTABLET 10 inch PenTablet Keyboard pad" button 9 key ctrl z
xsetwacom set "UGTABLET 10 inch PenTablet Keyboard pad" button 10 key ctrl z
xsetwacom set "UGTABLET 10 inch PenTablet Keyboard pad" button 11 key ctrl z
xsetwacom set "UGTABLET 10 inch PenTablet Keyboard pad" button 12 key ctrl z
```

Read about it and other methods on: https://github.com/DIGImend/digimend-kernel-drivers/issues/340
