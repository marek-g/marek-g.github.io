---
layout: page
title: Video conversion
parent: Tips & Tricks
---

# Video conversion

## Linux

## ffmpeg with nvidia HW encoding

Very fast (430 fps for 1280x720) with good/medium quality.

```
optirun ffmpeg -i input.mkv -c:v h264_nvenc -profile:v main -preset:v fast -b:v 1M output.mkv
```

It is medium speed (165 fps for 1280x720) with good quality.

```
optirun ffmpeg -i input.mkv -c:v h264_nvenc -profile:v main -preset:v medium -b:v 1M output.mkv
```

### ffmpeg with vaapi HW encoding

It is fast (240 fps for 1280x720), but quality below 5Mbps is poor.

```
ffmpeg -hwaccel vaapi -hwaccel_output_format vaapi -vaapi_device /dev/dri/renderD128 -i input.mkv -vf 'format=nv12|vaapi,hwupload,scale_vaapi=w=1280:h=720' -c:v h264_vaapi -b:v 5M output.mkv
```

### ffmpeg with software encoding

Slow (120 fps for 1280x720), but the encoding quality is good.

```
ffmpeg -i input.mkv -c:v libx264 -profile:v main -preset:v fast -b:v 1M output.mkv
```

Slow (90 fps for 1280x720), but the encoding quality is good.

```
ffmpeg -i input.mkv -c:v libx264 -profile:v main -preset:v medium -b:v 1M output.mkv
```
