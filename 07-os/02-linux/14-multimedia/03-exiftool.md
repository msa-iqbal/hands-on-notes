# ExifTool — Complete Guide

**ExifTool** is one of the most powerful command-line tools for reading, writing, copying, and managing metadata in photographs, videos, PDFs, audio files, and many other file types.

For a Linux photography workflow, I would use:

```p
darktable   → RAW development
GIMP        → pixel-level editing
ImageMagick → image processing/automation
ExifTool    → metadata
digiKam     → photo catalog
```

The key idea is:

> **ImageMagick works primarily on pixels; ExifTool works primarily on metadata.**

# 1. What is ExifTool?

ExifTool is a Perl-based command-line application developed by Phil Harvey.

It can read and write metadata such as:

- EXIF
- IPTC
- XMP
- GPS
- ICC profiles
- MakerNotes
- Camera information
- Lens information
- Copyright
- Author/creator
- Title
- Description
- Keywords
- Date/time
- File metadata
- Video metadata
- Audio metadata
- PDF metadata

It supports a very large number of file and metadata formats.

# 2. Install ExifTool on Ubuntu

On Ubuntu/Debian:

```bash
sudo apt update
sudo apt install libimage-exiftool-perl
```

Check installation:

```bash
exiftool -ver
```

For example:

```text
13.x
```

Find the executable:

```bash
which exiftool
```

Usually:

```text
/usr/bin/exiftool
```

Get help:

```bash
exiftool -h
```

Basic documentation:

```bash
man exiftool
```

# 3. Basic ExifTool syntax

The basic structure is:

```bash
exiftool [OPTIONS] [FILE...]
```

For reading:

```bash
exiftool photo.jpg
```

For writing:

```bash
exiftool -Artist="Muhammad S.A. Iqbal" photo.jpg
```

For deleting a tag:

```bash
exiftool -Artist= photo.jpg
```

For copying metadata:

```bash
exiftool -TagsFromFile source.jpg destination.jpg
```

# 4. Read all metadata

The first command to learn:

```bash
exiftool photo.jpg
```

Example output:

```text
ExifTool Version Number         : 13.x
File Name                       : photo.jpg
File Size                       : 4.2 MB
File Type                       : JPEG
Image Width                     : 6000
Image Height                    : 4000
Make                            : Canon
Camera Model Name               : EOS R5
Date/Time Original              : 2026:09:05 18:30:00
Exposure Time                   : 1/500
F Number                        : 2.8
ISO                             : 200
Focal Length                    : 50.0 mm
Artist                          : Muhammad S.A. Iqbal
Copyright                       : © 2026 Muhammad S.A. Iqbal
GPS Latitude                    : ...
GPS Longitude                   : ...
```

# 5. Detailed metadata

Use:

```bash
exiftool -a -u -g1 photo.jpg
```

These options are important:

```text
-a   Allow duplicate tags
-u   Show unknown tags
-g1  Group tags by metadata family
```

This is one of my preferred commands when investigating an unfamiliar photograph.

# 6. EXIF only

```bash
exiftool -EXIF:all photo.jpg
```

Or:

```bash
exiftool -EXIF:* photo.jpg
```

The first form is particularly useful when you want to inspect the EXIF family.

# 7. IPTC metadata

```bash
exiftool -IPTC:all photo.jpg
```

IPTC is commonly used for professional photographic metadata such as:

- Caption
- Headline
- Byline
- Copyright
- Keywords
- Location
- Credit
- Source

# 8. XMP metadata

```bash
exiftool -XMP:all photo.jpg
```

XMP is particularly important in professional photography workflows and applications such as Adobe Lightroom.

# 9. GPS metadata

```bash
exiftool -GPS:all photo.jpg
```

You can get just the coordinates:

```bash
exiftool -GPSLatitude -GPSLongitude photo.jpg
```

Or:

```bash
exiftool -GPSPosition photo.jpg
```

# 10. Get one specific tag

This is where ExifTool becomes extremely useful.

Camera manufacturer:

```bash
exiftool -Make photo.jpg
```

Camera model:

```bash
exiftool -Model photo.jpg
```

Lens:

```bash
exiftool -LensModel photo.jpg
```

ISO:

```bash
exiftool -ISO photo.jpg
```

