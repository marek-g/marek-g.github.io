---
layout: page
title: Keyboard
parent: Hardware
---

# Keyboard

Layout editor: https://www.keyboard-layout-editor.com
Paste this JSON data to `Raw data` tab:

``` json
[{c:"#AAAA00",t:"#222222",p:"DCS"},"Esc",{x:1,c:"#222222",t:"#666666"},"F1","F2","F3","F4",{t:"#222222"},"F5",{x:2.25,c:"#666666"},"Esc",{x:1,c:"#222222",t:"#666666"},"F1","F2","F3","F4",{x:0.5,c:"#666666",t:"#222222"},"F5","F6","F7","F8",{x:0.5,c:"#222222",t:"#666666"},"F9","F10","F11","F12",{x:0.25,c:"#666666",t:"#222222"},"PrtSc","Scroll Lock","Pause\nBreak"],
[{y:0.5,c:"#AAAA00"},"~\n`",{c:"#222222",t:"#666666"},"!\n1","@\n2","#\n3","$\n4","%\n5","^\n6",{x:2.25},"&\n7","*\n8","(\n9",")\n0","_\n-","+\n=",{c:"#AAAA00",t:"#222222"},"Backspace","Backspace",{c:"#222222",t:"#666666"},"*\n8","(\n9",")\n0","_\n-","+\n=",{c:"#666666",t:"#222222",w:2},"Backspace",{x:0.25},"Insert","Home","PgUp",{x:0.25},"Num Lock","/","*","-"],
[{c:"#AAAA00",w:1.5},"Tab",{c:"#222222",t:"#666666\n\n#ff8800"},"Q\n\n`","W\n\n<","E\n\n>","R\n\n\"","T\n\n.",{x:2.75,w:1.5},"Y\n\n&","U\n\n_","I\n\n[","O\n\n]","P\n\n%",{t:"#666666"},"{\n[","}\n]","|\n\\","I","O","P","{\n[","}\n]",{c:"#666666",t:"#222222",w:1.5},"|\n\\",{x:0.25},"Delete","End","PgDn",{x:0.25,c:"#222222",t:"#666666"},"7\nHome","8\n↑","9\nPgUp",{c:"#666666",t:"#222222",h:2},"+"],
[{c:"#AAAA00",w:1.75,l:true},"Caps Lock",{c:"#222222",t:"#666666\n\n#ff8800"},"A\n\n!","S\n\n-","D\n\n+",{n:true},"F\n\n=","G\n\n#",{x:2.5,w:1.75,l:true},"H\n\n|","J\n\n:","K\n\n(","L\n\n)",{n:true},":\n;\n?",{t:"#666666"},"\"\n'",{c:"#AAAA00",t:"#222222"},"Enter",{n:true},"Enter",{c:"#222222",t:"#666666"},"K","L",":\n;","\"\n'",{c:"#666666",t:"#222222",w:2.25},"Enter",{x:3.5,c:"#222222",t:"#666666"},"4\n←","5","6\n→"],
[{c:"#AAAA00",t:"#222222",w:2.25},"Shift",{c:"#222222",t:"#666666\n\n#ff8800"},"Z\n\n^","X\n\n/","C\n\n*","V\n\n\\","B\n\n`",{x:2,w:2.25},"N\n\n~","M\n\n$","<\n,\n{",">\n.\n}","?\n/\n@",{c:"#AAAA00",t:"#222222"},"Shift","Shift",{c:"#222222",t:"#666666"},"M","<\n,",">\n.","?\n/",{c:"#666666",t:"#222222",w:2.75},"Shift",{x:1.25},"↑",{x:1.25,c:"#222222",t:"#666666"},"1\nEnd","2\n↓","3\nPgDn",{c:"#666666",t:"#222222",h:2},"Enter"],
[{c:"#AAAA00",w:1.25},"Ctrl",{c:"#222222",t:"#666666",w:1.25},"Fn",{c:"#AAAA00",t:"#222222",w:1.25},"Alt","LAlt","Ctrl",{c:"#ff8800",t:"#002040",p:"DCS SPACE",a:7,w:2.25},"\n\n\n\nSYMBOLS LAYER",{x:1.25,c:"#AAAA00",t:"#222222",p:"DCS",a:4,w:1.25},"Delete",{w:1.25},"Backspace",{w:1.25},"Space",{p:"DCS SPACE",w:6.25},"RAlt",{c:"#666666",p:"DCS",w:1.25},"Alt",{w:1.25},"Win",{w:1.25},"Menu",{w:1.25},"Ctrl",{x:0.25},"←","↓","→",{x:0.25,c:"#222222",t:"#666666",w:2},"0\nIns",".\nDel"]
```

Keymapper configuration:

``` ini
# Keymapper project page: https://github.com/houmain/keymapper
#
# Different ideas:
# - https://www.reddit.com/r/ErgoMechKeyboards/comments/1ghemwu/timeless_home_row_mods_focused_on_convenient/
# - godmode w Emacs lepsze od sticky keys, bo automatycznie się wyłącza
# - evilmode w Emacs (dla mnie wygodniejsze od godmode)
# - Meow's keypad mode
# - https://github.com/jyp/boon
# - http://xahlee.info/emacs/misc/xah-fly-keys.html
# - http://xahlee.info/emacs/emacs/ergonomic_emacs_keybinding.html
#
# - hydra
# - https://github.com/meedstrom/deianira - hydra everywhere
#
# - swap lalt with lctrl?
#
# - one hand keyboard? https://inkeys.wiki/en/keymaps/taipo
#   (Artsey? Ardux?)
# - Miryoku: https://github.com/manna-harbour/miryoku

