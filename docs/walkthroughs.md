# Walkthrough checks

For whoever edits these instructions. The agent never reads this file during a run.

After any change to `AGENTS.md`, `instructions/` or the templates, walk through every scenario below against the new text. Each must work without contradictions. These are static checks of the instructions, not live application tests. Never claim a live test unless a real browser run was done separately.

## Setup

1. **Fresh setup.** Missing files are created from the templates; the resume is copied to `personal_data/resume.pdf`; the profile is filled in from the resume and shown once for corrections; only the missing facts are asked, one topic per message; empty contextual and learned answer sections are accepted; nothing is applied before the setup check passes.
2. **Interrupted setup.** Saved answers, learned answers included, stay; setup continues at the first missing item.
3. **Skipped optional fields.** `None` is saved once, never asked again, and never typed into a form.
4. **Portal login only when needed.** The company portal login is asked only when the plan has `workday`, `company_direct`, `discovery` or a job source, and before `ready`.

## Plan and start

5. **Same plan as last time.** With an earlier record and none today, `start` offers that plan in one line, with the model advice and the `ready` check; `ready` starts the run. A normal day takes three replies: `start`, `ready`, `ok`.
6. **First run ever.** With no earlier record, the plan is asked first, then the check is shown.
7. **Plan change at the check.** A change in the reply goes back through the plan steps (limits, permission sentence, portal login, model advice), becomes today's plan (written to the record after `ready`), and the check is shown again.
8. **Power check.** Shown at every `start` and in a new chat that resumes the day; nothing happens in the browser before `ready`.
9. **Model advice.** Given once before `ready`, from the plan: light for quick-apply sites only, medium with Workday, company sites, discovery or a job source. It never names a model, and the run never stops to ask for a switch.
10. **Permission prompts.** With `workday`, `company_direct`, `discovery` or a job source in the plan, one sentence says the AI app may ask once per company site.
11. **New chat or model handoff.** Everything is already in the files; `start` in the new chat resumes today's record without repeating setup or the plan.

## Applying

12. **Passed over in the results list.** Jobs whose title, location or work mode clearly do not fit are not opened, get no entry, are never sent to NorthList, and are counted in the session summary.
13. **Opened and turned down.** A job that was opened and then fails the fit check (required, not preferred, experience included) gets a `skipped` entry with the reason.
14. **Normal sign-in request.** A signed-out or login page: the user signs in in the current tab and replies `done`; the sign-in is checked and the same job continues. Not a site stop.
15. **Real security restriction.** A CAPTCHA, unusual activity, a security check, a restriction or a daily-limit message on a job site stops that site for the local date, with a `## Site Stops` line and a `skipped` entry whose reason starts with `site stopped:`. That plan item is finished for the day; later jobs get no entries of their own. A CAPTCHA on one employer's own form skips only that job.
16. **First instant submit.** The preview (the job and the profile details Naukri will send) and `ok` come before Naukri's one-click Apply.
17. **Unsupported upload.** The user uploads `personal_data/resume.pdf` in the current tab and replies `done`; the upload is checked.
18. **Missing answer, later retry.** A required question without a saved answer gives `needs_user`; the answer is saved with its context (under `## Contextual Answers` when it holds only for an employer, country, role or portal, otherwise under `## Learned Answers`); the job may then be tried again, as a new entry.
19. **Prefilled facts differ.** A portal's prefilled value that differs from `profile.md` is corrected from it, or the job becomes `needs_user`.

## Counts, duplicates and pacing