Aperture:

```bash
exiftool -FNumber photo.jpg
```

Shutter speed:

```bash
exiftool -ExposureTime photo.jpg
```

Focal length:

```bash
exiftool -FocalLength photo.jpg
```

Date:

```bash
exiftool -DateTimeOriginal photo.jpg
```

# 11. Get multiple tags

```bash
exiftool \
  -Make \
  -Model \
  -LensModel \
  -ISO \
  -FNumber \
  -ExposureTime \
  -FocalLength \
  photo.jpg
```

# 12. Get metadata without labels

Use `-s`:

```bash
exiftool -s -Make -Model photo.jpg
```

Example:

```text
Make : Canon
Model : EOS R5
```

Use `-s -s`:

```bash
exiftool -s -s -Make -Model photo.jpg
```

Use `-s -s -s`:

```bash
exiftool -s -s -s -Make -Model photo.jpg
```

This is useful for scripts.

# 13. JSON output

This is extremely useful for developers.

```bash
exiftool -json photo.jpg
```

Output:

```json
[
  {
    "SourceFile": "photo.jpg",
    "FileName": "photo.jpg",
    "FileSize": "4.2 MB",
    "Make": "Canon",
    "Model": "EOS R5",
    "ISO": 200,
    "FNumber": 2.8
  }
]
```

This makes ExifTool easy to integrate with:

- Node.js
- Python
- PHP
- databases
- APIs
- web applications

# 14. CSV output

```bash
exiftool -csv *.jpg > metadata.csv
```

This is excellent for building a photography inventory.

For example:

```text
FileName,Make,Model,ISO,FNumber,FocalLength
IMG_001.jpg,Canon,EOS R5,100,2.8,50mm
IMG_002.jpg,Canon,EOS R5,400,4.0,85mm
```

You can then open the CSV in LibreOffice Calc, Excel, Python, etc.

# 15. HTML output

ExifTool can generate HTML:

```bash
exiftool -htmlDump photo.jpg > metadata.html
```

This is useful when debugging the actual metadata structure.

# 16. Find available tags

One of the most important ExifTool skills is discovering tag names.

Use:

```bash
exiftool -list
```

You can also search documentation:

```bash
exiftool -list | grep -i copyright
```

For a specific metadata family:

```bash
exiftool -list | grep -i GPS
```

# 17. Identify writable tags

Not every metadata tag can necessarily be written.

Use:

```bash
exiftool -listw
```

This displays writable tags.

This distinction matters:

```text
Readable tag
      ≠
Writable tag
```

# 18. Write your name

For example:

```bash
exiftool -Artist="Muhammad S.A. Iqbal" photo.jpg
```

Verify:

```bash
exiftool -Artist photo.jpg
```

# 19. Add copyright

```bash
exiftool \
  -Copyright="© 2026 Muhammad S.A. Iqbal" \
  photo.jpg
```

Verify:

```bash
exiftool -Copyright photo.jpg
```

# 20. Add title

```bash
exiftool \
  -Title="Sunset Photography" \
  photo.jpg
```

# 21. Add description

```bash
exiftool \
  -Description="Sunset over the landscape" \
  photo.jpg
```

# 22. Add keywords

```bash
exiftool \
  -Keywords="sunset,landscape,nature,photography" \
  photo.jpg
```

You can also write multiple keyword values:

```bash
exiftool \
  -Keywords="sunset" \
  -Keywords="landscape" \
  -Keywords="nature" \
  photo.jpg
```

# 23. IPTC keywords

For professional photography workflows:

```bash
exiftool \
  -IPTC:Keywords="sunset" \
  -IPTC:Keywords="landscape" \
  -IPTC:Keywords="nature" \
  photo.jpg
```

# 24. XMP keywords

You can also write XMP:

```bash
exiftool \
  -XMP:Subject="sunset" \
  -XMP:Subject="landscape" \
  photo.jpg
```

This distinction is important because:

```text
EXIF
IPTC
XMP
```

are different metadata systems.

# 25. Set creator/artist information

A practical photography metadata set:

```bash
exiftool \
  -Artist="Muhammad S.A. Iqbal" \
  -Copyright="© 2026 Muhammad S.A. Iqbal" \
  -Title="Sunset" \
  -Description="Sunset landscape photography" \
  photo.jpg
```

