---
name: view-profile
description: View detailed information about a specific job profile including metadata, status, and the customized resume.
---

# View Job Profile

Display full details for a specific job profile.

## Input

The user should provide a **profile ID** or enough info to identify the profile (company name, partial ID, etc.).

If not provided or ambiguous, list available profiles and ask the user to choose.

## Steps

### 1. Locate Profile

Look up the profile in `~/.resumazing/profiles/`. If the user provided a partial match, find the best match by ID or company name.

```powershell
$dataDir = "$env:USERPROFILE\.resumazing"
if (-not (Test-Path $dataDir)) {
    foreach ($legacy in @("$env:USERPROFILE\.rez-umay-zing", "$env:USERPROFILE\.rez-ame-zing")) {
        if (Test-Path $legacy) { Rename-Item -LiteralPath $legacy -NewName ".resumazing"; break }
    }
}
Get-ChildItem -Path "$dataDir\profiles" -Directory | Where-Object { $_.Name -like "*<search>*" }
```

### 2. Load Profile Data

Read all files from the profile directory:
- `profile.json` — metadata
- `resume.md` — customized resume
- `job-description.md` — original job description

### 3. Display Profile

Present the profile in a clean, structured format:

```
## Profile: {company} — {title}

**ID:** {id}
**Status:** {status}
**Created:** {date}
**Updated:** {date}

### Metadata
- **Pay Range:** {payRange or "Not set"}
- **Contact:** {contactName} ({contactEmail}) or "Not set"
- **Application URL:** {url or "Not set"}
- **Notes:** {notes or "None"}

### Customized Resume
{resume content}

### Job Description
{job description — collapsed/summarized unless user asks for full}
```

### 4. Offer Actions

After displaying, suggest:
- `update-profile` to modify status, pay range, contacts, or notes
- `export-resume` to export the resume to a file
- `customize-resume` to create a new variation
