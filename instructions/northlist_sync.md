# NorthList sync

Read this file only when `personal_data/profile.md` has `NorthList server:` with an address. The older line `Career Companion server:` works the same. With `None` there is no sync at all: no tool calls, no messages, no sync lines.

NorthList is a separate app that tracks job applications. With sync on, each outcome is recorded there through two MCP tools, so a stopped session has already sent everything applied so far. The daily record stays the source of truth: sync never blocks, delays or changes applying.

## At start, right after `ready`

1. **Wake the server.** It may sleep when idle and take 30 to 60 seconds to wake. Request `<server>/health` once with a web-fetch or terminal tool, for example `curl -s -m 90 <server>/health`, without waiting for the answer when your tool allows that. Never open it in a browser tab. If no such tool exists, skip this step.
2. **Check the tools.** Look for `record_application_submission` and `record_not_submitted_job` by these names, whatever the server is called in the AI app's settings. If they are missing, tell the user once: "NorthList sync is on, but its tools are not available. Check the MCP server in your AI app's settings. An expired token makes the tools disappear without an error." Then keep applying; every send this session is marked `failed (tools not available)`.

## After each `applied` entry

Right after the entry is added (only after the site showed a confirmation), call `record_application_submission` once, with values copied from the entry. Leave out every field that is empty or `None`.

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

**One retry during the run.** When a send fails with `unavailable`, a timeout or a connection error, send that entry once more together with the next send, if that is at least 60 seconds later, and update its line. Never wait or slow applying for it. Never retry `invalid_input` yourself: tell the user at the end of the session which entry and field were refused, so they can fix the entry and ask for a replay. Never retry `rate_limited` the same day.

## At the end of a session

Also when the session limit ends it (`AGENTS.md` section 6):

1. For each `skipped` or `needs_user` entry in today's record that has a company and a job title and no sync line (including entries left by an earlier session that stopped early), call `record_not_submitted_job` once with: `sourceRecordRef` (as above), `outcome` (`skipped` or `needs_user`), `platform` (as above; use `discovery` when the entry names a job source), `company`, `jobTitle`, `loggedAt` (`Local date/time` as ISO 8601 with the offset), `reason` (`Skip reason / unanswered question / next action`), and `jobUrl` when known. Add the sync line as above. These jobs never become applications in NorthList.
2. Resend every entry of today's record whose sync line is `failed (...)`, except `invalid_input` and `rate_limited`, and update the line. Entries marked `sent` or `already recorded` are never sent again.

## Replay on request

When the user says `sync NorthList for YYYY-MM-DD` (or `sync Career Companion for YYYY-MM-DD`), read that day's record and send every entry whose sync line is missing or `failed`, with the matching tool, then update the lines. Repeats are safe: the same `sourceRecordRef` is recorded once, and a repeat only fills details NorthList does not have yet.
