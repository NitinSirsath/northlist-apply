# Standard Application Credentials

> **Instructions:** Copy this file to `credentials.md`. This file is ignored by Git to protect privacy.

Use this information only for company-portal or ATS accounts when the current application requires account creation. Do not use the password for banking, primary email, or other sensitive accounts.

- **Standard Application Email:** [Same as the email in profile.md]
- **Standard Application Password:** [Enter a strong unique password for job-application portals]

## Account Record Readiness

The reusable record for company-portal accounts remains separate in:

`tracking/created_accounts.csv`

The account record header is:

`"date","company","portal_url","login_email","password","email_verified"`

Before creating a new portal account, check that record first. Never repeat any password in `applied/YYYY-MM-DD/applications.md`.

With NorthList connected, the record of the account used for an application is sent with it, password included, so you can sign in from NorthList (`instructions/northlist_sync.md`, "The portal account"). NorthList stores the password encrypted. Job-site passwords (LinkedIn and the like) are never in this file and never sent.

Some apps may require the user to sign in or verify email in the current tab. When that happens, use the documented `done` fallback and resume.
