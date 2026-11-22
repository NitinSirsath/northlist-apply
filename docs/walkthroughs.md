# Walkthrough checks

For whoever edits these instructions. The agent never reads this file during a run.

After any change to `AGENTS.md`, `instructions/` or the templates, walk through every scenario below against the new text. Each must work without contradictions. These are static checks of the instructions, not live application tests. Never claim a live test unless a real browser run was done separately.

## Setup

1. **Fresh setup.** Missing files are created from the templates; the resume is copied to `personal_data/resume.pdf`; the profile is filled in from the resume and shown once for corrections; only the missing facts are asked, one topic per message; empty contextual and learned answer sections are accepted; nothing is applied before the setup check passes.
2. **Interrupted setup.** Saved answers, learned answers included, stay; setup continues at the first missing item.
3. **Skipped optional fields.** `None` is saved once, never asked again, and never typed into a form.
4. **Portal login only when needed.** The company portal login is asked only when the plan has `workday`, `company_direct`, `discovery` or a job source, and before `ready`.

## Plan and start

5. **Same plan as last time.** `start` asks which sites and how many today and shows the last plan in one line, with nothing else; nothing happens until the user answers. `same` keeps it; then the model advice and the `ready` check follow, and `ready` starts the run. A resumed day asks the same way, showing what is done so far. A normal day takes three replies: `start`, `same` and `ready` (two more, `sign in` and `done`, when a site needs signing in first).
6. **First run ever.** With no earlier record, the plan is asked first, then the check is shown.
7. **Plan change at the check.** A change in the reply goes back through the plan steps (limits, permission sentence, portal login, model advice), becomes today's plan (written to the record after `ready`), and the check is shown again.
8. **Power check.** Shown at every `start` and in a new chat that resumes the day; nothing happens in the browser before `ready` (picking the browser only looks at the app's tools), except the sign-in tabs after `sign in`.
9. **Model advice.** Given once before `ready`, from the plan: light for quick-apply sites only, medium with Workday, company sites, discovery or a job source. It never names a model, and the run never stops to ask for a switch.
10. **Permission prompts.** With `workday`, `company_direct`, `discovery` or a job source in the plan, one sentence says the AI app may ask once per company site.
11. **New chat or model handoff.** Everything is already in the files; `start` in the new chat resumes today's record without repeating setup or the plan.

## Applying

12. **Passed over in the results list.** Jobs whose title, location or work mode clearly do not fit are not opened, get no entry, are never sent to NorthList, and are counted in the session summary.
13. **Opened and turned down.** A job that was opened and then fails the fit check (required, not preferred, experience included) gets a `skipped` entry with the reason.
14. **Normal sign-in request.** A signed-out or login page: the user signs in in the current tab and replies `done`; the sign-in is checked and the same job continues. Not a site stop.
15. **Real security restriction.** A CAPTCHA, unusual activity, a security check, a restriction or a daily-limit message on a job site stops that site for the local date, with a `## Site Stops` line and a `skipped` entry whose reason starts with `site stopped:`. That plan item is finished for the day; later jobs get no entries of their own. A CAPTCHA on one employer's own form skips only that job.
16. **Instant submit.** The final values are checked before Naukri's one-click Apply; there is no preview and no `ok`, also for the first job of a run.
17. **Unsupported upload.** The user uploads `personal_data/resume.pdf` in the current tab and replies `done`; the upload is checked.
18. **Missing answer, later retry.** A required question without a saved answer gives `needs_user`; the answer is saved with its context (under `## Contextual Answers` when it holds only for an employer, country, role or portal, otherwise under `## Learned Answers`); the job may then be tried again, as a new entry.
19. **Prefilled facts differ.** A portal's prefilled value that differs from `profile.md` is corrected from it, or the job becomes `needs_user`.

## Counts, duplicates and pacing

20. **Counting.** At `start` and in a new chat, one search of today's headings and `- Platform:`, `- Selected plan choice:` and `- Status:` lines gives the counts, each site's last entry time and the plan table's counts; during the run the counts are kept in the chat; the whole record is never re-read.
21. **Duplicates.** A job is looked up by its ID, or its URL, with a search across `applied/` and the old `tracking/applied_jobs.csv`; a match counts only when its site and company match too. Any `applied` or `skipped` match: never again, except a job skipped only because its site was stopped or at its daily limit, which may be tried on a later day. Only `needs_user` matches: only after the answer is saved.
22. **Two sessions on one date.** Both add to the same record; the counts include both; each session has its own `Started` line.
23. **Midnight.** The old record's table is brought up to date; a new record starts with the same plan and the new date's limits, with this session's `Started` line copied; duplicate searches still cover earlier days.
24. **Short gap.** A submit less than 30 seconds after the last one on the same site waits out the rest. No job is `skipped` because of a wait, and there are never two unfinished forms.
25. **Plan order.** Plan items are worked in the order given; the run does not switch sites to fill a wait.
26. **Limits.** Per-site, company_direct plus discovery, and total limits hold across sessions; a lower limit in the profile and the plan's numbers only lower them.
27. **Selectable job sources.** Each of the five supported ones can be a plan item; the source is recorded, the real application page is looked at, and the job follows the discovery, company_direct or Workday steps by where it is submitted.
28. **Source with mixed destinations.** A job source gives one job going to Workday while Workday is at its limit (that job is `skipped` with the reason) and one going to Lever (applied): only the blocked job is skipped.
29. **Overlapping sources.** Discovery finds a Supersourcing job: `Selected plan choice` decides which plan item counts it, `Platform` which site limit, and the total counts it once.
30. **Discovery into Lever.** The found Lever form is continued where it is with the company_direct steps; no saved career URL is needed.
31. **Discovery into Workday.** The current Workday form is continued with the Workday steps and counts as `workday`.
32. **Unconfirmed action.** A profile, a talent sign-up, an account or an Apply click without a confirmation is never `applied`.
33. **Unavailable route.** A destination that none of the steps fit, or an email-only job, is reported as unavailable; no flow is invented and no email is sent.
34. **Freshness controls.** Newest and Past 24 hours are chosen and checked when present; missing controls are told to the user in the chat; a shortfall is reported; older jobs are never added.

## Records, browser and session

35. **Old record today.** A record made before this version is resumed by adding entries at its end; earlier entries change only in their sync line.
36. **Existing browser window.** The open window of the browser in use (the AI app's own, or the extension backup) is reused; no second window; one browser task per plan item's batch.
37. **Job opens in a new tab.** The form is done in the work tab, which is closed after the entry is added; the anchor tab stays; the Back button is never used.
38. **Tab cap.** With three plan items the cap is 5; the oldest work tab is closed first.
39. **Browser disconnects.** Two failures in a row show the checklist, wait for `ready`, and retry once without starting a new session; a second failure ends the session cleanly. The checklist asks about the extension only with the extension backup.
40. **Session limit.** Read from the profile (default 4 hours). When reached, only the job already in a form is finished and submitted, the summary is given, nothing is `skipped` because of it, and `start` in a new chat continues the day.
41. **Same step fails three times.** That workflow (for example `workday`) stops for the session; jobs that lead to it are skipped with that reason and may be tried on a later day; everything else goes on; the session summary suggests a stronger model for it next time.

## NorthList sync

42. **Sync off.** The AI app has no NorthList tools and the profile says `NorthList server: None`: no tool call, no message, no sync line.
43. **Old profile line.** A profile that still says `Career Companion server: <address>` turns sync on the same way.
44. **Sync on.** After `ready` the server is woken (when the profile has an address) and the user hears once that NorthList is connected; each `applied` entry is sent right after it is added, before the next job is opened, and gets `sent`; a stopped session keeps what was sent; at the end, the `skipped` (opened and turned down) and `needs_user` entries are sent with their reason.
45. **Server asleep or down.** A send fails, is retried once with the first later send that comes at least 60 seconds after the failure, and if it still fails is resent at the end of the session; applying never waits for it. An `invalid_input` answer is never retried by the agent; the user hears which entry and field, and can fix it and ask for a replay.
46. **Tools missing.** The profile has an address but the tools are not there, or a listed tool cannot be loaded: one message at start; sends are marked `failed (tools not available)`; applying goes on.
47. **Server named differently.** The tools are found by name even when the server is called `northlist` in the AI app, and also when the app lists them by name and loads them only on request.
48. **Replay.** `sync NorthList for YYYY-MM-DD` twice: the first time only entries not marked `sent` or `already recorded` are sent; the second time nothing new is recorded. The old wording `sync Career Companion for YYYY-MM-DD` works the same.
49. **Connected, profile line `None`.** The tools are there, and the profile line says `None` or is missing: sync is on; no wake request; the user hears once that NorthList is connected; every `applied` entry is sent.
50. **No send skipped.** Before each new job, the last `applied` entry has its sync line; if it has none, it is sent first. Sends are never left for later, and the agent never waits for the user to ask.
51. **Missed send in an earlier chat.** A new chat resumes the day while some `applied` entries have no sync line: they are sent right after `ready`, before the first job.
52. **Long run.** Late in a long run, or after the AI app shortened the chat, the field table is no longer at hand: the agent reads `instructions/northlist_sync.md` again before the next send, and every `applied` entry still gets its send and its line.
53. **End of session.** Any `applied` entry still without a sync line is sent first; the summary says how many of today's applications NorthList has, and names each failed or unsent entry with `sync NorthList for YYYY-MM-DD`.
54. **User says stop.** The user asks to stop sending to NorthList: nothing more is sent this session, including at its end; every entry of the session without a sync line, and every later one, gets `not sent (stopped by the user)`, so a later session does not send them; only a replay the user asks for does. The user hears once that removing NorthList from the AI app turns sync off for good.

## Connect NorthList

55. **Claude Code.** The pasted setup prompt leads to `instructions/northlist_connect.md`: one `claude mcp add --transport http --scope user northlist <server>/mcp --header ...` (an older `northlist` entry removed first), the profile line set, and the user told to open a new chat, then `start`, and to allow each NorthList tool for good the first time it runs. The app's permission settings are never changed by the agent. The token is never written into the folder or repeated, and `claude mcp get` is never run.
56. **Codex.** `~/.codex/config.toml` gets `[mcp_servers.northlist]` with `url` and `http_headers`, replacing an older table; one approval; then a restart; the tools are allowed on first use.
57. **Antigravity.** `~/.gemini/config/mcp_config.json` gets `northlist` with `serverUrl` and `headers`, kept valid JSON; never the folder's `.agents/mcp_config.json`; then Refresh.
58. **Another app.** The user is told the address and header to add by hand; the profile line is still set.
59. **Before setup.** With no `personal_data/profile.md`, it is created from the template with only the NorthList line set; `start` then runs setup.
60. **Reconnect.** A new prompt after a token expired replaces the old `northlist` entry; other servers stay; at `start` the tools are found again.
61. **Old profile line.** A `Career Companion server:` line becomes the NorthList line with the new address.

## Job sources

62. **We Work Remotely not supported yet.** The user asks for `we_work_remotely`: told in one line, left out of the plan. Discovery finds a job there: its page is not opened; the same job on the company's own career page is used instead, or it is left. No sign-up is ever opened.

## Which browser

63. **App with a built-in browser.** The run uses the AI app's own browser, picked from the app's tools before the check; check item 3 names it; no browser extension is used, even when one is installed. A new chat picks again, with the same result in the same app.
64. **App without one.** For example Claude Code in a terminal: the run uses the browser extension as the backup; item 3 says so, asks for one browser window and a connected extension, and offers `sign in` for the plan's sites.
65. **No switch during a session.** The built-in browser fails twice, also right after `ready`: the checklist (without the extension line), one retry, then the end of the session. The run never moves to the extension because the built-in browser fails.
66. **Signed out in the built-in browser.** The user is signed in to a site in their usual browser but not in the app's own: the run asks them to sign in in that tab and reply `done`; this is not a site stop.
67. **No upload tool.** The browser cannot take the resume file: the user is asked to upload `personal_data/resume.pdf` in the current tab and reply `done`, and the upload is checked. No script is run in the page to add the file.
68. **Sign in first.** First run, plan linkedin 10, naukri 10, workday 3: check item 3 names LinkedIn and Naukri (not Workday) and offers `sign in`. The user replies `sign in`: one tab per site opens on its home page; the user signs in and replies `done`; the agent says which tab still shows a sign-in page and shows the check again; it never types a job site's password. After `ready`, those tabs are reused as anchor tabs. A second `sign in`, or one in a new chat, reuses tabs already on those sites; the user may still reply `ready` with a site signed out, and that site asks for its sign-in when reached.
69. **Nothing to sign in to.** A plan with only `workday`, `company_direct`, `discovery` or job sources: item 3 says the run uses the AI app's own browser and there is nothing to open; no site list and no `sign in` offer.
70. **Only new sites.** Earlier records have `linkedin` in their plan; today's plan adds `naukri`: item 3 names only Naukri. A day with no new site shows the short line ("…nothing for you to open"); `sign in` still opens all of today's account sites when the user asks for it.

## Portal accounts

71. **New Workday account.** A Workday job at Acme: no record in `tracking/created_accounts.csv`, so an account is made with `credentials.md` and its record added at once. After the confirmation and the entry, the send includes `portalAccount` with `signInUrl` = the record's `portal_url`, `loginEmail`, `password`, `emailVerified`, `madeBy: northlist_apply` and `createdOn`. The password is not in the entry, the sync line or any message.
72. **Account reused.** A second Acme job on the same Workday site: the existing record is used to sign in, and the same `portalAccount` is sent again with this application.
73. **No portal account.** A LinkedIn Easy Apply, or a company form that needed no account: `portalAccount` is left out. A record for another company, or for the same company on another host, is never sent.
74. **Account made by the user.** The portal blocks account creation; the user makes the account and replies `done`: its record is added with the login email and an empty `password`, never asking for the password. The send has `portalAccount` without `password` and with `madeBy: user`.
75. **User says no passwords.** The user says not to send portal passwords: `- NorthList portal passwords: off` is saved in the profile and said once; from then on, also in a new chat and in `send portal accounts for ...`, `portalAccount` goes without `password`. "Don't send portal accounts" saves `- NorthList portal accounts: off` and leaves `portalAccount` out.
76. **Older days.** `send portal accounts for 2026-11-20`: that day's `applied` Workday and company-portal entries with a matching record are sent again with `portalAccount`; their sync lines stay as they are, and entries with no record are left.
77. **Failed send.** A send with `portalAccount` fails: the line is `failed (<reason>)` with no password in it, and the retry sends `portalAccount` again.
78. **Password question on a form.** A form asks to set a password or a security answer: the entry's `Submitted answers` line says `(not recorded)`, so neither the record nor the send holds it.