###############################################################################
#
# Keyboards
#
###############################################################################

[system = "Linux"]
#FullSizeKeyboard = 'BY Tech Gaming Keyboard'
FullSizeKeyboard = 'USB USB Keyboard'
LeftHandKeyboard = 'Evision RGB Keyboard'

[system = "Windows"]
#FullSizeKeyboard = 'Gaming Keyboard'
FullSizeKeyboard = 'USB Keyboard'
LeftHandKeyboard = 'RGB Keyboard'

###############################################################################
#
# Key sequences
#
###############################################################################

Combo     = $0 !250ms $1              ; press both keys almost immediatelly
DoubleTap = $0 !250ms $0{!250ms}      ; dobule tap a key
Tap       = $0{!200ms}  ; shorter than
Hold      = $0{200ms}   ; longer than

###############################################################################
#
# Virtual keys
#
###############################################################################

#
# keyboard layouts
#

SplitKeyboardMode = Virtual1
OneHandMode = Virtual2
MiryokuMode = Virtual3

#
# thumb keys defined as virtual keys
#
# as a workaround to a bug in keymapper (as of version 4.9.1)
# when using modifiers and two keyboards,
# see: https://github.com/houmain/keymapper/issues/207
#

ThumbLeft1 = Virtual100
ThumbLeft2 = Virtual101
ThumbLeft3 = Virtual102
ThumbLeft4 = Virtual103

ThumbRight1 = Virtual104
ThumbRight2 = Virtual105
ThumbRight3 = Virtual106
ThumbRight4 = Virtual107

#
# Switch keyboard layouts
#
ScrollLock >> !OneHandMode SplitKeyboardMode
Pause >> OneHandMode !SplitKeyboardMode

# double tab CapsLock to toggle it
#DoubleTap[CapsLock] >> Virtual1
#Virtual1 >> ControlLeft)

#Tap[CapsLock] >> Escape
#	CapsLock      >> ControlLeft

#DoubleTap[CapsLock] >> Virtual2
#Hold[E] >> Virtual2 ^ Virtual2
#Virtual2{I} >> ArrowUp
#Virtual2{K} >> ArrowDown
#Virtual2{J} >> ArrowLeft
#Virtual2{L} >> ArrowRight
#Virtual2{H} >> ControlLeft{ArrowLeft}
#Virtual2{Semicolon} >> ControlLeft{ArrowRight}

#Boss = Virtual1
#ScrollLock >> Boss
#Boss >> A

#A >> Virtual2 ^ Virtual2
#Virtual2 >> B

#[modifier="!Boss"]
#A >> B
#B >> A

#Hold[A]{Any} >> Shift{Any}
#Tap[A] >> A

