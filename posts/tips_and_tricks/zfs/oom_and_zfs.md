---
layout: page
title: OOM & ZFS
parent: ZFS
grand_parent: Tips & Tricks
---

# OOM & ZFS

## earlyoom

This was the daemon of my choice to kill apps in low memory conditions. Unfortunately, it doesn't count ZFS' ARC memory:
https://github.com/rfjakob/earlyoom/pull/191 (it's closed by it's not merged)

## nohang

So as an alternative, I'm using nohang:
https://github.com/hakavlad/nohang

It tries to improve the situation:
https://github.com/hakavlad/nohang/commit/16f7db180a97e1af13196304789b7548d99dcf04
(it says that situation is improved but not solved yet)


## OpenZFS

The best solution would be to solve the problem in OpenZFS. Here is the issue:
https://github.com/openzfs/zfs/issues/10255
