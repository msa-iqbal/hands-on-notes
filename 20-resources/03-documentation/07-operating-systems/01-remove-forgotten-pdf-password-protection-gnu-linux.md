# How to Remove Forgotten PDF Password Protection in GNU/Linux

Getting locked out of your own important PDF document is frustrating—especially when you need to print, edit, or extract data from it right away. Fortunately, Ubuntu and other Linux distributions offer a powerful suite of open-source command-line tools that let you inspect PDF encryption, recover a forgotten password for your own files, and generate a permanent, unlocked copy.

In this step-by-step guide, we will walk through the exact workflow using native Linux utilities.

# 🗁 Tools We Will Use

Before diving into the commands, here is a quick overview of the toolkit we are going to install:
  
* **qpdf:** Inspects PDF encryption details and decrypts files once authorized credentials are available.
* **John the Ripper:** A widely used password-auditing tool capable of recovering forgotten PDF passwords.
* **pdf2john:** A utility script that extracts password hashes from encrypted PDFs so John the Ripper can analyze them.
* **PDFtk:** A versatile toolkit for manipulating, merging, and decrypting PDF documents.
* **Poppler Utilities (`poppler-utils`):** A collection of tools (`pdfinfo`, `pdftotext`, etc.) used to inspect metadata and extract content.

# 🗁 Understanding PDF Password Protection

Not all PDF locks work the same way. PDF documents generally support two distinct types of password protection:

| Password Type | Description |
| --- | --- |
| **User Password (Open Password)** | Required just to open and read the contents of the PDF. |
| **Owner Password (Permissions Password)** | Allows anyone to view the document, but restricts actions like printing, editing, or copying text. |

> **Important Distinction:** If your PDF opens normally without a prompt but blocks you from printing or copying text, it has **owner-password protection**. If a dialog box asks for a password before displaying the pages, it has **open-password protection**.

### Step 1: Update Ubuntu and Install Required Applications

First, open your terminal (`Ctrl + Alt + T`) and update your system's package repositories to ensure you get the latest versions of the tools:

```bash
sudo apt update && sudo apt upgrade -y
```

Next, install all the required PDF utilities in one command:

```bash
sudo apt install qpdf pdftk-java john poppler-utils -y
```

Verify that every application was installed properly by checking their version numbers:

```bash
qpdf --version
pdftk --version
john --version
pdfinfo -v
```

### Step 2: Inspect the PDF Encryption

Navigate to the folder where your locked PDF is stored (for example, your `Downloads` directory) and make a backup copy before making any changes:

```bash
cd ~/Downloads
cp protected.pdf protected_backup.pdf
```

Run `qpdf` to inspect the document's encryption settings:

```bash
qpdf --show-encryption protected.pdf
```

You can also inspect the document's metadata using `pdfinfo`:

```bash
pdfinfo protected.pdf
```

Look closely at the **Encrypted** field in the terminal output. To quickly verify whether the file blocks reading without a password, test extracting text to standard output:

```bash
pdftotext protected.pdf -
```

If the terminal prompts you for a password or throws an authentication error, the file has **open-password protection**.

### Step 3: Handle Owner-Password Protection

If your PDF opens normally in a viewer but restricts printing, copying, or editing, inspect its permissions first:

```bash
qpdf --show-encryption protected.pdf
```

If you have the necessary authorization and the document permits access without an open password, you can often generate an unrestricted copy directly with `qpdf`:

```bash
qpdf --decrypt protected.pdf unlocked.pdf
```

Verify that the newly created file is structurally sound:

```bash
qpdf --check unlocked.pdf
```

*(Note: If the file enforces an owner password that requires credentials you do not have, `--decrypt` alone will not bypass it.)*

### Step 4: Recover a Forgotten Open Password

If your PDF requires a password just to open, you will need to recover the password first using **John the Ripper**.

#### 1. Locate the `pdf2john` Extraction Utility

Search your system to find where the `pdf2john` script is located:

```bash
find /usr -name 'pdf2john*' 2>/dev/null
```

*(Depending on your Ubuntu release, this script may be named `pdf2john.pl` or `pdf2john.py`, or included inside a John the Ripper Jumbo installation.)*

Check its usage instructions:

```bash
pdf2john.pl --help
```

#### 2. Extract the PDF Password Hash

Extract the cryptographic hash from your PDF and save it to a text file:

```bash
pdf2john.pl protected.pdf > pdf_hash.txt
```

This creates a small text file containing the encryption signature needed for password auditing.

#### 3. Run a Targeted Password-Recovery Strategy

John the Ripper supports multiple auditing methods:

* **Dictionary-based auditing:** Testing against lists of common or previously used passwords.
* **Custom wordlists:** Feeding a list of words, dates, or phrases you typically use.
* **Rule-based variations:** Automatically testing capitalization, numbers, and symbol substitutions on base words.
* **Brute-force auditing:** Testing character combinations within a specific length or pattern.

A **targeted recovery strategy** is dramatically faster than blind brute-forcing. If you remember that your password included a specific word, birth year, or character pattern, narrow your wordlist or mask rules accordingly.

#### 4. Check Your Recovery Results

Once John finishes (or while it runs), check if the password has been found:

```bash
john --show pdf_hash.txt
```

If successful, your password will appear right in the terminal output. Keep in mind that recovery is not guaranteed—long, randomly generated passwords may be computationally impractical to recover.

### Step 5: Create and Verify the Unlocked PDF

Once you have the recovered open password, use `qpdf` to strip the encryption and create a clean, unlocked PDF:

```bash
qpdf \
  --password='RECOVERED_PASSWORD' \
  --decrypt \
  protected.pdf \
  unlocked.pdf
```

*(Be sure to replace `RECOVERED_PASSWORD` with your actual password.)*

Alternatively, you can accomplish the exact same thing using **PDFtk**:

```bash
pdftk protected.pdf \
  input_pw 'RECOVERED_PASSWORD' \
  output unlocked.pdf
```

### Verify the New File

Before deleting anything, confirm that the new file is completely unlocked and free of errors:

```bash
# Confirm encryption is removed
qpdf --show-encryption unlocked.pdf

# Check internal file integrity
qpdf --check unlocked.pdf

# Review document metadata
pdfinfo unlocked.pdf
```

Finally, open the unlocked PDF in your default viewer to verify everything renders properly:

```bash
xdg-open unlocked.pdf
```

# 🗁 Useful Linux PDF Commands

Keep this cheat sheet handy for everyday PDF troubleshooting on Linux:

| Command | Purpose |
| --- | --- |
| `qpdf --check file.pdf` | Check PDF structural integrity |
| `qpdf --show-encryption file.pdf` | Inspect PDF encryption status and permissions |
| `qpdf --decrypt input.pdf output.pdf` | Remove encryption when permitted |
| `pdfinfo file.pdf` | Display PDF metadata and encryption flags |
| `pdftotext file.pdf -` | Extract plain text directly to the terminal |
| `pdftoppm -png file.pdf page` | Convert PDF pages into PNG images |
| `pdftk input.pdf dump_data` | Display detailed internal PDF document info |

*(Disclaimer: Always ensure you own or have explicit authorization to access and decrypt the PDF files you work on, and always keep an untouched backup of your original file.)*
