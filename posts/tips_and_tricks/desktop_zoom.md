---
layout: page
title: Desktop zoom
parent: Tips & Tricks
---

# Desktop zoom with MMB in KDE

1. Install `xbindkeys`.

2. Create `~/.xbindkeysrc.scm` file:

```Scheme
;
; Middle mouse button - zoom screen in and out
;
(xbindkey-function '("b:2")
	(let ((count 0))
		(lambda ()
			(set! count (+ count 1))

			(if (= count 1)
				(begin
					(run-command "qdbus org.kde.kglobalaccel /component/kwin invokeShortcut view_zoom_in")
					(run-command "qdbus org.kde.kglobalaccel /component/kwin invokeShortcut view_zoom_in")
				)
			)
			
			(if (= count 2)
				(begin
					(run-command "qdbus org.kde.kglobalaccel /component/kwin invokeShortcut view_zoom_in")
					(run-command "qdbus org.kde.kglobalaccel /component/kwin invokeShortcut view_zoom_in")
				)
			)
			
			(if (= count 3)
				(begin
					(set! count 0)
					(run-command "qdbus org.kde.kglobalaccel /component/kwin invokeShortcut view_zoom_out")
					(run-command "qdbus org.kde.kglobalaccel /component/kwin invokeShortcut view_zoom_out")
					(run-command "qdbus org.kde.kglobalaccel /component/kwin invokeShortcut view_zoom_out")
					(run-command "qdbus org.kde.kglobalaccel /component/kwin invokeShortcut view_zoom_out")
					(run-command "qdbus org.kde.kglobalaccel /component/kwin invokeShortcut view_zoom_out")
					(run-command "qdbus org.kde.kglobalaccel /component/kwin invokeShortcut view_zoom_out")
				)
			)
		)
	)
)

;
; alt + scroll mouse wheel - zoom screen in and out
;
(xbindkey '(alt "b:4") "qdbus org.kde.kglobalaccel /component/kwin invokeShortcut view_zoom_in")
(xbindkey '(alt "b:5") "qdbus org.kde.kglobalaccel /component/kwin invokeShortcut view_zoom_out")
```

3. Add `xbindkeys` command to autostart in KDE Settings.
