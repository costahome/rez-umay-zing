# resumazing 🎯

_Pronounced **rez-uh-may-zing** (like "resume" + "amazing")._

AI-powered resume customization agent for GitHub Copilot CLI.

## What It Does

resumazing helps you tailor your resume for specific job opportunities using AI, then tracks your application pipeline — all from within GitHub Copilot CLI.

### Core Features

1. **Initialize** — Parse your base resume from DOCX or PDF
2. **Customize** — AI-tailors your resume for a specific job description with keyword alignment, skill matching, and tone customization
3. **Manage Profiles** — Track applications with status, pay range, contacts, and notes

## Installation

Install as a Copilot CLI plugin:

```bash
copilot plugin install costahome/resumazing
```

Or install directly from local path:

```bash
copilot plugin install --path /path/to/resumazing
```

## Usage

### Initialize with your base resume

```
> Use the init-resume skill with my resume at ~/Documents/resume.docx
```

### Customize for a job

```
> Use customize-resume for a Senior Engineer role at Acme Corp.
> Here's the job description: [paste or provide file path]
> Focus: emphasize distributed systems and leadership experience
```

### Manage your pipeline

```
> List my profiles
> Update the acme profile status to applied
> View the acme profile
> Export the resume from my acme profile
```

## Skills

| Skill | Description |
|-------|-------------|
| `init-resume` | Parse and store a base resume from DOCX/PDF |
| `customize-resume` | AI-tailor resume for a job description |
| `list-profiles` | View all job profiles in table format |
| `view-profile` | See full profile details |
| `update-profile` | Change status, pay, contacts, notes |
| `export-resume` | Export customized resume to file |

## Data Storage

All data is stored locally at `~/.resumazing/`:

```
~/.resumazing/
├── config.json            # Base resume metadata
├── base-resume.md         # Extracted base resume (markdown)
└── profiles/
    └── {id}/
        ├── profile.json       # Metadata + status
        ├── resume.md          # Customized resume
        └── job-description.md # Original JD
```

> **Upgrading?** If you previously used this project under its old names
> (`~/.rez-umay-zing/` or `~/.rez-ame-zing/`), your existing data is preserved.
> The skills automatically migrate the legacy folder to `~/.resumazing/` on first
> use, so all your profiles, base resume, and config carry over.

## Profile Statuses

| Status | Meaning |
|--------|---------|
| `considering` | Interested, resume ready, not applied |
| `applied` | Application submitted |
| `interviewing` | Active interview process |
| `offer` | Received an offer |
| `declined` | You declined |
| `rejected` | Application rejected |
| `hired` | Accepted! |
| `archived` | No longer tracking |

## Requirements

- GitHub Copilot CLI (authenticated)
- Python 3.x (for DOCX/PDF text extraction)
  - `pdfminer.six` (auto-installed on first PDF parse)

## Philosophy

- **Never fabricates** — AI aligns your real experience, never invents
- **Keyword-focused** — Matches job description terminology naturally
- **Clean formatting** — Professional, ATS-friendly markdown output
- **Iterative** — Work with AI until you're satisfied
- **Local-first** — Your data stays on your machine
