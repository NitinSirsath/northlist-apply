# NorthList Apply

Your own AI app applies to jobs for you, one at a time, with your OK first.

NorthList Apply is a set of written instructions, not a program. Open this folder in an AI app that can use your browser, such as Codex, Claude Code or Antigravity, and type `start`. It finds jobs that fit on the sites you choose, fills in the forms from your profile, saved answers and resume, and keeps a daily record in this folder. Connect [NorthList](https://northlist.site/automation) if you like, and every application sits next to the mails that follow.

In beta. Tried so far on LinkedIn Easy Apply, Naukri and Indeed. Like any AI, it can make mistakes, so watch its first runs.

## What you need

- An AI app that can read files and use your browser, for example Codex, or Claude Code with its browser extension.
- Your resume as a PDF.
- Your accounts on the job sites you want, signed in in that browser.

## Get started

1. Download this repository (**Code › Download ZIP**, then unzip it) or clone it.
2. Open the folder in your AI app.
3. Type `start`.

The first time, it asks for your resume and fills in your profile from it. It shows you what it filled in, then asks only for what is still missing, such as your notice period, salary, work authorization and the jobs you want. Your profile, saved answers, portal login and resume stay in `personal_data/` on your computer, and Git ignores them, so they are never committed.

## A normal run

1. **`start`**: it checks your setup and shows today's plan, for example "Same as last time: linkedin 10, naukri 10", with a short check (see "Before a run"). Tell it if you want a different plan.
2. **`ready`**: once the check is done. It opens the browser and starts.
3. **`ok`**: it shows you the first application of the run, with every answer, and sends it only after your OK. Then it goes on, one job at a time.

When a site asks you to sign in, it waits for you: sign in in that tab and reply `done`. At the end, it sums up the run and asks you the questions it could not answer, and saves your answers for next time.

## Before a run

A run can take up to your session limit (4 hours by default). If the computer sleeps, the browser disconnects and the run stops. Jobs already in today's record are safe.

1. Plug the computer in.
2. Turn sleep off for the run:
   - **macOS:** System Settings › Lock Screen › "Turn display off when inactive" › Never. Also System Settings › Battery › Options (or Displays › Advanced) › "Prevent automatic sleeping on power adapter when the display is off" › On.
   - **Windows 11:** Settings › System › Power & battery › Screen and sleep: set "When plugged in, put my device to sleep after" and "When plugged in, turn off my screen after" to Never.
3. Keep the lid open.
4. Open the browser your AI app controls, with one window.
5. Don't use the computer for other work during the run.

## Sites and limits

- **Tried:** LinkedIn (Easy Apply), Naukri, Indeed.
- **Written up, not tested yet:** Wellfound, Instahyre, Workday, company career pages (`company_direct`), web search (`discovery`), and the job sources The Reliable Jobs, TEKsystems, Teksands, D4hire, Supersourcing and We Work Remotely. Job sources only find jobs; each job is applied to on the site where its form is.

Most applications per day: LinkedIn 20, Indeed 20, Naukri 25, Wellfound 10, Instahyre 10, Workday 5, company career pages and web search together 10, all sites together 50. Your plan's numbers, and an optional lower limit in your profile, can only make these smaller.

## What it always does

- Applies to one job at a time, with at least 2 minutes between two applications on the same site.
- Applies only to jobs that match your titles, places and work modes, and skips the companies you list.
- Fills in only facts from your profile, saved answers and resume. When a question has no saved answer, it asks you instead of guessing.
- Shows you the first application of each run and waits for your `ok`.
- Counts a job as applied only after the site confirms it.
- Stops a site for the day at a CAPTCHA, a security check or a limit message, and never tries to get around it.
- Never emails recruiters, and never turns on a site's own auto-apply.
- Keeps one record per day in `applied/YYYY-MM-DD/applications.md`, and never applies to the same job twice.

## Which model

Once per run, it tells you which kind of model fits today's plan; it never names one. A light model is enough for the quick-apply sites (LinkedIn, Indeed, Naukri, Wellfound, Instahyre). Workday, company career pages, web search and the job sources go better with a medium one. For the first setup, use your app's strongest model.

## Connect NorthList (optional)

[NorthList](https://northlist.site/automation) tracks your job applications. Connected, each job this automation applies to shows up there with its links and the answers it sent, and mail from the company moves it on. Without it nothing changes: the daily record in this folder is enough.

**Set up once:**
1. In NorthList, open **Automation › Setup** and create a token. Copy it: it is shown once.
2. In your AI app's **user-level** MCP settings (never a file inside this folder), add NorthList as a server with the server address from the Setup tab (it ends in `/mcp`) and the header `Authorization: Bearer <token>`. If your app cannot send headers, use the `mcp-remote` bridge with that same address: `npx mcp-remote <server address> --header "Authorization: Bearer <token>"`.
3. Allow its two tools: `record_application_submission` and `record_not_submitted_job`.
4. In `personal_data/profile.md`, set the profile line the Setup tab shows, `- NorthList server: <address>` (without `/mcp`). An older `- Career Companion server: <address>` line works the same.

**What NorthList gets:**
- For each job it applied to: the company, job title, site, job and apply page links, the apply site's address, the job ID, where the job was found, location, work mode, time, the site's confirmation, your resume's file name and the answers it sent.
- For each job it opened but did not apply to: the company, job title, site, job link, whether it skipped the job or left it for you, the reason and the time.
- Never: your passwords, your resume file or its text, or your plan and limits.

Each application is sent right after it is recorded, so a stopped run keeps what was sent. Jobs it did not apply to are sent at the end of the run, and failed sends are tried again then. To send a day again later, say `sync NorthList for YYYY-MM-DD`. If you delete an application in NorthList, what this automation sent for it goes too. Tokens expire: when it says the tools are not available, create a new token and replace it in your AI app's settings.

## Files

- `AGENTS.md`: the rules for every run (`CLAUDE.md` loads it for Claude Code).
- `instructions/`: setup, NorthList sync, and one file per site, each read only when needed.
- `personal_data/*_template.md`: templates for your profile, saved answers and portal login.
- `docs/walkthroughs.md`: checks for anyone who changes the instructions.

Made while running and ignored by Git: your files in `personal_data/` (profile, saved answers, portal login, resume), `applied/` (one record per day) and `tracking/` (company portal accounts).

## Troubleshooting

**Many browser permission prompts.** Many AI apps ask once per new website, and Workday, company career pages, web search and the job sources open a new company site for almost every job. Allow it for the session if your app offers that, and stay near. For hands-off runs, use the quick-apply sites.

**"Browser connection lost. The run is paused."** The browser closed, the computer slept, or the browser extension lost its connection. Open the browser with one window, check the extension, keep the computer awake and plugged in, then reply `ready`. It tries once more, then ends the run cleanly.

**Many windows or tabs, and a slow computer.** Stop the run, close the extra windows and tabs, keep one window, and type `start`. A run uses at most two more tabs than it has plan items.

**The run stopped after about 4 hours.** That is the session limit. Type `start` in a new chat to continue the same day. To change it, edit `Session time limit (hours)` in `personal_data/profile.md`.

**It keeps asking me to sign in.** The site is signed out. Sign in yourself in that tab, then reply `done`.

**A site stopped for the day after a CAPTCHA or "unusual activity".** That is a site stop. Nothing more on that site today; the other sites go on. Try it again tomorrow.

**Setup asks again, or loops.** A required field in `personal_data/profile.md` or `personal_data/form_answers.md` still has a `[bracketed placeholder]`, or `personal_data/resume.pdf` is missing. Fill it in, or write `None` for an optional field, then type `start`.

**Counts look wrong.** Only the entries in `applied/YYYY-MM-DD/applications.md` count, and there is never a second file for the same day. The table at the top of that file is brought up to date at the start and the end of each session, not after every job.

**The run is slow.** It applies to one job at a time, reads each job, waits for your first `ok`, and leaves 2 minutes between applications on one site. Choose 3 to 5 sites, so the waits overlap, and 10 to 15 jobs per site.

**"NorthList sync is on, but its tools are not available."** The token has probably expired. Create a new one under Automation › Setup in NorthList and replace it in your AI app's settings.
