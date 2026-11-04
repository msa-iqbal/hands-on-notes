# PDF Compression with Ghostscript and OCRmyPDF

A practical guide to reducing PDF file sizes using free, open-source command-line tools on Ubuntu and other Debian-based Linux distributions.

## Table of Contents

1. [Introduction](#1-introduction)
2. [How PDF Compression Works](#2-how-pdf-compression-works)
3. [Install Required Tools](#3-install-required-tools)
4. [Check PDF Information](#4-check-pdf-information)
5. [Compress a PDF with Ghostscript](#5-compress-a-pdf-with-ghostscript)
6. [Understand Compression Profiles](#6-understand-compression-profiles)
7. [Control Image Resolution and Quality](#7-control-image-resolution-and-quality)
8. [Compress Scanned PDFs](#8-compress-scanned-pdfs)
9. [Optimize PDFs with OCRmyPDF](#9-optimize-pdfs-with-ocrmypdf)
10. [Compress Multiple PDFs](#10-compress-multiple-pdfs)
11. [Compress PDFs Recursively](#11-compress-pdfs-recursively)
12. [Create a Reusable Compression Script](#12-create-a-reusable-compression-script)
13. [Create a Global Compression Command](#13-create-a-global-compression-command)
14. [Compare File Sizes](#14-compare-file-sizes)
15. [Troubleshooting](#15-troubleshooting)
16. [Best Practices](#16-best-practices)
17. [Quick Reference](#17-quick-reference)

---

## 1. Introduction

PDF compression reduces the amount of storage required by a PDF document.

It is useful when preparing documents for:

* Email attachments
* Online submissions
* Websites
* Document archives
* Mobile devices
* Cloud storage
* Sharing over messaging applications

The amount of compression depends on the PDF's contents. A scanned document containing hundreds of high-resolution images can often be reduced considerably, while a text-only PDF may already be close to its minimum practical size.

This guide uses local Linux applications, so documents can be processed without uploading them to an online service.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 2. How PDF Compression Works

PDF optimization can involve several techniques.

### 2.1 Image Downsampling

Reduces image resolution, measured in dots per inch (DPI).

For example, reducing an image from 600 DPI to 150 DPI can significantly reduce its size.

### 2.2 Image Compression

Re-encodes images using compression methods such as JPEG.

Lossy compression can reduce file size substantially but may introduce visible artifacts.

### 2.3 Font and Resource Optimization

PDF optimization may reduce duplicated resources, optimize embedded fonts, and rewrite internal objects.

The result depends on how the original PDF was created.

### 2.4 Lossless and Lossy Compression

| Type | Characteristics |
| --- | --- |
| Lossless | Preserves the original encoded image or data information where supported |
| Lossy | Discards some image information to reduce file size |
| Mixed optimization | Combines resource optimization and image compression |

Ghostscript's standard compression profiles may downsample images and change their encoding. They should not be assumed to preserve every original object or visual detail.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 3. Install Required Tools

Update the package index:

```bash
sudo apt update
```

Install Ghostscript and Poppler utilities:

```bash
sudo apt install ghostscript poppler-utils
```

Install OCRmyPDF if you also need OCR and scanned-document optimization:

```bash
sudo apt install ocrmypdf
```

Verify the installations:

```bash
gs --version
pdfinfo -v
pdfimages -v
ocrmypdf --version
```

The commands serve different purposes:

| Tool | Purpose |
| --- | --- |
| `gs` | PDF generation and compression |
| `pdfinfo` | PDF metadata and document information |
| `pdfimages` | List or extract embedded images |
| `pdftotext` | Extract searchable text |
| `ocrmypdf` | Add OCR text layers and optimize scanned PDFs |
<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 4. Check PDF Information

Before compressing a document, inspect its size and structure.

### 4.1 Check File Size

```bash
ls -lh document.pdf
```

Example:

```text
-rw-r--r-- 1 user user 25M document.pdf
```

### 4.2 Inspect PDF Metadata

```bash
pdfinfo document.pdf
```

Useful fields include:

```text
Pages:          25
Page size:      595 x 842 pts (A4)
File size:      26214400 bytes
PDF version:    1.7
```

The exact output depends on the document.

### 4.3 Check Embedded Images

```bash
pdfimages -list document.pdf
```

This lists embedded images, their dimensions, color spaces, and other information.

A PDF containing large page-sized images may benefit from image downsampling.

### 4.4 Check Whether Text Is Searchable

```bash
pdftotext document.pdf -
```

If text appears in the terminal, the document contains extractable text.

If the output is empty, the PDF may consist of scanned images, although other document restrictions or unusual encodings can also affect extraction.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 5. Compress a PDF with Ghostscript

Ghostscript is the primary tool used in this guide.

### 5.1 Basic Compression

Run:

```bash
gs -sDEVICE=pdfwrite \
  -dCompatibilityLevel=1.4 \
  -dPDFSETTINGS=/ebook \
  -dNOPAUSE \
  -dQUIET \
  -dBATCH \
  -sOutputFile="compressed.pdf" \
  "document.pdf"
```

This creates `compressed.pdf` while leaving `document.pdf` unchanged.

### 5.2 Explanation of the Options

| Option | Description |
| --- | --- |
| `-sDEVICE=pdfwrite` | Produces a PDF file |
| `-dCompatibilityLevel=1.4` | Sets the output PDF compatibility level |
| `-dPDFSETTINGS=/ebook` | Selects a predefined optimization profile |
| `-dNOPAUSE` | Prevents page-by-page pauses |
| `-dQUIET` | Suppresses most routine messages |
| `-dBATCH` | Exits after processing |
| `-sOutputFile=...` | Specifies the destination file |

**Important:** Ghostscript rewrites the PDF. Some digital signatures, interactive features, forms, annotations, or specialized PDF structures may not survive unchanged. Keep the original and verify the output.

### 5.3 Use Filenames Containing Spaces

Always quote filenames containing spaces:

```bash
gs -sDEVICE=pdfwrite \
  -dPDFSETTINGS=/ebook \
  -dNOPAUSE -dQUIET -dBATCH \
  -sOutputFile="compressed document.pdf" \
  "original document.pdf"
```

### 5.4 Specify Absolute Paths

```bash
gs -sDEVICE=pdfwrite \
  -dPDFSETTINGS=/ebook \
  -dNOPAUSE -dQUIET -dBATCH \
  -sOutputFile="/home/user/Documents/compressed.pdf" \
  "/home/user/Documents/original.pdf"
```

Replace `/home/user/Documents/` with the actual directory path.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 6. Understand Compression Profiles

Ghostscript provides four common PDF settings.

### 6.1 Screen Profile

```bash
-dPDFSETTINGS=/screen
```

Use when small file size is the priority and reduced image quality is acceptable.

Suitable for:

* Screen-only reading
* Quick previews
* Documents with relaxed quality requirements

### 6.2 Ebook Profile

```bash
-dPDFSETTINGS=/ebook
```

A practical starting point for everyday documents.

Suitable for:

* Email attachments
* Online submissions
* Digital reading
* General document sharing

### 6.3 Printer Profile

```bash
-dPDFSETTINGS=/printer
```

Designed for higher-quality output than the more aggressive screen profile.

Suitable for:

* Documents intended for printing
* Reports with illustrations
* Documents requiring clearer images

### 6.4 Prepress Profile

```bash
-dPDFSETTINGS=/prepress
```

Designed for print-oriented workflows where image quality and print characteristics matter.

It is not necessarily the best choice for the smallest file size.

### 6.5 Profile Comparison

| Profile | Compression tendency | Typical use |
| --- | --- | --- |
| `/screen` | Most aggressive | Screen viewing |
| `/ebook` | Moderate to aggressive | General sharing |
| `/printer` | More conservative | Printing |
| `/prepress` | Print-oriented | Prepress workflows |

Actual file sizes and quality vary with the source PDF. These profiles are presets, not guaranteed size targets.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 7. Control Image Resolution and Quality

For image-heavy PDFs, explicit settings can provide more control than predefined profiles.

### 7.1 Custom Compression Command

```bash
gs -sDEVICE=pdfwrite \
  -dCompatibilityLevel=1.4 \
  -dNOPAUSE -dQUIET -dBATCH \
  -dDownsampleColorImages=true \
  -dColorImageDownsampleType=/Bicubic \
  -dColorImageResolution=150 \
  -dDownsampleGrayImages=true \
  -dGrayImageDownsampleType=/Bicubic \
  -dGrayImageResolution=150 \
  -dDownsampleMonoImages=true \
  -dMonoImageDownsampleType=/Subsample \
  -dMonoImageResolution=300 \
  -dAutoFilterColorImages=false \
  -dColorImageFilter=/DCTEncode \
  -dAutoFilterGrayImages=false \
  -dGrayImageFilter=/DCTEncode \
  -dJPEGQ=70 \
  -sOutputFile="compressed.pdf" \
  "document.pdf"
```

This example targets 150 DPI for color and grayscale images and 300 DPI for monochrome images.

The JPEG quality setting applies to JPEG encoding performed by Ghostscript; it does not guarantee that every image in the PDF will be re-encoded as JPEG.

### 7.2 Understand DPI Settings

| Resolution | Typical use |
| --- | --- |
| 72–100 DPI | Small screen-only documents |
| 100–150 DPI | Everyday digital reading |
| 150–200 DPI | General documents and many scans |
| 200–300 DPI | Higher-quality printing |
| 300 DPI or higher | Detailed scans and specialized requirements |

These are practical starting points, not universal rules. Small text, line drawings, diagrams, and archival scans may need higher resolution.

### 7.3 Understand JPEG Quality

The `-dJPEGQ` parameter controls JPEG encoding quality when that encoder is used.

| Setting | General tendency |
| --- | --- |
| 90 | Higher quality, usually larger |
| 80 | Good general image quality |
| 70 | More aggressive compression |
| 60 | Smaller files with increased risk of artifacts |
| 40 | Strong compression; visible degradation is more likely |

Text rendered as images can become blurry at low quality. Inspect the smallest text and fine lines after compression.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 8. Compress Scanned PDFs

Scanned documents are often large because each page is stored as an image.

### 8.1 Recommended Starting Command

```bash
gs -sDEVICE=pdfwrite \
  -dCompatibilityLevel=1.4 \
  -dNOPAUSE -dQUIET -dBATCH \
  -dDownsampleColorImages=true \
  -dColorImageResolution=200 \
  -dDownsampleGrayImages=true \
  -dGrayImageResolution=200 \
  -dDownsampleMonoImages=true \
  -dMonoImageResolution=300 \
  -dJPEGQ=75 \
  -sOutputFile="scanned-compressed.pdf" \
  "scanned-original.pdf"
```

This is a balanced starting point for many scanned documents.

### 8.2 For Smaller Files

Try 150 DPI for color and grayscale images and reduce JPEG quality to 60–70.

### 8.3 For Better Readability

Use 200–300 DPI for grayscale or color scans and a higher JPEG quality setting.

For black-and-white pages containing tiny text or line art, preserve sufficient resolution and avoid overly aggressive lossy compression.

### 8.4 Check the Result

Inspect:

* Small text
* Page edges
* Tables and diagrams
* Photographs
* Signatures and stamps
* Faint marks and footnotes

Never judge the output solely by its file size.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 9. Optimize PDFs with OCRmyPDF

OCRmyPDF is particularly useful for scanned documents that need searchable text.

### 9.1 Basic Optimization

```bash
ocrmypdf --optimize 1 \
  "scanned-original.pdf" \
  "optimized.pdf"
```

Optimization levels:

| Level | Description                                                           |
| ----- | --------------------------------------------------------------------- |
| `0`   | No additional optimization                                            |
| `1`   | Lossless optimization                                                 |
| `2`   | Additional optimization; lossy image optimization may be used         |
| `3`   | More aggressive optimization; greater quality trade-offs are possible |

Higher optimization levels do not guarantee smaller files. The result depends on the document.

### 9.2 Add OCR and Optimize

```bash
ocrmypdf --optimize 2 \
  "scanned-original.pdf" \
  "searchable.pdf"
```

This adds an OCR text layer where possible and optimizes the output.

### 9.3 Use OCR with Image Processing

For a document that needs rotation correction and deskewing:

```bash
ocrmypdf --rotate-pages \
  --deskew \
  --optimize 2 \
  "scanned-original.pdf" \
  "searchable.pdf"
```

These options can improve usability, but OCR results should be checked for accuracy.

### 9.4 Check Searchability

```bash
pdftotext "searchable.pdf" -
```

If the text is extracted correctly, the document is searchable.

**Important:** OCRmyPDF is not simply a replacement for Ghostscript. Use it when OCR or scanned-document processing is required. For an already searchable PDF, Ghostscript may be sufficient.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 10. Compress Multiple PDFs

To compress every PDF in the current directory, create an output folder:

```bash
mkdir -p compressed
```

Run:

```bash
for file in ./*.pdf; do
    [ -f "$file" ] || continue

    name="${file##*/}"
    output="compressed/${name%.pdf}-compressed.pdf"

    if gs -sDEVICE=pdfwrite \
        -dPDFSETTINGS=/ebook \
        -dNOPAUSE -dQUIET -dBATCH \
        -sOutputFile="$output" \
        "$file"; then
        printf 'Created: %s\n' "$output"
    else
        printf 'Failed: %s\n' "$file" >&2
    fi
done
```

This script:

* Processes PDF files in the current directory.
* Handles filenames containing spaces.
* Saves results in `compressed/`.
* Leaves original files untouched.
* Reports individual failures.

The pattern `./*.pdf` matches lowercase `.pdf` extensions. To include uppercase `.PDF`, use the recursive approach below or adjust the script.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 11. Compress PDFs Recursively

To process PDFs in the current directory and all its subdirectories, use:

```bash
find . -type f -iname '*.pdf' \
  ! -iname '*-compressed.pdf' -print0 |
while IFS= read -r -d '' file; do
    output="${file%.*}-compressed.pdf"

    if gs -sDEVICE=pdfwrite \
        -dPDFSETTINGS=/ebook \
        -dNOPAUSE -dQUIET -dBATCH \
        -sOutputFile="$output" \
        "$file"; then
        printf 'Created: %s\n' "$output"
    else
        printf 'Failed: %s\n' "$file" >&2
    fi
done
```

**Note:** This places compressed files beside the originals. For large directory trees, consider a separate destination directory to keep the folder structure organized. Avoid running the script repeatedly against previously generated files.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 12. Create a Reusable Compression Script

Create a script:

```bash
nano pdf-compress.sh
```

Add the following content:

```bash
#!/usr/bin/env bash

set -u

usage() {
    printf 'Usage: %s input.pdf output.pdf [screen|ebook|printer|prepress]\n' "$0"
}

if [[ $# -lt 2 || $# -gt 3 ]]; then
    usage
    exit 1
fi

input=$1
output=$2
profile=${3:-ebook}

if [[ ! -f "$input" ]]; then
    printf 'Error: Input file does not exist: %s\n' "$input" >&2
    exit 1
fi

if [[ ! -r "$input" ]]; then
    printf 'Error: Input file is not readable: %s\n' "$input" >&2
    exit 1
fi

case "$profile" in
    screen|ebook|printer|prepress) ;;
    *)
        printf 'Error: Invalid compression profile: %s\n' "$profile" >&2
        usage
        exit 1
        ;;
esac

if [[ "$input" -ef "$output" ]]; then
    printf 'Error: Input and output must be different files.\n' >&2
    exit 1
fi

if [[ -e "$output" ]]; then
    printf 'Error: Output already exists: %s\n' "$output" >&2
    printf 'Choose another filename to avoid overwriting it.\n' >&2
    exit 1
fi

if gs -sDEVICE=pdfwrite \
    -dCompatibilityLevel=1.4 \
    "-dPDFSETTINGS=/$profile" \
    -dNOPAUSE -dQUIET -dBATCH \
    -sOutputFile="$output" \
    "$input"; then

    printf 'Compression completed.\n'
    printf 'Output: %s\n' "$output"
    ls -lh -- "$input" "$output"
else
    rm -f -- "$output"
    printf 'Error: PDF compression failed.\n' >&2
    exit 1
fi
```

Save and exit the editor.

Make the script executable:

```bash
chmod +x pdf-compress.sh
```

### Usage

Default to the ebook profile:

```bash
./pdf-compress.sh original.pdf compressed.pdf
```

Use a specific profile:

```bash
./pdf-compress.sh original.pdf compressed.pdf screen
```

Other options:

```bash
./pdf-compress.sh original.pdf compressed.pdf printer
./pdf-compress.sh original.pdf compressed.pdf prepress
```

The script refuses to overwrite an existing output file, helping protect your documents.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 13. Create a Global Compression Command

To use the script from any directory, install it in `/usr/local/bin`.

```bash
sudo install -m 755 pdf-compress.sh /usr/local/bin/pdf-compress
```

Now run:

```bash
pdf-compress original.pdf compressed.pdf
```

For aggressive compression:

```bash
pdf-compress original.pdf compressed.pdf screen
```

Verify the command is available:

```bash
command -v pdf-compress
```

Expected result:

```text
/usr/local/bin/pdf-compress
```

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 14. Compare File Sizes

### 14.1 Display Human-Readable Sizes

```bash
ls -lh original.pdf compressed.pdf
```

### 14.2 Calculate the Percentage Reduction

Run:

```bash
original=$(stat -c '%s' original.pdf)
compressed=$(stat -c '%s' compressed.pdf)

awk -v original="$original" -v compressed="$compressed" '
BEGIN {
    if (original <= 0) {
        print "Error: Original file is empty."
        exit 1
    }

    printf "Original size:   %.2f MB\n", original / 1048576
    printf "Compressed size: %.2f MB\n", compressed / 1048576
    printf "Size reduction:  %.2f%%\n",
        (original - compressed) * 100 / original
}'
```

For example, reducing a 25 MB PDF to 5 MB gives an 80% reduction.

If the result is negative, the compressed file is larger than the original. This can happen when the original PDF is already optimized or the rewriting process adds overhead.

### 14.3 Verify the Output

Check its metadata:

```bash
pdfinfo compressed.pdf
```

Check that text remains extractable:

```bash
pdftotext compressed.pdf -
```

Open the file in a PDF viewer and inspect representative pages.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 15. Troubleshooting

### 15.1 `gs: command not found`

Install Ghostscript:

```bash
sudo apt update
sudo apt install ghostscript
```

### 15.2 The Output File Is Larger

Try a more aggressive profile:

```bash
gs -sDEVICE=pdfwrite \
  -dPDFSETTINGS=/screen \
  -dNOPAUSE -dQUIET -dBATCH \
  -sOutputFile=smaller.pdf \
  original.pdf
```

If the file is still large, inspect embedded images with `pdfimages -list`.

### 15.3 Text or Images Look Blurry

Use a less aggressive profile:

```bash
-dPDFSETTINGS=/printer
```

Alternatively, increase the image resolution in your custom command and use a higher JPEG quality setting.

### 15.4 Some Fonts or Layouts Change

Ghostscript rewrites the document, which can affect specialized fonts, transparency, forms, annotations, and other features. Compare the output against the original. If fidelity is critical, test a copy and consider a lossless optimization workflow instead.

### 15.5 The PDF Is Password-Protected

Some PDFs require a password before they can be processed. Supply the appropriate credentials using the tool's supported password options, or unlock the document through an authorized PDF application first.

Do not remove restrictions from documents you are not authorized to modify.

### 15.6 Digital Signatures Become Invalid

Rewriting a signed PDF typically invalidates its existing digital signature. Preserve the original signed document and compress an unsigned copy only when appropriate.

### 15.7 OCRmyPDF Reports an Error

Check the installed version:

```bash
ocrmypdf --version
```

Read its help:

```bash
ocrmypdf --help
```

Some options and behavior vary by version. For password-protected files, invalid input PDFs, or files with existing text layers, follow the specific error message.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 16. Best Practices

1. **Keep the original file.** Write compressed output to a different filename.
2. **Start with `/ebook`.** It is a reasonable general-purpose setting.
3. **Use `/screen` only when appropriate.** Smaller files can come at the cost of readability.
4. **Inspect scanned documents carefully.** Fine text and line drawings can degrade quickly.
5. **Use OCRmyPDF when searchable text is required.** Compression alone does not create an OCR text layer.
6. **Verify the output.** Check file size, metadata, text extraction, and visual quality.
7. **Preserve signed originals.** Rewriting can invalidate signatures.
8. **Avoid repeated lossy compression.** Recompressing an already compressed document can reduce quality further.
9. **Test specialized PDFs.** Interactive forms, embedded media, annotations, and accessibility features may need special handling.
10. **Choose resolution based on the document's purpose.** Archival scans and professional printing may need more detail than screen-only reading.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## 17. Quick Reference

Install the toolkit:

```bash
sudo apt update
sudo apt install ghostscript poppler-utils ocrmypdf
```

Compress a PDF for general use:

```bash
gs -sDEVICE=pdfwrite \
  -dPDFSETTINGS=/ebook \
  -dNOPAUSE -dQUIET -dBATCH \
  -sOutputFile=compressed.pdf \
  original.pdf
```

Compress aggressively:

```bash
gs -sDEVICE=pdfwrite \
  -dPDFSETTINGS=/screen \
  -dNOPAUSE -dQUIET -dBATCH \
  -sOutputFile=compressed.pdf \
  original.pdf
```

Inspect PDF metadata:

```bash
pdfinfo original.pdf
```

List embedded images:

```bash
pdfimages -list original.pdf
```

Extract text:

```bash
pdftotext original.pdf -
```

Optimize a scanned PDF with OCR:

```bash
ocrmypdf --optimize 2 original.pdf searchable.pdf
```

Compare sizes:

```bash
ls -lh original.pdf compressed.pdf
```

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->

## Conclusion

Ghostscript is a practical choice for routine PDF compression on Ubuntu/Linux. Use `/ebook` for everyday documents, `/screen` when file size is the priority, and custom image-resolution settings for scanned PDFs that need more precise control.

Use OCRmyPDF when scanned documents need searchable text and OCR-related processing. Always retain the original and verify that the resulting PDF meets your requirements for readability, document structure, and quality.

<!-- START "Jump to Top"-->
 <p align="right">
   <a href="#table-of-contents">Jump to Top ▲</a>
 </p>
<!-- END "Jump to Top" -->
