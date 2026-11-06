# Setup

Read this file only while the setup check in `AGENTS.md` section 3 fails, or when step 4 of `AGENTS.md` section 4 sends you here. Never apply to jobs during setup.

Before starting, suggest in one sentence the strongest model of the user's current AI app for setup. Never name a model or version, and never claim to switch models.

## 1. Missing files

Create each missing file from its template. Never replace a file that already has answers.
- `personal_data/profile.md` from `personal_data/profile_template.md`
- `personal_data/form_answers.md` from `personal_data/form_answers_template.md`
- `personal_data/credentials.md` from `personal_data/credentials_template.md`

## 2. The resume

If `personal_data/resume.pdf` is missing, ask for the resume file or its path, and copy it to exactly `personal_data/resume.pdf`. Never invent a path. If you cannot reach the file, ask the user to upload it in the AI app.

## 3. Fill in from the resume

Read the resume and fill in every `[placeholder]` in `profile.md` that the resume answers: names, email, phone, city and country, links, current job title, total years of experience, core skills, each employer (company, title, start and end month, main work) and each education entry. Never guess what the resume does not say.

Then show the user what you filled in, as one short list, and ask them to correct anything. Save their corrections at once.

## 4. Ask only what is still missing

Ask for every `[placeholder]` that is left, one topic per message, in this order:
1. contact details the resume does not have, such as the phone country code, or an address when needed;
2. employment and education details the resume does not have;
3. notice period, and current and expected salary: amount, currency, and annual or monthly;
4. work authorization and sponsorship, for each country;
5. job preferences: target job titles, locations, work modes, relocation, companies to skip, and company career page URLs;
6. common screening answers in `form_answers.md`.

Save each answer at once. Never ask about anything the files already answer.

Ask for the company portal login in `credentials.md` only when today's plan has `workday`, `company_direct`, `discovery` or a job source.

## 5. Skipped answers

When the user skips an optional question, save `None` at once and never ask it again. `None` is never typed into a form and never turned into a guess.

## 6. Finish

Run the setup check again (`AGENTS.md` section 3). If something is still missing, continue at the first missing item. When it passes, go on to today's plan (`AGENTS.md` section 4) in the same chat.

Never ask about NorthList during setup. Sync turns on by itself once NorthList is connected in the AI app (README).
