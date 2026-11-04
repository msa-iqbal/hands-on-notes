# yt-dlp + FFmpeg on Ubuntu

A practical command reference for downloading, converting, organizing, and troubleshooting video and audio using **yt-dlp + FFmpeg**.

# 🗁 Introduction

## What is yt-dlp?

**yt-dlp** is a command-line audio/video downloader supporting thousands of websites.

It supports:

- Video downloads
- Audio downloads
- Playlists
- Channels
- Subtitles
- Thumbnails
- Metadata
- Format selection
- Download archives
- Output templates
- Browser cookies
- Post-processing

**Basic Syntax**

```bash
yt-dlp [OPTIONS] URL
```

## What is FFmpeg?

**FFmpeg** is a multimedia framework used for:

- Video conversion
- Audio conversion
- Stream merging
- Encoding
- Decoding
- Transcoding
- Metadata processing
- Subtitle processing
- Thumbnail embedding

FFmpeg is particularly important because many platforms provide video and audio as separate streams.

```text
Video Stream
     +
Audio Stream
     |
     v
  FFmpeg
     |
     v
Final Media
```

## How yt-dlp + FFmpeg Work Together

```text
             yt-dlp
                |
       ┌────────┼────────┐
       ↓        ↓        ↓
     Video    Audio   Subtitles
       |        |        |
       └────────┼────────┘
                ↓
             FFmpeg
                |
       ┌────────┼────────┐
       ↓        ↓        ↓
     Merge   Convert   Process
                |
                ↓
           Final Media
```

In simple terms:

```text
yt-dlp  →  Download
FFmpeg  →  Merge / Convert / Process
FFprobe →  Inspect
```

## Prerequisites

This guide assumes:

- Ubuntu Linux
- Internet connection
- Terminal access
- `sudo` privileges

Check your Ubuntu version:

```bash
lsb_release -a
```

or:

```bash
cat /etc/os-release
```

# 🗁 Installation & Verification

## Install FFmpeg

Update package lists and install FFmpeg:

```bash
sudo apt update
sudo apt install ffmpeg -y
```

Optional additional codec support:

```bash
sudo apt install libavcodec-extra -y
```

Verify the installation:

```text
ffmpeg -version
ffprobe -version
which ffmpeg
which ffprobe
```

## Install yt-dlp

Install `curl`:

```bash
sudo apt install curl -y
```

Download the latest standalone yt-dlp binary:

```bash
sudo curl -L \
  https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp \
  -o /usr/local/bin/yt-dlp
```

Make it executable:

```bash
sudo chmod a+rx /usr/local/bin/yt-dlp
```

Verify the installation:

```bash
yt-dlp --version
which yt-dlp
```

Expected binary path:

```text
/usr/local/bin/yt-dlp
```

## Install Deno

Deno provides the JavaScript runtime needed for modern YouTube extraction in yt-dlp.

Install Deno:

```bash
curl -fsSL https://deno.land/install.sh | sh
```

Add Deno to the current terminal's PATH:

```bash
export PATH="$HOME/.deno/bin:$PATH"
```

Make the PATH configuration permanent:

```bash
echo 'export PATH="$HOME/.deno/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Verify the installation:

```bash
deno --version
which deno
```

Expected executable path:

```text
/home/YOUR_USERNAME/.deno/bin/deno
```

## Verify All Installations

Run:

```bash
ffmpeg -version
ffprobe -version
yt-dlp --version
deno --version
```

## Download YouTube Videos (Testing)

Use this command to download the best available MP4 video up to 1080p with M4A audio:

```bash
yt-dlp \
  --remote-components ejs:npm \
  -f "bv*[height<=1080][ext=mp4]+ba[ext=m4a]/b[height<=1080][ext=mp4]/best" \
  --merge-output-format mp4 \
  --embed-thumbnail \
  --no-playlist \
  "https://youtu.be/VIDEO_ID"
```

Replace `https://youtu.be/VIDEO_ID` with the actual YouTube video URL.

**Command options**

- `--remote-components ejs:npm`: enables yt-dlp's remote JavaScript challenge-solving components.
- `-f`: selects MP4 video and M4A audio, preferring resolutions up to 1080p.
- `--merge-output-format mp4`: merges separate video and audio streams into an MP4 container.
- `--embed-thumbnail`: embeds the thumbnail when supported.
- `--no-playlist`: downloads only the specified video.
## Update yt-dlp

