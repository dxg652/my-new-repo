# New England $300k+ Job Search Agent

This directory backs a recurring Claude Code Remote Routine that searches job
boards for roles matching David Goldsztajn Farelo's profile, paying more than
$300,000/year total comp, based in (or open to remote from) New England.

## Files

- `PROFILE.md` — distilled candidate profile and target-role criteria, derived
  from the CV in Google Drive ("David Goldsztajn Farelo PhD CV"). Update this
  file if the CV changes materially.
- `seen_jobs.json` — dedup state. Every run reads it, only reports postings
  not already listed, then appends newly reported postings and commits the
  update. Without this, every run would re-report the same listings.

## What each run should do

1. Read `job-search-agent/PROFILE.md` and `job-search-agent/seen_jobs.json`
   from this repo (branch `claude/job-board-agent-new-england-uampm1`).
2. Search job boards for open roles matching the target role types in
   PROFILE.md, located in or open to remote from New England (MA, CT, RI, NH,
   VT, ME), with total annual comp > $300,000. Cover:
   - General boards: LinkedIn Jobs, Indeed, company career pages
   - Executive/high-comp boards: Ladders, ExecThread, Otta
   Use web search against these sites (e.g. `site:linkedin.com/jobs`,
   `site:theladders.com`, etc.) plus general search for senior AI/research
   leadership roles at Boston/Cambridge-area biotech, health systems,
   universities, and foundations.
3. Filter out any posting whose identifier (URL, or company+title if no
   stable URL) already appears in `seen_jobs.json`.
4. For each new match, capture: title, company, location, comp (if
   disclosed, else mark "not disclosed — inferred fit"), posting URL, and a
   one-line rationale for why it fits the profile.
5. Append the new matches to `seen_jobs.json` (`seen` array) and commit +
   push that update to the branch with a short commit message.
6. Reply with the run's findings as the final message of the session:
   - If new matches: list them (title, company, location, comp, link,
     1-line fit rationale), grouped by state.
   - If no new matches: say so briefly — do not pad the report.
   Keep the report skimmable; this is what the user sees as a push
   notification / email summary.

## Cadence

Runs weekly (see the Routine `New England $300k+ Job Search`, created via
`create_trigger`). Adjust the Routine's schedule via `update_trigger` if a
different cadence is wanted.
