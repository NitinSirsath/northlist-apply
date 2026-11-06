# NorthList sync

Read this file when sync is on (`AGENTS.md` section 4, step 7): the AI app has the NorthList tools `record_application_submission` and `record_not_submitted_job`, or `personal_data/profile.md` has `NorthList server:` with an address. The older line `Career Companion server:` works the same. With neither, there is no sync at all: no tool calls, no messages, no sync lines.

NorthList is a separate app that tracks job applications. With sync on, each outcome is recorded there through two MCP tools, so a stopped session has already sent everything applied so far. The daily record stays the source of truth: sync never blocks, delays or changes applying.

## Rules that always hold

1. **Connected means on.** The user turns sync on by connecting NorthList in their AI app; the profile line only holds the server address. When the two tools are there, send.
2. **Every application is sent at once.** Right after an `applied` entry is added, and before the next job is opened, it is sent (`AGENTS.md` section 7, step 10). Never skip a send or leave it for later; the user does not have to ask for it.
3. **Every entry ends with a sync line:** `sent`, `already recorded`, `failed (<reason>)`, or `not sent (stopped by the user)` (rule 5). An `applied` entry without one is a missed send: send it at the next chance, before the next job, right after `ready`, or at the end of the session.
4. **Keep this file at hand.** In a long run, or in a new chat, if the field table below is no longer in the chat, read this file again before the next send.
5. **The user decides.** If the user asks to stop sending to NorthList, send nothing more this session. Every entry of this session that has no sync line yet, and every later one, gets `- NorthList sync: not sent (stopped by the user)`; only a replay the user asks for sends them. Say once that removing NorthList from the AI app's settings turns sync off for good.

## At start, right after `ready`

1. **Wake the server.** When `profile.md` has a server address, request `<server>/health` once: the server may sleep when idle and take 30 to 60 seconds to wake. Use a web-fetch or terminal tool, for example `curl -s -m 90 <server>/health`, without waiting for the answer when your tool allows that. Never open it in a browser tab. With no address, or no such tool, skip this step: a first send that fails while the server wakes is retried (below).
2. **Check the tools.** Look for `record_application_submission` and `record_not_submitted_job` by these names, whatever the server is called in the AI app's settings. A tool the app lists by name but loads only on request counts as there: load it now.
   - If they are there, tell the user once, in one line: "NorthList is connected: each application goes to NorthList right after the site confirms it. If your app asks to allow a NorthList tool, allow it for good."
   - If they are missing, or a listed tool cannot be loaded, tell the user once: "NorthList sync is on, but its tools are not available. If you just connected NorthList, open a new chat or restart the app. An expired token makes the tools disappear without an error: create a new setup prompt in NorthList (Automation › Setup) and paste it here." Then keep applying; every send this session is marked `failed (tools not available)`.
3. **Send what was missed.** Every `applied` entry of today's record that has no sync line is sent now, before the first job, as below.

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
| `discoverySource` | `Discovery source`, for example `we_work_remotely`. |
| `location` | `Location`. |
| `workMode` | `remote`, `hybrid` or `onsite`; leave it out for anything else. |
| `confirmationText` | `Confirmation shown`. |
| `resumeFilename` | `Resume filename`: the file name only. |
| `submittedAnswers` | Every `Question: Answer` line under `Submitted answers`, as `{ "question": ..., "answer": ... }`, in order. |

Never send a password, a credential, the resume file or its content, or plan, limit, pacing, tab or session details.

Then add the sync line to that entry:
- `- NorthList sync: sent` for the results `created`, `linked`, `needs_review` or `recorded`;
- `- NorthList sync: already recorded` for `already_recorded`;
- `- NorthList sync: failed (<reason>)` for an error or no answer, for example `failed (unavailable)`, `failed (invalid_input: platform)` or `failed (rate_limited)`.

Entries written before the rename say `- Career Companion sync:`. Read them the same way.

**One retry during the run.** When a send fails with `unavailable`, a timeout or a connection error, send that entry once more together with the first later send that comes at least 60 seconds after the failure, and update its line. Never wait or slow applying for it. Never retry `invalid_input` yourself: tell the user at the end of the session which entry and field were refused, so they can fix the entry and ask for a replay. Never retry `rate_limited` the same day.

## At the end of a session

Also when the session limit ends it (`AGENTS.md` section 6):

1. Send every `applied` entry of today's record that still has no sync line, as above.
2. For each `skipped` or `needs_user` entry in today's record that has a company and a job title and no sync line (including entries left by an earlier session that stopped early), call `record_not_submitted_job` once with: `sourceRecordRef` (as above), `outcome` (`skipped` or `needs_user`), `platform` (as above; use `discovery` when the entry names a job source), `company`, `jobTitle`, `loggedAt` (`Local date/time` as ISO 8601 with the offset), `reason` (`Skip reason / unanswered question / next action`), and `jobUrl` when known. Add the sync line as above. These jobs never become applications in NorthList.
3. Resend every entry of today's record whose sync line is `failed (...)`, except `invalid_input` and `rate_limited`, and update the line. Entries marked `sent` or `already recorded` are never sent again.
4. In the summary (`AGENTS.md` section 12), say how many of today's applications NorthList has, for example "NorthList: 5 of today's 5 applications sent." (`sent` and `already recorded` both count). Name each entry that failed or is still unsent, and say that `sync NorthList for YYYY-MM-DD` sends them later.

## Replay on request

When the user says `sync NorthList for YYYY-MM-DD` (or `sync Career Companion for YYYY-MM-DD`), read that day's record and send every entry whose sync line is missing, `failed` or `not sent`, with the matching tool, then update the lines. Repeats are safe: the same `sourceRecordRef` is recorded once, and a repeat only fills details NorthList does not have yet.