Update the standalone binary when needed:

```bash
sudo yt-dlp -U
```

Verify the updated version:

```bash
yt-dlp --version
```

## Troubleshooting

If Deno is not found:

```
export PATH="$HOME/.deno/bin:$PATH"
source ~/.bashrc
```

If YouTube extraction still fails, test the video:

```bash
yt-dlp \
  --remote-components ejs:npm \
  --simulate \
  --verbose \
  "https://youtu.be/VIDEO_ID"
```

If the video remains unavailable, check whether it is private, deleted, region-restricted, or requires authentication.
# 🗁 Videos

## Basic Download

The simplest video download:

```bash
yt-dlp "YOUTUBE_URL"
```

The file is normally saved in the current directory.

Check:

```bash
ls
```

## Best Quality

```bash
yt-dlp -f "bv*+ba/b" "YOUTUBE_URL"
```

**Format meaning**

```text
bv* = best video
ba  = best audio
/b  = fallback to best combined format
```

## List Available Formats

Before selecting a specific resolution:

```bash
yt-dlp -F "YOUTUBE_URL"
```

Equivalent:

```bash
yt-dlp --list-formats "YOUTUBE_URL"
```

Example:

```text
ID     EXT     RESOLUTION
--------------------------
18     mp4     360p
22     mp4     720p
137    mp4     1080p
271    webm    1440p
313    webm    2160p
140    m4a     audio
251    webm    audio
```

Format IDs vary between videos, so don't blindly reuse IDs.

## Specific Resolution

### Maximum 720p

```bash
yt-dlp \
  -f "bv*[height<=720]+ba/b[height<=720]" \
  --merge-output-format mp4 \
  "YOUTUBE_URL"
```

### Maximum 1080p

```bash
yt-dlp \
  -f "bv*[height<=1080]+ba/b[height<=1080]" \
  --merge-output-format mp4 \
  "YOUTUBE_URL"
```

### Maximum 1440p

```bash
yt-dlp \
  -f "bv*[height<=1440]+ba/b" \
  --merge-output-format mp4 \
  "YOUTUBE_URL"
```

### Maximum 4K

```bash
yt-dlp \
  -f "bv*[height<=2160]+ba/b" \
  --merge-output-format mp4 \
  "YOUTUBE_URL"
```

### Maximum 8K

```bash
yt-dlp \
  -f "bv*[height<=4320]+ba/b" \
  --merge-output-format mp4 \
  "YOUTUBE_URL"
```

## MP4

General MP4:

```bash
yt-dlp \
  -f "bv*+ba/b" \
  --merge-output-format mp4 \
  "YOUTUBE_URL"
```

Compatibility-focused MP4:

```bash
yt-dlp \
  -f "bv*[ext=mp4]+ba[ext=m4a]/b[ext=mp4]" \
  --merge-output-format mp4 \
  "YOUTUBE_URL"
```

### 1080p MP4

### Single Video (Recommended)

```bash
yt-dlp \
  -f "bv*[height=1080][ext=mp4]+ba[ext=m4a]/b[height=1080][ext=mp4]" \
  --merge-output-format mp4 \
  --embed-thumbnail \
  --no-playlist \
  "YOUTUBE_URL"
```

**What it does**

```text
Video     → 1080p MP4
Audio     → M4A
Container → MP4
Thumbnail → Embedded
Playlist  → Disabled
```

## 720p MP4

### Single Video (Recommended)

```bash
yt-dlp \
  -f "bv*[height=720][ext=mp4]+ba[ext=m4a]/b[height=720][ext=mp4]" \
  --merge-output-format mp4 \
  --embed-thumbnail \
  --no-playlist \
  "YOUTUBE_URL"
```

### Playlist (Recommended)

```bash
yt-dlp \
  -f "bv*[height=720][ext=mp4]+ba[ext=m4a]/b[height=720][ext=mp4]" \
  --merge-output-format mp4 \
  --write-thumbnail \
  --convert-thumbnails jpg \
  --embed-thumbnail \
  -o "%(playlist)s/%(playlist_index)02d - %(title)s.%(ext)s" \
  "PLAYLIST_URL" && \
find . -type f -name '*.jpg' -delete
```