# 26. Set metadata across EXIF/IPTC/XMP

For a professional workflow, you may want corresponding fields in several metadata namespaces.

For example:

```bash
exiftool \
  -EXIF:Artist="Muhammad S.A. Iqbal" \
  -EXIF:Copyright="© 2026 Muhammad S.A. Iqbal" \
  -IPTC:By-line="Muhammad S.A. Iqbal" \
  -IPTC:CopyrightNotice="© 2026 Muhammad S.A. Iqbal" \
  -XMP:Creator="Muhammad S.A. Iqbal" \
  -XMP:Rights="© 2026 Muhammad S.A. Iqbal" \
  photo.jpg
```

This is more explicit than relying on ExifTool's tag shortcuts.

# 27. Delete a metadata field

To remove Artist:

```bash
exiftool -Artist= photo.jpg
```

Remove Description:

```bash
exiftool -Description= photo.jpg
```

Remove GPS:

```bash
exiftool -GPS:all= photo.jpg
```

# 28. Remove all metadata

You can remove all metadata with:

```bash
exiftool -all= photo.jpg
```

But there is an important distinction between:

```bash
exiftool -all= photo.jpg
```

and:

```bash
exiftool -all= -overwrite_original photo.jpg
```

The second version prevents ExifTool from creating its usual backup.

I'd normally **not use `-overwrite_original` while experimenting**.

# 29. Remove metadata from copies

A safer approach is:

```bash
mkdir clean
```

Then:

```bash
exiftool \
  -all= \
  -o clean/photo.jpg \
  photo.jpg
```

Your original remains untouched.

This is an excellent workflow for privacy processing.

# 30. Remove GPS but preserve everything else

This is much better than deleting all metadata:

```bash
exiftool -GPS:all= photo.jpg
```

You can verify:

```bash
exiftool -GPS:all photo.jpg
```

# 31. Change date/time

A common photography problem is an incorrect camera clock.

Set the original capture time:

```bash
exiftool \
  "-DateTimeOriginal=2026:09:05 18:30:00" \
  photo.jpg
```

You may also want to synchronize related date fields:

```bash
exiftool \
  "-DateTimeOriginal=2026:09:05 18:30:00" \
  "-CreateDate=2026:09:05 18:30:00" \
  "-ModifyDate=2026:09:05 18:30:00" \
  photo.jpg
```

Note the EXIF date format:

```text
YYYY:MM:DD HH:MM:SS
```

not:

```text
YYYY-MM-DD
```

# 32. Shift photo dates

This is extremely useful if a camera clock was wrong.

For example, shift the original capture time by one hour:

```bash
exiftool "-DateTimeOriginal+=1:0:0" photo.jpg
```

Shift by one day:

```bash
exiftool "-DateTimeOriginal+=0:0:0 1" photo.jpg
```

For large batches, ExifTool's date-shifting functionality becomes extremely powerful.

# 33. GPS coordinates

You can write GPS coordinates.

For example:

```bash
exiftool \
  -GPSLatitude=23.8103 \
  -GPSLatitudeRef=N \
  -GPSLongitude=90.4125 \
  -GPSLongitudeRef=E \
  photo.jpg
```

You can then check:

```bash
exiftool -GPS:all photo.jpg
```

# 34. GPS in degrees/minutes/seconds

ExifTool can also work with coordinates such as:

```text
23 deg 48' 37.08" N
90 deg 24' 45" E
```

For example:

```bash
exiftool \
  '-GPSLatitude=23 deg 48 min 37.08 sec' \
  -GPSLatitudeRef=N \
  '-GPSLongitude=90 deg 24 min 45 sec' \
  -GPSLongitudeRef=E \
  photo.jpg
```

# 35. Copy metadata from one photo to another

Suppose:

```text
original.jpg
edited.jpg
```

Copy metadata:

```bash
exiftool \
  -TagsFromFile original.jpg \
  edited.jpg
```

This is incredibly useful after editing photographs in another application.

# 36. Copy only selected metadata

Instead of copying everything:

```bash
exiftool \
  -TagsFromFile original.jpg \
  -EXIF:DateTimeOriginal \
  -EXIF:Make \
  -EXIF:Model \
  -EXIF:LensModel \
  edited.jpg
```

