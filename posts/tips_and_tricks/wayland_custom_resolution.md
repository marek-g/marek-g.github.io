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

- Edit `bin` file and add custom resolutions with `Custom Resoltion Utility` (CRU) Windows app.

The app works in Wine, but it's better to run it in the Windows. When run in Wine the exported monitor name is `Microsoft`.

- Copy new `bin` file to `/usr/lib/firmware/edid` folder

- Add `drm_edid_firmware` to your kernel command line

Example parameter (overrides EDIDs for multiple outputs):

``` shell
drm.edid_firmware=DP-1:edid/EDID_AOC_Q32_50Hz.bin,HDMI-A-1:edid/EDID_LG_55_OLED_50Hz.bin
```

When using zfs file system & `ZFSBootMenu` it can be set with zfs property (example from my desktop PC):

``` shell
sudo zfs set org.zfsbootmenu:commandline="nvidia-drm.modeset=1 spl.spl_hostid=0x00bab10c rw drm.edid_firmware=DP-1:edid/EDID_AOC_Q32_50Hz.bin,HDMI-A-1:edid/EDID_LG_55_OLED_50Hz.bin" ssd/manjaro
```

# How to go to monitor's service mode

## EA244WMi

1. Menu.
2. Left.
3. Hold select (input) and press reset/dv mode (eco).
4. Select

There is an option to turn off EDID Write Protection. I was able to add 50Hz resolution for DVI input.

## AOC Q3279VWFD8

1. Unplug monitor's cord.
2. Hold Menu button.
3. Plug monitor's cord.
4. Release Menu button.
5. Enter OSD (press Menu button).
6. A little "F" will be visible in the left top corner.
7. Press left arrow and "menu" button to enter service menu.

There is an option to turn off EDID Write Protection, however it worked for me for DVI source, but not for DP.

## LG OLED 55EG9100

1. Install `irplus` app on Android phone with irda hardware.
2. Add `LG - LG Service` pilot.
3. Push `In-Start` button.
4. Type `0413` code.

The problem is I don't know how or if it is possible at all to make EDID writable from the service menu.
