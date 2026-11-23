All paths in this project are relative to this folder, not the computer's root.

# NorthList Apply

These instructions let an AI agent apply to jobs for the user through the browser, one job at a time. This file holds the rules for every run. Read another file only when a step here says so.

Never run setup, collect personal details or apply to jobs just because this repository is being edited. These instructions are the product itself.

## 1. Commands

- `start`: check setup (section 3), agree today's plan (section 4), then apply.
- `ready`: the user's answer to the check before a run (section 4), or to the browser checklist (section 9), where it only retries the connection.
- `sign in`: at the check before a run, open job sites so the user can sign in to them first (section 4, step 6).
- `same`: at the plan question, keep the plan it shows (section 4).
- `done`: the user finished a step in the browser, such as a sign-in, an upload or an email link.
- `sync NorthList for YYYY-MM-DD`: send that day's entries to NorthList again (`instructions/northlist_sync.md`). The older wording `sync Career Companion for YYYY-MM-DD` means the same.
- `send portal accounts for YYYY-MM-DD`: send that day's company-portal accounts to NorthList with their applications (`instructions/northlist_sync.md`, "Replay on request").
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

## 3. Update check and setup check

**Update check, first at every `start`.** Read `VERSION` from `https://raw.githubusercontent.com/NitinSirsath/northlist-apply/master/VERSION` (this exact address, never another). If its `version:` is newer than the one in this folder's `VERSION`, read `instructions/update.md` and follow it. If it is the same, or the check fails (offline, any error), say nothing and go on. Never check or update during a run.

Then, at every `start`, check the real files:
- `personal_data/resume.pdf` exists and is not empty;
- `personal_data/profile.md` and `personal_data/form_answers.md` exist and have no `[bracketed placeholders]` in required fields (empty `## Contextual Answers` and `## Learned Answers` sections are fine);
- every required fact is present, and every optional one that is unknown says `None`.

If anything is missing, read `instructions/setup.md` and finish setup first. Never apply before this check passes.

`None` means "not provided". Never type it into a form, and never turn it into a guess.

## 4. Today's plan and the `ready` check

1. **Ask for the plan first and wait.** At every `start`, ask which plan items and how many of each, in one line, then wait for the answer. Show nothing else in that message (no check, no advice; only the long-chat line of section 6 may come above it) and never open a browser or apply before the answer. Show the last plan with it: "Which sites and how many today? Last time: linkedin 10, naukri 10. Reply `same` to keep it." When today's record exists, show its plan with what is already done instead ("Today so far: linkedin 4 of 10, naukri 0 of 10. Reply `same` to go on."). With no earlier record, ask without a last plan.
2. The answer is `same` (keep that plan) or new plan items and counts. Only then go on with steps 3 to 6. Today's record keeps the plan the user confirmed.
3. Plan items, kept in the order the user gives:
   - sites: `linkedin`, `indeed`, `naukri`, `wellfound`, `instahyre`, `workday`;
   - `company_direct` (company career pages saved in `profile.md`) and `discovery` (web search);
   - job sources: `the_reliable_jobs`, `teksystems`, `teksands`, `d4hire`, `supersourcing`. `we_work_remotely` is not supported yet: if the user asks for it, say so in one line and leave it out of the plan.

   For quick-apply sites (LinkedIn, Indeed, Naukri, Wellfound, Instahyre) offer 10 or 15. Never plan more than the limits in section 5 or a lower limit in `profile.md`. `company_direct` needs career page URLs in `profile.md`; the others do not.
4. If the plan has `workday`, `company_direct`, `discovery` or a job source:
   - say in one sentence that the AI app may ask for browser permission once for each new company site, so the user should stay near the computer;
   - check that `personal_data/credentials.md` has no placeholders; if it has, finish that part of setup (`instructions/setup.md`) now.
