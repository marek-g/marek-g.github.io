---
layout: page
title: Emacs on Android
parent: Tips & Tricks
---

# Emacs on Android - use with Termux

This page describes how to use Emacs for Android with Termux. You can then use all the useful tools like `git`, search tools and even programming languages.

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

If you don't want to make below steps manually, you can use my [prebuilt packages](https://github.com/marek-g/emacs-config/releases).

## Termux steps

Termux already has configured `sharedUserId`. However, both packages need to be signed with the same certificate, so we need to resign Termux APK with our own certificate (we assume you have already generated one). If you want to install more Termux packages, you will have to resign them all.

1. Download Termux APK (e.g. from F-Droid).
1. Unpack: `apktool d <termux.apk>`.
1. Build new APK for Termux: `apktool b <termux_folder> -o termux_for_emacs.apk`.
1. Align zip file: `/opt/android-sdk/build-tools/<version>/zipalign -f -p 4 termux_for_emacs.apk termux_for_emacs_aligned.apk`
1. Sign the APK: `/opt/android-sdk/build-tools/<version>/apksigner sign --ks ~/.android/debug.keystore ./termux_for_emacs_aligned.apk`.

## Emacs steps

1. Download Emacs APK (e.g. from [sourceforge](https://sourceforge.net/projects/android-ports-for-gnu-emacs/files/)).
1. Unpack: `apktool d <emacs.apk>`.
1. Add `android:sharedUserId="com.termux" android:sharedUserLabel="@string/shared_user_label"` to Emacs' `AndroidManifest.xml`.
1. Copy `@string/shared_user_label` from Termux's `res/values/strings.xml` to Emacs' resources. The content should be like this:
   ```
   <resources>
    ...
    <string name="shared_user_label">Termux user</string>
   </resources>
   ```
1. Build new APK for Emacs: `apktool b emacs_and_termux -o emacs_for_termux.apk`.
1. Align zip file: `/opt/android-sdk/build-tools/<version>/zipalign -f -p 4 emacs_for_termux.apk emacs_for_termux_aligned.apk`
1. Sign the APK: `/opt/android-sdk/build-tools/<version>/apksigner sign --ks ~/.android/debug.keystore ./emacs_for_termux_aligned.apk`.

## Setup

Now you can install both APKs. Emacs will be able to use files in `/data/data/com.termux/` folder.

You can setup path environment to include Termux binaries by creating `~/.emacs.d/early-init.el` file:

```elisp
(when (string-equal system-type "android")
  ;; Add Termux binaries to PATH environment
  (let ((termuxpath "/data/data/com.termux/files/usr/bin"))
    (setenv "PATH" (concat (getenv "PATH") ":" termuxpath))
    (setq exec-path (append exec-path (list termuxpath)))))
```

The best source to get Emacs binaries for Android is: https://sourceforge.net/projects/android-ports-for-gnu-emacs/files/.
But if you are using Emacs build from F-Droid you may want to configure `gnutls-cli` program to have access to SSL sites (like Melpa), because it's compiled without GnuTLS library support:

```elisp
;; Fix SSL connection for F-Droid version of Emacs.
;;
;; Android port is builded without gnutls support and uses 'gnutls-cli' command.
;; 'gnutls' command line must be installed in Termux.
;; This removes '%t' (filepath with trused certificates) from original command line, as it causes the error.
;; "--x509cafile /data/data/com.termux/files/usr/etc/tls/cert.pem' was also not working".
;; 'openssl' is not working at all.
(when (string-equal system-type "android")
  (setq tls-program '("gnutls-cli -p %p %h"
          "gnutls-cli -p %p %h --protocols ssl3"
          ;"openssl s_client -connect %h:%p -no_ssl2 -ign_eof"
  )))
```