This gives you much better control.

# 37. Copy metadata but exclude certain fields

For example:

```bash
exiftool \
  -TagsFromFile original.jpg \
  --GPS:all \
  edited.jpg
```

Conceptually:

```text
Original
   │
   ├── EXIF ──────────┐
   ├── IPTC           │
   ├── XMP            ├──> Edited image
   ├── GPS ── X       │
   └── ICC ───────────┘
```

# 38. Copy metadata to an entire directory

```bash
exiftool \
  -TagsFromFile original.jpg \
  /path/to/photos/
```

For complex batch operations, use a loop or ExifTool's conditional/file matching capabilities rather than assuming every file should receive identical metadata.

# 39. Recursive processing

To process directories recursively:

```bash
exiftool -r /path/to/photos
```

For example:

```bash
exiftool -r -GPS:all= /home/user/Pictures
```

This processes files under the directory tree.

Use this carefully on a real photo library.

# 40. Process only JPEG files

```bash
exiftool -ext jpg -ext jpeg /path/to/photos
```

Recursive:

```bash
exiftool -r -ext jpg -ext jpeg /path/to/photos
```

# 41. Process only RAW files

For example:

```bash
exiftool -r -ext CR3 -ext CR2 -ext NEF -ext ARW /path/to/photos
```

Common RAW extensions include:

```text
CR2
CR3
NEF
ARW
RAF
ORF
RW2
DNG
```

# 42. Rename files using metadata

This is one of ExifTool's killer features.

For example:

```bash
exiftool '-FileName<DateTimeOriginal' -d '%Y-%m-%d_%H-%M-%S%%-c.%%e' *.jpg
```

You can turn:

```text
IMG_4821.jpg
```

into something like:

```text
2026-09-05_18-30-00.jpg
```

The `%-c` component helps avoid collisions by adding a copy number when necessary.

# 43. Organize photos by date

You can move files into directories based on capture date.

For example:

```bash
exiftool \
  '-Directory<DateTimeOriginal' \
  -d '/home/user/Pictures/%Y/%m/%d' \
  *.jpg
```

You can create a structure like:

```text
Pictures/
├── 2026/
│   ├── 08/
│   │   ├── 20/
│   │   └── 21/
│   └── 09/
│       └── 05/
```

This is extremely useful for building a structured photo archive.

# 44. Rename based on camera model

For example:

```bash
exiftool '-FileName<Model.%e' *.jpg
```

This concept becomes much more powerful when combined with date/time and sequence numbers.

# 45. File modification time

ExifTool can also work with filesystem timestamps.

View:

```bash
exiftool -FileModifyDate photo.jpg
```

You can set it from EXIF capture time:

```bash
exiftool '-FileModifyDate<DateTimeOriginal' photo.jpg
```

This is useful when organizing old photography archives.

# 46. Copy EXIF date to file modification date

For a directory:

```bash
exiftool '-FileModifyDate<DateTimeOriginal' *.jpg
```

Now the Linux filesystem modification time corresponds to the photo's capture time.

# 47. Metadata in videos

ExifTool isn't limited to photographs.

Try:

```bash
exiftool video.mp4
```

You may see:

```text
File Type
File Size
Duration
Video Width
Video Height
Frame Rate
Codec
Creation Date
GPS
Camera information
```

For example:

```bash
exiftool \
  -CreateDate \
  -Duration \
  -ImageWidth \
  -ImageHeight \
  video.mp4
```

This pairs nicely with your **FFmpeg + ImageMagick** toolkit.

# 48. Metadata in audio

Try:

```bash
exiftool song.mp3
```

You can inspect:

```bash
exiftool \
  -Title \
  -Artist \
  -Album \
  -Genre \
  song.mp3
```

For example:

```bash
exiftool \
  -Title="My Song" \
  -Artist="Artist Name" \
  -Album="My Album" \
  song.mp3
```

# 49. Metadata in PDFs

ExifTool can inspect PDF metadata:

```bash
exiftool document.pdf
```

You may encounter:

```text
Author
Title
Subject
Keywords
Creator
Producer
Create Date
Modify Date
```

For example:

```bash
exiftool \
  -Title="My Document" \
  -Author="Muhammad S.A. Iqbal" \
  document.pdf
```