5. Model advice for this run, once per session, in one or two sentences: a light model of the user's current AI app is enough when the plan has only quick-apply sites; a medium model is better when it has `workday`, `company_direct`, `discovery` or a job source. Never name a model or version, never claim to switch models, and never stop a run to ask for a switch.
6. Pick the browser for this session (section 9, "Which browser"). Show the plan, the advice and this check together, then wait for exactly `ready` (`sign in` and its `done` may come first, see below):

   > **Before the run starts.** A run can take up to your session limit (default 4 hours).
   > 1. Keep the laptop on charging. It helps the run stay active.
   > 2. Turn off sleep for this run (how: README, "Before a run").
   > 3. You are signed in to <sites> in the AI app's own browser. NorthList Apply recommends this browser, and it does not share sign-ins with your usual browser. Not signed in there yet? Reply `sign in` and I will open each site for you. If a site needs you to sign in during the run, I will pause and ask you in that browser.
   >
   > Reply `ready` when you are set.

   `<sites>` names today's **new** job sites that need the user's account: LinkedIn, Indeed, Naukri, Wellfound or Instahyre in today's plan that no record in `applied/` (today's included) has in its `## Application Plan` table yet. Find them with one search of those tables. Workday and company portals are never named: their accounts are made per company during the run (section 10). With no new site, item 3 says: "This run uses the AI app's own browser, so there is nothing for you to open. If a site needs you to sign in during the run, I will pause and ask you in that browser." With the extension backup, item 3 says instead: "This app has no built-in browser, so this run uses your browser through the AI app's browser extension. Your browser is open with one window and the extension is connected. You are signed in there to <sites>; not signed in yet? Reply `sign in` and I will open each site for you. If a site needs you to sign in during the run, I will pause and ask you in that browser." With no new site, leave out its sentence that starts "You are signed in there".

   **`sign in`.** The sites are the ones item 3 names; when it names none, the user may still ask, and then they are all of today's sites that need the user's account. First list the open tabs (section 9, rule 1): use the one open window, and reuse a tab already on one of these sites. Open the home page of each one that has no tab yet, one tab per site, and say: "Please sign in in each tab, then reply `done`." Never type these sites' passwords yourself, and never ask for them in the chat. After `done`, check every one of these tabs and say which still show a sign-in page, or that all are signed in; then show the check again. If the user replies `ready` while a site is still signed out, start anyway: that site asks for its sign-in when it is reached (section 10). If the plan changes, close the tabs of sites no longer in it. Keep the other tabs: after `ready` each one becomes its plan item's anchor tab and opens that site's search (section 9). This is the only browser use before `ready`.

   Show the check at every `start`, also in a new chat that resumes the day. If the reply is not `ready`, answer it, then show the check again. If the reply changes the plan, go back to step 3 with the new plan; it goes into today's record after `ready`.
