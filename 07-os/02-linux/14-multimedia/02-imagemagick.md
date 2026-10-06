# ImageMagick — Complete Guide

## 1. What is ImageMagick?

ImageMagick is an open-source image-processing suite that lets you manipulate images from the **terminal, scripts, applications, and automation pipelines**.

It can:

- Convert image formats
- Resize images
- Crop images
- Rotate and flip
- Compress images
- Optimize images
- Add/remove metadata
- Add text and watermarks
- Apply filters
- Adjust brightness, contrast, saturation, and colors
- Create thumbnails
- Combine images
- Create GIFs
- Extract frames
- Generate images
- Compare images
- Batch-process thousands of images
- Integrate image processing into shell scripts
- Work with PDFs and other document/image formats when the relevant delegates and security policy permit it

The important distinction is:

> **GIMP is primarily an interactive image editor. ImageMagick is primarily an image-processing engine and automation toolkit.**

# 2. ImageMagick 7 vs ImageMagick 6

You will encounter commands from both versions online.

For **ImageMagick 7**, the main command is:

```bash
magick
```

For example:

```bash
magick input.jpg output.png
```

Older tutorials often show:

```bash
convert input.jpg output.png
```

For new scripts, I recommend learning the **ImageMagick 7 syntax**.

There are also specialized commands such as:

```bash
magick identify
magick mogrify
magick montage
magick compare
```

ImageMagick's command-line syntax is essentially built from inputs, settings, operators, sequence operators, and an output. ([ImageMagick](https://imagemagick.org/command-line-processing "ImageMagick | Command-line Processing"))

# 3. Installation on Ubuntu/Linux

Since you're using Ubuntu, start with the distribution package:

```bash
sudo apt update
sudo apt install imagemagick
```

Check:

```bash
magick --version
```

You should get something similar to:

```text
Version: ImageMagick 7.x.x
```

Also check:

```bash
which magick
```

and:

```bash
magick -version
```

### Check supported formats

```bash
 magick identify -list format
```