#Hold[J] >> ControlLeft
#Tap[J] >> J

#J{Any} >> ControlLeft{Any}


###
#
# Działająca wersja shifty to nawiasy,
# ale psuje chyba opcję ctrl + shift + strzałki
#
##
#ShiftLeft{Any} >> ShiftLeft{Any}
#ShiftLeft >> "("
#ShiftRight{Any} >> ShiftRight{Any}
#ShiftRight >> ")"

#Space{F} >> Control
#Space{Any} >> Control{Any}

#? Tap[Space] >> Space
#Hold[Space] >> ControlLeft


#Hold[ShiftLeft] >> ShiftLeft
#Hold[ShiftLeft]{Any} >> ShiftLeft{Any}
#Tap[ShiftLeft] >> "("

#Hold[ShiftRight] >> ShiftRight
#Tap[ShiftRight] >> ")"

#Ext = CapsLock
#Ext >>
#Ext{S}               >> Alt
#Ext{D}               >> Shift
#Ext{F} >> Control
#Ext{I}               >> ArrowUp
#Ext{K}               >> ArrowDown
#Ext{J}               >> ArrowLeft
#Ext{L}               >> ArrowRight

#CapsLock >> Backspace

###############################################################################
#
# One hand mode - Taipo
#
###############################################################################
Thumb1 = N
Thumb2 = P

[system = "Linux"]
#OneHandMode >> $(hid_ctrl rk618 solid-color 255 255 0) ^ $(hid_ctrl rk618 solid-color 0 255 0)
OneHandMode >> $(notify-send 'OneHandMode ON') ^ $(notify-send 'OneHandMode OFF')

[modifier="OneHandMode"]
A >> E
S >> T
D >> O
F >> A

(Q W) >> Y
(E R) >> B
(A S) >> H
(D F) >> L

(Thumb2 Q W) >> 5
(Thumb2 E R) >> 9
(Thumb2 A S) >> 0
(Thumb2 D F) >> 4

(Thumb1 A) >> ShiftLeft{E}
(Thumb1 S) >> ShiftLeft{T}
(Thumb1 D) >> ShiftLeft{O}
(Thumb1 F) >> ShiftLeft{A}

(Thumb2 A) >> ShiftLeft{9}
(Thumb2 S) >> BracketLeft
(Thumb2 D) >> ShiftLeft{BracketLeft}
(Thumb2 F) >> ShiftLeft{Comma}

Q >> I
W >> N
E >> S
R >> R

(Thumb1 Q) >> ShiftLeft{I}
(Thumb1 W) >> ShiftLeft{N}
(Thumb1 E) >> ShiftLeft{S}
(Thumb1 R) >> ShiftLeft{R}

(Thumb2 Q) >> ShiftLeft{0}
(Thumb2 W) >> BracketRight
(Thumb2 E) >> ShiftLeft{BracketRight}
(Thumb2 R) >> ShiftLeft{Period}

Thumb1 >> Space
Thumb2 >> Backspace

###############################################################################
#
# SplitKeyboardMode
#
# Two keyboards. One for the left hand. One for the right hand.
#
###############################################################################

#
# notify user about turning SplitKeyboardMode on / off
#

[system = "Linux"]
SplitKeyboardMode >> $(notify-send 'Split Keyboard Mode ON') ^ $(notify-send 'Split Keyboard Mode OFF')
#SplitKeyboardMode >> $(hid_ctrl rk618 custom-colors 0F0:2 040:13 F00 040:5 00F:4 0F0:2 FF0:2 000:6 0F0:7 040:5 0F0:3 000:6 0F0:7 040 F00:4 040 FF0:2 000:6 0F0:7 040:6 FF0:2 000:6 0F0:7 FF0:2 F00:1 FF0:5 000:6 0F0:6) ^ $(hid_ctrl rk618 solid-color 0 255 0)
#SplitKeyboardMode >> $(hid_ctrl rk618 custom-colors 0F0:1) ^ $(hid_ctrl rk618 solid-color 0 255 0)
#SplitKeyboardMode >> $(hid_ctrl rk618 solid-color 255 0 0) ^ $(hid_ctrl rk618 solid-color 0 255 0)

