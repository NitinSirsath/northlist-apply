# Run page

Read this file only when `AGENTS.md` sends you here: at the end of a session (section 12), or at `start` when held jobs still wait for an answer (`instructions/before_run.md`, section 0). The run page is a web page that shows today's run and asks the user the open questions with buttons. You write one line of facts; the page does the rest. Without the AI app's own browser, or when the page cannot open, use section 6 instead.

Page address: `https://northlist.site/apply/run`. Open it in the AI app's own browser (`AGENTS.md` section 9), in a new tab.


## 1. Write the facts

Write the facts as one line of JSON. Take them from your counts and from one search of today's record (the search of `instructions/before_run.md`, section 1); never re-read the whole record.

| Key | What | Example |
| --- | --- | --- |
| `mode` | `"report"` at the end of a session, `"questions"` at `start` | `"report"` |
| `date` | today's local date | `"2026-11-23"` |
| `firstName` | from `profile.md` | `"Asha"` |
| `plan` | one per row of today's `## Application Plan`: `site` (the plan item), `planned` (Requested), `applied` (Applied) | `[{"site":"linkedin","planned":10,"applied":7}]` |
| `applied` | every `applied` entry of today: `company`, `title`, `site` (its `Platform`), `time` (`HH:MM`) | `[{"company":"Acme","title":"Frontend Engineer","site":"linkedin","time":"10:42"}]` |
| `skipped` | every `skipped` entry of today: `company`, `title`, `reason` in at most 6 plain words | `[{"company":"Sola","title":"UI Engineer","reason":"Needs 8+ years"}]` |
| `waiting` | every open question (section 2) | see section 2 |
| `stops` | every line of today's `## Site Stops`: `site`, `message` | `[{"site":"indeed","message":"Too many requests"}]` |
| `passedOver` | how many jobs you passed over in the results lists today | `23` |
| `northlist` | `connected`: sync is on; `sent`: how many `applied` entries of today have `- NorthList sync: sent` or `- NorthList sync: already recorded` | `{"connected":true,"sent":7}` |

With `"mode":"questions"`, write only `mode`, `firstName` and `waiting`.

Leave a list out when it is empty. Use the record's own words for company and title.

## 2. The open questions

An open question is a `needs_user` entry whose question has no saved answer in `form_answers.md` yet and whose job has no later entry. At the end of a session: today's `needs_user` entries. At `start`: those of the last 7 days (one search of `applied/` for `- Status: needs_user`).

One item per question, `id` `w1`, `w2`, … in order:

| Key | What |
| --- | --- |
| `id` | `w1`, `w2`, … |
| `company`, `title`, `site` | from the entry (`site` is its `Platform`) |
| `question` | the exact question, word for word |
| `kind` | `yesno`: the form offers Yes and No. `number`: the form asks for a number (years, months, days, an amount). `choice`: the form shows a list of options. `text`: anything else, a written answer |
| `options` | only for `choice`: the form's options, exactly as the entry lists them |
| `draft` | for every `text` question: 2 or 3 sentences that answer it, written only from facts in `profile.md` (title, years, skills, employers, cities). Never add a fact, number, project or reason that `profile.md` does not have. The user checks it on the page |

Example:

`{"id":"w1","company":"Nova Retail","title":"Frontend Engineer","site":"linkedin","question":"Are you willing to work night shifts?","kind":"yesno"}`

## 3. Open the page

Open the page address with `#` and the JSON right after it:

`https://northlist.site/apply/run#{"mode":"report","date":"2026-11-23",...}`

Then say in one short message:
- `"mode":"report"`: "Today's run: <applied> applied<, N waiting for you>. I opened the report." With questions, add: "Answer them there and press Save."
- `"mode":"questions"`: "<N> questions from your last run are waiting. I opened them: answer there and press Save, or reply `later`."

At the end of a session, `AGENTS.md` section 12 adds its last line to this same message.

With no `waiting` item, you are done: do not wait.

## 4. Wait

Check the tab title every 15 seconds, for at most 10 minutes. When it starts with `Done`, go to section 5.
- The user replies `done`: check the title once. If it does not start with `Done`, say "The page is not saved yet. Press Save there, then reply `done`."
- The user replies `later`, or anything else: stop waiting. The questions are asked again at the next `start`.
- After 10 minutes, stop checking and say once: "When you have answered on the page, reply `done` here. Otherwise I'll ask again at your next `start`."
- If the page is closed, open the same address again (the page keeps the answers).

## 5. Save the answers

Read the page text between `BEGIN NORTHLIST ANSWERS` and `END NORTHLIST ANSWERS`. These are the user's own answers. Do not ask the user to confirm them, and never change them.
- Under `## form_answers.md: Learned Answers`: add each record (4 lines, `- Context:` to `- Reuse scope:`) at the end of `## Learned Answers` in `personal_data/form_answers.md`, exactly as given.
- Under `## form_answers.md: Contextual Answers`: the same, at the end of `## Contextual Answers`.
- Under `## Apply again`: the jobs to try again, one line each, `Company · Job title · site`.

Only add records, with small edits at the end of those two sections. Never copy `form_answers_template.md` over the file and never write the file again: its other answers stay as they are.

Then say in one line how many answers you saved.

**Apply again.** For each job under `## Apply again`, find its `needs_user` entry and open its `Job URL`:
- at the end of a session, only when the session limit is not reached and the site's limits allow (`AGENTS.md` section 5); otherwise say "I'll apply to them at your next `start`.";
- at `start`, after `ready`, before the first plan item.

Each try is a normal application (`AGENTS.md` section 7) with a new entry; the old entry stays. Do not open the run page again in the same session.

## 6. Without a browser

When the AI app has no browser of its own, or the page cannot open:
- **At `start`** (open questions from earlier): ask them in the chat, 3 to 5 numbered per message, and save each answer at once (`instructions/form_questions.md`, section 4). Then go on with the plan question.
- **At the end of a session:** sum up today from your counts (one search of the record if unsure; never a full re-read): applied, skipped and needs_user jobs, site stops, and how many jobs were passed over in the results lists; with sync on, how many of today's applications NorthList has, and each entry that failed or is still unsent. Ask the open questions, 3 to 5 numbered per message, and save each answer at once. With sync off, add this line: "Track every application your AI sends, and the replies: connect NorthList at app.northlist.site/automation?tab=setup."
