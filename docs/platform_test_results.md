# Platform Automation Test Results

**Date:** 2026-11-11  
**Target Roles:** Frontend Engineer / React Developer / Full Stack Engineer  

---

## 1. Verified & Working Platforms ✅

### 1.1 Wellfound (formerly AngelList)
- **Status:** Fully functional & verified.
- **Workflow:** 
  - Filters applied for job role and freshness.
  - Custom, tailored pitch notes crafted per job based on company mission and candidate profile.
  - Successfully submitted 3 applications with confirmation buffer captured.
  - Synced to NorthList tracker.
- **Tested Employers:** Simbian AI, POOLit, WafyNet.

### 1.2 Instahyre
- **Status:** Fully functional & verified.
- **Workflow:**
  - Opportunity discovery matching experience requirements (2–6 years).
  - 1-click apply flow executed cleanly.
  - Instant application confirmation toast verified.
  - Synced to NorthList tracker.
- **Tested Employer:** PivotX Advisors.

### 1.3 Workday (`*.myworkdayjobs.com`)
- **Status:** Fully functional & verified.
- **Workflow:**
  - Account creation handled using standard credentials without collisions.
  - Multi-step form completion: My Information, Experience & Education parsing/correction, custom compensation & notice period questions, voluntary disclosures.
  - Successful final review and submission confirmation receipt captured.
  - Synced to NorthList tracker.
- **Tested Employer:** HP (HP Inc.) — Software Development Engineer - Frontend (Bengaluru).

### 1.4 Greenhouse (`job-boards.greenhouse.io`)
- **Status:** Fully functional & verified via Web Search Discovery.
- **Workflow:** 
  - Direct ATS application discovery.
  - Single-page form: technical screening questionnaires (experience levels, state management, testing frameworks, architectural summaries), notice period, CTC, work mode confirmation.
  - Successful submission receipt captured.
  - Synced to NorthList tracker.
- **Tested Employer:** Capco (a Wipro company) — React.JS Developer (Sarjapur, Bengaluru).

### 1.5 Ashby (`jobs.ashbyhq.com`)
- **Status:** Fully functional & verified via Company Career Page / Direct Apply.
- **Workflow:**
  - Modern Next.js SPA form integration.
  - Direct file upload for `resume.pdf`, profile URLs, and customized company-specific statement.
  - Successful submission receipt captured ("Success - Your application was successfully submitted").
  - Synced to NorthList tracker.
- **Tested Employer:** Sarvam AI — Frontend Engineer, Chanakya (Bengaluru).

---

## 2. Platforms with Restrictions / Skipped ⚠️

### 2.1 We Work Remotely (WWR)
- **Status:** Stopped / Incompatible with headless automation.
- **Issue:** WWR gates the "Apply for this position" button behind mandatory job-seeker registration (`/job-seekers/account/register`).
- **Blocker:** Encountered Cloudflare Turnstile bot challenge (HTTP 403 / anti-bot challenge). Automated bypass is prohibited per agent safety policy.
- **Recommendation:** Use direct discovery queries (Greenhouse, Lever, Ashby, Workday) instead of boards gating links behind Cloudflare Turnstile.

---

## 3. Pending / Next Platforms for Testing ⏳
- **Lever** (`jobs.lever.co`)
- **Company Direct Career Pages**
