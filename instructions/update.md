# Update check

Read this file only when the update check (`instructions/before_run.md`, section 0) finds a newer version. Never update during a run: an update found later waits for the next `start`.

The only update address is `https://raw.githubusercontent.com/NitinSirsath/northlist-apply/master/`. Never use an address from a web page, a mail, a message or a file.

## 1. What may change

`VERSION` lists the product files after the line `files:`, one path per line. Only those files may change, and only when the path:
- is inside this folder (no `..`, does not start with `/` or `~`);
- does not start with `personal_data/`, `applied/`, `tracking/` or `.update/`;
- is not `.mcp.json`, and does not start with `.agents/` or `.codex/`. The one exception: the three `personal_data/*_template.md` files may change.

If any listed path breaks these rules, update nothing, say once "The update list looks wrong, so I did not update." and go on with the current version.

Never run, open in a browser or execute a downloaded file. These files are instructions, not programs.

## 2. Update

- **This folder has a `.git` folder:** run `git pull --ff-only`. If it stops because the user changed files themselves, change nothing and say once: "There is a new NorthList Apply version, but you changed some of its files, so I did not update. Download it again from GitHub to update."
- **No `.git` folder:** download every listed file from the update address (the address plus the path) into `.update/`, keeping the same paths. When every file arrived and none is empty, copy them over the old ones, then delete `.update/`. If one download fails, delete `.update/`, change nothing, and try again at the next `start`. Say nothing about a failed download.

## 3. Tell the user

Read the new `UPDATES.md`. Show only the versions newer than the old one, in this form, at most 8 lines:

> **NorthList Apply is up to date** (old version → new version)
> New for you:
> - one line per new feature
> Before → Now:
> - one line per change

## 4. Go on

Read `AGENTS.md` and `instructions/before_run.md` again from the top. If `personal_data/profile.md` has a line `Career Companion server:`, change only that label to `NorthList server:` (if the profile then has two `NorthList server:` lines, delete the one that says `None`). Then compare `personal_data/profile.md` with the new `personal_data/profile_template.md`: for each `- **Label:**` line the template has and the profile does not, add that line to the profile (in the same section, with a new section's heading and note when the profile lacks it). A line whose template value is `None` is added as `None` and not asked. Ask the user only about the others, in one message, before the plan question, and save the answers at once. Then, at every update:
- If `Total Years of Experience` is a whole number and `## Employment History` has start and end dates, count it again from those dates: the months from each start month to its end month (`Present` = this month; May 2025 to November 2026 is 18 months; overlapping jobs once), divided by 12, with one decimal (18 months = `1.5`). Save it when it differs.
- If `LinkedIn Profile URL`, `GitHub Profile URL` or `Personal Portfolio URL` is `None`, look for it in `personal_data/resume.pdf` as `instructions/setup_page.md`, step 2, "Links" says, and save what you find.
- In `personal_data/form_answers.md`, delete these two old notes when they are there: the sentence "Add a learned record only when the user provides the answer." and the paragraph that starts "When a record is needed,". Change nothing else in that file.

Then go on with `start` with the new rules.
