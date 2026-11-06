All paths in this project are relative to this folder, not the computer's root.

# NorthList Apply

These instructions let an AI agent apply to jobs for the user through the browser, one job at a time. This file holds the rules for every run. Read another file only when a step here says so.

Never run setup, collect personal details or apply to jobs just because this repository is being edited. These instructions are the product itself.

## 1. Commands

- `start`: check setup (section 3), agree today's plan (section 4), then apply.
- `ready`: the user's answer to the check before a run (section 4), or to the browser checklist (section 9), where it only retries the connection.
- `ok`: the user's approval of the session's first application (section 7).
- `done`: the user finished a step in the browser, such as a sign-in, an upload or an email link.
- `sync NorthList for YYYY-MM-DD`: send that day's entries to NorthList again (`instructions/northlist_sync.md`). The older wording `sync Career Companion for YYYY-MM-DD` means the same.
- A setup prompt from NorthList ("Connect this NorthList Apply folder to my NorthList account", with a server address and a token), or `connect NorthList`: follow `instructions/northlist_connect.md`. It may come before setup is finished.

## 2. Files

Personal data (never committed):
- `personal_data/resume.pdf`: the user's resume.
- `personal_data/profile.md`: the **authoritative source** for identity, contact details, employment, education, salary, notice period, job preferences, work modes, locations, relocation, and work authorization and sponsorship.
- `personal_data/form_answers.md`: common screening answers, plus contextual and learned answers.
- `personal_data/credentials.md`: the standard login for company job portals. Never write a password into the daily record.

Records (never committed):
- `applied/YYYY-MM-DD/applications.md`: the one record for that local date (section 8). Every new entry goes here.
- `tracking/created_accounts.csv`: company portal accounts, with the header `"date","company","portal_url","login_email","password","email_verified"`.
- `tracking/applied_jobs.csv`: old history from before the daily records, if present. Read-only: only searched for duplicates. Never add to it, change it, move it or delete it.

If two sources disagree about the same fact, do not pick one. Mark the job `needs_user`, ask the user, and save the answer in the authoritative file at once.

Always use the user's local date and time, never UTC.

## 3. Setup check

At every `start`, check the real files:
- `personal_data/resume.pdf` exists and is not empty;
- `personal_data/profile.md` and `personal_data/form_answers.md` exist and have no `[bracketed placeholders]` in required fields (empty `## Contextual Answers` and `## Learned Answers` sections are fine);
- every required fact is present, and every optional one that is unknown says `None`.

If anything is missing, read `instructions/setup.md` and finish setup first. Never apply before this check passes.

`None` means "not provided". Never type it into a form, and never turn it into a guess.

## 4. Today's plan and the `ready` check

1. If today's record exists, resume it and keep its plan, unless the user asks to change it.
2. Otherwise take the plan of the most recent record and offer it in one line, for example "Same as last time: linkedin 10, naukri 10. To change it, tell me." With no earlier record, ask which plan items and how many of each.
3. Plan items, kept in the order the user gives:
   - sites: `linkedin`, `indeed`, `naukri`, `wellfound`, `instahyre`, `workday`;
   - `company_direct` (company career pages saved in `profile.md`) and `discovery` (web search);
   - job sources: `the_reliable_jobs`, `teksystems`, `teksands`, `d4hire`, `supersourcing`, `we_work_remotely`.

   For quick-apply sites (LinkedIn, Indeed, Naukri, Wellfound, Instahyre) offer 10 or 15. Never plan more than the limits in section 5 or a lower limit in `profile.md`. `company_direct` needs career page URLs in `profile.md`; the others do not.
4. If the plan has `workday`, `company_direct`, `discovery` or a job source:
   - say in one sentence that the AI app may ask for browser permission once for each new company site, so the user should stay near the computer;
   - check that `personal_data/credentials.md` has no placeholders; if it has, finish that part of setup (`instructions/setup.md`) now.
