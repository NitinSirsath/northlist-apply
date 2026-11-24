# Before a run

Read this file at every `start` (`AGENTS.md` section 3) and follow it from the top: the update and setup checks, held jobs, the plan question, the advice and the check, the `sign in` steps, and the first steps after `ready`. Its last section is the message for a lost browser connection.

## 0. First: the update check, the setup check and held jobs

**Update check.** Read `VERSION` from `https://raw.githubusercontent.com/NitinSirsath/northlist-apply/master/VERSION` (this exact address, never another). If its `version:` is newer than the one in this folder's `VERSION`, read `instructions/update.md` and follow it. If it is the same, or the check fails (offline, any error), say nothing and go on. Never check or update during a run. When this chat has just updated the folder, read the new `instructions/update.md`, section 4, once more and do every step there that is not done yet: the older file may not have had them.

**Setup check.** Then check the real files:
- `personal_data/resume.pdf` exists and is not empty;
- `personal_data/profile.md` and `personal_data/form_answers.md` exist and have no `[bracketed placeholders]` in required fields (empty `## Contextual Answers` and `## Learned Answers` sections are fine);
- every required fact is present, and every optional one that is unknown says `None`.

If anything is missing, read `instructions/setup.md` and finish setup first. Never apply before this check passes.

**Held jobs from earlier.** Then search `applied/` of the last 7 days once for `- Status: needs_user`, and keep only the entries whose job has no later entry. A held job whose exact question is not in `form_answers.md` yet is an open question: read `instructions/run_page.md` and ask them as it says for `start`, then go on with section 1. A held job answered since is applied to after `ready` (section 4). With no held job, say nothing about it.

## 1. The plan question

Ask it first and wait for the answer. Before the answer, never open a browser (except the run page of section 0) and never apply. Ask it in this form; nothing else goes in the message (no check, no advice), except the long-chat line below when it applies:

> **What should I apply to today?** Last time: linkedin 10, naukri 10. Reply `same` to keep it, or write sites and numbers, like `linkedin 10, workday 2`.
> - **Quick apply:** `linkedin` (up to 20), `naukri` (25), `indeed` (20), `wellfound` (10), `instahyre` (10)
> - **Company sites,** an account per company, slower: `workday` (5), `company_direct` (your saved career pages), `discovery` (web search); company_direct and discovery share 10
> - **Job sources,** each job applied to on its company's site: `the_reliable_jobs`, `teksystems`, `teksands`, `d4hire`, `supersourcing`

- The numbers are today's daily maximums (`AGENTS.md` section 5), lowered by a lower limit in `profile.md`.
- With no earlier record, leave out "Last time … to keep it," and say "Write sites and numbers, like `linkedin 10, naukri 10`."
- When today's record exists, first search it once for its `### ` entry headings and its `- Platform:`, `- Selected plan choice:`, `- Status:` and `- NorthList sync:` lines: from that one search, count the entries by status, per plan item and per site, note each site's last entry time and each `applied` entry that has no sync line, and bring the `## Application Plan` counts up to date. Then put today's plan in place of the last plan, applied of requested: "Today so far: linkedin 4 of 10, naukri 0 of 10. Reply `same` to go on, or write sites and numbers."
- Leave out `company_direct` when `profile.md` has no career page URLs; that line then ends with "`discovery` (web search, up to 10)".
- Always show all three lines, every time.
- **Long chat.** When this chat already had a `ready` earlier (in its own messages, or in the app's summary of this same chat), put this line above the question, once per chat: "This chat already has an earlier run. A new chat works best: open one and type `start`, and today's run continues from your record. Or answer below to go on here." A new chat the user opened never counts, even when it starts with notes from an earlier chat.

## 2. The plan, the advice and the check

The answer to the plan question is `same` (keep that plan) or plan items with numbers. Keep the items in the order the user gives:
- sites: `linkedin`, `indeed`, `naukri`, `wellfound`, `instahyre`, `workday`;
- `company_direct` (company career pages; it needs their URLs in `profile.md`) and `discovery` (web search);
- job sources: `the_reliable_jobs`, `teksystems`, `teksands`, `d4hire`, `supersourcing`. `we_work_remotely` is not supported yet: if the user asks for it, say so in one line and leave it out of the plan.

Never plan more than the daily maximums (`AGENTS.md` section 5) or a lower limit in `profile.md`.

Then show the plan, the advice and the check together, in one message:

**Company sites,** only when the plan has `workday`, `company_direct`, `discovery` or a job source:
- say in one sentence that these jobs take longer (about 5 to 10 minutes each, the first one at a company the longest), and that the AI app may ask for browser permission once for each new company site, so the user should stay near the computer;
- if `personal_data/credentials.md` still has a placeholder, ask for the password first, in the words of `instructions/setup.md`, section 4, and save the answer before the check.

**Model advice,** once per session, in one or two sentences: a medium model of the user's current AI app works best for every plan, because it makes fewer mistakes than a light one. When the app also has a thinking level (effort), add that low or medium is much faster than high. Never name a model or version, never claim to switch models, and never stop a run to ask for a switch.

**The check.** First pick the browser for this session from the tools the app has, never by opening a page (`AGENTS.md` section 9, "Which browser"). Then show:

> **Before the run starts.** A run can take up to your session limit (default 4 hours).
> 1. Keep the laptop on charging. It helps the run stay active.
> 2. Turn off sleep for this run (how: README, "Before a run").
> 3. You are signed in to <sites> in the AI app's own browser. It does not share sign-ins with your usual browser. Not signed in there yet? Reply `sign in` and I will open each site for you.
> 4. Stay for the first job or two. When your AI app asks to allow something, choose always allow, so the run does not wait for you later. If a site needs your sign-in during the run, I ask in that browser, wait up to 5 minutes, then go on with the other jobs.
>
> Reply `ready` when you are set.

Wait for exactly `ready`. Before it, the user may reply `sign in` (section 3) and then `done`. Any other reply: answer it, then show the check again; if it changes the plan, do this section again with the new plan.

**Item 3** in other cases:
- Nothing to name: "This run uses the AI app's own browser, so there is nothing for you to open."
- The extension backup (`AGENTS.md` section 9): "This app has no built-in browser, so this run uses your browser through the AI app's browser extension. Your browser is open with one window and the extension is connected. You are signed in there to <sites>. Not signed in yet? Reply `sign in` and I will open each site for you." With nothing to name, leave out its last two sentences.

`<sites>` names today's **new** sites that need the user's sign-in: LinkedIn, Indeed, Naukri, Wellfound or Instahyre in today's plan that no record in `applied/` (today's included) has in its `## Application Plan` table yet. Find them with one search of those tables. Workday and company portals are never named: their accounts are made per company during the run (`AGENTS.md` section 10).