[system = "Windows"]
SplitKeyboardMode >> $(msg * Split Keyboard Mode ON) ^ $(msg * Split Keyboard Mode OFF)

#
# define thumb clusters
#

[device = LeftHandKeyboard, modifier="SplitKeyboardMode"]
Space >> ThumbLeft1 ^ ThumbLeft1
P >> ThumbLeft2 ^ ThumbLeft2
N >> ThumbLeft3 ^ ThumbLeft3
AltLeft >> ThumbLeft4 ^ ThumbLeft4

[device = FullSizeKeyboard, modifier="SplitKeyboardMode"]
ControlLeft >> ThumbRight1 ^ ThumbRight1
MetaLeft >> ThumbRight2 ^ ThumbRight2
AltLeft >> ThumbRight3 ^ ThumbRight3
Space >> ThumbRight4 ^ ThumbRight4

[stage]

[modifier="SplitKeyboardMode"]
SymbolsLayer = ThumbLeft1
ThumbLeft2 >> ControlLeft
ThumbLeft3 >> AltLeft
Extend = ThumbLeft4
ThumbRight1 >> Delete
ThumbRight2 >> Backspace
ThumbRight3 >> Space
#Tap[ThumbRight4] >> Space
#Hold[ThumbRight4] >> AltRight
ThumbRight4 >> AltRight
#ThumbRight4{Any} >> AltRight{Any}
#ThumbRight4 >> Space

#
# full keyboard - letters
#

[device = FullSizeKeyboard, modifier="SplitKeyboardMode"]
Backquote >> 7
1 >> 8
2 >> 9
3 >> 0
4 >> Minus
5 >> Equal
6 >> Backspace
7 >> Backspace

Tab >> Y
Q >> U
W >> I
E >> O
R >> P
T >> BracketLeft
Y >> BracketRight
U >> Backslash

CapsLock >> H
A >> J
S >> K
D >> L
F >> Semicolon
G >> Quote
H >> Enter
J >> Enter

ShiftLeft >> N
Z >> M
X >> Comma
C >> Period
V >> Slash
B >> ShiftRight
N >> ShiftRight

[stage]

#
# Extend key
#

Extend >>
Extend{S}               >> Alt
Extend{D}               >> Shift
Extend{F} >> Control
Extend{I}               >> ArrowUp
Extend{K}               >> ArrowDown
Extend{J}               >> ArrowLeft
Extend{L}               >> ArrowRight

#
# symbols layer
#
# inspired by: https://getreuer.info/posts/keyboards/symbol-layer/index.html
#

[modifier="SplitKeyboardMode SymbolsLayer"]
CapsLock >> Enter
Q >> "'"
W >> "<"
E >> ">"
R >> '"'
T >> "."

A >> "!"
S >> "-"
D >> "+"
F >> "="
G >> "#"

Z >> "^"
X >> "/"
C >> "*"
V >> Backslash
B >> "`"

Y >> "&"
U >> "_"
I >> "["
O >> "]"
P >> "%"

H >> "|"
J >> ":"
K >> "("
L >> ")"
Semicolon >> "?"

N >> "~"
M >> "$"
Comma >> "{"
Period >> "}"
Slash >> "@"

###############################################################################
#
# Emacs
#
###############################################################################

#[class=/emacs/i]
[title=/GNU Emacs/i]
Hold[CapsLock] >> Control
#Tap[CapsLock] >> Escape
Escape >> Control{G}
#ControlLeft{CapsLock} >> CapsLock
#CapsLock >>
Control{X} >> Control{W}                      ; cut
Control{C} >> AltLeft{W}                      ; copy
Control{V} >> Control{Y}                      ; paste
Control{A} >> Control{X} H                    ; select all
Control{S} >> Control{X} Control{S}           ; save
Control{O} >> Control{X} Control{F}           ; open
Control{F} >> Control{S}                      ; search
(Shift Control){F} >> Control{R}              ; search backward
Control{W} >> Control{X} K                    ; kill buffer
Control{Z} >> Control{X} U                    ; undo
Control{Y} >> (Control AltLeft Shift){Minus}  ; redo
```
