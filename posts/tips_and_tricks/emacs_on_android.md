---
layout: page
title: Emacs on Android
parent: Tips & Tricks
---

# Emacs on Android - use with Termux

This page describes how to use Emacs for Android with Termux. You can then use all the usefull tools like `git`, search tools and even programming languages.

Normally, Emacs keeps its data at:
```
/data/data/org.gnu.emacs/
```
and Termux keeps its data at:
```
/data/data/com.termux/
```

Termux has this path embedded in its packages.

The idea is to use `sharedUserId` manifest attribute in both packages to share the same Android user, so the packages can access both data folders.

## Prebuilt binaries

If you don't want to make below steps manually, you can use my prebuilt packages: https://github.com/marek-g/doom-emacs-config/releases/tag/Android_2023-02-13

## Termux steps

Termux already has configured `sharedUserId`. However, both packages need to be signed with the same certificate, so we need to resign Termux APK with our own certificate (we assume you have already generated one). If you want to install more Termux packages, you will have to resign them all.

1. Download Termux APK (e.g. from F-Droid).
1. Unpack: `apktool d <termux.apk>`.
1. Build new APK for Termux: `apktool b <termux_folder> -o termux_for_emacs.apk`.
1. Sign the APK: `jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore ~/.android/debug.keystore termux_for_emacs.apk androiddebugkey`.

## Emacs steps

1. Download Emacs APK (e.g. from F-Droid).
1. Unpack: `apktool d <emacs.apk>`.
1. Add `android:sharedUserId="com.termux" android:sharedUserLabel="@string/shared_user_label"` to Emacs' `AndroidManifest.xml`.
1. Copy `@string/shared_user_label` from Termux's `res/values/strings.xml` to Emacs' resources.
1. Build new APK for Emacs: `apktool b emacs_and_termux -o emacs_for_termux.apk`.
1. Sign the APK: `jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore ~/.android/debug.keystore emacs_for_termux.apk androiddebugkey`.

## Setup

Now you can install both APKs. Emacs will be able to use files in `/data/data/com.termux/` folder.

You can setup path environment to include Termux binaries by creating `~/.emacs.d/early-init.el` file:
```elisp
;; Add Termux binaries to PATH environment
(setenv "PATH"
  (let ((current (getenv "PATH"))
        (new "/data/data/com.termux/files/usr/bin"))
    (if current (concat new ":" current) new)))
```
