# Offerpath

A private job-search portal with a daily automated search, built as a plugin for Claude.

Designed and product-managed by Asmita Khude as a side project, and built with AI-assisted development.

## The problem

Searching for a senior role means checking dozens of job sites and careers pages every day, re-reading the same postings, and rewriting a resume for each application. Most of that time goes to finding and filtering, not to applying.

## What Offerpath does

Offerpath runs the search once a day and puts the results in one portal.

- **Finds jobs across many sources.** Indeed, ZipRecruiter and Dice through their connectors, plus employer careers pages, applicant systems and open job boards.
- **Filters on what matters to you.** Titles, locations, minimum pay, sponsorship, direct hire, and employers you never want to see.
- **Scores every job from 1 to 100 against your resume** and explains what matches and which gaps to expect.
- **Writes a tailored resume for each job**, using only what is in your own resume. Nothing is invented.
- **Tracks your pipeline.** Applied, responses, interviews and offers, with follow-up reminders and notes.
- **Watches at least 50 employers** chosen for your locations and field, on top of the open search.

It does not submit applications. You review, decide and apply.

## Components

| Skill | Use it when |
|---|---|
| `offerpath-setup` | First time: builds your portal, asks about you and your search, builds the watch list, connects job sites, schedules the daily search |
| `offerpath-manage` | Afterwards: run a search now, record an application, change settings, refresh the watch list, troubleshoot |

## Setup

1. Download `dist/offerpath.plugin` from this repository and open it in Claude to install the plugin.
2. Start a new task and say "Set up Offerpath".
3. Answer two short rounds of questions and attach your resume.
4. Connect the job-site connectors when Claude offers them (optional, recommended).

Requirements: a Claude plan that includes published pages with a database and scheduled tasks. Each person's portal, data and daily search run in their own Claude account.

## Sources

| Type | Sources |
|---|---|
| Job-site connectors | Indeed, ZipRecruiter, Dice |
| Employer applicant systems | Greenhouse, Lever, Ashby, Workday, SmartRecruiters, Workable, iCIMS |
| Open job boards | Wellfound, The Muse, We Work Remotely, Remotive, Y Combinator jobs, Built In |
| Sponsorship history | MyVisaJobs (past visa filings by employer) |
| Watch list | Careers pages of 50+ employers chosen for your locations and field |

Not covered: LinkedIn and Glassdoor block automated reading.

## Product decisions

- **Honest scoring over flattering scoring.** Scores name the hard requirements a resume does not meet, so a 60 means a stretch.
- **Tailoring without invention.** Each tailored resume lists what changed and what was deliberately left out.
- **Settings live in the portal.** Titles, pay, locations, sponsorship, watch list and exclusions are edited in one place, on a phone or a laptop, and apply from the next search.
- **One click to say no.** Skip with a reason or exclude an employer; skip reasons lower the score of similar jobs later.
- **Private by default.** Each user's portal, resume and results stay in their own account.

## Limits

- Postings rarely state whether they sponsor visas. Offerpath drops postings that rule it out and labels the rest.
- Some sites block automated reading; those are skipped, never worked around.
- The daily search uses the user's own Claude plan.

## Roadmap

- Shared updates so existing portals pick up new versions.
- Interview preparation notes for jobs marked Interview.
- Weekly summary of applications, responses and score trends.

## Version

0.1.0