Note: Thumbnail embedding may not work reliably with every MP4 combination/container.

## Playlists

Download an entire playlist:

```bash
yt-dlp "PLAYLIST_URL"
```

Maximum quality:

```bash
yt-dlp \
  -f "bv*+ba/b" \
  --merge-output-format mp4 \
  "PLAYLIST_URL"
```

### Specific playlist items

Items 1–10:

```bash
yt-dlp \
  --playlist-items 1-10 \
  "PLAYLIST_URL"
```

Specific items:

```bash
yt-dlp \
  --playlist-items 1,5,10 \
  "PLAYLIST_URL"
```

## Subtitles

List subtitles:

```bash
yt-dlp --list-subs "YOUTUBE_URL"
```

Download English subtitles:

```bash
yt-dlp \
  --write-subs \
  --sub-langs en \
  "YOUTUBE_URL"
```

Download automatic subtitles:

```bash
yt-dlp \
  --write-auto-subs \
  --sub-langs en \
  "YOUTUBE_URL"
```

Convert subtitles to SRT:

```bash
yt-dlp \
  --write-subs \
  --sub-langs en \
  --convert-subs srt \
  "YOUTUBE_URL"
```

Embed subtitles:

```bash
yt-dlp \
  -f "bv*+ba/b" \
  --merge-output-format mp4 \
  --write-subs \
  --sub-langs en \
  --embed-subs \
  "YOUTUBE_URL"
```

## Thumbnails & Metadata

Download thumbnail:

```bash
yt-dlp \
  --write-thumbnail \
  "YOUTUBE_URL"
```

Download and convert to JPG:

```bash
yt-dlp \
  --write-thumbnail \
  --convert-thumbnails jpg \
  "YOUTUBE_URL"
```

Embed metadata:

```bash
yt-dlp \
  --embed-metadata \
  "YOUTUBE_URL"
```

Embed metadata + thumbnail:

```bash
yt-dlp \
  --embed-metadata \
  --embed-thumbnail \
  "YOUTUBE_URL"
```

## Output Organization

Create a dedicated YouTube directory:

```bash
mkdir -p ~/Downloads/YouTube
```

Download there:

```bash
yt-dlp \
  -P ~/Downloads/YouTube \
  "YOUTUBE_URL"
```

Use video title as filename:

```bash
yt-dlp \
  -o "%(title)s.%(ext)s" \
  "YOUTUBE_URL"
```

Useful output fields:

```text
%(title)s
%(id)s
%(uploader)s
%(upload_date)s
%(playlist)s
%(playlist_index)s
%(ext)s
```

### Organize by channel

```bash
yt-dlp \
  -P "~/Downloads/YouTube/%(uploader)s" \
  -o "%(title)s.%(ext)s" \
  "YOUTUBE_URL"
```

Result:

```text
YouTube/
└── Channel Name/
    ├── Video One.mp4
    ├── Video Two.mp4
    └── Video Three.mp4
```

### Organize playlists

```bash
yt-dlp \
  -P "~/Downloads/YouTube/%(uploader)s/%(playlist)s" \
  -o "%(playlist_index)s - %(title)s.%(ext)s" \
  "PLAYLIST_URL"
```

Result:

```text
YouTube/
└── Channel/
    └── Playlist/
        ├── 01 - Video One.mp4
        ├── 02 - Video Two.mp4
        └── 03 - Video Three.mp4
```

# 🗁 Audio

## Audio Only

Best available audio:

```bash
yt-dlp -f "ba" "YOUTUBE_URL"
```

Equivalent:

```bash
yt-dlp -f "bestaudio" "YOUTUBE_URL"
```

## MP3

Basic MP3 extraction:

```bash
yt-dlp \
  -x \
  --audio-format mp3 \
  "YOUTUBE_URL"
```

Here:

```text
-x
```

means:

> Extract audio.

### MP3 320K (Recommended)

```bash
yt-dlp \
  -f "ba/b" \
  --extract-audio \
  --audio-format mp3 \
  --audio-quality 320K \
  --no-playlist \
  -o "%(title)s.%(ext)s" \
  "YOUTUBE_URL"
```

