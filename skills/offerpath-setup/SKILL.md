---
name: offerpath-setup
description: >
  This skill should be used when the user asks to "set up Offerpath", "set up my job search",
  "build my job portal", "start an automated job search", "create a daily job search", or wants a
  private portal that finds jobs every day, scores them against their resume and writes a tailored
  resume for each. It connects the Indeed, ZipRecruiter and Dice job-site connectors and builds an
  employer watch list of at least 50 companies for the user's locations.
metadata:
  version: "0.1.0"
---

# Offerpath setup

Build the user's own private copy of Offerpath: a job-search portal plus a daily scheduled search that runs on their account. Everything created here belongs to this user. Never contact, reference or reuse anyone else's portal.

Work quietly. Keep messages short and in plain language; do not mention file names, placeholders or tool names to the user. Never invent an answer the user did not give.

If the user already has an Offerpath portal (check their artifacts for one titled "Offerpath"), do not build a second one. Use the offerpath-manage skill instead.

## Step 0. Check what is available

Three things are required: the Artifact tool (to publish a page), the ArtifactData tool (the page's database), and the scheduled-task tools (create_trigger and fire_trigger). Load deferred tools with ToolSearch. If any is missing, tell the user plainly which capability is unavailable in this session and stop.

## Step 1. Publish the portal

1. Read `references/portal-page.html` from this skill's directory and write it to a working file named `offerpath.html`, byte for byte. Do not redesign, shorten or improve it. It contains two placeholders: `__RUN_TIME__` and `__TRIGGER_ID__`.
2. For now replace every `__RUN_TIME__` with `7:00 PM` and leave `__TRIGGER_ID__` as it is.
3. Publish with the Artifact tool, icon `briefcase`, description "Your daily job pipeline: ranked matches, tailored resumes, application and response tracking.", and exactly these capabilities:
   `{"db": {"rules": [{"path": "", "read": "admin", "write": "admin"}]}, "downloads": true, "mcp": {"servers": [{"server": "Claude Code Remote", "tools": ["fire_trigger"]}]}}`
4. Keep the artifact URL as PORTAL_URL.

## Step 2. Ask the basics, then the search settings

Ask in two short rounds with the question tool. Skip anything the user already said.

Round 1, about them:
- Full name, email and phone (used in the header of their tailored resumes).
- Current or most recent job title.
- Their resume: ask them to attach the file or paste the text. Read it and keep the full plain text.

Round 2, the search:
- Job titles they want. Offer 5 to 8 suggestions drawn from the resume and let them edit.
- Focus areas or industries, suggested from the resume.
- Locations: cities, and whether remote roles in their country are wanted.
- Minimum base pay per year, and whether to keep jobs that do not post pay.
- Whether they need visa sponsorship, and which visa.
- Whether to include senior individual-contributor roles, and whether full-time direct hire only.
- What time of day the search should run, and their time zone.
- Employers to exclude, if any.

## Step 3. Build the watch list (at least 50 employers)

Build this automatically from their answers; do not ask the user to list employers. Follow `references/watch-list.md`. The result is at least 50 real employers that fit their locations and field. Show the list grouped by reason, let them remove or add names, then save it. Make clear the watch list only adds direct checks: the daily search covers any employer.

## Step 4. Save everything to the portal database

With ArtifactData and PORTAL_URL, write these documents in collection `meta`:

- `settings`: `{"fullName", "currentTitle", "email", "phone", "roles": [..], "keywords": [..], "locations": [..], "minSalary": number, "keepUnpostedPay": bool, "sponsorship": bool, "sponsorshipType": "", "includePrincipal": bool, "directHireOnly": bool, "targetEmployers": [..], "excludeEmployers": [..], "excludeKeywords": [], "minScore": 60, "maxPerNight": 10, "coverLetters": false, "recheckOpen": true, "updatedAt": ISO time}`
- `resume`: `{"text": full resume text, "fileName": their file's name or "Resume", "updatedAt": ISO time}`
- `profile`: `{"resumeBase": {...}, "resumeSourceUpdatedAt": the same updatedAt as resume}`. resumeBase is the resume restructured as `{"name", "phone", "email", "facts": one or two sentences of headline facts, "experience": [{"title", "company", "location", "dates", "bullets": [..]}], "skills": [..], "education": [..], "certifications": [..]}`. Use only what the resume says. Fix obvious typos, write bullets in plain natural language, invent nothing.
- `run`: `{"lastRun": ""}`

Read one document back to confirm the writes landed.

## Step 5. Connect the job-site connectors

Search the connector directory for job-search connectors (keywords: job search, jobs, indeed, ziprecruiter, dice, job board) and suggest every job-seeker search connector found. At the time of writing these are Indeed, ZipRecruiter and Dice; suggest any new ones the directory returns too. Ignore employer-side recruiting and HR tools. Explain in one line that connecting them widens coverage and that the search still works without them. Do not block on this step.

## Step 6. Create the daily scheduled task

Create a scheduled task named "Offerpath daily job search" at the time and time zone they chose, with notifications on. If the time is exactly on the hour or half hour, schedule it one or two minutes earlier. Its prompt is the text in `references/daily-search-prompt.md` below that file's divider line, with `{{PORTAL_URL}}` replaced by PORTAL_URL and `{{PERSON}}` by their full name. Keep the prompt otherwise unchanged; scheduled runs cannot see this plugin.

Then finish the page: in `offerpath.html` replace `__TRIGGER_ID__` with the task id, replace the temporary `7:00 PM` run time with their real time and zone (for example `8:00 AM Eastern`), and publish again to the same artifact with capabilities unchanged.

## Step 7. First search and hand-over

Start the scheduled task once now so they see real results. Then tell them, briefly:

- The portal is ready; suggest pinning it. It works on phone and laptop.
- First results arrive in roughly 10 to 15 minutes.
- Everything can be changed later under Settings in the portal.
- The search finds and ranks jobs and writes a tailored resume for each. It never applies on their behalf.
- Tailored resumes only reorder and reword what is in their resume.
- Which job-site connectors are connected, and that LinkedIn and Glassdoor are not covered because they block automated reading.

State plainly if any step could not be completed.
