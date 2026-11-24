# Setup

Read this file only while the setup check in `instructions/before_run.md`, section 0, fails, or when `instructions/before_run.md`, section 2, sends you here for the portal password (section 4). Never apply to jobs during setup.

Before starting, suggest in one sentence the strongest model of the user's current AI app for setup. Never name a model or version, and never claim to switch models.

## 0. First: NorthList (only for a brand-new setup)

When `personal_data/profile.md` does not exist yet and NorthList sync is off (`AGENTS.md` section 4: the tools are not there), say this once, before anything else, then wait:

> **Before we set up: connect NorthList.**
> See everything your AI applies to in one place: every job, where the application went, the answers it sent, and every reply from the company, sorted from your Gmail.
> 1. Open app.northlist.site/automation?tab=setup and sign in (new here? sign up there).
> 2. Create your setup prompt and paste it here.
>
> Reply `skip` to set up without it. You can connect any time.

- The user pastes the setup prompt: follow `instructions/northlist_connect.md`. After the app restart, `start` goes on with setup; the offer is not shown again.
- `skip`, or anything else: go on with setup.

Right after the answer, create the missing files (section 1), so the offer is never shown again, also not in a new chat.

## 1. Missing files

Create each missing file from its template. Never replace a file that already has answers.
- `personal_data/profile.md` from `personal_data/profile_template.md`
- `personal_data/form_answers.md` from `personal_data/form_answers_template.md`
- `personal_data/credentials.md` from `personal_data/credentials_template.md`

## 2. The setup page (the usual way)

When the AI app has its own browser (`AGENTS.md` section 9) and the user has not asked to answer in the chat, read `instructions/setup_page.md` and follow it. It gets the resume, fills in what the resume says, and the user answers the rest on the page. Then go to section 5.

## 3. In the chat instead

Use the chat when the app has no browser tool, the page cannot open, or the user asks to answer in the chat (for example "just ask me here").

1. **Resume.** If `personal_data/resume.pdf` is missing, ask for the file or its path and copy it there. Never invent a path.
2. **From the resume.** Fill in every `[placeholder]` in `profile.md` that the resume answers, the same facts as `instructions/setup_page.md` step 2 (also notice period, salary, work modes, relocation and work permission when the resume says them, and the links, also those behind words: `instructions/setup_page.md`, step 2, "Links"). Never guess. Show what you filled in as one short list and ask for corrections; save them at once.
3. **The rest, 3 to 5 numbered questions per message,** in this order, leaving out every question the files already answer:
   - a. notice period; current salary; expected salary (amount, currency, per year or month);
   - b. work permission and visa sponsorship for each country; work modes; cities; open to moving;
   - c. job titles; companies to skip; company career page links;
   - d. years per main skill; English level; what to answer for a skill not in the profile;
   - e. details company sites often require, each one optional (`skip` saves `None`): date of birth; gender; nationality; postal code; marks or CGPA of the highest degree; ever convicted of a crime; bound by a non-compete agreement.
   The user answers in one reply ("1. 30 days 2. …"). Save each answer at once. Ask only the missing ones again in the next message.

## 4. Portal password

Ask only when today's plan has `workday`, `company_direct`, `discovery` or a job source, and `credentials.md` still has a placeholder. Ask in exactly these words, with the email from `profile.md`:

> **One password for company job sites.** Workday and company career sites need an account for each company. Reply with one password, and I will use it with your email (<email>) to make those accounts. Use 8 or more characters with a capital letter, a number and a symbol, and not the password of your email or LinkedIn.

Save the reply as `Standard Application Password` and the profile's email as `Standard Application Email` in `credentials.md`, with small edits. Never ask for the email. If the password misses one of the four needs (8 characters, a capital letter, a number, a symbol), say which one and ask once more. Never repeat the password in a message.

## 5. Skipped answers

When the user skips an optional question, save `None` at once and never ask it again. `None` is never typed into a form and never turned into a guess.

## 6. Finish

Run the setup check again (`instructions/before_run.md`, section 0). If something is still missing, ask only that, in the chat. When it passes, go on from there to today's plan in the same chat.