# 50. Metadata databases

For large photo collections, you may want to maintain metadata separately.

ExifTool supports writing metadata to sidecar files in various workflows.

A common concept is:

```text
photo.RAW
photo.XMP
```

The RAW file remains untouched while metadata is stored in an XMP sidecar.

This is especially useful for RAW photography workflows.

# 51. XMP sidecar files

For example, you can write XMP metadata to a sidecar:

```bash
exiftool -o %d%f.xmp \
  -XMP:all \
  photo.jpg
```

However, exact sidecar workflows depend on the source format and the metadata you want to preserve.

For serious RAW workflows, I'd recommend understanding **XMP sidecars** before using them across an entire library.

# 52. Preserve original files

By default, ExifTool protects you by creating backup copies when modifying files.

For example:

```bash
exiftool -Artist="Muhammad S.A. Iqbal" photo.jpg
```

You may end up with:

```text
photo.jpg
photo.jpg_original
```

This is very useful while learning.

# 53. Disable backup

Once you're confident:

```bash
exiftool -overwrite_original \
  -Artist="Muhammad S.A. Iqbal" \
  photo.jpg
```

Don't make `-overwrite_original` your default habit until you're comfortable with your commands.

# 54. Dry-run before changing files

For large operations, previewing the intended filename changes is particularly useful.

For example:

```bash
exiftool \
  -p '$FileName -> $DateTimeOriginal' \
  *.jpg
```

You can use formatted output to inspect what your command will operate on before performing a destructive operation.

# 55. ExifTool and shell scripting

Example:

```bash
#!/usr/bin/env bash

for file in *.jpg; do
    echo "Processing: $file"

    exiftool \
        -Artist="Muhammad S.A. Iqbal" \
        -Copyright="© 2026 Muhammad S.A. Iqbal" \
        "$file"
done
```

Now ExifTool becomes part of your automation pipeline.

# 56. ExifTool + JSON + Node.js

This is particularly relevant to you as a developer.

Run:

```bash
exiftool -json photo.jpg
```

From Node.js:

```javascript
import { execFile } from "node:child_process";

execFile(
  "exiftool",
  ["-json", "photo.jpg"],
  (error, stdout, stderr) => {
    if (error) {
      console.error(error);
      return;
    }

    const metadata = JSON.parse(stdout);

    console.log(metadata[0]);
  }
);
```

You could build an API like:

```text
POST /images
       │
       ▼
   Node.js API
       │
       ├── ExifTool
       │      │
       │      └── Extract metadata
       │
       ├── ImageMagick
       │      │
       │      └── Resize/convert
       │
       ▼
    Database
       │
       ▼
 Object Storage
```

That's a very realistic architecture for a web application.

# 57. ExifTool + Python

```python
import json
import subprocess

result = subprocess.run(
    ["exiftool", "-json", "photo.jpg"],
    capture_output=True,
    text=True,
    check=True,
)

metadata = json.loads(result.stdout)[0]

print(metadata.get("Make"))
print(metadata.get("Model"))
print(metadata.get("ISO"))
```

# 58. ExifTool for a photo archive

Imagine:

```text
Photos/
├── 2024/
├── 2025/
└── 2026/
```

You can inspect the entire library:

```bash
exiftool -r Photos/
```

Export metadata:

```bash
exiftool -r -csv Photos/ > photo-metadata.csv
```

Then analyze the CSV with:

- LibreOffice Calc
- Python/pandas
- PostgreSQL
- DuckDB
- your own application

This is where ExifTool starts looking less like a "photo utility" and more like a **metadata extraction engine**.

# 59. Useful metadata queries

### Camera statistics

```bash
exiftool -Make -Model -LensModel *.jpg
```

### Exposure information

```bash
exiftool \
  -ISO \
  -FNumber \
  -ExposureTime \
  -FocalLength \
  *.jpg
```

### Location

```bash
exiftool \
  -GPSLatitude \
  -GPSLongitude \
  -GPSAltitude \
  *.jpg
```

### Dates

```bash
exiftool \
  -DateTimeOriginal \
  -CreateDate \
  -ModifyDate \
  *.jpg
```

# 60. Metadata search

You can use ExifTool to find photographs matching particular metadata.

For example:

```bash
exiftool \
  -if '$Make eq "Canon"' \
  -filename \
  /home/user/Pictures
```

Or ISO:

```bash
exiftool \
  -if '$ISO > 1600' \
  -filename \
  /home/user/Pictures
```

This becomes extremely powerful for large collections.

# 61. Find photos taken with a specific lens

```bash
exiftool \
  -if '$LensModel =~ /50mm/' \
  -FileName \
  /home/user/Pictures
```

The `-if` expression is one of the features worth learning once you move beyond basic metadata editing.

# 62. Find photos from a specific date

```bash
exiftool \
  -if '$DateTimeOriginal =~ /^2026:09:05/' \
  -FileName \
  /home/user/Pictures
```

# 63. Metadata filtering

You can combine conditions.

For example:

```bash
exiftool \
  -if '$ISO > 800 and $FNumber < 4' \
  -FileName \
  /home/user/Pictures
```

Now you're effectively querying your photo collection.

# 64. Custom output templates

ExifTool's `-p` option is useful.

For example:

```bash
exiftool \
  -p '$FileName | $Make | $Model | $ISO | $FNumber | $FocalLength' \
  *.jpg
```

Output:

```text
IMG_001.jpg | Canon | EOS R5 | 100 | 2.8 | 50mm
IMG_002.jpg | Canon | EOS R5 | 400 | 4.0 | 85mm
```

This is excellent for shell pipelines.

# 65. Create a photography report

For example:

```bash
exiftool \
  -r \
  -csv \
  -FileName \
  -DateTimeOriginal \
  -Make \
  -Model \
  -LensModel \
  -ISO \
  -FNumber \
  -ExposureTime \
  -FocalLength \
  Photos/ > photography-report.csv
```

You now have a searchable dataset.

# 66. The metadata hierarchy you should understand

A modern photograph can contain several metadata families:

```text
                    IMAGE
                      │
       ┌──────────────┼──────────────┐
       │              │              │
      EXIF           IPTC           XMP
       │              │              │
       │              │              │
 Camera settings   Editorial       Workflow
 GPS               Keywords        Ratings
 Date              Caption         Labels
 Lens              Copyright       Editing data
```

There can also be:

```text
ICC Profile
MakerNotes
File System metadata
Container metadata
```

That's why ExifTool is so much more capable than simply running:

```bash
strings photo.jpg
```

# 67. EXIF vs IPTC vs XMP

A useful mental model:

|Metadata|Primarily used for|
|---|---|
|**EXIF**|Camera/technical information|
|**IPTC**|Editorial/professional photography information|
|**XMP**|Extensible workflow metadata|
|**GPS**|Location|
|**ICC**|Color management|
|**MakerNotes**|Manufacturer-specific camera data|

For example:

```text
EXIF
├── Camera
├── Lens
├── ISO
├── Shutter
├── Aperture
└── GPS

IPTC
├── Caption
├── Keywords
├── Credit
└── Copyright

XMP
├── Rating
├── Labels
├── Creator
├── Rights
└── Editing/workflow information
```

# 68. A professional metadata template

For your own photographs, you could establish a standard metadata template:

```text
Creator:
Muhammad S.A. Iqbal

Copyright:
© 2026 Muhammad S.A. Iqbal

Title:
[Photo title]

Description:
[Photo description]

Keywords:
[comma-separated keywords]

Location:
[optional]

Website:
[your portfolio]
```

Then apply it consistently using ExifTool.

# 69. Example complete command

For example:

```bash
exiftool \
  -Artist="Muhammad S.A. Iqbal" \
  -Copyright="© 2026 Muhammad S.A. Iqbal" \
  -Title="Sunset at Dusk" \
  -Description="Landscape photograph captured at sunset." \
  -Keywords="sunset,landscape,nature,photography" \
  photo.jpg
```

Verify:

```bash
exiftool \
  -Artist \
  -Copyright \
  -Title \
  -Description \
  -Keywords \
  photo.jpg
```

# 70. Recommended workflow with ImageMagick

For your Linux media toolkit, I'd structure things like this:

```text
                  ORIGINAL PHOTO
                        │
                        ▼
                    ExifTool
                        │
             Read camera metadata
                        │
                        ▼
                   darktable
                  RAW processing
                        │
                        ▼
                      GIMP
                Manual editing
                        │
                        ▼
                  ImageMagick
             Resize / convert / web
                        │
                        ▼
                    ExifTool
              Metadata verification
                        │
                        ▼
                    FINAL FILE
```

For web publishing:

```bash
magick photo.jpg \
  -auto-orient \
  -resize '1920x1920>' \
  -colorspace sRGB \
  -quality 82 \
  output.webp
```

Then inspect:

```bash
exiftool output.webp
```

If you don't want metadata in the public version, deliberately remove it:

```bash
exiftool -all= output.webp
```

Or, better, selectively remove sensitive metadata such as GPS while retaining copyright information.

# 71. The ExifTool commands I'd memorize

If you don't want to memorize the entire manual, learn these first:

### Read

```bash
exiftool photo.jpg
```

### Detailed

```bash
exiftool -a -u -g1 photo.jpg
```

### EXIF

```bash
exiftool -EXIF:all photo.jpg
```

### IPTC

```bash
exiftool -IPTC:all photo.jpg
```

### XMP

```bash
exiftool -XMP:all photo.jpg
```

### GPS

```bash
exiftool -GPS:all photo.jpg
```

### JSON

```bash
exiftool -json photo.jpg
```

### CSV

```bash
exiftool -csv *.jpg > metadata.csv
```

### Write

```bash
exiftool -Artist="Your Name" photo.jpg
```

### Delete one tag

```bash
exiftool -Artist= photo.jpg
```

### Remove GPS

```bash
exiftool -GPS:all= photo.jpg
```

### Remove all metadata

```bash
exiftool -all= photo.jpg
```

### Copy metadata

```bash
exiftool -TagsFromFile source.jpg destination.jpg
```

### Recursive

```bash
exiftool -r directory/
```

### Rename

```bash
exiftool '-FileName<DateTimeOriginal' -d '%Y-%m-%d_%H-%M-%S%%-c.%%e' *.jpg
```

### Search/filter

```bash
exiftool -if '$ISO > 1600' -FileName directory/
```

# 72. Your recommended learning progression

I'd learn ExifTool in this order:

### Level 1 — Reading

```bash
exiftool photo.jpg
exiftool -a -u -g1 photo.jpg
```

### Level 2 — Individual tags

```bash
exiftool -Make photo.jpg
exiftool -Model photo.jpg
exiftool -ISO photo.jpg
exiftool -DateTimeOriginal photo.jpg
```

### Level 3 — Writing

```bash
exiftool -Artist="..." photo.jpg
exiftool -Copyright="..." photo.jpg
```

### Level 4 — Removing

```bash
exiftool -GPS:all= photo.jpg
exiftool -all= photo.jpg
```

### Level 5 — Metadata families

Learn:

```text
EXIF
IPTC
XMP
GPS
ICC
MakerNotes
```

### Level 6 — Batch processing

Learn:

```bash
-r
-ext
-o
-overwrite_original
```

### Level 7 — Querying

Learn:

```bash
-if
-p
-format
```

### Level 8 — Automation

Integrate with:

```text
Bash
Python
Node.js
PostgreSQL
REST APIs
CI/CD
```

## The big three for your Linux media toolkit

You now have a very solid foundation with these three:

```text
┌──────────────────────────────────────────┐
│              LINUX MEDIA STACK            │
├──────────────────────────────────────────┤
│                                          │
│  FFmpeg                                  │
│  ├── Video                               │
│  ├── Audio                               │
│  ├── Transcoding                         │
│  └── Streaming                           │
│                                          │
│  ImageMagick                             │
│  ├── Image conversion                    │
│  ├── Resize                              │
│  ├── Crop                                │
│  ├── Compression                         │
│  └── Image automation                   │
│                                          │
│  ExifTool                                │
│  ├── EXIF                                │
│  ├── IPTC                                │
│  ├── XMP                                 │
│  ├── GPS                                 │
│  └── Metadata automation                 │
│                                          │
└──────────────────────────────────────────┘
```

For a **software engineer**, the particularly powerful combination is:

FFmpeg + ImageMagick + ExifTool + Bash + Node.js/Python.

That gives you a command-line media-processing toolkit that can handle everything from a single photograph to an automated image/video processing backend.
