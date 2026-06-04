---
name: export-resume
description: Export a customized resume from a job profile to a file. Supports markdown output with clean formatting.
---

# Export Resume

Export the customized resume from a job profile to a file.

## Input

Required:
- **Profile ID** (or partial match / company name)

Optional:
- **Output path** — where to save the file (defaults to current directory with auto-generated name)
- **Format** — `md` (markdown, default). Future: `docx`, `pdf`

If no profile ID is provided, list profiles and ask the user to choose.

## Steps

### 1. Locate Profile

Find the profile by ID or partial match in `~/.rez-ame-zing/profiles/`.

### 2. Load Resume

Read `resume.md` from the profile directory.

### 3. Determine Output Path

If the user specified an output path, use it. Otherwise generate:
```
{company}-{title}-resume.md
```

in the current working directory.

### 4. Export

Write the resume content to the output file:

```powershell
$content = Get-Content -Path "$profileDir\resume.md" -Raw
Set-Content -Path "<output-path>" -Value $content -Encoding UTF8
```

### 5. Confirm

Display:
- Output file path
- File size
- Remind user of the profile it came from
- Suggest reviewing the file and formatting as needed for submission

## Future Format Support

When DOCX/PDF export is needed, this skill can be extended to:
- Use `pandoc` for markdown → DOCX/PDF conversion
- Apply professional templates
- Handle formatting that markdown can't express

For now, markdown is the primary format — it's clean, portable, and easily converted with external tools.