7. After `ready`:
   - create today's record if needed (section 8) and add `- Started HH:MM:SS — limit N h` under `## Sessions`;
   - **NorthList check.** Look for the tools `record_application_submission` and `record_not_submitted_job` by these names, whatever the server is called in the AI app. A tool the app lists by name but loads only on request (for example through a tool search) counts as there: load it now. **Sync is on** when they are there (the user connected NorthList), or when `profile.md` has `NorthList server:` (or the older `Career Companion server:`) with an address. With sync on, read `instructions/northlist_sync.md` and follow it for the whole session. With neither, sync is off: no tool calls, no messages, no sync lines. If the user asks to stop sending to NorthList, nothing more is sent this session (`instructions/northlist_sync.md`, rule 5);
   - open the browser if it is not open yet (section 9), and start with the first plan item.

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
- Work through the plan items in order.
- Leave at least 30 seconds between two submit actions on the same site (Naukri's one-click **Apply** counts). If you reach a submit sooner, wait out the rest. Waiting never makes a job `skipped`.

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
- Never record `skipped` because of the limit.
- If a new chat is needed for any reason, everything is already in the files: tell the user to type `start` there.
- **Long chat.** The AI app does not show you how full this chat is, so watch for these signs. None of these lines pauses the run. The first two are said at most once per chat, the third once each time the app shortens the chat; after a shortening, a line the summary does not show as said counts as not said:
  - `start` in a chat that already had a `ready` earlier (in its own messages, or in the app's summary of this same chat): put this line above the plan question (section 4, step 1): "This chat already has an earlier run. A new chat works best: open one and type `start`, and today's run continues from your record. Or answer below to go on here." A new chat the user opened never counts, even when it starts with notes from an earlier chat.
  - 20 entries of any status (`applied`, `skipped`, `needs_user`) written in this chat, all runs together: when another job follows, say between the two jobs "This chat is getting long. After this run, open a new chat and type `start` next time."
  - The app has shortened this chat into a summary: before the next job, read sections 5 and 7 of this file again and the current plan item's file (section 11); with sync on, also `instructions/northlist_sync.md`. Then say "This chat got long and your app shortened it. I re-read the rules and will go on. For the rest of today, a new chat works best: open one and type `start`." After this line, the 20-entries line is not said.

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
   - upload `personal_data/resume.pdf` yourself, never asking the user first: use the browser's file-upload tool; if it has none or it fails, attach the file to the page's file field with the app's script tool (read the file, put it on the field, fire the field's change event). If the app asks to allow either one, that is an Allow prompt (section 9, "Hands-free"). Check that the page shows the file. Only when both ways failed twice, ask the user to upload that file in the current tab and reply `done`, then check the upload.
6. **Check before submitting.** Before every submit action (Naukri's one-click **Apply** included), check the form's final values. Never wait for the user's approval of an application: after `ready` the run applies on its own. Still follow every approval the AI app itself asks for.
7. **Submit**, keeping the 30-second gap (section 5).
8. **Applied only after the site shows a confirmation.** Never invent one.
9. **Add the entry** to today's record at once (section 8).
10. **Send it to NorthList** (sync on, `applied` entries): at once, before you open the next job, call `record_application_submission` for this entry, with the portal account it used (Workday and company portals), and add its `- NorthList sync:` line, as `instructions/northlist_sync.md` says. If that file's field table is no longer in this chat, read the file again first. This step is part of every application: never skip it or leave it for later; the user does not have to ask for it. A failed send gets its `failed (...)` line, and applying goes on.
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
- `Submitted answers`: every question and the answer given, one `Question: Answer` line each. An answer that is a password, PIN or security answer is written as `(not recorded)`.
- `Confirmation shown`: the site's own words, for `applied`.
- Leave a field empty when it does not apply. Never write a password into the record. With sync on, each entry also gets a `- NorthList sync:` line: an `applied` entry at once (section 7, step 10), the others at the end of the session.

Records made before this version have per-site sections and more tables. Leave them as they are. If today's record is one of them, add new entries at its end.

## 9. Browser

**Which browser.** Use the AI app's own built-in browser, the one that runs inside the app. A browser extension that drives the user's own browser (for example Claude in Chrome) is only the backup: use it only when the app has no built-in browser. Pick the browser once, before the check in section 4, from the tools the app has: never open a page to pick it, because nothing is opened before `ready` except the `sign in` tabs (section 4). Use it for the whole session, and never switch browsers during a session: sign-ins are not shared between them. A built-in browser that fails is a lost connection (rule 11 below), not a reason to use the extension.

**Hands-free.** The user should not have to do browser work. Before handing a step to the user, try every tool the app has for it. When a tool needs the AI app's permission, use it anyway: the app shows the user an Allow prompt, and you say in one line: "Please allow it, and choose always allow (or allow for this session) if your app offers it, so I don't ask again." Hand a step to the user only for a job-site sign-in (their password), a CAPTCHA or security check (a site stop, section 10), or a step whose tools failed twice.

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
    > 1. Does the AI app's own browser still work? (Extension backup: is your browser open with exactly one window?)
    > 2. Extension backup only: is the browser extension enabled and connected?
    > 3. Did the computer sleep or the lid close? Wake it and keep it awake.
    > 4. Is the computer plugged into power?
    >
    > Nothing was recorded as `applied` without a confirmation shown by the site.

    After `ready`, retry once. If it fails again, do the end of session (section 12) and stop.

## 10. Sign-ins, accounts and stops

- **Signed out, or a normal login page:** ask the user to sign in in the current tab and reply `done`. Check that the sign-in worked, then continue the same job. This is not a site stop.
- **Company portal accounts** (Workday and other company systems): check `tracking/created_accounts.csv` first. If the company has a record, sign in with it; never create a second account. If not, create one with `personal_data/credentials.md` and add its record at once.
- **Email check** (a company portal sends a verification link or code): open the user's webmail in a new tab of the same browser. If it is signed in, search only for that company's message from the last hour, use its link or code, close the tab, and set the record's `email_verified` to `yes`. Read no other mail. If webmail is signed out, or no message comes within 3 minutes, use the message below.
- If the app blocks a sign-in, an account creation or an email check, say exactly: "Please sign in / create the account / click the email verification link in this tab, then reply done". Wait for `done`. When the user made a company portal account this way, add its record at once: `portal_url` the portal's sign-in page (else the page where the account was made), the login email the page shows (else the profile email), the `password` column empty and `email_verified` `no`; never ask for that password. After an email check is done, set that record's `email_verified` to `yes`.
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
5. With sync off, add this line: "Track every application your AI sends, and the replies: connect NorthList at app.northlist.site/automation?tab=setup."
6. Put this line last, in the same message as the open questions, and never wait for their answers to say it: "Next time, open a new chat and type `start`."

Checks for whoever edits these instructions are in `docs/walkthroughs.md`. They are not part of a run.
