# Workday

Adds only Workday's steps to `AGENTS.md`; if the two ever conflict, `AGENTS.md` wins. Any application on a Workday form counts as `workday`, wherever the job was found.

## Start
- `workday` as a plan item: search with the target job titles, locations and work modes from `personal_data/profile.md`, and Workday's freshness controls when it has them (`AGENTS.md` section 9).
- A job arriving from discovery, a job source or a company page: continue that job's form. Never restart a Workday search for it.

## Apply
1. The fit, duplicate and limit checks pass before any sign-in or account creation.
2. Account: check `tracking/created_accounts.csv`. If this company has a record, use **Sign In**. If not, create an account with `personal_data/credentials.md` and add its record at once.
3. Upload `personal_data/resume.pdf` yourself (`AGENTS.md` section 7): never ask the user to upload it first.
4. Compare what Workday read from the resume with `profile.md`, and fix material differences from it. Do not get stuck on harmless old-experience details; fix the required facts and go on.
5. Go through the steps the site shows. Labels vary, so never assume a fixed order.
6. Answer questions from `form_answers.md` only when the answer and its context match.
7. Check the final values, follow any extra Workday approval, submit, and wait for Workday's confirmation.

For an email check, follow `AGENTS.md` section 10 (webmail first); only if it cannot be finished, use its message and wait for `done`.
