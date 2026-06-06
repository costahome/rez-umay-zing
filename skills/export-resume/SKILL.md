---
name: export-resume
description: Export a customized resume from a job profile to a file. Supports markdown, DOCX, and PDF output formats via pandoc conversion.
---

# Export Resume

Export the customized resume from a job profile to a file. Supports multiple output formats including DOCX and PDF via pandoc.

## Input

Required:
- **Profile ID** (or partial match / company name)

Optional:
- **Output path** — where to save the file (defaults to current directory with auto-generated name)
- **Format** — `md` (markdown, default), `docx` (Word), or `pdf`

If no profile ID is provided, list profiles and ask the user to choose.

## Steps

### 1. Locate Profile

Find the profile by ID or partial match in `~/.resumazing/profiles/` (migrate any
legacy folder from a previous name first so existing profiles are preserved):

```powershell
$dataDir = "$env:USERPROFILE\.resumazing"
if (-not (Test-Path $dataDir)) {
    foreach ($legacy in @("$env:USERPROFILE\.rez-umay-zing", "$env:USERPROFILE\.rez-ame-zing")) {
        if (Test-Path $legacy) { Rename-Item -LiteralPath $legacy -NewName ".resumazing"; break }
    }
}
```

### 2. Load Resume

Read `resume.md` from the profile directory.

### 3. Determine Output Path

If the user specified an output path, use it. Otherwise generate:
```
{company}-{title}-resume.{format}
```

in the current working directory.

### 4. Export

#### For markdown (`md`):

Write the resume content directly:

```powershell
$content = Get-Content -Path "$profileDir\resume.md" -Raw
Set-Content -Path "<output-path>" -Value $content -Encoding UTF8
```

#### For DOCX (`docx`):

Convert using pandoc:

```powershell
pandoc "$profileDir\resume.md" -o "<output-path>" --from markdown --to docx
```

#### For PDF (`pdf`):

Convert using pandoc (requires a PDF engine — tries pdflatex, falls back to built-in):

```powershell
pandoc "$profileDir\resume.md" -o "<output-path>" --from markdown --pdf-engine=pdflatex
```

If pdflatex is unavailable, try:
```powershell
pandoc "$profileDir\resume.md" -o "<output-path>" --from markdown --to pdf --pdf-engine=wkhtmltopdf
```

If no PDF engine is available, inform the user and suggest exporting to DOCX instead, which can be saved as PDF from Word or a similar application.

### 5. Verify

Check that the output file was created successfully:

```powershell
Test-Path "<output-path>"
(Get-Item "<output-path>").Length
```

### 6. Confirm

Display:
- Output file path and format
- File size
- Remind user of the profile it came from
- For DOCX/PDF: note that further formatting (fonts, margins, templates) can be adjusted in the target application