Output:

```text
Song Title.mp3
```

**Important**

`320K` specifies the target MP3 bitrate. It does **not** create additional quality beyond the original source.

### MP3 + Thumbnail

```bash
yt-dlp \
  -f "ba/b" \
  --extract-audio \
  --audio-format mp3 \
  --audio-quality 320K \
  --write-thumbnail \
  --convert-thumbnails jpg \
  --embed-thumbnail \
  --no-playlist \
  -o "%(title)s.%(ext)s" \
  "YOUTUBE_URL"
```

**Thumbnail workflow**

```text
YouTube
   ↓
Download audio
   ↓
Convert → MP3
   ↓
Download thumbnail
   ↓
Convert → JPG
   ↓
Embed JPG into MP3
```

## Temporary Thumbnail Cleanup (Recommended)

> NOTE: Deletes JPG files in the current directory. For safety, use a dedicated download directory.

For a single video:

```bash
yt-dlp \
  -f "ba/b" \
  --extract-audio \
  --audio-format mp3 \
  --audio-quality 320K \
  --write-thumbnail \
  --convert-thumbnails jpg \
  --embed-thumbnail \
  --no-playlist \
  -o "%(title)s.%(ext)s" \
  "YOUTUBE_URL" && \
rm -f -- *.jpg
```

**Why `&&`?**

```bash
command1 && command2
```

means:

> Execute `command2` only when `command1` succeeds.

 **JPG cleanup**

```bash
rm -f -- *.jpg
```

## Playlist MP3 (Recommended)

### MP3 + 320K + Thumbnail

```bash
yt-dlp \
  -f "ba/b" \
  --extract-audio \
  --audio-format mp3 \
  --audio-quality 320K \
  --write-thumbnail \
  --convert-thumbnails jpg \
  --embed-thumbnail \
  -o "%(playlist)s/%(playlist_index)02d - %(title)s.%(ext)s" \
  "PLAYLIST_URL" && \
find . -type f -name '*.jpg' -delete
```

**Output structure**

```text
Playlist Name/
├── 01 - Song One.mp3
├── 02 - Song Two.mp3
├── 03 - Song Three.mp3
└── ...
```

**Output template**

```bash
%(playlist)s/
```

Creates a playlist directory.

```bash
%(playlist_index)02d
```

Creates two-digit numbering:

```text
01
02
03
...
09
10
11
```

```bash
%(title)s
```

Uses the video/song title.

## Other Audio Formats

### M4A

```bash
yt-dlp \
  -x \
  --audio-format m4a \
  "YOUTUBE_URL"
```

### FLAC

```bash
yt-dlp \
  -x \
  --audio-format flac \
  "YOUTUBE_URL"
```

### WAV

```bash
yt-dlp \
  -x \
  --audio-format wav \
  "YOUTUBE_URL"
```

FFmpeg performs the audio conversion.

# 🗁 Troubleshooting

## `yt-dlp: command not found`

Check:

```bash
ls -l /usr/local/bin/yt-dlp
```

Check PATH:

```bash
echo $PATH
```

Temporarily add `/usr/local/bin`:

```bash
export PATH="/usr/local/bin:$PATH"
```

Then:

```bash
yt-dlp --version
```

## `ffmpeg: command not found`

Install FFmpeg:

```bash
sudo apt update
sudo apt install ffmpeg
```

Verify:

```bash
ffmpeg -version
```

## FFmpeg Is Not Detected

Check:

```bash
which ffmpeg
```

```bash
which ffprobe
```

If necessary:

```bash
yt-dlp \
  --ffmpeg-location /usr/bin \
  "YOUTUBE_URL"
```

## Download Only Gives Low Resolution

First inspect available formats:

```bash
yt-dlp -F "YOUTUBE_URL"
```

Then try:

```bash
yt-dlp \
  -f "bv*+ba/b" \
  --merge-output-format mp4 \
  "YOUTUBE_URL"
```

High-quality downloads frequently require separate video and audio streams, so FFmpeg must be available.

## MP3 Conversion Fails

Check FFmpeg:

```bash
ffmpeg -version
```

Then retry:

```bash
yt-dlp \
  -x \
  --audio-format mp3 \
  "YOUTUBE_URL"
```

