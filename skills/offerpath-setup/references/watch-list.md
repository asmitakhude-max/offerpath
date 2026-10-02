# Building the watch list

The watch list is a set of employers whose careers pages the daily search checks directly. It is built automatically during setup and must hold at least 50 employers. It never limits the search: jobs from any employer are still found through the job sites and the open web.

## How to build it

Draw from four groups and combine them, removing duplicates and anything on the user's exclusion list.

1. **Local employers, for each city in their locations (aim for 20 to 30 per metro).** The largest and best-known employers in that metro that hire for the user's kind of role: headquarters, major regional offices, and well-funded local companies. Use web search for current "largest employers" and "top companies hiring" lists for that metro and field rather than relying on memory alone.
2. **Employers in their focus areas (aim for 15 to 25).** Companies whose product or business is the user's domain, wherever they are based, when the user accepts remote roles or the company has an office in one of their cities.
3. **Remote-friendly employers (aim for 10 to 15, only when remote is wanted).** Companies known to hire remotely in the user's country for this kind of role.
4. **Past sponsors (only when the user needs sponsorship).** Prefer employers with a record of visa filings. myvisajobs.com publishes past filings by employer; use it to favor sponsors and to drop employers with no filing history when a group is over-full. Past filings are history, not a promise.

If the user gave only "remote" and no city, build the list from groups 2, 3 and 4.

## Quality rules

- Every name must be a real, currently operating employer. Do not pad the list with invented or doubtful names; if fewer than 50 solid names come from the first pass, widen the metro radius or add adjacent industries until there are at least 50.
- Use the name people would recognize on a careers page ("JPMorgan Chase", not a legal entity name).
- Leave out staffing agencies and recruiters.
- Leave out the user's current employer unless they ask for it.
- Do not include an employer the user excluded.

## Confirm with the user

Show the list in short groups with a one-line reason per group (for example "Large employers in Chicago", "Data platform companies", "Remote-friendly"). Let them remove or add names. Save the confirmed list as `targetEmployers` in `meta/settings`.

## Refreshing later

When the user asks to refresh or extend the watch list, or changes their locations, rebuild with the same method, keep the names they added by hand, and never re-add a name on their exclusion list.
