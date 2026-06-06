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

## Example Walkthrough

Here's a typical end-to-end session, from importing your resume to exporting a
tailored version for a specific job.

### 1. Initialize your base resume

**Prompt:**

```
> Use init-resume with my resume at ~/Documents/jane-doe-resume.docx
```

**Result:**

```
✓ Base resume imported from jane-doe-resume.docx
  Stored at: ~/.resumazing/base-resume.md
  9 sections detected · 642 words

You're all set! Run customize-resume to tailor it for a job.
```

### 2. Customize for a job

**Prompt:**

```
> Use customize-resume for a Senior Software Engineer role at Acme Corp.
> Job description: ~/Downloads/acme-sse-jd.txt
> Focus: emphasize distributed systems and team leadership
```

**Result:**

```
Tailored resume for Acme Corp — Senior Software Engineer

Key changes I made:
  • Rewrote the summary to lead with distributed systems + mentoring
  • Surfaced your Kafka / event-driven work to match the JD's "high-throughput" requirement
  • Reordered skills so Go, gRPC, and Kubernetes appear first
  • Pulled forward the payments-platform project (matches "fault-tolerant services")
  • Kept all dates, titles, and employers unchanged — nothing fabricated

Keywords aligned: distributed systems, event-driven, observability, SLOs, mentoring

Accept / Revise / Cancel?
```

**Prompt (iterate):**

```
> Revise: tighten the summary to two sentences and add my AWS experience
```

**Result:**

```
Updated. The summary is now two sentences and calls out AWS (EKS, SQS, DynamoDB).

Accept / Revise / Cancel?

> Accept

✓ Saved profile: acme-corp-senior-software-engineer-20260606-7f3a
  Status: considering
```

### 3. Track your pipeline

**Prompt:**

```
> List my profiles
```

**Result:**

```
| # | Company   | Title                    | Status       | Pay Range  | Updated    |
|---|-----------|--------------------------|--------------|------------|------------|
| 1 | Acme Corp | Senior Software Engineer | considering  | —          | 2026-06-06 |
| 2 | BigTech   | Staff Engineer           | interviewing | $190-220k  | 2026-06-02 |
```

**Prompt:**

```
> Update the acme profile: status applied, pay range $170-200k,
> notes "Submitted via referral from Sam"
```

**Result:**

```
✓ Updated profile: Acme Corp — Senior Software Engineer

Changes:
  • Status: considering → applied
  • Pay Range: set to $170-200k
  • Notes: Added "[2026-06-06] Submitted via referral from Sam"
```

### 4. Export the tailored resume

**Prompt:**

```
> Export the resume from my acme profile as docx
```

**Result:**

```
✓ Exported resume
  Profile: Acme Corp — Senior Software Engineer
  File:    ./acme-corp-senior-software-engineer-resume.docx
  Format:  docx · 18.4 KB

Open it in Word to fine-tune fonts and margins, or export to PDF from there.
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