## YouTube / Extractor Errors

First update yt-dlp:

```bash
sudo yt-dlp -U
```

Check version:

```bash
yt-dlp --version
```

If the stable version has a current extractor problem, try the nightly build:

```bash
sudo yt-dlp --update-to nightly
```

## Verbose Debugging

When a command fails:

```bash
yt-dlp --verbose "YOUTUBE_URL"
```

For format-related problems:

```bash
yt-dlp \
  --verbose \
  -f "bv*+ba/b" \
  "YOUTUBE_URL"
```

Verbose output can reveal:

- yt-dlp version
- Runtime information
- Extractor
- Selected formats
- FFmpeg
- Network behavior
- Post-processing errors

## Media Inspection with FFprobe

Check FFprobe:

```bash
ffprobe -version
```

Inspect a video:

```bash
ffprobe "video.mp4"
```

Concise output:

```bash
ffprobe -hide_banner "video.mp4"
```

# 🗁 Update yt-dlp and FFmpeg

### Update yt-dlp

```bash
sudo yt-dlp -U
```

### Update FFmpeg

```bash
sudo apt update
sudo apt upgrade
```

Verify:

```bash
yt-dlp --version
```

```bash
ffmpeg -version
```

# 🗁 Conclusion

## Daily Command Cheat Sheet

### Download

```bash
yt-dlp "URL"
```

### Best video + audio

```bash
yt-dlp -f "bv*+ba/b" "URL"
```

### Best MP4

```bash
yt-dlp \
  -f "bv*+ba/b" \
  --merge-output-format mp4 \
  "URL"
```

### 1080p MP4

```bash
yt-dlp \
  -f "bv*[height=1080][ext=mp4]+ba[ext=m4a]/b[height=1080][ext=mp4]" \
  --merge-output-format mp4 \
  --no-playlist \
  "URL"
```

### MP3 320K

```bash
yt-dlp \
  -x \
  --audio-format mp3 \
  --audio-quality 320K \
  "URL"
```

### MP3 + Thumbnail

```bash
yt-dlp \
  -x \
  --audio-format mp3 \
  --audio-quality 320K \
  --write-thumbnail \
  --convert-thumbnails jpg \
  --embed-thumbnail \
  "URL"
```

### List formats

```bash
yt-dlp -F "URL"
```

### Playlist

```bash
yt-dlp "PLAYLIST_URL"
```

### Continue download

```bash
yt-dlp -c "URL"
```

### Download archive

```bash
yt-dlp \
  --download-archive archive.txt \
  "PLAYLIST_URL"
```

### Browser cookies

```bash
yt-dlp \
  --cookies-from-browser chrome \
  "URL"
```

### Debug

```bash
yt-dlp --verbose "URL"
```

### Update

```bash
sudo yt-dlp -U
```

# 🗁 Recommended Workflow

For everyday usage, think of the workflow as:

```text
                 ┌───────────────┐
                 │    Ubuntu     │
                 └───────┬───────┘
                         │
             ┌───────────┴───────────┐
             ↓                       ↓
        ┌─────────┐             ┌─────────┐
        │ yt-dlp  │             │ FFmpeg  │
        └────┬────┘             └────┬────┘
             │                       │
          Download              Merge / Convert
             │                       │
             └───────────┬───────────┘
                         ↓
                  ┌──────────────┐
                  │ Final Media  │
                  │ MP4 / MP3    │
                  └──────────────┘
```

The most important commands to remember are:

```bash
yt-dlp "URL"
```

```bash
yt-dlp -F "URL"
```

```bash
yt-dlp -f "bv*+ba/b" "URL"
```

```bash
yt-dlp -x --audio-format mp3 "URL"
```

```bash
sudo yt-dlp -U
```

When something goes wrong:

```bash
yt-dlp -F "URL"
```

then:

```bash
yt-dlp --verbose "URL"
```

This gives you a simple progression:

```text
Download
   ↓
Choose Format
   ↓
Download Video / Audio
   ↓
FFmpeg Processing
   ↓
Organize Output
   ↓
Troubleshoot if Necessary
```

For a Linux workstation, **yt-dlp + FFmpeg + FFprobe + a small configuration file** provides a flexible, scriptable multimedia workflow.
