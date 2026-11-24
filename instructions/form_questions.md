# Form questions

Read this file before the first application form of a session, and again after the app shortened the chat (`AGENTS.md` sections 6 and 7). It says how to answer a form's questions and add the resume without stopping the run.

## 1. Where answers come from

In this order: `personal_data/profile.md` (the facts), `personal_data/form_answers.md` (saved answers; a contextual answer only when its recorded context matches this job, and never one employer's or country's answer turned into a general one), then the resume. A question none of them answers is a judgment (section 2), a fact only the user knows (section 3), or a test or essay to leave (section 5). Never stop the run to ask in the chat: after `ready` the user may be away.

`profile.md` also has the details company sites ask (`## Details forms ask`: date of birth, gender, nationality, postal code, marks, a conviction, a non-compete). Use them as written. `None` there means the user skipped it: a required question about it is a fact (section 3).

Number fields get digits only. A years field that takes whole numbers only gets the nearest whole number (1.5 becomes 2).

**LinkedIn link.** When `LinkedIn Profile URL` is `None` and a form asks for it: open `https://www.linkedin.com/in/me/` once in a work tab; when it shows the user's own profile, save its address (without the part from `?`) in `profile.md` and use it. Signed out: do not ask for a sign-in; leave an optional field empty, a required one makes the job `needs_user`.

## 2. Judgments: answer them yourself

A question that asks for a judgment, not a fact only the user knows, gets a standard answer, and the run goes on:
- **Rate yourself in a skill:** on 1 to 10, 8 for a skill in `Core Tech Stack` and 7 for another skill the resume names; on 1 to 5, 4; on words, "Advanced" for a `Core Tech Stack` skill and "Intermediate" for another. A skill that the profile and the resume do not name: the saved answer for skills not in the profile; on a scale, its lowest step (1, or "Beginner").
- **Comfort or willingness:** shifts, travel, learning a new tool, some days at the office, a background check, the usual work setup (a laptop, internet): Yes, unless `profile.md` or a saved answer says otherwise.
- **How did you hear about us?** The site the job was found on (LinkedIn, Naukri, the company's page).
- **A short written answer** (why this company, about yourself, a project): 2 or 3 plain sentences only from `profile.md`, the resume and this job's post. Never add a fact, number, project or employer they do not have.
- **Voluntary questions** about gender, ethnicity, disability or veteran status: "Prefer not to say", or the form's closest words. When the form has no such choice and the answer is required: the profile's `Gender` for a gender question when it has one; anything else is a fact (section 3).

Write each answer into the entry's `Submitted answers` as usual. Save a self-rating or a comfort or willingness answer under `## Learned Answers` (section 4) with `Reuse scope: AI standard answer`, so the same question gets the same answer next time; the user may change it there. Written answers and "how did you hear" are made fresh for each job and not saved.

## 3. Facts only the user knows: hold the job

Never guess these: an ID or tax number (PAN, Aadhaar, passport, social security number), the date of birth, a salary, date or number that `profile.md` does not have, a legal yes or no (a criminal record, a non-compete), references and their contacts, a bond, and anything that agrees to pay money. With such a required question:
- add a `needs_user` entry with the exact question and, when the form shows them, its options, as `Question: <exact question> | Options: <option>; <option>`;
- go on with the next job at once: never ask in the chat and never wait for this;
- the run page asks the user at the end of the session (`instructions/run_page.md`), and the job is tried again once the answer is saved.

## 4. Saving answers

When the user answers a question (in the chat, or on the run page, which gives the records ready to copy), or you gave a standard answer, add it to `personal_data/form_answers.md` at once: under `## Contextual Answers` when it holds only for an employer, country, role or portal, otherwise under `## Learned Answers`, each with its context and the exact question. A job left as `needs_user` may then be tried again; the retry gets a new entry, and the old one stays. Use this format, and add a record only when a real question comes up:

```md
- Context: Employer=<value or Any>; Country=<value or Any>; Role=<value or Any>; Portal/ATS=<value or Any>
- Exact question: <verbatim question>
- Answer: <saved answer>
- Reuse scope: <exact context only | broader context explicitly approved by the user | AI standard answer>
```

The user's own answer always wins: when the user answers a question that has an `AI standard answer` record, replace that record.

## 5. Tests and essays about the past: skip the job

Never try a required skills test or assessment, a recorded video, or an essay about a past event that the profile and the resume do not tell ("Tell us about a time you disagreed with a manager"), unless a saved answer covers it. Add a `skipped` entry ("Needs a skills test", "Needs an essay about a past event") and go on with the next job.

## 6. The resume

When the form already offers a resume named `resume.pdf` (uploaded by an earlier job), pick it and upload nothing. Otherwise upload `personal_data/resume.pdf` yourself, never asking the user first: with the browser's file-upload tool, or, when it has none or it fails, with the app's script tool (read the file, put it on the page's file field, fire the field's change event). An Allow prompt for either is normal (`AGENTS.md` section 9, "Hands-free"). Check that the page shows the file. Use the way that worked first for the rest of the session; a way that failed twice is not tried again. When both ways have failed, say "Please upload your resume in this tab (the file `personal_data/resume.pdf` in the NorthList Apply folder). I'll check for 5 minutes, then go on." and wait (`AGENTS.md` section 9, "Never stop"). Later uploads this chat session: ask at once, or once a wait ran out, skip at once.
