---
layout: page
title: Wayland - custom resolution
parent: Tips & Tricks
---

# Custom resolution

- Get EDID file from correct card & output:

``` shell
cat /sys/class/drm/card0-HDMI-A-1/edid | parse-edid
cat /sys/class/drm/card0-HDMI-A-1/edid > EDID_LG_55_OLED.bin
```

- Edit `bin` file and add custom resolutions with `Custom Resoltion Utility` (CRU) Windows app (runs fine in Wine).

- Copy new `bin` file to `/usr/lib/firmware/edid` folder

- Add `drm_edid_firmware` to your kernel command line

Example parameter (overrides EDIDs for multiple outputs):

``` shell
drm.edid_firmware=DP-1:edid/EDID_AOC_Q32_50Hz.bin,DVI-D-2:edid/EDID_NEC_EA24_50Hz.bin,HDMI-A-1:edid/EDID_LG_55_OLED_50Hz.bin
```

When using zfs file system & `ZFSBootMenu` it can be set with zfs property (example from my desktop PC):

``` shell
sudo zfs set org.zfsbootmenu:commandline="nvidia-drm.modeset=1 spl.spl_hostid=0x00bab10c rw drm.edid_firmware=DP-1:edid/EDID_AOC_Q32_50Hz.bin,DVI-D-2:edid/EDID_NEC_EA24_50Hz.bin,HDMI-A-1:edid/EDID_LG_55_OLED_50Hz.bin" ssd/manjaro
```
