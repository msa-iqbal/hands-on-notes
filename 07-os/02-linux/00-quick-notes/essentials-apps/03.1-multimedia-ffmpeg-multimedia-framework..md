# FFmpeg Complete Guide for Linux

# 1. What is FFmpeg?

FFmpeg is a command-line multimedia framework.

Its ecosystem includes:

```text
FFmpeg
│
├── ffmpeg     → Convert, encode, decode, filter, record, stream
├── ffprobe    → Inspect media information
├── ffplay     → Play/test media
│
├── libavcodec → Audio/video codecs
├── libavformat → Containers/formats
├── libavfilter → Audio/video filters
├── libswscale → Video scaling/conversion
└── libswresample → Audio resampling
```

It supports a very large range of formats and codecs. You can inspect your particular build with `-formats`, `-codecs`, `-encoders`, and related options.

**Official documentation:** [FFmpeg Documentation](https://ffmpeg.org/documentation.html)

# 2. Install FFmpeg on Ubuntu

For Ubuntu, I recommend the **native package**, rather than trying to use Flatpak for the CLI.

```bash
sudo apt update
sudo apt install ffmpeg
```

Verify:

```bash
ffmpeg -version
```

Also:

```bash
ffprobe -version
```

And:

```bash
ffplay -version
```

Ubuntu provides FFmpeg as a package; for example, Ubuntu 26.04's package repositories list FFmpeg 8.0.1, while the FFmpeg project currently lists **9.0.1** as the latest upstream stable release. ([Ubuntu Packages](https://packages.ubuntu.com))

This distinction is important: **Ubuntu's repository version doesn't necessarily equal the newest upstream FFmpeg release.**

# 3. Understand the basic syntax

The fundamental structure is:

```bash
ffmpeg [options] -i input [options] output
```

For example:

```bash
ffmpeg -i input.mkv output.mp4
```

Conceptually:

```text
Input
  │
  ▼
-i input.mkv
  │
  ▼
Decode
  │
  ▼
Filter / process
  │
  ▼
Encode
  │
  ▼
output.mp4
```

The official syntax follows this general input → processing → output model. ([FFmpeg](https://ffmpeg.org/documentation.html))

# 4. FFmpeg's most important concept: Container vs Codec

This is one of the most important things to understand.

### Container

Examples:

```text
MP4
MKV
MOV
WebM
AVI
FLV
```

### Video codec

Examples:

```text
H.264
H.265 / HEVC
AV1
VP9
ProRes
```

### Audio codec

Examples:

```text
AAC
MP3
Opus
Vorbis
FLAC
PCM
```

For example:

```text
video.mp4
│
├── Container: MP4
├── Video: H.264
└── Audio: AAC
```

Whereas:

```text
video.mkv
│
├── Container: Matroska
├── Video: H.265
└── Audio: Opus
```

Changing the **container** doesn't necessarily mean re-encoding.

That's where `-c copy` becomes extremely useful.

# 5. Inspect a media file with FFprobe

Before converting something, inspect it.

```bash
ffprobe video.mp4
```

More readable:

```bash
ffprobe -v error -show_format -show_streams video.mp4
```

Show only video information:

```bash
ffprobe -v error -select_streams v:0 \
-show_entries stream=codec_name,width,height,r_frame_rate \
-of default=noprint_wrappers=1 video.mp4
```

Show audio:

```bash
ffprobe -v error -select_streams a:0 \
-show_entries stream=codec_name,sample_rate,channels \
-of default=noprint_wrappers=1 video.mp4
```

This is particularly useful in scripts.

# 6. Basic format conversion

## MKV → MP4

```bash
ffmpeg -i input.mkv output.mp4
```

## MOV → MP4

```bash
ffmpeg -i input.mov output.mp4
```

## AVI → MP4

```bash
ffmpeg -i input.avi output.mp4
```

## WebM → MP4

```bash
ffmpeg -i input.webm output.mp4
```

Remember: these commands may **re-encode** the media.

---

# 7. Remux without re-encoding

If the streams are compatible with the destination container:

```bash
ffmpeg -i input.mkv -c copy output.mp4
```

This is called **stream copying/remuxing**.

It's dramatically faster because:

```text
Decode ❌
Encode ❌

Copy streams ✅
```

For example:

```bash
ffmpeg -i movie.mkv -c copy movie.mp4
```

However, not every codec is valid in every container.

# 8. Overwrite behavior

By default, FFmpeg may ask before overwriting an existing output.

Force overwrite:

```bash
ffmpeg -y -i input.mp4 output.mp4
```

Never overwrite:

```bash
ffmpeg -n -i input.mp4 output.mp4
```

For automation, `-y` is commonly useful, but use it carefully.

# 9. Extract audio from video

Extract audio without re-encoding when possible:

```bash
ffmpeg -i video.mp4 -vn -c:a copy audio.aac
```

`-vn` means:

```text
no video
```

Convert video audio to MP3:

```bash
ffmpeg -i video.mp4 -vn audio.mp3
```

Specify MP3 encoder:

```bash
ffmpeg -i video.mp4 -vn -c:a libmp3lame audio.mp3
```

# 10. Convert audio

## WAV → MP3

```bash
ffmpeg -i input.wav output.mp3
```

## MP3 → WAV

```bash
ffmpeg -i input.mp3 output.wav
```

## WAV → FLAC

```bash
ffmpeg -i input.wav output.flac
```

## FLAC → MP3

```bash
ffmpeg -i input.flac output.mp3
```

## MP3 → AAC

```bash
ffmpeg -i input.mp3 output.aac
```

## MP3 → Opus

```bash
ffmpeg -i input.mp3 output.opus
```

# 11. Audio bitrate

For MP3:

```bash
ffmpeg -i input.wav -c:a libmp3lame -b:a 192k output.mp3
```

Common choices:

```text
128k → small
192k → good
256k → very good
320k → high bitrate
```

For AAC:

```bash
ffmpeg -i input.wav -c:a aac -b:a 192k output.m4a
```

For Opus:

```bash
ffmpeg -i input.wav -c:a libopus -b:a 128k output.opus
```

# 12. Video → H.264 MP4

A very useful general-purpose command:

```bash
ffmpeg -i input.mkv \
-c:v libx264 \
-c:a aac \
output.mp4
```

For better compatibility:

```bash
ffmpeg -i input.mkv \
-c:v libx264 \
-preset medium \
-crf 23 \
-c:a aac \
-b:a 192k \
output.mp4
```

# 13. Understanding CRF

CRF means **Constant Rate Factor**.

For x264, a rough starting point:

```text
CRF 18 → very high quality / larger file
CRF 20 → high quality
CRF 23 → good general default
CRF 26 → smaller file
CRF 28 → noticeably more compression
```

Example:

```bash
ffmpeg -i input.mp4 \
-c:v libx264 \
-crf 23 \
-c:a aac \
output.mp4
```

Lower CRF generally means:

```text
better quality
+
larger file
```

Higher CRF generally means:

```text
lower quality
+
smaller file
```

# 14. H.265 / HEVC

H.265 can provide better compression than H.264 at comparable visual quality, at the cost of encoding complexity and potentially broader compatibility considerations.

```bash
ffmpeg -i input.mp4 \
-c:v libx265 \
-crf 28 \
-c:a aac \
output.mp4
```

A common starting point:

```text
H.264 → CRF ~18–23
H.265 → CRF ~24–30
```

These aren't universal quality equivalents; treat them as starting points and evaluate your actual material.

# 15. AV1

If your FFmpeg build includes an AV1 encoder such as `libsvtav1`:

```bash
ffmpeg -i input.mp4 \
-c:v libsvtav1 \
-c:a libopus \
output.mkv
```

Check available AV1 encoders:

```bash
ffmpeg -encoders | grep -i av1
```

Don't assume a particular encoder exists—your installed FFmpeg build determines what's available.

# 16. Resize video

Resize to 1920×1080:

```bash
ffmpeg -i input.mp4 -vf scale=1920:1080 output.mp4
```

Resize to 1280×720:

```bash
ffmpeg -i input.mp4 -vf scale=1280:720 output.mp4
```

Maintain aspect ratio:

```bash
ffmpeg -i input.mp4 \
-vf "scale=1280:-2" \
output.mp4
```

Here FFmpeg calculates the height automatically.

# 17. Change frame rate

Convert to 30 FPS:

```bash
ffmpeg -i input.mp4 -r 30 output.mp4
```

Or using a video filter:

```bash
ffmpeg -i input.mp4 -vf fps=30 output.mp4
```

For many workflows, the filter form gives you more explicit control.

# 18. Crop video

Example:

```bash
ffmpeg -i input.mp4 \
-vf "crop=1280:720:0:0" \
output.mp4
```

The syntax is:

```text
crop=width:height:x:y
```

Example:

```text
crop=1280:720:100:50
```

means:

```text
width  = 1280
height = 720
x      = 100
y      = 50
```

# 19. Rotate video

Rotate 90° clockwise:

```bash
ffmpeg -i input.mp4 \
-vf "transpose=1" \
output.mp4
```

Rotate 90° counter-clockwise:

```bash
ffmpeg -i input.mp4 \
-vf "transpose=2" \
output.mp4
```

# 20. Extract a thumbnail

Extract a frame at 10 seconds:

```bash
ffmpeg -ss 00:00:10 -i video.mp4 \
-frames:v 1 thumbnail.jpg
```

Another useful form:

```bash
ffmpeg -i video.mp4 \
-ss 00:00:10 \
-frames:v 1 \
thumbnail.jpg
```

For seeking through compressed video, placement of `-ss` can affect speed and accuracy.

# 21. Create thumbnails automatically

Create one frame every 10 seconds:

```bash
ffmpeg -i video.mp4 \
-vf fps=1/10 \
thumb_%04d.jpg
```

You'll get:

```text
thumb_0001.jpg
thumb_0002.jpg
thumb_0003.jpg
...
```

This is very useful for:

* Video websites
* Media servers
* Thumbnail generation
* Video indexing
* Content management systems

# 22. Convert images

FFmpeg can also process images.

PNG → JPG:

```bash
ffmpeg -i input.png output.jpg
```

JPG → PNG:

```bash
ffmpeg -i input.jpg output.png
```

WebP → PNG:

```bash
ffmpeg -i input.webp output.png
```

For heavy-duty image manipulation, though, I'd use **ImageMagick** rather than FFmpeg.

# 23. Create video from images

Suppose:

```text
image001.jpg
image002.jpg
image003.jpg
...
```

Create video:

```bash
ffmpeg -framerate 30 \
-i image%03d.jpg \
-c:v libx264 \
-pix_fmt yuv420p \
output.mp4
```

For example:

```text
image001.jpg
image002.jpg
image003.jpg
```

requires:

```bash
-i image%03d.jpg
```

# 24. Convert video into GIF

```bash
ffmpeg -i input.mp4 output.gif
```

Better quality workflow:

```bash
ffmpeg -i input.mp4 \
-vf "fps=10,scale=640:-1:flags=lanczos" \
output.gif
```

GIF can become enormous, so WebM/MP4 is usually much more efficient for web video.

# 25. GIF → video

```bash
ffmpeg -i animation.gif output.mp4
```

Better:

```bash
ffmpeg -i animation.gif \
-c:v libx264 \
-pix_fmt yuv420p \
output.mp4
```

# 26. Cut a video

Extract from 1 minute to 2 minutes:

```bash
ffmpeg -ss 00:01:00 \
-i input.mp4 \
-t 00:01:00 \
output.mp4
```

Or:

```bash
ffmpeg -ss 00:01:00 \
-i input.mp4 \
-to 00:02:00 \
output.mp4
```

### Fast cut without re-encoding

```bash
ffmpeg -ss 00:01:00 \
-i input.mp4 \
-t 00:01:00 \
-c copy \
output.mp4
```

`-c copy` is very fast, but cuts may land on keyframes rather than exactly where you expect.

# 27. Remove audio

```bash
ffmpeg -i input.mp4 -an output.mp4
```

`-an` means:

```text
audio: none
```

# 28. Remove video

```bash
ffmpeg -i input.mp4 -vn output.mp3
```

# 29. Extract subtitles

List streams first:

```bash
ffprobe input.mkv
```

Then:

```bash
ffmpeg -i input.mkv -map 0:s:0 subtitles.srt
```

For a particular subtitle stream:

```bash
ffmpeg -i input.mkv \
-map 0:s:1 \
subtitles.srt
```

# 30. Add subtitles

For a subtitle stream:

```bash
ffmpeg -i video.mp4 \
-i subtitles.srt \
-map 0:v \
-map 0:a \
-map 1:0 \
-c:v copy \
-c:a copy \
-c:s mov_text \
output.mp4
```

This adds the subtitles as a selectable subtitle track.

# 31. Burn subtitles permanently

```bash
ffmpeg -i video.mp4 \
-vf subtitles=subtitles.srt \
output.mp4
```

Unlike a selectable subtitle track, burned subtitles become part of the video image.

# 32. Add watermark

Example:

```bash
ffmpeg -i video.mp4 \
-i logo.png \
-filter_complex "overlay=10:10" \
output.mp4
```

Position:

```text
overlay=x:y
```

Bottom-right example:

```bash
ffmpeg -i video.mp4 \
-i logo.png \
-filter_complex "overlay=W-w-20:H-h-20" \
output.mp4
```

# 33. Change volume

Increase volume:

```bash
ffmpeg -i input.mp4 \
-af "volume=2" \
output.mp4
```

Decrease:

```bash
ffmpeg -i input.mp4 \
-af "volume=0.5" \
output.mp4
```

Normalize audio:

```bash
ffmpeg -i input.mp4 \
-af loudnorm \
output.mp4
```

# 34. Change audio sample rate

44.1 kHz:

```bash
ffmpeg -i input.wav \
-ar 44100 \
output.wav
```

48 kHz:

```bash
ffmpeg -i input.wav \
-ar 48000 \
output.wav
```

# 35. Change audio channels

Stereo:

```bash
ffmpeg -i input.wav \
-ac 2 \
output.wav
```

Mono:

```bash
ffmpeg -i input.wav \
-ac 1 \
output.wav
```

# 36. Extract a specific stream

Suppose your video contains:

```text
Stream 0:0 → video
Stream 0:1 → English audio
Stream 0:2 → Bengali audio
Stream 0:3 → subtitles
```

Select video:

```bash
-map 0:v:0
```

Select first audio:

```bash
-map 0:a:0
```

Select second audio:

```bash
-map 0:a:1
```

Select subtitle:

```bash
-map 0:s:0
```

For example:

```bash
ffmpeg -i movie.mkv \
-map 0:v:0 \
-map 0:a:1 \
output.mkv
```

This is where understanding **stream mapping** becomes extremely useful.

# 37. Keep only video

```bash
ffmpeg -i input.mkv \
-map 0:v:0 \
-c:v copy \
output.mkv
```

Keep only audio:

```bash
ffmpeg -i input.mkv \
-map 0:a:0 \
-c:a copy \
output.m4a
```

# 38. Keep multiple audio tracks

```bash
ffmpeg -i input.mkv \
-map 0:v \
-map 0:a \
-c copy \
output.mkv
```

This copies all video and audio streams.

# 39. Metadata

Show metadata:

```bash
ffprobe -v quiet \
-show_entries format_tags \
input.mp4
```

Set title:

```bash
ffmpeg -i input.mp4 \
-metadata title="My Video" \
output.mp4
```

Set artist:

```bash
ffmpeg -i input.mp3 \
-metadata artist="Artist Name" \
output.mp3
```

# 40. File size control

You can target a video bitrate:

```bash
ffmpeg -i input.mp4 \
-c:v libx264 \
-b:v 2M \
-c:a aac \
-b:a 128k \
output.mp4
```

This targets approximately:

```text
Video: 2 Mbps
Audio: 128 kbps
```

Bitrate is useful when you need predictable bandwidth/file-size characteristics.

For quality-based local encoding, CRF is often more convenient.

# 41. Two-pass encoding

Useful when you need a target bitrate.

First pass:

```bash
ffmpeg -i input.mp4 \
-c:v libx264 \
-b:v 2M \
-pass 1 \
-an \
-f null /dev/null
```

Second pass:

```bash
ffmpeg -i input.mp4 \
-c:v libx264 \
-b:v 2M \
-pass 2 \
-c:a aac \
-b:a 128k \
output.mp4
```

Two-pass encoding is particularly useful when you need to hit a specific bitrate/file-size target.

# 42. Presets

For x264:

```bash
-preset ultrafast
-preset superfast
-preset veryfast
-preset faster
-preset fast
-preset medium
-preset slow
-preset slower
-preset veryslow
```

General principle:

```text
Faster preset
    ↓
less encoding time
    ↓
larger file at equivalent quality

Slower preset
    ↓
more encoding time
    ↓
smaller file at equivalent quality
```

A good general choice:

```bash
-preset medium
```

If encoding speed matters:

```bash
-preset fast
```

# 43. Hardware acceleration

Check:

```bash
ffmpeg -hwaccels
```

You might see things such as:

```text
vaapi
cuda
vulkan
qsv
```

depending on your hardware/build.

For Linux Intel/AMD GPUs, **VA-API** is particularly relevant.

For NVIDIA, CUDA/NVENC may be available if the required drivers and FFmpeg support are present.

Check encoders:

```bash
ffmpeg -encoders | grep -Ei 'nvenc|vaapi|qsv|amf'
```

For example, if available:

```bash
ffmpeg -encoders | grep nvenc
```

Hardware encoding can dramatically improve throughput, but quality-per-bit and supported features vary by encoder generation.

# 44. Hardware decoding

List:

```bash
ffmpeg -hwaccels
```

You can also inspect codecs and hardware-specific encoders.

Don't blindly copy hardware commands from tutorials. First check what your actual FFmpeg build supports.

# 45. Progress information

Normal:

```bash
ffmpeg -i input.mp4 output.mp4
```

FFmpeg displays progress including:

```text
frame
fps
time
bitrate
speed
```

For machine-readable progress:

```bash
ffmpeg -i input.mp4 \
-progress progress.txt \
output.mp4
```

This becomes useful for applications.

# 46. FFmpeg in shell scripts

Example:

```bash
#!/bin/bash

for file in *.mkv; do
    ffmpeg -i "$file" \
        -c:v libx264 \
        -crf 23 \
        -c:a aac \
        "${file%.mkv}.mp4"
done
```

This converts every:

```text
.mkv → .mp4
```

in the current directory.

# 47. Batch audio conversion

```bash
for file in *.wav; do
    ffmpeg -i "$file" \
        -c:a libmp3lame \
        -b:a 192k \
        "${file%.wav}.mp3"
done
```

# 48. Batch image conversion

```bash
for file in *.png; do
    ffmpeg -i "$file" "${file%.png}.jpg"
done
```

For serious image batch operations, however, ImageMagick is usually the better tool.

# 49. Useful FFmpeg information commands

### Version

```bash
ffmpeg -version
```

### Help

```bash
ffmpeg -h
```

### Full help

```bash
ffmpeg -h full
```

### Formats

```bash
ffmpeg -formats
```

### Codecs

```bash
ffmpeg -codecs
```

### Encoders

```bash
ffmpeg -encoders
```

### Decoders

```bash
ffmpeg -decoders
```

### Filters

```bash
ffmpeg -filters
```

### Protocols

```bash
ffmpeg -protocols
```

### Pixel formats

```bash
ffmpeg -pix_fmts
```

### Audio sample formats

```bash
ffmpeg -sample_fmts
```

# 50. Find a specific encoder

```bash
ffmpeg -encoders | grep libx264
```

```bash
ffmpeg -encoders | grep libx265
```

```bash
ffmpeg -encoders | grep av1
```

```bash
ffmpeg -encoders | grep opus
```

# 51. Find a specific format

```bash
ffmpeg -formats | grep matroska
```

```bash
ffmpeg -formats | grep mp4
```

```bash
ffmpeg -formats | grep webm
```

# 52. Useful aliases

You can create convenient shell aliases.

Edit:

```bash
nano ~/.bashrc
```

Add:

```bash
alias ffv='ffmpeg -version'
alias ffp='ffprobe'
alias ffh='ffmpeg -h'
```

Reload:

```bash
source ~/.bashrc
```

# 53. FFmpeg for web development

This is where FFmpeg becomes especially powerful for a programmer.

Imagine your backend receives:

```text
user-uploaded-video.mov
```

You can automatically create:

```text
original.mov
        │
        ├── 1080p.mp4
        ├── 720p.mp4
        ├── 480p.mp4
        ├── thumbnail.jpg
        └── audio.mp3
```

For example:

```bash
ffmpeg -i input.mov \
-vf "scale=1280:-2" \
-c:v libx264 \
-crf 23 \
-c:a aac \
output-720p.mp4
```

Thumbnail:

```bash
ffmpeg -ss 00:00:05 \
-i input.mov \
-frames:v 1 \
thumbnail.jpg
```

Audio:

```bash
ffmpeg -i input.mov \
-vn \
-c:a libmp3lame \
-b:a 192k \
audio.mp3
```

This is the foundation of many media-processing pipelines.

# 54. FFmpeg with Node.js

For your JavaScript/TypeScript development, FFmpeg is particularly useful.

Typical architecture:

```text
Node.js API
     │
     ▼
Upload media
     │
     ▼
Temporary storage
     │
     ▼
FFmpeg
     │
     ├── Transcode
     ├── Thumbnail
     ├── Audio extraction
     └── Metadata
     │
     ▼
Object storage
     │
     ▼
Database
```

You can invoke FFmpeg using Node's `child_process`, or use an FFmpeg wrapper library.

For production systems, I generally prefer understanding and controlling the actual FFmpeg command rather than hiding everything behind a wrapper.

# 55. FFmpeg filters

Filters are one of FFmpeg's most powerful features.

Video:

```bash
-vf
```

Audio:

```bash
-af
```

Example:

```bash
ffmpeg -i input.mp4 \
-vf "scale=1280:-2,fps=30" \
output.mp4
```

Multiple filters:

```text
scale
  ↓
fps
  ↓
crop
  ↓
overlay
  ↓
encode
```

FFmpeg supports simple and complex filtergraphs. ([FFmpeg](https://ffmpeg.org/documentation.html))

# 56. Useful video filters

### Scale

```bash
-vf "scale=1280:-2"
```

### Crop

```bash
-vf "crop=1280:720"
```

### FPS

```bash
-vf "fps=30"
```

### Blur

```bash
-vf "boxblur=10:1"
```

### Sharpen

```bash
-vf "unsharp"
```

### Grayscale

```bash
-vf "format=gray"
```

### Horizontal flip

```bash
-vf "hflip"
```

### Vertical flip

```bash
-vf "vflip"
```

Combine:

```bash
-vf "scale=1280:-2,fps=30,hflip"
```

# 57. Useful audio filters

### Volume

```bash
-af "volume=2"
```

### Normalize

```bash
-af loudnorm
```

### Fade in

```bash
-af "afade=t=in:st=0:d=3"
```

### Fade out

```bash
-af "afade=t=out:st=57:d=3"
```

# 58. Combine two videos

This is a more advanced operation and uses a complex filtergraph.

For example, side-by-side:

```bash
ffmpeg \
-i left.mp4 \
-i right.mp4 \
-filter_complex "[0:v][1:v]hstack=inputs=2" \
output.mp4
```

This is where FFmpeg moves beyond simple conversion and into actual media processing.

# 59. Combine audio + video

Suppose:

```text
video.mp4
music.mp3
```

Use:

```bash
ffmpeg -i video.mp4 -i music.mp3 \
-map 0:v \
-map 1:a \
-c:v copy \
-c:a aac \
output.mp4
```

This replaces the original audio with the MP3.

# 60. Keep video and mix audio

For more advanced audio mixing:

```bash
ffmpeg \
-i video.mp4 \
-i music.mp3 \
-filter_complex "[0:a][1:a]amix=inputs=2:duration=first" \
-c:v copy \
-c:a aac \
output.mp4
```

# 61. Concatenate videos

If videos have compatible streams, create `files.txt`:

```text
file 'video1.mp4'
file 'video2.mp4'
file 'video3.mp4'
```

Then:

```bash
ffmpeg -f concat -safe 0 \
-i files.txt \
-c copy \
output.mp4
```

If the files aren't compatible, you'll need to normalize/transcode them first.

# 62. Screen recording on Linux

FFmpeg can capture screens and devices, but the exact command depends on your display/audio stack.

For example, X11 capture can involve:

```bash
ffmpeg -f x11grab ...
```

Modern Linux systems may use Wayland, PipeWire, or desktop-specific capture mechanisms, so don't blindly use old X11 commands.

# 63. Record audio

The input device depends on your Linux audio stack.

With PulseAudio/PipeWire setups, first inspect devices using your system's audio tools, then provide the appropriate FFmpeg input.

FFmpeg's device support is documented separately from normal file input/output. ([FFmpeg](https://ffmpeg.org/documentation.html))

# 64. Network streaming

FFmpeg can work with network protocols and live streams.

Its protocol subsystem includes many input/output protocols, and the available set can be checked with:

```bash
ffmpeg -protocols
```

([FFmpeg](https://ffmpeg.org/documentation.html))

This makes FFmpeg useful for:

```text
RTMP
HLS
UDP
TCP
SRT
RTSP
HTTP
```

depending on your build and environment.

# 65. Media server workflow

Since you were also exploring Linux media applications such as Jellyfin, FFmpeg is especially relevant.

A typical media-server workflow looks like:

```text
Movie.mkv
    │
    ▼
FFprobe
    │
    ├── Resolution
    ├── Video codec
    ├── Audio codec
    ├── Subtitle streams
    └── Metadata
    │
    ▼
FFmpeg
    │
    ├── Direct Play
    ├── Remux
    └── Transcode
```

Media servers commonly depend heavily on FFmpeg for media processing.

# 66. Important options cheat sheet

| Option            | Meaning                             |
| ----------------- | ----------------------------------- |
| `-i`              | Input                               |
| `-c:v`            | Video codec                         |
| `-c:a`            | Audio codec                         |
| `-c copy`         | Copy streams without encoding       |
| `-vn`             | Disable video                       |
| `-an`             | Disable audio                       |
| `-sn`             | Disable subtitles                   |
| `-map`            | Select streams                      |
| `-vf`             | Video filter                        |
| `-af`             | Audio filter                        |
| `-b:v`            | Video bitrate                       |
| `-b:a`            | Audio bitrate                       |
| `-crf`            | Quality factor                      |
| `-preset`         | Encoding speed/compression tradeoff |
| `-r`              | Frame rate                          |
| `-s`              | Video size                          |
| `-ar`             | Audio sample rate                   |
| `-ac`             | Audio channels                      |
| `-ss`             | Seek/start time                     |
| `-t`              | Duration                            |
| `-to`             | End position                        |
| `-y`              | Overwrite                           |
| `-n`              | Never overwrite                     |
| `-metadata`       | Metadata                            |
| `-filter_complex` | Complex filtering                   |

# 67. The 15 commands I'd memorize first

If you're learning FFmpeg, don't try to memorize 500 commands.

Start with these:

```bash
# 1. Version
ffmpeg -version

# 2. Inspect
ffprobe input.mp4

# 3. Convert
ffmpeg -i input.mkv output.mp4

# 4. Remux
ffmpeg -i input.mkv -c copy output.mp4

# 5. Extract audio
ffmpeg -i input.mp4 -vn output.mp3

# 6. Remove audio
ffmpeg -i input.mp4 -an output.mp4

# 7. Resize
ffmpeg -i input.mp4 -vf "scale=1280:-2" output.mp4

# 8. Extract frame
ffmpeg -ss 00:00:10 -i input.mp4 -frames:v 1 frame.jpg

# 9. Cut
ffmpeg -ss 00:01:00 -i input.mp4 -t 00:01:00 output.mp4

# 10. Fast cut
ffmpeg -ss 00:01:00 -i input.mp4 -t 00:01:00 -c copy output.mp4

# 11. H.264
ffmpeg -i input.mkv -c:v libx264 -crf 23 -c:a aac output.mp4

# 12. H.265
ffmpeg -i input.mkv -c:v libx265 -crf 28 -c:a aac output.mp4

# 13. MP3
ffmpeg -i input.wav -c:a libmp3lame -b:a 192k output.mp3

# 14. List encoders
ffmpeg -encoders

# 15. List formats
ffmpeg -formats
```

# 68. A practical learning roadmap

I'd learn FFmpeg in this order:

```text
Level 1
│
├── ffmpeg -i
├── ffprobe
├── containers
└── codecs

Level 2
│
├── -c:v
├── -c:a
├── -c copy
├── -b:v
├── -b:a
└── -crf

Level 3
│
├── -ss
├── -t
├── -to
├── -vn
├── -an
└── -map

Level 4
│
├── -vf
├── -af
├── scale
├── crop
├── fps
└── overlay

Level 5
│
├── filter_complex
├── concat
├── subtitles
├── multiple streams
└── metadata

Level 6
│
├── hardware acceleration
├── streaming
├── screen capture
└── live processing

Level 7
│
├── Bash automation
├── Node.js integration
├── Docker
├── media APIs
└── production pipelines
```

### For your particular use case

I'd focus heavily on **`ffprobe` + stream mapping + codecs + filters + Bash + Node.js integration**. That's where FFmpeg stops being merely a "video converter" and becomes a genuinely useful component in a software/media-processing stack.

The best reference to keep bookmarked is the **official FFmpeg documentation**, which includes the complete command-line reference, codecs, formats, filters, protocols, devices, and libraries. [Official FFmpeg Documentation](https://www.ffmpeg.org/documentation.html) [FFmpeg Download / Releases](https://www.ffmpeg.org/download.html)
