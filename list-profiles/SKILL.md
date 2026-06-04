---
name: list-profiles
description: List all job profiles with their status, company, title, and dates in a scannable table format.
---

# List Job Profiles

Display all saved job profiles in a clean, scannable format.

## Steps

### 1. Check for Profiles

```powershell
$profilesDir = "$env:USERPROFILE\.rez-ame-zing\profiles"
if (-not (Test-Path $profilesDir)) {
    Write-Output "No profiles found"
} else {
    Get-ChildItem -Path $profilesDir -Directory | Select-Object -ExpandProperty Name
}
```

If no profiles exist, inform the user and suggest using `customize-resume` to create one.

### 2. Load Profile Data

For each profile directory, read `profile.json` and extract:
- ID
- Company
- Title
- Status
- Created date
- Last updated date
- Pay range (if set)

### 3. Display as Table

Present profiles in a markdown table sorted by most recently updated:

```
| # | Company | Title | Status | Pay Range | Updated |
|---|---------|-------|--------|-----------|---------|
| 1 | Acme Co | Sr Engineer | interviewing | $150-180k | 2026-01-20 |
| 2 | BigTech | Staff Dev | applied | — | 2026-01-18 |
| 3 | StartupX | Lead | considering | $140-160k | 2026-01-15 |
```

### 4. Offer Actions

After listing, suggest available actions:
- `view-profile` to see full details of a specific profile
- `update-profile` to change status or add metadata
- `export-resume` to get the customized resume
- `customize-resume` to create a new profile