ImageMagick itself recommends `identify -list format` for inspecting available format support. ([ImageMagick](https://imagemagick.org/advanced-linux-installation))

### Official binaries

The official ImageMagick project currently provides a prebuilt Linux AppImage, while source installation is also available. ([ImageMagick](https://imagemagick.org/download "ImageMagick | Download"))

For Ubuntu, however, I'd start with **APT** unless you specifically need a newer/custom build.

# 4. The basic syntax

The fundamental pattern is:

```bash
magick input options output
```

Example:

```bash
magick input.jpg output.png
```

Think of it as:

```text
Input
  │
  ▼
┌──────────────────┐
│ ImageMagick      │
│                  │
│ resize           │
│ crop             │
│ rotate           │
│ adjust           │
│ filter           │
│ convert          │
└──────────────────┘
  │
  ▼
Output
```

For example:

```bash
magick photo.jpg -resize 1200x800 resized.jpg
```

# 5. Inspect an image

One of the first commands you should learn is:

```bash
magick identify image.jpg
```

Example:

```text
image.jpg JPEG 6000x4000 ...
```

For detailed information:

```bash
magick identify -verbose image.jpg
```

This can reveal:

- Format
- Width
- Height
- Color space
- Bit depth
- Compression
- Profiles
- EXIF information
- File size
- Image statistics

For a developer, this is extremely useful.

# 6. Get specific image information

Width:

```bash
magick identify -format "%w\n" image.jpg
```

Height:

```bash
magick identify -format "%h\n" image.jpg
```

Dimensions:

```bash
magick identify -format "%wx%h\n" image.jpg
```

File format:

```bash
magick identify -format "%m\n" image.jpg
```

File size:

```bash
magick identify -format "%b\n" image.jpg
```

# 7. Convert image formats

This is probably the simplest ImageMagick use case.

### JPEG → PNG

```bash
magick image.jpg image.png
```

### PNG → JPEG

```bash
magick image.png image.jpg
```

### JPEG → WebP

```bash
magick image.jpg image.webp
```

### PNG → WebP

```bash
magick image.png image.webp
```

### WebP → JPEG

```bash
magick image.webp image.jpg
```

### TIFF → PNG

```bash
magick image.tiff image.png
```

The output extension generally tells ImageMagick which format to write.

# 8. Explicitly specify formats

You can also explicitly specify the format:

```bash
magick input.jpg PNG:output.png
```

Or:

```bash
magick input.png JPEG:output.jpg
```

This is useful when filenames/extensions aren't enough or when working with streams.

# 9. Resize images

This is one of the most important ImageMagick operations.

```bash
magick input.jpg -resize 1200x800 output.jpg
```

This means:

> Fit the image within 1200×800 while preserving its aspect ratio.

For example, a 6000×4000 photograph becomes approximately:

```text
1200 × 800
```

### Resize by percentage

```bash
magick input.jpg -resize 50% output.jpg
```

Or:

```bash
magick input.jpg -resize 25% output.jpg
```

### Resize width only

```bash
magick input.jpg -resize 1200x output.jpg
```

Height is automatically calculated.

### Resize height only

```bash
magick input.jpg -resize x800 output.jpg
```

### Force exact dimensions

```bash
magick input.jpg -resize 1200x800! output.jpg
```

The `!` means:

> Don't preserve the original aspect ratio.

This can distort photographs.

### Only shrink large images

A very useful pattern:

```bash
magick input.jpg -resize '1920x1920>' output.jpg
```

The `>` prevents enlarging smaller images.

ImageMagick's geometry syntax supports forms such as `1200x800`, `1200x800!`, percentages, `>`, `<`, and more. ([ImageMagick](https://imagemagick.org/command-line-processing "ImageMagick | Command-line Processing"))

# 10. Thumbnail creation

Create a thumbnail:

```bash
magick photo.jpg -thumbnail 300x300 thumbnail.jpg
```

For web thumbnails:

```bash
magick photo.jpg -thumbnail 400x400 thumbnail.webp
```

`-thumbnail` is particularly useful when you don't need to preserve all image information.

# 11. Crop images

Basic crop:

```bash
magick input.jpg -crop 800x600+0+0 output.jpg
```

The structure is:

```text
WIDTHxHEIGHT+X+Y
```

For example:

```text
800x600+100+50
```

means:

```text
width  = 800
height = 600
x      = 100
y      = 50
```

# 12. Center crop

Suppose you want a 1000×1000 square:

```bash
magick input.jpg -gravity center -crop 1000x1000+0+0 +repage output.jpg
```

`+repage` removes the virtual canvas offset created by cropping.

# 13. Crop to aspect ratio

For example, convert a photograph to 16:9:

```bash
magick input.jpg -resize '1920x1080^' \
  -gravity center \
  -crop 1920x1080+0+0 \
  +repage \
  output.jpg
```

This is a very useful pattern for:

- YouTube thumbnails
- Website banners
- Hero images
- Social media images

# 14. Rotate

90° clockwise:

```bash
magick input.jpg -rotate 90 output.jpg
```

90° counter-clockwise:

```bash
magick input.jpg -rotate -90 output.jpg
```

180°:

```bash
magick input.jpg -rotate 180 output.jpg
```

# 15. Flip

Vertical flip:

```bash
magick input.jpg -flip output.jpg
```

Horizontal flip:

```bash
magick input.jpg -flop output.jpg
```

# 16. Brightness

You can modify brightness using:

```bash
magick input.jpg -brightness-contrast 20x0 output.jpg
```

Here:

```text
20 = brightness
 0 = contrast
```

For example:

```bash
magick input.jpg -brightness-contrast 10x5 output.jpg
```

# 17. Contrast

Increase contrast:

```bash
magick input.jpg -brightness-contrast 0x20 output.jpg
```

Decrease contrast:

```bash
magick input.jpg -brightness-contrast 0x-20 output.jpg
```

# 18. Saturation

You can use:

```bash
magick input.jpg -modulate 100,130,100 output.jpg
```

The three values represent:

```text
brightness, saturation, hue
```

So:

```bash
-modulate 100,130,100
```

means approximately:

```text
Brightness = 100%
Saturation = 130%
Hue        = 100%
```

Desaturate:

```bash
magick input.jpg -modulate 100,0,100 output.jpg
```

This creates grayscale-like output.

# 19. Grayscale

Simple grayscale:

```bash
magick input.jpg -colorspace Gray output.jpg
```

This is cleaner than manually manipulating RGB channels.

# 20. Negative / invert

```bash
magick input.jpg -negate output.jpg
```

# 21. Sharpen

```bash
magick input.jpg -sharpen 0x1 output.jpg
```

More sharpening:

```bash
magick input.jpg -sharpen 0x2 output.jpg
```

Use sharpening carefully. Too much produces halos and artificial edges.

# 22. Blur

```bash
magick input.jpg -blur 0x5 output.jpg
```

Another example:

```bash
magick input.jpg -blur 0x10 output.jpg
```

# 23. Add a border

```bash
magick input.jpg \
  -bordercolor black \
  -border 10 \
  output.jpg
```

White border:

```bash
magick input.jpg \
  -bordercolor white \
  -border 20 \
  output.jpg
```

# 24. Add rounded corners

A common approach uses alpha masks, but for simple workflows ImageMagick can use:

```bash
magick input.jpg \
  \( +clone -alpha extract -draw 'roundrectangle 0,0 %[fx:w-1],%[fx:h-1] 30,30' \) \
  -alpha off -compose CopyOpacity -composite \
  output.png
```

PNG is preferable here because transparency is required.

# 25. Add text

Basic text:

```bash
magick input.jpg \
  -gravity south \
  -pointsize 48 \
  -fill white \
  -annotate +0+30 "My Photograph" \
  output.jpg
```

The important options are:

```text
-gravity
-pointsize
-fill
-annotate
```

# 26. Text with background

For example:

```bash
magick input.jpg \
  -gravity south \
  -fill white \
  -undercolor black \
  -pointsize 48 \
  -annotate +0+30 "My Photograph" \
  output.jpg
```

# 27. Watermark an image

Suppose:

```text
photo.jpg
logo.png
```

You can composite the logo:

```bash
magick photo.jpg logo.png \
  -gravity southeast \
  -geometry +30+30 \
  -composite \
  watermarked.jpg
```

For a smaller watermark:

```bash
magick photo.jpg \
  \( logo.png -resize 200x \) \
  -gravity southeast \
  -geometry +30+30 \
  -composite \
  watermarked.jpg
```

# 28. Adjust watermark opacity

```bash
magick logo.png -alpha set -channel A -evaluate set 40% watermark.png
```

Then:

```bash
magick photo.jpg watermark.png \
  -gravity southeast \
  -geometry +30+30 \
  -composite \
  output.jpg
```

This gives you a semi-transparent watermark.

# 29. Combine images horizontally

```bash
magick image1.jpg image2.jpg image3.jpg +append output.jpg
```

Result:

```text
┌────────┐┌────────┐┌────────┐
│ image1 ││ image2 ││ image3 │
└────────┘└────────┘└────────┘
```

# 30. Combine images vertically

```bash
magick image1.jpg image2.jpg image3.jpg -append output.jpg
```

Result:

```text
┌────────┐
│ image1 │
├────────┤
│ image2 │
├────────┤
│ image3 │
└────────┘
```

# 31. Create a contact sheet

This is extremely useful for photography.

```bash
magick *.jpg \
  -thumbnail 300x300 \
  -background white \
  -gravity center \
  -extent 300x300 \
  -tile 4x \
  -geometry +10+10 \
  montage.jpg
```

You get something like:

```text
┌──────┬──────┬──────┬──────┐
│ IMG1 │ IMG2 │ IMG3 │ IMG4 │
├──────┼──────┼──────┼──────┤
│ IMG5 │ IMG6 │ IMG7 │ IMG8 │
├──────┼──────┼──────┼──────┤
│ IMG9 │ ...  │ ...  │ ...  │
└──────┴──────┴──────┴──────┘
```

# 32. Create GIFs

Given:

```text
frame01.png
frame02.png
frame03.png
```

Create GIF:

```bash
magick -delay 10 -loop 0 frame*.png animation.gif
```

Where:

```text
-delay 10
```

controls frame timing.

```text
-loop 0
```

means infinite looping.

# 33. Extract frames from GIF

```bash
magick animation.gif frame-%03d.png
```

Output:

```text
frame-000.png
frame-001.png
frame-002.png
...
```

# 34. Create animated WebP

```bash
magick -delay 10 -loop 0 frame*.png animation.webp
```

# 35. Image sequences

ImageMagick can process multiple images as an image sequence.

For example:

```bash
magick *.jpg output.pdf
```

Depending on your ImageMagick configuration and security policy, this can create a PDF from the images.

# 36. PDF → image

ImageMagick can interact with PDF through delegate software, commonly Ghostscript.

For example:

```bash
magick input.pdf page.png
```

Or:

```bash
magick -density 150 input.pdf page-%03d.png
```

The `-density` setting is important because PDF is a page/vector-oriented format rather than simply a raster image.

**Important operational point:** ImageMagick installations commonly restrict PDF/PS operations through `policy.xml`; don't blindly remove those restrictions. The official project specifically recommends configuring an appropriate security policy for the local environment. ([ImageMagick](https://imagemagick.org/download "ImageMagick | Download"))

For document conversion, tools such as `pdftoppm` or `pdftocairo` may sometimes be a better choice.

---

# 37. Image compression

JPEG:

```bash
magick input.jpg -quality 80 output.jpg
```

Typical values:

```text
60 → smaller
70 → good
80 → very good
90 → high quality
95 → very high quality
100 → maximum setting, but not necessarily mathematically lossless
```

For websites, I'd generally start around:

```bash
-quality 80
```

and visually inspect the result.

# 38. WebP conversion

Excellent for web images:

```bash
magick input.jpg -quality 80 output.webp
```

You can compare:

```bash
ls -lh input.jpg output.webp
```

# 39. AVIF

If your ImageMagick build has AVIF support:

```bash
magick input.jpg -quality 80 output.avif
```

Check whether your installation supports it:

```bash
magick identify -list format | grep -i AVIF
```

# 40. PNG optimization

PNG is normally lossless, but there are several ways to reduce unnecessary data.

For example:

```bash
magick input.png -strip output.png
```

`-strip` removes profiles/comments and other ancillary image information.

Be careful: this can remove useful metadata such as ICC profiles and EXIF information.

# 41. Remove metadata

```bash
magick input.jpg -strip output.jpg
```

This is useful for:

- Website images
- Privacy
- Reducing file size
- Removing unnecessary metadata

But if you need:

- GPS
- Camera model
- Lens information
- Copyright metadata

don't use `-strip`.

# 42. EXIF information

Inspect metadata:

```bash
magick identify -verbose photo.jpg
```

Search:

```bash
magick identify -verbose photo.jpg | grep -i exif
```

For serious EXIF workflows, I'd also learn **ExifTool**. ImageMagick can manipulate image metadata, but ExifTool is considerably more specialized for metadata.

# 43. Auto-orient photographs

Camera images can contain orientation metadata.

A useful operation is:

```bash
magick input.jpg -auto-orient output.jpg
```

This physically rotates the pixels according to the orientation metadata.

Very useful before processing photographs.

# 44. Batch resize

Suppose you have:

```text
photo01.jpg
photo02.jpg
photo03.jpg
photo04.jpg
```

You can use:

```bash
mkdir resized

for file in *.jpg; do
    magick "$file" -resize '1920x1920>' "resized/$file"
done
```

Now:

```text
photos/
├── photo01.jpg
├── photo02.jpg
├── photo03.jpg
├── photo04.jpg
└── resized/
    ├── photo01.jpg
    ├── photo02.jpg
    ├── photo03.jpg
    └── photo04.jpg
```

This is where ImageMagick becomes significantly more powerful than a traditional GUI editor.

# 45. Batch conversion to WebP

```bash
mkdir webp

for file in *.jpg; do
    magick "$file" -quality 80 "webp/${file%.jpg}.webp"
done
```

The Bash expression:

```bash
${file%.jpg}
```

removes `.jpg` from the filename.

So:

```text
photo.jpg
```

becomes:

```text
photo.webp
```

# 46. Batch thumbnail generation

```bash
mkdir thumbnails

for file in *.jpg; do
    magick "$file" \
      -thumbnail 400x400 \
      "thumbnails/${file%.jpg}.jpg"
done
```

# 47. Batch image optimization

```bash
mkdir optimized

for file in *.jpg; do
    magick "$file" \
      -auto-orient \
      -resize '1920x1920>' \
      -strip \
      -quality 82 \
      "optimized/$file"
done
```

This is an excellent practical workflow for web images.

# 48. Create a photography processing pipeline

You can combine multiple operations:

```bash
magick input.jpg \
  -auto-orient \
  -resize '2400x2400>' \
  -brightness-contrast 5x5 \
  -sharpen 0x1 \
  -strip \
  -quality 85 \
  output.jpg
```

Pipeline:

```text
RAW/JPEG
   │
   ▼
Auto orientation
   │
   ▼
Resize
   │
   ▼
Brightness/Contrast
   │
   ▼
Sharpen
   │
   ▼
Remove metadata
   │
   ▼
JPEG compression
   │
   ▼
Output
```

# 49. Use `mogrify`

`mogrify` is designed for modifying images, often in batches.

For example:

```bash
magick mogrify -resize '1920x1920>' *.jpg
```

**But be careful.**

`mogrify` normally modifies the original files.

That makes this:

```bash
magick mogrify -resize 50% *.jpg
```

potentially destructive to your originals.

For important photographs, I'd rather do:

```bash
mkdir output

for file in *.jpg; do
    magick "$file" -resize 50% "output/$file"
done
```

# 50. Compare two images

ImageMagick provides image comparison functionality.

For example:

```bash
magick compare image1.jpg image2.jpg difference.png
```

You can also get a numerical metric:

```bash
magick compare -metric RMSE image1.jpg image2.jpg null:
```

This is particularly useful in:

- Automated testing
- Screenshot testing
- Image pipelines
- Computer vision workflows
- Regression testing

# 51. Generate an image

ImageMagick can create images from scratch.

Example:

```bash
magick -size 800x600 canvas:white output.png
```

Create black:

```bash
magick -size 800x600 canvas:black output.png
```

Create transparent canvas:

```bash
magick -size 800x600 canvas:none output.png
```

# 52. Create a simple graphic

```bash
magick -size 1200x630 \
  canvas:white \
  -gravity center \
  -pointsize 72 \
  -fill black \
  -annotate +0+0 "Hello ImageMagick" \
  social-card.png
```

This is useful for automatically generating:

- Social media cards
- Blog thumbnails
- OG images
- Documentation graphics
- Certificates
- Reports
- Banners

# 53. Create social media cards

For example:

```bash
magick \
  -size 1200x630 canvas:'#222222' \
  -gravity center \
  -fill white \
  -font DejaVu-Sans \
  -pointsize 64 \
  -annotate +0-30 "ImageMagick" \
  -pointsize 32 \
  -annotate +0+60 "Command-line image processing" \
  card.png
```

This is where ImageMagick becomes very interesting for a web developer.

You could generate hundreds of these automatically from a database.

# 54. Find installed fonts

```bash
magick -list font
```

Or:

```bash
magick -list font | less
```

You can then specify:

```bash
-font "DejaVu-Sans"
```

# 55. Use gravity

`-gravity` controls positioning.

Common values:

```text
NorthWest
North
NorthEast
West
Center
East
SouthWest
South
SouthEast
```

Example:

```bash
magick image.jpg \
  -gravity southeast \
  -pointsize 40 \
  -fill white \
  -annotate +20+20 "© 2026" \
  output.jpg
```

# 56. Geometry

ImageMagick's geometry system is one of the concepts you should learn well.

Common forms:

```text
100x100
100x
x100
100x100!
100x100>
100x100<
100%
50%
```

Think:

```text
100x100
│  │
│  └── height
└───── width
```

Examples:

```bash
-resize 800x600
```

Fit within 800×600.

```bash
-resize 800x
```

Width = 800.

```bash
-resize x600
```

Height = 600.

```bash
-resize 800x600!
```

Force 800×600.

```bash
-resize 800x600>
```

Only shrink images larger than 800×600.

This geometry system is central to ImageMagick. ([ImageMagick](https://imagemagick.org/command-line-processing "ImageMagick | Command-line Processing"))

# 57. Aspect ratio

Suppose the source is:

```text
6000 × 4000
```

Aspect ratio:

```text
6000 / 4000 = 1.5
```

which is:

```text
3:2
```

For a 16:9 crop:

```bash
magick input.jpg \
  -resize '1920x1080^' \
  -gravity center \
  -crop 1920x1080+0+0 \
  +repage \
  output.jpg
```

The `^` means:

> Resize until the image completely covers the requested geometry.

Then crop the excess.

# 58. Add padding

Suppose you want a 1200×1200 canvas:

```bash
magick input.jpg \
  -resize 1000x1000 \
  -gravity center \
  -background white \
  -extent 1200x1200 \
  output.jpg
```

This is useful for:

- Product images
- Profile pictures
- E-commerce images
- Logos
- Social media images

# 59. Transparent backgrounds

PNG supports transparency.

Create:

```bash
magick input.jpg \
  -alpha on \
  output.png
```

Make white transparent:

```bash
magick input.png \
  -fuzz 10% \
  -transparent white \
  output.png
```

The `-fuzz` value determines how similar a color can be while still being treated as the target color.

# 60. Remove a background color

For example:

```bash
magick logo.png \
  -fuzz 15% \
  -transparent white \
  output.png
```

This can work well for simple logos.

For complicated photographic backgrounds, ImageMagick isn't a replacement for modern segmentation/background-removal tools.

# 61. Color replacement

Example:

```bash
magick input.png \
  -fill red \
  -opaque blue \
  output.png
```

This replaces pixels matching blue with red.

With fuzz:

```bash
magick input.png \
  -fuzz 10% \
  -fill red \
  -opaque blue \
  output.png
```

# 62. Color spaces

Check:

```bash
magick identify -verbose image.jpg | grep -i colorspace
```

Convert:

```bash
magick image.jpg -colorspace sRGB output.jpg
```

For general web images, **sRGB** is usually the practical target.

# 63. Profiles and ICC color management

ImageMagick supports image profiles, including ICC color profiles. ([ImageMagick](https://imagemagick.org/command-line-options "ImageMagick | Command-line Options"))

You can inspect:

```bash
magick identify -verbose image.jpg
```

Look for:

```text
Profile-icc
```

You can attach a profile:

```bash
magick input.jpg -profile profile.icc output.jpg
```

This is more advanced territory, but important for professional photography and print workflows.

# 64. Channels

RGB images consist of:

```text
Red
Green
Blue
```

You can extract channels.

Red:

```bash
magick input.jpg -channel R -separate red.png
```

Green:

```bash
magick input.jpg -channel G -separate green.png
```

Blue:

```bash
magick input.jpg -channel B -separate blue.png
```

This is useful for understanding how digital images actually work.

# 65. Image statistics

```bash
magick input.jpg -format "%[mean]\n" info:
```

You can obtain various image statistics using `-format` and ImageMagick expressions.

This becomes useful for automated image analysis.

# 66. Read from stdin

ImageMagick can work with streams.

For example:

```bash
cat image.jpg | magick - output.png
```

Here:

```text
- 
```

means standard input.

This makes ImageMagick excellent for Unix pipelines.

# 67. Output to stdout

Similarly:

```bash
magick input.jpg PNG:-
```

The result goes to standard output.

This is useful when integrating ImageMagick with:

- Bash
- Python
- Node.js
- PHP
- HTTP servers
- CI/CD
- Unix pipelines

# 68. ImageMagick + Bash

A very practical script:

```bash
#!/usr/bin/env bash

mkdir -p output

for file in *.jpg; do
    magick "$file" \
        -auto-orient \
        -resize '1920x1920>' \
        -strip \
        -quality 82 \
        "output/$file"
done
```

Save as:

```text
process-images.sh
```

Make executable:

```bash
chmod +x process-images.sh
```

Run:

```bash
./process-images.sh
```

# 69. ImageMagick + Node.js

Because you're working in the JavaScript/TypeScript ecosystem, this is particularly relevant.

You can call ImageMagick from Node:

```javascript
import { execFile } from "node:child_process";

execFile(
  "magick",
  [
    "input.jpg",
    "-resize",
    "1200x1200>",
    "output.jpg"
  ],
  (error) => {
    if (error) {
      console.error(error);
      return;
    }

    console.log("Image processed.");
  }
);
```

This lets you build:

```text
Upload
   │
   ▼
Node.js
   │
   ▼
ImageMagick
   │
   ├── Resize
   ├── Convert
   ├── Compress
   ├── Watermark
   └── Optimize
   │
   ▼
Storage
```

For a web application, though, you should carefully validate inputs and isolate image-processing workloads.

# 70. ImageMagick + Python

Python can invoke ImageMagick:

```python
import subprocess

subprocess.run([
    "magick",
    "input.jpg",
    "-resize",
    "1200x1200>",
    "output.jpg"
], check=True)
```

Or you can use Python libraries that provide native image-processing functionality.

# 71. Generate multiple sizes for a website

This is one of my favorite practical applications.

Given:

```text
hero.jpg
```

Generate:

```text
hero-400.webp
hero-800.webp
hero-1200.webp
hero-1920.webp
```

Commands:

```bash
magick hero.jpg -resize 400x -quality 80 hero-400.webp
magick hero.jpg -resize 800x -quality 80 hero-800.webp
magick hero.jpg -resize 1200x -quality 80 hero-1200.webp
magick hero.jpg -resize 1920x -quality 80 hero-1920.webp
```

Then your HTML can use responsive images:

```html
<img
  src="hero-1200.webp"
  srcset="
    hero-400.webp 400w,
    hero-800.webp 800w,
    hero-1200.webp 1200w,
    hero-1920.webp 1920w
  "
  sizes="100vw"
  alt="Hero image"
>
```

That's a very practical **Web Developer + ImageMagick** workflow.

# 72. ImageMagick for web optimization

A typical pipeline:

```bash
magick source.jpg \
    -auto-orient \
    -resize '1920x1920>' \
    -colorspace sRGB \
    -strip \
    -quality 82 \
    output.webp
```

Pipeline:

```text
Original
   │
   ├── orientation
   │
   ├── resize
   │
   ├── sRGB
   │
   ├── metadata cleanup
   │
   └── compression
   │
   ▼
WebP
```

# 73. ImageMagick vs GIMP vs darktable

|Feature|ImageMagick|GIMP|darktable|
|---|--:|--:|--:|
|CLI|⭐⭐⭐⭐⭐|⭐⭐|⭐⭐|
|Automation|⭐⭐⭐⭐⭐|⭐⭐|⭐⭐|
|Batch processing|⭐⭐⭐⭐⭐|⭐⭐⭐|⭐⭐⭐|
|RAW photography|⭐⭐|⭐⭐|⭐⭐⭐⭐⭐|
|Photoshop-like editing|⭐⭐⭐|⭐⭐⭐⭐⭐|⭐⭐⭐|
|Layers|⭐⭐⭐|⭐⭐⭐⭐⭐|⭐⭐⭐|
|Photo catalog|❌|❌|⭐⭐⭐⭐|
|Web optimization|⭐⭐⭐⭐⭐|⭐⭐⭐|⭐⭐⭐|
|Developer workflows|⭐⭐⭐⭐⭐|⭐⭐|⭐⭐|
|Image conversion|⭐⭐⭐⭐⭐|⭐⭐⭐|⭐⭐⭐|
|Interactive editing|⭐⭐|⭐⭐⭐⭐⭐|⭐⭐⭐⭐|

My recommendation:

```text
RAW Photography
      ↓
  darktable
      ↓
Advanced editing
      ↓
     GIMP
      ↓
Automation / Web / Batch
      ↓
 ImageMagick
```

They complement each other rather than compete directly.

# 74. The commands you should memorize

If you're learning ImageMagick seriously, start with these:

```bash
magick
magick identify
magick mogrify
magick montage
magick compare
```

And these operations:

```text
-resize
-thumbnail
-crop
-rotate
-flip
-flop
-auto-orient
-quality
-strip
-sharpen
-blur
-colorspace
-brightness-contrast
-modulate
-negate
-gravity
-geometry
-extent
-composite
-annotate
-border
```

# 75. A practical cheat sheet

### Information

```bash
magick identify image.jpg
magick identify -verbose image.jpg
magick identify -format "%wx%h\n" image.jpg
```

### Conversion

```bash
magick input.jpg output.png
magick input.jpg output.webp
```

### Resize

```bash
magick input.jpg -resize 1200x output.jpg
```

### Thumbnail

```bash
magick input.jpg -thumbnail 300x300 thumbnail.jpg
```

### Crop

```bash
magick input.jpg -crop 800x600+0+0 output.jpg
```

### Rotate

```bash
magick input.jpg -rotate 90 output.jpg
```

### Flip

```bash
magick input.jpg -flip output.jpg
```

### Grayscale

```bash
magick input.jpg -colorspace Gray output.jpg
```

### Brightness/contrast

```bash
magick input.jpg -brightness-contrast 10x10 output.jpg
```

### Sharpen

```bash
magick input.jpg -sharpen 0x1 output.jpg
```

### Blur

```bash
magick input.jpg -blur 0x5 output.jpg
```

### Compress

```bash
magick input.jpg -quality 80 output.jpg
```

### Remove metadata

```bash
magick input.jpg -strip output.jpg
```

### Watermark

```bash
magick image.jpg logo.png \
  -gravity southeast \
  -geometry +20+20 \
  -composite \
  output.jpg
```

### Horizontal combination

```bash
magick a.jpg b.jpg +append output.jpg
```

### Vertical combination

```bash
magick a.jpg b.jpg -append output.jpg
```

### GIF

```bash
magick -delay 10 -loop 0 frame*.png animation.gif
```

### Compare

```bash
magick compare image1.jpg image2.jpg difference.png
```

### Batch

```bash
for file in *.jpg; do
    magick "$file" -resize '1920x1920>' "output-$file"
done
```

# 76. A good learning path

Don't try to memorize 1,000 ImageMagick options. Learn it in layers.

### Level 1 — Fundamentals

Learn:

```bash
magick
identify
```

Then:

```text
format conversion
resize
crop
rotate
quality
```

### Level 2 — Photography

Learn:

```text
-auto-orient
-colorspace
-brightness-contrast
-modulate
-sharpen
-strip
-profile
```

### Level 3 — Composition

Learn:

```text
-gravity
-geometry
-extent
-composite
-annotate
-alpha
```

### Level 4 — Automation

Learn:

```bash
mogrify
montage
compare
```

Then Bash:

```bash
for
while
find
xargs
```

### Level 5 — Developer workflows

Learn:

```text
stdin/stdout
pipes
Node.js
Python
HTTP upload processing
CI/CD
image pipelines
```

### Level 6 — Advanced ImageMagick

Eventually study:

```text
image sequences
channels
masks
alpha compositing
color spaces
ICC profiles
FX expressions
image stacks
distortions
morphology
convolution
HDR
advanced geometry
```

The official documentation separates settings, image operators, channel operators, sequence operators, geometry, and image stacks. Understanding those categories is much more valuable than memorizing isolated commands. ([ImageMagick](https://imagemagick.org/command-line-processing "ImageMagick | Command-line Processing"))

## The most important concept

The real power of ImageMagick isn't:

```bash
magick photo.jpg photo.webp
```

It's this:

```bash
magick input \
    -auto-orient \
    -resize '1920x1920>' \
    -colorspace sRGB \
    -brightness-contrast 5x5 \
    -sharpen 0x1 \
    -strip \
    -quality 82 \
    output.webp
```

And then putting that into:

```text
Bash script
     ↓
Node.js/Python application
     ↓
CI/CD pipeline
     ↓
Image processing service
     ↓
Thousands of images automatically
```

**That's where ImageMagick becomes exceptionally powerful for a software engineer.**
