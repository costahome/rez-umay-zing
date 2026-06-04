---
name: update-profile
description: Update a job profile's status, pay range, contact info, application URL, or notes.
---

# Update Job Profile

Modify metadata on an existing job profile.

## Input

Required:
- **Profile ID** (or partial match / company name)

Optional (at least one required):
- **status** — new status value
- **pay-range** — salary/compensation range (e.g., "$150-180k", "€80-100k")
- **contact-name** — recruiter or hiring manager name
- **contact-email** — contact email address
- **application-url** — URL to the job posting or application
- **notes** — freeform notes (appended, not replaced, unless user says to replace)

If no profile ID or update fields are provided, ask the user.

## Valid Status Values

- `considering` — Interested, resume customized, not yet applied
- `applied` — Application submitted
- `interviewing` — In active interview process
- `offer` — Received an offer
- `declined` — User declined the opportunity
- `rejected` — Application was rejected
- `hired` — Accepted and hired
- `archived` — No longer actively tracking

## Steps

### 1. Locate Profile

Find the profile by ID or partial match in `~/.rez-ame-zing/profiles/`.

If ambiguous, list matches and ask user to confirm.

### 2. Load Current Profile

Read `profile.json` from the profile directory.

### 3. Apply Updates

Update the specified fields. For notes:
- If the profile has existing notes and user is adding, append with a timestamp
- If user says "replace notes" or "clear notes", replace entirely
- Format: `[YYYY-MM-DD] note text`

Update `updatedAt` timestamp.

### 4. Save

Write the updated `profile.json` back to disk.

```powershell
# Example: write JSON
$json | ConvertTo-Json -Depth 10 | Set-Content -Path "$profilePath\profile.json" -Encoding UTF8
```

### 5. Confirm

Display the updated profile summary showing what changed:

```
✓ Updated profile: {company} — {title}

Changes:
  • Status: considering → applied
  • Contact: Added john@acme.com
  • Notes: Added "Submitted via LinkedIn"
```
