# Plan page

Read this file only at `start`, for the plan question (`instructions/before_run.md`, section 1), when the AI app has its own browser. The plan page is a web page where the user picks today's sites and numbers with buttons. You write one line of facts; the page does the rest. When the page cannot open, ask the plan question in the chat instead (`instructions/before_run.md`, section 1).

Page address: `https://northlist.site/apply/plan`. Open it in the AI app's own browser (`AGENTS.md` section 9), in a new tab.

## 1. Write the facts

Write the facts as one line of JSON, from `profile.md` and the start search (`instructions/before_run.md`, section 1); never re-read the whole record. Leave a key out when it has nothing.

| Key | What | Example |
| --- | --- | --- |
| `last` | the last plan, as the chat question would show it after "Last time:" | `"linkedin 10, naukri 2"` |
| `today` | only when today's record exists: applied so far per plan item | `"linkedin 4"` |
| `limits` | only the lower daily limits in `profile.md` | `"linkedin 15"` |
| `careerPages` | `false` when `profile.md` has no career page URLs | `false` |
| `hours` | the session limit in hours (`AGENTS.md` section 6) | `4` |

Open the page address with `#` and the JSON right after it:

`https://northlist.site/apply/plan#{"last":"linkedin 10, naukri 2","hours":4}`

Then say in one short message, with the long-chat line of `instructions/before_run.md` section 1 above it when it applies: "I opened today's plan. Pick the sites and numbers there and press Start. Or reply `same`, or write a plan here, like `linkedin 10, workday 2`."

## 2. Wait

Check the tab title every 15 seconds, for at most 10 minutes. When it starts with `Done`, go to section 3.
- The user replies `same` or a plan: stop waiting. That reply is the answer (`instructions/before_run.md`, section 2).
- The user replies `done`: check the title once. If it does not start with `Done`, say "The plan is not started yet. Press Start there, then reply `done`."
- Anything else: answer it, and keep waiting.
- After 10 minutes, stop checking and say once: "When you have picked the plan, reply `done` here, or write it here, like `linkedin 10`."
- If the page is closed, open the same address again (the page keeps the picks).

## 3. Read the plan

Read the page text between `BEGIN NORTHLIST PLAN` and `END NORTHLIST PLAN`: one line, like `linkedin 10, workday 2`. That is the user's answer to the plan question, as if written in the chat: go on with `instructions/before_run.md`, section 2. Do not ask the user to confirm it, and never change it. The page keeps every number inside the limits.
