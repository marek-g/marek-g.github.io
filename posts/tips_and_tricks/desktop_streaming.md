---
layout: page
title: Desktop streaming
parent: Tips & Tricks
---

# Desktop streaming

## Linux

### ffmpeg with vaapi HW encoding + vlc

```
ffmpeg -vaapi_device /dev/dri/renderD128 \
-f x11grab -s 1920x1080 -r 25 -i :0.0+0,0 \
-thread_queue_size 512 \
-f alsa -ac 2 -i pulse \
-vf 'format=bgr0,hwupload,scale_vaapi=w=1920:h=1080' -c:v h264_vaapi -qp 18 \
-acodec aac \
-f mpegts - | vlc -I dummy - --sout '#std{access=http,mux=ts,dst=:3030}'
```

### ffmpeg with software encoding + vlc

```
ffmpeg -f x11grab -s 1920x1080 -r 25 -i :0.0+0,0 \
-thread_queue_size 1024 \
-f alsa -ac 2 -i pulse \
-vcodec libx264 -crf 12 -preset ultrafast -s 1920x1080 \
-acodec aac -threads 0 \
-f mpegts - | vlc -I dummy - --sout '#std{access=http,mux=ts,dst=:3030}'
```

### How to enable audio recording

Enable desktop audio recording in Pulse:
Start `pavucontrol`
Go to the *Recording* tab and you'll find `ffmpeg` or `Lavf56.15.102` (or similar) listed there.
Change audio capture from *Internal Audio Analog Stereo* to *Monitor of Internal Audio Analog Stereo*.