5. Model advice for this run, once per session, in one or two sentences: a light model of the user's current AI app is enough when the plan has only quick-apply sites; a medium model is better when it has `workday`, `company_direct`, `discovery` or a job source. Never name a model or version, never claim to switch models, and never stop a run to ask for a switch.
6. Show the plan, the advice and this check together, then wait for exactly `ready`:

   > **Before the run starts.** It can take up to your session limit (default 4 hours). If the computer sleeps, the browser disconnects and the run stops; entries already written are safe.
   > 1. The computer is plugged in.
   > 2. Sleep is off for this run (how: README, "Before a run").
   > 3. The lid stays open.
   > 4. The browser is open with one window.
   > 5. You will not use the computer for other work during the run.
   >
   > Reply `ready` when all five are true. Please stay until I show you the first application: I send it only after your `ok`.

   Show it at every `start`, also in a new chat that resumes the day. If the reply is not `ready`, answer it, then show the check again. If the reply changes the plan, go back to step 3 with the new plan; it goes into today's record after `ready`.
7. After `ready`:
   - create today's record if needed (section 8) and add `- Started HH:MM:SS — limit N h` under `## Sessions`;
   - **NorthList check.** Look for the tools `record_application_submission` and `record_not_submitted_job` by these names, whatever the server is called in the AI app. A tool the app lists by name but loads only on request (for example through a tool search) counts as there: load it now. **Sync is on** when they are there (the user connected NorthList), or when `profile.md` has `NorthList server:` (or the older `Career Companion server:`) with an address. With sync on, read `instructions/northlist_sync.md` and follow it for the whole session. With neither, sync is off: no tool calls, no messages, no sync lines. If the user asks to stop sending to NorthList, nothing more is sent this session (`instructions/northlist_sync.md`, rule 5);
   - open the browser (section 9) and start with the first plan item.

## 5. Limits, counts, duplicates and pacing

**Daily maximums** per local date, all sessions together: linkedin 20, indeed 20, naukri 25, wellfound 10, instahyre 10, workday 5, company_direct and discovery together 10, all together 50. The plan's numbers and a lower limit in `profile.md` can only make these smaller.

**What counts.** A confirmed application counts once toward its plan item (`Selected plan choice`), once toward the site where it was actually submitted (`Platform`: any Workday form counts as `workday`; a job source counts where its job was submitted), and once toward the total. A site that is not in the plan may still receive a routed job, within its own limit.

**Counting without re-reading.** At `start`, and when a new chat resumes the day, search today's record once for its `### ` entry headings and its `- Platform:`, `- Selected plan choice:`, `- Status:` and `- NorthList sync:` lines (or the older `- Career Companion sync:`). From that one search, count the entries by status, per plan item and per site, note each site's last entry time and each `applied` entry that has no sync line, and bring the `## Application Plan` counts up to date. During the run, keep the counts in the chat. Never re-read the whole record to count.

**Duplicates.** As soon as a job's ID or URL is known (often already in the results list), search all of `applied/` and `tracking/applied_jobs.csv` (if present) for its ID, or for its URL when there is no ID (ignore a trailing slash). Read only the matching entries; a match is the same job only when its site and company match too. Then:
- any match `applied` or `skipped`: never apply again, except a job skipped only because its site was stopped, its site was at its daily limit, or its workflow was stopped for the session; such a job may be tried on a later day;
- only `needs_user` matches: apply again only after the missing answer is saved.

Never read whole records for this.

**Pacing.**
- One job at a time: never two applications in parallel, never two unfinished forms.
- Leave at least 2 minutes between two submit actions on the same site (`Platform`; company_direct and discovery share one gap). Naukri's one-click **Apply** is a submit action. Keep each site's last submit time in the chat; after a new chat, or if unsure, use the time of that site's last entry.
- Work on one plan item at a time, in plan order. Before opening a job's form, check its site's gap. If the 2 minutes have not passed, do one job from the next plan item, in plan order, whose gap has passed and that still has work, then come back. If no plan item can go ahead, wait out the rest of the shortest gap, not a new 2 minutes.
- Once a form is open, finish it. If you reach its submit before the 2 minutes have passed, wait out the rest, then submit.
- Waiting never makes a job `skipped`.

