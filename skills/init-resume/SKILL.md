---
name: init-resume
description: Initialize rez-ame-zing with a base resume from DOCX or PDF format. Extracts text and stores it for future customizations.
---

# Initialize Base Resume

Parse and store a base resume from DOCX or PDF format.

## Input

The user must provide a **file path** to their resume. Supported formats:
- `.docx` (Microsoft Word)
- `.pdf` (PDF document)

If no path is provided, ask for it.

## Steps

### 1. Validate the File

Check that the file exists and has a supported extension (`.docx` or `.pdf`).

```powershell
Test-Path "<resume-path>"
```

If the file doesn't exist or has an unsupported extension, inform the user and ask for a valid path.

### 2. Create Data Directory

```powershell
New-Item -ItemType Directory -Path "$env:USERPROFILE\.rez-ame-zing" -Force
```

### 3. Extract Text

#### For DOCX files:

Use PowerShell to extract text from the DOCX (which is a ZIP file containing XML):

```powershell
python -c "
import zipfile
import xml.etree.ElementTree as ET
import sys

docx_path = sys.argv[1]
with zipfile.ZipFile(docx_path) as z:
    with z.open('word/document.xml') as f:
        tree = ET.parse(f)
        
ns = {'w': 'http://schemas.openxmlformats.org/wordprocessingml/2006/main'}
paragraphs = tree.getroot().iter('{http://schemas.openxmlformats.org/wordprocessingml/2006/main}p')
lines = []
for p in paragraphs:
    texts = [t.text for t in p.iter('{http://schemas.openxmlformats.org/wordprocessingml/2006/main}t') if t.text]
    line = ''.join(texts)
    if line.strip():
        lines.append(line)
print('\n'.join(lines))
" "<resume-path>"
```

#### For PDF files:

```powershell
python -c "
import subprocess, sys
# Try using pdfminer.six
try:
    from pdfminer.high_level import extract_text
    text = extract_text(sys.argv[1])
    print(text)
except ImportError:
    # Fallback: install and retry
    subprocess.check_call([sys.executable, '-m', 'pip', 'install', 'pdfminer.six', '-q'])
    from pdfminer.high_level import extract_text
    text = extract_text(sys.argv[1])
    print(text)
" "<resume-path>"
```

### 4. Review and Format Extracted Text

After extraction, review the raw text. Use your AI capabilities to:
- Clean up formatting artifacts (page numbers, headers/footers, odd spacing)
- Structure it as clean markdown with proper sections
- Preserve all content faithfully — do NOT add, remove, or modify actual resume content
- Present the formatted version to the user for review

### 5. Save Configuration

Save the formatted resume to `~/.rez-ame-zing/base-resume.md`.

Save metadata to `~/.rez-ame-zing/config.json`:

```json
{
  "baseResumePath": "<original-file-path>",
  "baseResumeFormat": "docx|pdf",
  "initializedAt": "<ISO-8601-timestamp>",
  "lastUpdatedAt": "<ISO-8601-timestamp>"
}
```

### 6. Confirm to User

Display:
- The formatted resume for review
- Confirmation that initialization succeeded
- The storage location
- Remind them they can re-run init to update their base resume

If the user wants changes to the extracted text, iterate with them until they're satisfied, then save.
