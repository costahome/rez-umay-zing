---
name: customize-resume
description: Customize the base resume for a specific job. Takes a job description and optional focus prompt, uses AI to tailor the resume, iterates with the user, then saves as a job profile.
---

# Customize Resume for Job

Create a tailored resume for a specific job opportunity using AI customization.

## Input

Required:
- **Company name** — the company you're applying to
- **Job title** — the position title
- **Job description** — either inline text or a file path to read

Optional:
- **Focus prompt** — specific guidance for tone, emphasis, style, or other customization preferences

If any required input is missing, ask the user for it.

## Prerequisites

Resolve the data directory (migrating any legacy folder from a previous name so
existing data is preserved), then check that the base resume exists:

```powershell
$dataDir = "$env:USERPROFILE\.resumazing"
if (-not (Test-Path $dataDir)) {
    foreach ($legacy in @("$env:USERPROFILE\.rez-umay-zing", "$env:USERPROFILE\.rez-ame-zing")) {
        if (Test-Path $legacy) { Rename-Item -LiteralPath $legacy -NewName ".resumazing"; break }
    }
}
Test-Path "$dataDir\base-resume.md"
```

If not found, tell the user to run the `init-resume` skill first.

## Steps

### 1. Load Base Resume

Read the base resume from `~/.resumazing/base-resume.md`.

### 2. Load or Read Job Description

If the user provided a file path, read it. Otherwise use the inline text provided.

### 3. Generate Customized Resume

Using your AI capabilities, customize the resume following these principles:

**MUST DO:**
- Align skills, keywords, and terminology with the job description
- Reorder and emphasize experience that matches the role's requirements
- Tailor the professional summary/objective to the specific opportunity
- Incorporate relevant keywords from the job description naturally
- Quantify achievements where the source material supports it
- Maintain clean, professional markdown formatting
- Keep the same core information and truthful content

**MUST NOT:**
- Fabricate experience, skills, certifications, or qualifications
- Add technologies or tools not mentioned in the base resume
- Invent metrics, numbers, or achievements
- Change employment dates, company names, or job titles
- Remove significant experience (though de-emphasizing is fine)

**FOCUS PROMPT GUIDANCE:**
If a focus prompt is provided, apply it as an overlay. Examples:
- "Make it more concise" → Tighten language, remove redundancy
- "Emphasize leadership" → Highlight management, team-leading examples
- "Technical focus" → Lead with technical skills, detailed project descriptions
- "Casual tone" → Lighter language while remaining professional

### 4. Present to User

Show the customized resume and explain:
- Key changes made
- Keywords/skills aligned with the job description
- What was emphasized or de-emphasized
- How the focus prompt influenced the output (if provided)

### 5. Iterate

Ask the user if they'd like changes. Options:
- **Accept** — save the resume and create the profile
- **Revise** — provide feedback and regenerate
- **Cancel** — discard and exit

If revising, incorporate their feedback and generate an updated version. Repeat until they accept or cancel.

### 6. Save Profile

Generate a profile ID: `{company-slug}-{title-slug}-{YYYYMMDD}-{4-char-hex}`

Create the profile directory and files:

```
~/.resumazing/profiles/{id}/
  profile.json
  resume.md
  job-description.md
```

**profile.json:**
```json
{
  "id": "<generated-id>",
  "company": "<company-name>",
  "title": "<job-title>",
  "status": "considering",
  "createdAt": "<ISO-8601>",
  "updatedAt": "<ISO-8601>",
  "focusPrompt": "<focus-prompt-or-null>",
  "payRange": null,
  "contactName": null,
  "contactEmail": null,
  "applicationUrl": null,
  "notes": null
}
```

**resume.md:** The accepted customized resume.

**job-description.md:** The full job description text.

### 7. Confirm

Display:
- Profile ID and location
- Status (considering)
- Remind user they can use `update-profile` to change status, add contact info, etc.
- Remind user they can use `export-resume` to get the resume in a specific format
