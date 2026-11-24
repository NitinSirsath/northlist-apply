# Workday

Adds only Workday's steps to `AGENTS.md`; if the two ever conflict, `AGENTS.md` wins. Any application on a Workday form counts as `workday`, wherever the job was found.

## Find jobs
Workday has no single job board: each company has its own Workday site. As a plan item, `workday` finds jobs with web searches.
1. Use the AI app's own web search tool when it has one (faster than a browser tab); otherwise a search engine in the browser. A search engine's CAPTCHA is not a site stop: use another search engine once.
2. Search `site:myworkdayjobs.com` with each target job title from `personal_data/profile.md`, then 2 or 3 close titles from the profile's skills (for "Full Stack Developer": "Software Engineer", "React Developer", "Node.js Developer"), each with the user's city or country. Example: `site:myworkdayjobs.com "React Developer" Bengaluru`. Use the search engine's past-week filter when it has one.
3. Take jobs posted in the last 7 days, by the Workday page's own line ("Posted Today", "Posted 3 Days Ago"). Search engines show new Workday jobs a few days late, so there is no 24-hour filter here.
4. Make at least 5 different searches before saying no job fits, and tell the user how many you made.

A job arriving from discovery, a job source or a company page: continue that job's form. Never restart a Workday search for it.

## Apply
A Workday job takes about 5 to 10 minutes; the first one at a company takes longest (account and email check).
1. The fit, duplicate and limit checks pass before any sign-in or account creation.
2. Account: follow `instructions/company_accounts.md` (sign in with a saved account, or create one with `personal_data/credentials.md` and record it at once).
3. Choose **Use My Last Application** when Workday offers it (a second job at this company), else **Autofill with Resume**: they fill most pages for you. Upload `personal_data/resume.pdf` yourself (`instructions/form_questions.md`, section 6): never ask the user to upload it first.
4. Compare what Workday filled in with `profile.md`, and fix material differences in this form. Do not get stuck on harmless old-experience details; fix the required facts and go on.
5. Go through the steps the site shows. Labels vary, so never assume a fixed order.
6. Answer questions from `form_answers.md` only when the answer and its context match.
7. Check the final values, follow any extra Workday approval, submit, and wait for Workday's confirmation.

For an email check after a new account, follow `instructions/company_accounts.md`, section 2.
