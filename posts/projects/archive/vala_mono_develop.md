---
layout: page
title: Vala in Mono Develop (2014-07)
---

I took Vala Binding Mono Develop plugin (it's development was abandoned a few years ago) and I upgraded it to Mono Develop 5. I've rewritten the references (packages) mechanism and prepared portable version for Windows.

![Mono Develop with Vala Binding](vala_mono_develop/vala_mono_develop_01.png)

There is still code complete feature missing. Debugger is not working because of lack of MonoDevelop.Debugger.gdb plugin for Windows. Code complete was working in the previous version so you can monitor my github page - maybe I'll make it working again.

The project is located on the git-hub: https://github.com/marek-g/ValaBinding

Portable version (MonoDevelop 5.1.4, vala 0.25.1, mingw64 4.9.1) can be downloaded from here:

https://www.dropbox.com/s/l9p6hkpxsjkyxci/Vala_workspace_2014-07-30.zip
