# Company accounts and email checks

Read this file before the first Workday or other company-portal job of a session (`AGENTS.md` section 10). Workday and company career systems need an account for each company.

## 1. The account

- Check `tracking/created_accounts.csv` first. If the company has a record for this portal, sign in with it; never create a second account.
- If not, create one with the email and password in `personal_data/credentials.md`, and add its record at once (header `"date","company","portal_url","login_email","password","email_verified"`; `email_verified` is `no` until the email check is done).
- If the app blocks the account creation, say: "Please create your <Company> account in this tab with your email (<email>). I'll check for 5 minutes, then go on." and wait (`AGENTS.md` section 9, "Never stop"). When the account is made, add its record at once: `portal_url` the portal's sign-in page (else the page where the account was made), the login email the page shows (else the profile email), the `password` column empty and `email_verified` `no`. Never ask for that password.

## 2. The email check

A company portal often sends a verification link or code after a new account.

1. Open the user's webmail in a new tab of the same browser: Gmail (`https://mail.google.com`) for a `gmail.com` address, Outlook (`https://outlook.live.com/mail`) for `outlook.com`, `hotmail.com` or `live.com`, Yahoo Mail (`https://mail.yahoo.com`) for a `yahoo.` address.
2. If it is signed in, search only for that company's message from the last hour. Use its link or code, close the tab, and set the record's `email_verified` to `yes`. Read no other mail.
3. If webmail is signed out, the address is another kind, or no message comes within 3 minutes, say: "<Company> sent a check to your email (<email>). Please open that mail and click its link (or type its code in this tab). I'll check for 5 minutes, then go on." and wait (`AGENTS.md` section 9, "Never stop"). When the portal shows the check is done, set `email_verified` to `yes`.

Next time, the check before the run offers to sign in to the email first (`instructions/before_run.md`, section 2). After one such wait ran out, later email checks this session still try the webmail steps 1 and 2; only the request to the user is left out, and that job is skipped at once (`AGENTS.md` section 9, "Never stop").

## 3. With NorthList sync on: send the account with the application

When an application used a company-portal account, its send to NorthList (`instructions/northlist_sync.md`) also carries `portalAccount`, so the user can sign in from NorthList. NorthList keeps the password encrypted and shows it only to the user.

1. **When.** Only for an `applied` entry whose `Platform` is `workday`, `company_direct` or `discovery`. Find the record in `tracking/created_accounts.csv` whose `company` is this entry's company and whose `portal_url` has the same host as `Apply page URL` (when that is empty, `Actual application destination`). If two records match, use the one with the latest `date`. With no such record, leave `portalAccount` out. One search of the file is enough; never guess an account.
2. **Fields** of `portalAccount`, copied from that record:

| `portalAccount` field | Value from the record |
| --- | --- |
| `signInUrl` | `portal_url` (the full address). |
| `loginEmail` | `login_email`. |
| `password` | `password`, exactly as written. Leave it out when the column is empty. |
| `emailVerified` | `true` for `yes`/`true`, `false` for `no`/`false`; leave it out for anything else. |
| `madeBy` | `northlist_apply` when the record has a password, `user` when its password is empty (the user made the account in the browser). |
| `createdOn` | `date` when it is `YYYY-MM-DD`; otherwise leave it out. |

3. **The user decides.** If the user asks not to send portal passwords, save `- NorthList portal passwords: off` in `personal_data/profile.md` and say so once; from then on, in every session and replay, `password` is left out (the rest of `portalAccount` is still sent). If the user asks not to send portal accounts at all, save `- NorthList portal accounts: off` instead, and `portalAccount` is left out. Read these lines before each send. Only the user's own words turn them back on.
4. The password is never written into the daily record, a sync line, a message or the summary, also not when a send fails.
