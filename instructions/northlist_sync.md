# NorthList sync

Read this file when sync is on (`AGENTS.md` section 4). NorthList is a separate app that tracks the user's job applications. Each outcome is recorded there through two MCP tools, `record_application_submission` and `record_not_submitted_job`, so a stopped session has already sent everything applied so far. The daily record stays the source of truth: sync never blocks, delays or changes applying.

## Rules that always hold

1. **Every application is sent at once.** Right after an `applied` entry is added, and before the next job is opened, it is sent (`AGENTS.md` section 7, step 10). Never skip a send or leave it for later; the user does not have to ask for it.
2. **Jobs not applied to: only the ones the run tried.** A `needs_user` entry, and a `skipped` entry of a job the run started to apply to (a sign-in, an upload, a form or a site stop ended it; its reason says which), are sent at the end of the session, with that reason. A `skipped` entry whose reason starts with `Not a fit:` is never sent, also not in a replay, and gets no sync line.
3. **Each entry that is sent gets a sync line:** `sent`, `already recorded`, `failed (<reason>)`, or `not sent (stopped by the user)` (rule 5). An `applied` entry without one is a missed send: send it at the next chance, before the next job, right after `ready`, or at the end of the session.
4. **Keep this file at hand.** In a long run, or in a new chat, if the field table below is no longer in the chat, read this file again before the next send.
5. **The user decides.** If the user asks to stop sending to NorthList, send nothing more this session. Every entry of this session that would be sent and has no sync line yet, and every later one, gets `- NorthList sync: not sent (stopped by the user)`; only a replay the user asks for sends them. Say once that removing NorthList from the AI app's settings turns sync off for good.

## At start, right after `ready`

1. **Wake the server.** When `profile.md` has a server address, request `<server>/health` once: the server may sleep when idle and take 30 to 60 seconds to wake. Use a web-fetch or terminal tool, for example `curl -s -m 90 <server>/health`, without waiting for the answer when your tool allows that. Never open it in a browser tab. With no address, or no such tool, skip this step: a first send that fails while the server wakes is retried (below).
2. **Check the tools.** Look for `record_application_submission` and `record_not_submitted_job` by these names, whatever the server is called in the AI app's settings. A tool the app lists by name but loads only on request counts as there: load it now.
   - If they are there, tell the user once, in one line: "NorthList is connected: each application goes to NorthList right after the site confirms it. If your app asks to allow a NorthList tool, allow it for good."
   - If they are missing, or a listed tool cannot be loaded, tell the user once: "NorthList sync is on, but its tools are not available. If you just connected NorthList, open a new chat or restart the app. An expired token makes the tools disappear without an error: create a new setup prompt in NorthList (Automation › Setup) and paste it here." Then keep applying; every send this session is marked `failed (tools not available)`.
3. **Send what was missed.** Every `applied` entry of today's record that has no sync line is sent now, before the first job, as below. The start search (`instructions/before_run.md`, section 1) shows them, for example after a chat that stopped between an entry and its send.

## After each `applied` entry

Right after the entry is added (only after the site showed a confirmation), and before the next job is opened, call `record_application_submission` once, with values copied from the entry. Leave out every field that is empty or `None`.