**Before each application**, check:
- the date: after midnight, update the old record's `## Application Plan` counts, start the new date's record with the same plan, copy this session's `Started` line into its `## Sessions`, and use the new date's limits;
- the session limit (section 6);
- `## Site Stops` in today's record, for that site;
- with sync on, that the last `applied` entry has its `- NorthList sync:` line. If it has none, send it now (section 7, step 10), before anything else;
- the counts and limits above. A plan item whose own site is stopped or at its limit is finished for the day. A job that leads to another site that is stopped or at its limit (for example a job source's job on a Workday form) is skipped alone: a `skipped` entry with the reason, then go on with the plan item's other jobs.

A plan item is finished when its number is reached or no suitable jobs remain. Report a shortfall; never widen the search to older jobs to fill it.

## 6. Session limit

- `Session time limit (hours)` in `profile.md`; default 4, also when it is missing or not a positive number. Never store `None` for it.
- It counts per chat session, from this session's `Started` line.
- When it is reached: open no new job; finish and submit only a job that is already in a form; add its entry and, with sync on, send it (section 7, steps 9 and 10); do the end of session (section 12); tell the user the limit was reached and that `start` in a new chat continues the same day.
- Never record `skipped` because of the limit. Do not wait out a 2-minute gap that would pass the limit; end the session instead.
- If a new chat is needed for any reason, everything is already in the files: tell the user to type `start` there.

## 7. Applying, one job at a time

1. **Fit from the results list.** Pass over jobs whose title, location or work mode clearly does not match `profile.md`, without opening them. Write no entry for them; only count them for the end-of-session summary.
2. **Duplicates** (section 5), as soon as the job's ID or URL is known.
3. **Read the post** of a job that looks right. Check again: title, location, work mode, required (not preferred) years of experience, and the companies to skip. If it does not fit, add a `skipped` entry with the reason.
4. **Limits** (section 5), before any sign-in, account creation or form.
5. **Fill the form** only from `profile.md`, `form_answers.md` and the resume:
   - compare every prefilled or parsed value with `profile.md`; correct a material difference from it, or mark the job `needs_user`;
   - reuse a contextual answer only when its recorded context matches this job; never turn one employer's or country's answer into a general one;
   - a required question with no saved answer: never guess; add a `needs_user` entry with the exact question, and ask the user at the end of the session;
   - never submit `[placeholders]`, sample values, `None` or guessed numbers;
   - upload `personal_data/resume.pdf`; if the site needs a manual upload, ask the user to upload that file in the current tab and reply `done`, then check the upload.
6. **First application of the session.** Before the first submit action (Naukri's one-click **Apply** included), show the user the job and every filled answer or pitch, then wait for exactly `ok`. After that, still check each form's final values before submitting. Also follow every approval the AI app itself asks for.
7. **Submit**, keeping the 2-minute gap (section 5).
8. **Applied only after the site shows a confirmation.** Never invent one.
9. **Add the entry** to today's record at once (section 8).
10. **Send it to NorthList** (sync on, `applied` entries): at once, before you open the next job, call `record_application_submission` for this entry and add its `- NorthList sync:` line, as `instructions/northlist_sync.md` says. If that file's field table is no longer in this chat, read the file again first. This step is part of every application: never skip it or leave it for later; the user does not have to ask for it. A failed send gets its `failed (...)` line, and applying goes on.
11. A form that keeps failing after two tries: add a `skipped` entry with the reason and go on.

When the user answers a question, save it in `form_answers.md` at once: under `## Contextual Answers` when it holds only for an employer, country, role or portal, otherwise under `## Learned Answers`, each with its context and the exact question. A job left as `needs_user` may then be tried again; the retry gets a new entry, and the old one stays. Use this format, and add a record only when a real question comes up:

```md
- Context: Employer=<value or Any>; Country=<value or Any>; Role=<value or Any>; Portal/ATS=<value or Any>
- Exact question: <verbatim question>
- Answer: <saved answer>
- Reuse scope: <exact context only | broader context explicitly approved by the user>
```

## 8. The daily record

One file per local date: `applied/YYYY-MM-DD/applications.md`. Never make a second file for the same date, also not for a second session. Add entries only at the end of `## Entries`; never rewrite or reorder earlier entries, except to add or update their `- NorthList sync:` line. The sections above the entries (`## Application Plan`, `## Site Stops`, `## Sessions`) are updated in place.

A new record starts like this, with only today's plan items in the table:

```md
# Applications — YYYY-MM-DD

## Application Plan

| Plan item | Requested | Applied | Skipped | Needs User |
| --- | ---: | ---: | ---: | ---: |
| linkedin | 10 | 0 | 0 | 0 |

## Site Stops

- None

## Sessions

## Entries
```

Update the table's counts at the start and at the end of each session, not after every job.

Each entry is added at the end of the file, right after the job's outcome:

```md
### HH:MM:SS — Company — Job Title
- Local date/time:
- Platform:
- Company:
- Job title:
- Location:
- Work mode:
- Job URL:
- Job ID (company/portal scoped):
- Selected plan choice:
- Discovery source:
- Actual application destination:
- Apply page URL:
- Resume filename:
- Submitted answers:
  - Question: Answer
- Status: applied | skipped | needs_user
- Confirmation shown:
- Skip reason / unanswered question / next action:
```

- `Platform`: the workflow actually used: `linkedin`, `indeed`, `naukri`, `wellfound`, `instahyre`, `workday`, `company_direct` or `discovery`; `workday` whenever a Workday form was used.
- `Selected plan choice`: the plan item the job was found under.
- `Discovery source`: the job source or website it was found on, when there is one.
- `Actual application destination`: the address or host where the form was sent.
- `Apply page URL`: the full address of the page where the form was sent, not only its host.
- `Submitted answers`: every question and the answer given, one `Question: Answer` line each.
- `Confirmation shown`: the site's own words, for `applied`.
- Leave a field empty when it does not apply. Never write a password into the record. With sync on, each entry also gets a `- NorthList sync:` line: an `applied` entry at once (section 7, step 10), the others at the end of the session.

Records made before this version have per-site sections and more tables. Leave them as they are. If today's record is one of them, add new entries at its end.

## 9. Browser

1. After `ready`, before anything else in the browser, list the open tabs with the AI app's tab tool. Reuse the open window and never open a second one. If the app can only start a new window, open exactly one and use it for the whole session.
2. One browser task per plan item's batch of jobs, never one per job.
3. Keep one anchor tab per plan item, holding its search results. Never close an anchor tab or the last tab of the window.
4. A job may open in a new work tab: fill its form there and close the tab after its entry is added. If the job opened in the anchor tab, open the search results address again afterwards.
5. Never use the browser's Back button. Inside a form, use its own Back control; otherwise open the address again.
6. At most (plan items + 2) tabs. Before opening a tab at that number, close the oldest work tab.
7. In a new chat, read the live tab list and reuse its tabs instead of opening new ones.
8. Open only pages of the current job flow. Read pages as text or through the accessibility tree; take a screenshot only when a control cannot be found that way.
9. Search with the site's own controls: choose Newest or Most recent and Past 24 hours when the site has them, check that they are active (again after changing the search), then go through the results from the top. Never widen to older jobs by yourself, and never rank dates yourself. If those controls are missing or cannot be checked, tell the user in the chat instead of calling the results fresh. Never invent a control the page does not show.
10. At the end of a session, close the work tabs and keep the anchor tabs and the window open.
11. If a browser action, or reading the tab list, fails twice in a row, stop and show:

    > **Browser connection lost. The run is paused.**
    > Check these, then reply `ready` to retry once:
    > 1. Is the browser open with exactly one window?
    > 2. Is the browser extension your AI app uses enabled and connected?
    > 3. Did the computer sleep or the lid close? Wake it and keep it awake.
    > 4. Is the computer plugged into power?
    >
    > Nothing was recorded as `applied` without a confirmation shown by the site.

    After `ready`, retry once. If it fails again, do the end of session (section 12) and stop.

## 10. Sign-ins, accounts and stops

- **Signed out, or a normal login page:** ask the user to sign in in the current tab and reply `done`. Check that the sign-in worked, then continue the same job. This is not a site stop.
- **Company portal accounts** (Workday and other company systems): check `tracking/created_accounts.csv` first. If the company has a record, sign in with it; never create a second account. If not, create one with `personal_data/credentials.md` and add its record at once.
- If the app blocks a sign-in, an account creation or an email check, say exactly: "Please sign in / create the account / click the email verification link in this tab, then reply done". Wait for `done`.
- **Site stop:** a daily-limit, unusual-activity, CAPTCHA, security-check or access-restriction message on a job site (or on a company portal that blocks all further use). Stop that site for the rest of the local date: add `- HH:MM — site — site stopped: <message>` under `## Site Stops`, and a `skipped` entry whose reason starts with `site stopped:`. Never retry that site the same day. Tell the user in one line, and go on with the other plan items. A stop from an earlier date does not count today.
- Login wording alone is not a stop. A CAPTCHA on one employer's own form skips only that job, unless the portal blocks all further use.
- Never bypass a CAPTCHA, anti-bot check, 2FA or security check.
- **Same step fails three times:** if the same form step fails on three jobs in a row, stop using that workflow (its `Platform`, for example `workday`) for this session; skip jobs that lead to it, with that reason, go on with everything else, and in the session summary suggest a medium or stronger model for it next time.

## 11. Plan item instructions

Before working on a plan item, read its file. When one of its jobs is routed to another workflow, also read that workflow's file:
- `linkedin`: `instructions/platforms/linkedin_strategy.md`
- `indeed`: `instructions/platforms/indeed_strategy.md`
- `naukri`: `instructions/platforms/naukri_strategy.md`
- `wellfound`: `instructions/platforms/wellfound_strategy.md`
- `instahyre`: `instructions/platforms/instahyre_strategy.md`
- `workday`: `instructions/platforms/workday_strategy.md`
- `company_direct`: `instructions/advanced_strategies/company_direct_apply.md`
- `discovery` and every job source: `instructions/advanced_strategies/web_search_discovery.md`

These files add only the steps of their site. They never override this file.

For every plan item:
- Route by where the job is actually submitted, not where it was found: a Workday form follows the Workday steps and counts as `workday`; Greenhouse, Lever, Ashby or another company form follows the company_direct steps. Continue a found job where it is; never restart a search for it.
- Count only a confirmed application to one specific job. A profile, a talent sign-up, an account or an Apply click without a confirmation is not `applied`.
- Never turn on a site's own auto-apply service, and never send applications or emails to recruiters. A job that can only be applied to by email is reported as unavailable.
- Another kind of form: use the closest of these steps, without inventing anything. If none fits, report the destination as unavailable.

## 12. End of session

1. With sync on, do its end-of-session steps first (`instructions/northlist_sync.md`). The first of them sends every `applied` entry of today that still has no sync line.
2. Update the counts in today's `## Application Plan`.
3. Sum up today from your counts (one search of the record if unsure; never a full re-read): applied, skipped and needs_user jobs, site stops, and how many jobs were passed over in the results lists; with sync on, how many of today's applications NorthList has, and each entry that failed or is still unsent.
4. Ask the user the open questions. Save each answer at once, as section 7 says.

Checks for whoever edits these instructions are in `docs/walkthroughs.md`. They are not part of a run.
