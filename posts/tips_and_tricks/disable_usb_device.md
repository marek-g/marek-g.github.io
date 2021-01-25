---
layout: page
title: Disable USB device
parent: Tips & Tricks
---

# Disable USB device (Linux)

1. Run `lsusb` to detect vendor:product id of unwanted device. For example:

```
Bus 001 Device 003: ID 26ce:01a2 ASRock LED Controller
```

2. Create `/etc/udev/rules.d/10-remove_asrock_led_joystick.rules` file with content:

```
SUBSYSTEM=="usb", ATTRS{idVendor}=="26ce", ATTRS{idProduct}=="01a2", ATTR{authorized}="0"
```