**Email.** When today's plan has `workday`, `company_direct`, `discovery` or a job source, and either no record in `applied/` has any of them in its plan table yet (the same search) or `tracking/created_accounts.csv` has an account whose `email_verified` is `no` (an email check the user had to do), `<sites>` also names the user's email, so the run can open the verification mails that company sites send: "your email (Gmail)" for an address at `gmail.com`, "your email (Outlook)" at `outlook.com`, `hotmail.com` or `live.com`, "your email (Yahoo Mail)" at a `yahoo.` address. For any other address, leave the email out.

## 3. `sign in`

The sites are the ones item 3 names, the email included; when it names none, the user may still ask, and then they are all of today's sites that need the user's sign-in (and the email, when the plan has company sites). First list the open tabs (section 4, step 3): use the one open window, and reuse a tab already on one of these sites. Open the home page of each one that has no tab yet, one tab per site (Gmail `https://mail.google.com`, Outlook `https://outlook.live.com/mail`, Yahoo Mail `https://mail.yahoo.com`), and say: "Please sign in in each tab, then reply `done`." Never type these passwords yourself, and never ask for them in the chat. After `done`, check every one of these tabs and say which still show a sign-in page, or that all are signed in; then show the check again. If the user replies `ready` while a site is still signed out, start anyway: that site asks for its sign-in when it is reached (`AGENTS.md` section 10). If the plan changes, close the tabs of sites no longer in it. Keep the other tabs: after `ready` each job site's tab becomes its plan item's anchor tab and opens that site's search (section 4). Close the email tab after `ready`: the browser keeps that sign-in for the email checks. This is the only browser use before `ready`.

## 4. After `ready`

1. Create today's record if needed (`AGENTS.md` section 8), with the plan the user confirmed, and add `- Started HH:MM:SS — limit N h` under `## Sessions`.
2. With sync on (`AGENTS.md` section 4), read `instructions/northlist_sync.md` and do its start steps.
3. Before anything else in the browser, list the open tabs with the AI app's tab tool. Reuse the open window and never open a second one. If the app can only start a new window, open exactly one and use it for the whole session. In a new chat, reuse the tabs that are open instead of opening new ones.
4. First apply to the held jobs whose answers are saved (section 0), within the limits: open each one's `Job URL` from its entry; each try gets a new entry. Then start with the first plan item (`AGENTS.md` sections 7 and 11).

For the whole run:
- **Tabs.** One browser task per plan item's batch of jobs, never one per job. Keep one anchor tab per plan item, holding its search results; never close an anchor tab or the last tab of the window. A job may open in a new work tab: fill its form there and close the tab after its entry is added; if the job opened in the anchor tab, open the search results address again afterwards. At most (plan items + 2) tabs: before opening one more, close the oldest work tab. Never use the browser's Back button: inside a form use its own Back control, otherwise open the address again.
- **After midnight.** Update the old record's `## Application Plan` counts, start the new date's record with the same plan, copy this session's `Started` line into its `## Sessions`, and use the new date's limits.
- **20 entries in this chat** (any status, all runs together): when another job follows, say once per chat, between the two jobs, "This chat is getting long. After this run, open a new chat and type `start` next time." It never pauses the run, and it is not said once the app has shortened the chat (`AGENTS.md` section 6).

## 5. Browser connection lost

When a browser action, or reading the tab list, fails twice in a row (`AGENTS.md` section 9, "Lost connection"), stop and show:

> **Browser connection lost. The run is paused.**
> Check these. I'll try again every minute for 5 minutes:
> 1. Does the AI app's own browser still work? (Extension backup: is your browser open with exactly one window?)
> 2. Extension backup only: is the browser extension enabled and connected?
> 3. Did the computer sleep or the lid close? Wake it and keep it awake.
> 4. Is the computer plugged into power?
>
> Nothing was recorded as `applied` without a confirmation shown by the site.

Then try again every minute for 5 minutes, without waiting for a reply (a `ready` from the user tries at once). Go on as soon as it works. Still failing after 5 minutes: do the end of session (`AGENTS.md` section 12) and stop.