| Tool field | Value from the entry |
| --- | --- |
| `sourceRecordRef` | `YYYY-MM-DD/HH:MM:SS`: the date folder of the daily record, `/`, and the time in the entry heading. Copy it exactly; never invent or reformat it. |
| `platform` | One of `linkedin`, `indeed`, `naukri`, `wellfound`, `instahyre`, `workday`, `company_direct`, `discovery`: the workflow actually used. Workday wins whenever a Workday form was used. Never a job-source name. |
| `company`, `jobTitle` | `Company`, `Job title`. |
| `submittedAt` | `Local date/time` as ISO 8601 with the local UTC offset, for example `2026-10-20T14:14:05+05:30` (offset from `date +%z`). |
| `jobUrl` | `Job URL`. |
| `applyUrl` | `Apply page URL` (the full address). |
| `destinationHost` | The host name of `Apply page URL` (or of `Actual application destination`), for example `jobs.lever.co`. |
| `portalJobId` | `Job ID`. |
| `discoverySource` | `Discovery source`, for example `the_reliable_jobs`. |
| `location` | `Location`. |
| `workMode` | `remote`, `hybrid` or `onsite`; leave it out for anything else. |
| `confirmationText` | `Confirmation shown`. |
| `resumeFilename` | `Resume filename`: the file name only. |
| `submittedAnswers` | Every `Question: Answer` line under `Submitted answers`, as `{ "question": ..., "answer": ... }`, in order. |
| `portalAccount` | Workday and company portals only: the account used for this application, as `instructions/company_accounts.md`, section 3, says. Leave it out when no portal account was used. |
| `task` | The run this entry belongs to, the same on every entry of the session: `{ "startedAt": ..., "plan": [...] }`. `startedAt`: when this session started, the `Started` time of `## Sessions` on the date the session started (after midnight still the earlier date), as ISO 8601 with the local UTC offset, for example `2026-11-24T21:02:10+05:30`. `plan`: the rows of `## Application Plan` in order, each `{ "site": "<plan item>", "planned": <Requested> }`. For an entry of an earlier session (a replay, a missed send), use that session's `Started` time: the last one before the entry's time. |

The only password ever sent is a company-portal account's, inside `portalAccount`. Never send any other password or credential: job-site sign-ins (LinkedIn, Indeed, Naukri, Wellfound, Instahyre) are the user's and never sent. Never send the resume file or its content, or limit, pacing, tab or other session details: `task` is the only plan and session detail sent.

Then add the sync line to that entry:
- `- NorthList sync: sent` for the results `created`, `linked`, `needs_review` or `recorded`;
- `- NorthList sync: already recorded` for `already_recorded`;
- `- NorthList sync: failed (<reason>)` for an error or no answer, for example `failed (unavailable)`, `failed (invalid_input: platform)` or `failed (rate_limited)`.

**One retry during the run.** When a send fails with `unavailable`, a timeout or a connection error, send that entry once more together with the first later send that comes at least 60 seconds after the failure, and update its line. Never wait or slow applying for it. Never retry `invalid_input` yourself: tell the user at the end of the session which entry and field were refused, so they can fix the entry and ask for a replay. Never retry `rate_limited` the same day.

## At the end of a session

Also when the session limit ends it (`AGENTS.md` section 6):

1. Send every `applied` entry of today's record that still has no sync line, as above.
2. For each `needs_user` entry, and each `skipped` entry that rule 2 lets through (never a `Not a fit:` one), in today's record that has a company and a job title and no sync line (including entries left by an earlier session that stopped early), call `record_not_submitted_job` once with: `sourceRecordRef` (as above), `outcome` (`skipped` or `needs_user`), `platform` (as above; use `discovery` when the entry names a job source), `company`, `jobTitle`, `loggedAt` (`Local date/time` as ISO 8601 with the offset), `reason` (`Skip reason / unanswered question / next action`, as written: it tells the user in NorthList why the job was not applied to), `task` (as above), and `jobUrl` when known. Add the sync line as above. These jobs never become applications in NorthList.
3. Resend every entry of today's record whose sync line is `failed (...)`, except `invalid_input` and `rate_limited`, and update the line. Entries marked `sent` or `already recorded` are never sent again.
4. When every application of today is `sent` or `already recorded`, say nothing more: the run page shows the count. Otherwise add one line to the end-of-session message (`AGENTS.md` section 12): "NorthList: 5 of today's 7 applications sent. Not sent: <Company> (<reason>), <Company> (<reason>). `sync NorthList for YYYY-MM-DD` sends them later." For an `invalid_input`, the reason names the refused field.

## Replay on request

When the user says `sync NorthList for YYYY-MM-DD`, read that day's record and send every entry whose sync line is missing, `failed` or `not sent` (never a `Not a fit:` entry, rule 2), with the matching tool, then update the lines. Repeats are safe: the same `sourceRecordRef` is recorded once, and a repeat only fills details NorthList does not have yet.

When the user says `send portal accounts for YYYY-MM-DD`, send that day's `applied` entries that used a portal account again, with `portalAccount` (`instructions/company_accounts.md`, section 3), whatever their sync line says, and leave the lines as they are. This adds the account to applications sent before this version; NorthList keeps each application once.
