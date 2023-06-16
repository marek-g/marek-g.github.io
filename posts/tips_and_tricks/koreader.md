---
layout: page
title: KOReader
parent: Tips & Tricks
---

# KOReader tips & tricks

## Kindle 4 PW - enable PowerOff menu option

TODO: create PR

To enable `Power Off` option to KOReader's exit menu:

1. Run KOReader
2. Go to the `Tools/Text Editor`. Open `frontend/device/kindle/device.lua` file.
3. Add `canPowerOff = true,` line before `canSuspend = yes,` in `local Kindle = Generic:extend(` declaration.
4. Add the following function (in any place, can be below above declaration):

   ```lua
   function Kindle:powerOff()
       os.execute("shutdown -P 0")
   end
   ```
