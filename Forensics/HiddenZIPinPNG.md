## Challenge Writeup

An image file was recovered during digital forensics. At first glance, it appeared to be a simple gradient image, but the challenge hinted that hidden data might exist beyond what is immediately visible.

### Step 1: Metadata Analysis
The image metadata was inspected using standard tools. A suspicious comment field was discovered:
Comment : password=ctf72026


This suggested that the image might contain protected embedded data.

### Step 2: File Extraction
Next, `binwalk` was used to analyze and extract hidden content from the image:

```bash
binwalk -e challenge.png
```

This revealed and extracted an embedded ZIP archive.

### Step 3: ZIP Extraction
Attempting to unzip the archive prompted for a password. The password found in the metadata (`ctf72026`) was used successfully.

### Step 4: Retrieve the Flag
Inside the extracted archive, a file named `flas.txt` was found. Reading this file revealed the flag:

```bash
ctf7{zip_in_hidden_701fb29d}
```