20. **Counting.** At `start` and in a new chat, one search of today's headings and `- Platform:`, `- Selected plan choice:` and `- Status:` lines gives the counts, each site's last entry time and the plan table's counts; during the run the counts are kept in the chat; the whole record is never re-read.
21. **Duplicates.** A job is looked up by its ID, or its URL, with a search across `applied/` and the old `tracking/applied_jobs.csv`; a match counts only when its site and company match too. Any `applied` or `skipped` match: never again, except a job skipped only because its site was stopped or at its daily limit, which may be tried on a later day. Only `needs_user` matches: only after the answer is saved.
22. **Two sessions on one date.** Both add to the same record; the counts include both; each session has its own `Started` line.
23. **Midnight.** The old record's table is brought up to date; a new record starts with the same plan and the new date's limits, with this session's `Started` line copied; duplicate searches still cover earlier days; the 2-minute gap is still measured from the last submit.
24. **Waiting with other work.** The gap is checked before a job's form is opened. While that site waits, the agent does one job from the next plan item whose gap has passed, then comes back. A form that is already open is finished, and its submit waits for the rest of the gap. No job is `skipped` because of a wait, and there are never two unfinished forms.
25. **Only one plan item.** It waits only the rest of each gap, then continues.
26. **Limits.** Per-site, company_direct plus discovery, and total limits hold across sessions; a lower limit in the profile and the plan's numbers only lower them.
27. **Selectable job sources.** Each of the six can be a plan item; the source is recorded, the real application page is looked at, and the job follows the discovery, company_direct or Workday steps by where it is submitted.
28. **Source with mixed destinations.** A job source gives one job going to Workday while Workday is at its limit (that job is `skipped` with the reason) and one going to Lever (applied): only the blocked job is skipped.
29. **Overlapping sources.** Discovery finds a We Work Remotely job: `Selected plan choice` decides which plan item counts it, `Platform` which site limit, and the total counts it once.
30. **Discovery into Lever.** The found Lever form is continued where it is with the company_direct steps; no saved career URL is needed.
31. **Discovery into Workday.** The current Workday form is continued with the Workday steps and counts as `workday`.
32. **Unconfirmed action.** A profile, a talent sign-up, an account or an Apply click without a confirmation is never `applied`.
33. **Unavailable route.** A destination that none of the steps fit, or an email-only job, is reported as unavailable; no flow is invented and no email is sent.
34. **Freshness controls.** Newest and Past 24 hours are chosen and checked when present; missing controls are told to the user in the chat; a shortfall is reported; older jobs are never added.

## Records, browser and session

35. **Old record today.** A record made before this version is resumed by adding entries at its end; earlier entries change only in their sync line.
36. **Existing browser window.** The open window is reused; no second window; one browser task per plan item's batch.
37. **Job opens in a new tab.** The form is done in the work tab, which is closed after the entry is added; the anchor tab stays; the Back button is never used.
38. **Tab cap.** With three plan items the cap is 5; the oldest work tab is closed first.
39. **Browser disconnects.** Two failures in a row show the checklist, wait for `ready`, and retry once without starting a new session; a second failure ends the session cleanly.
40. **Session limit.** Read from the profile (default 4 hours). When reached, only the job already in a form is finished and submitted, the summary is given, nothing is `skipped` because of it, and `start` in a new chat continues the day.
41. **Same step fails three times.** That workflow (for example `workday`) stops for the session; jobs that lead to it are skipped with that reason and may be tried on a later day; everything else goes on; the session summary suggests a stronger model for it next time.

## NorthList sync

42. **Sync off.** `NorthList server: None`: no tool call, no message, no sync line.
43. **Old profile line.** A profile that still says `Career Companion server: <address>` turns sync on the same way.
44. **Sync on.** After `ready` the server is woken; each `applied` entry is sent right after it is added and gets `sent`; a stopped session keeps what was sent; at the end, the `skipped` (opened and turned down) and `needs_user` entries are sent with their reason.
45. **Server asleep or down.** A send fails, is retried once with the next send, and if it still fails is resent at the end of the session; applying never waits for it. An `invalid_input` answer is never retried by the agent; the user hears which entry and field, and can fix it and ask for a replay.
46. **Tools missing.** One message at start; sends are marked `failed (tools not available)`; applying goes on.
47. **Server named differently.** The tools are found by name even when the server is called `northlist` in the AI app.
48. **Replay.** `sync NorthList for YYYY-MM-DD` twice: the first time only entries not marked `sent` or `already recorded` are sent; the second time nothing new is recorded. The old wording `sync Career Companion for YYYY-MM-DD` works the same.

## Connect NorthList

49. **Claude Code.** The pasted setup prompt leads to `instructions/northlist_connect.md`: one `claude mcp add --transport http --scope user northlist <server>/mcp --header ...` (an older `northlist` entry removed first), the profile line set, and the user told to open a new chat, then `start`, and to allow each NorthList tool for good the first time it runs. The app's permission settings are never changed by the agent. The token is never written into the folder or repeated, and `claude mcp get` is never run.
50. **Codex.** `~/.codex/config.toml` gets `[mcp_servers.northlist]` with `url` and `http_headers`, replacing an older table; one approval; then a restart; the tools are allowed on first use.
51. **Antigravity.** `~/.gemini/config/mcp_config.json` gets `northlist` with `serverUrl` and `headers`, kept valid JSON; never the folder's `.agents/mcp_config.json`; then Refresh.
52. **Another app.** The user is told the address and header to add by hand; the profile line is still set.
53. **Before setup.** With no `personal_data/profile.md`, it is created from the template with only the NorthList line set; `start` then runs setup.
54. **Reconnect.** A new prompt after a token expired replaces the old `northlist` entry; other servers stay; at `start` the tools are found again.
55. **Old profile line.** A `Career Companion server:` line becomes the NorthList line with the new address.
