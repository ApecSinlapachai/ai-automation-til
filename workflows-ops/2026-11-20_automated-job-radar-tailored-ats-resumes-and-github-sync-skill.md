---
title: "TIL #020: Building a Repeatable Job Search Engine — Daily Job Radar, Tailored ATS Resumes & GitHub Portfolio Sync Skill"
date: 2026-11-20
last_updated: 2026-11-20
domain: learning/til
category: workflows-ops
status: ready-to-post
public_channels: [linkedin, x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, job-search, ats-resume, github, automation, career-ops]
---

# 🚀 TIL #020: Building a Repeatable Job Search Engine — Daily Job Radar, Tailored ATS Resumes & GitHub Portfolio Sync Skill

> **🎯 Goal:** Replace ad-hoc, high-friction job hunting with a structured, low-friction daily pipeline: automated role discovery (`Daily Job Radar`), HTML-to-PDF role-tailored resumes, a centralized application tracker, and a reusable `github-portfolio-sync` agent skill to keep all 4 Flagship Public Repos continuously verified.

---

## 1. TL;DR (1–3 Lines)
- **What I shipped:** Built a two-track **Daily Job Radar** (`10:00` local schedule) with pipeline de-duplication, generated a custom role-tailored HTML→PDF resume plus a LinkedIn banner (`HTML/CSS` → Chrome Headless), shipped **5 applications + 2 direct speculative outreach emails** in one day, and codified the **`github-portfolio-sync`** agent skill.
- **Why it matters:** Treating job search like a GTM pipeline removes morning decision fatigue, ensures every resume maps directly to the target JD, and guarantees that any hiring manager clicking into GitHub sees tested, 100% English proof of work.

---

## 2. Problem → Architecture & Solution

### Pain Points
1. **Fragmented Job Discovery:** Manually checking JobsDB, LinkedIn, MNC career portals, and remote startup boards every day wastes focus and leads to re-reading already-applied listings.
2. **One-Size-Fits-All Resumes Underperform:** A single generic PDF misses role-specific keywords when applying across `AI & Automation Specialist`, `GTM Engineer`, and `Transformation Associate` roles.
3. **Portfolio Drift:** Shipping new TIL notes or automation upgrades locally without a repeatable audit/sync workflow causes the public GitHub profile (`ApecSinlapachai`) to fall behind local work.

### Architecture & Execution
1. **Daily Job Radar (`01-Personal/Career/daily_job_radar.md`):**
   - Split discovery into **Track A (Local Thailand: JobsDB & MNC Graduate/Analyst Careers)** and **Track B (Global/Remote: Himalayas, Wellfound, RevOps Careers, HN "Who is hiring?")**, plus 60-second 1-click X/LinkedIn search queries.
   - Enforced negative filters (`3+ yrs Senior/Lead`, pure ML research, graphic design/admin, unpaid) and automatic exclusion against `job_application_pipeline.md`.
2. **HTML/CSS Role-Tailored Resumes & Career Assets:**
   - Exported the base ATS PDF (`Sinlapachai_Kaeobunrueang_Resume_AI_Automation.pdf`) and created a dedicated role-tailored HTML→PDF resume for a Fast-Track Transformation Associate role (`Tailored/resume_outlier_transformation_associate.html`).
   - Generated a custom LinkedIn banner (`linkedin_banner_apec.html` → `linkedin_banner_apec.png`) via HTML/CSS + Chrome Headless, plus a 75-second intro video script and company-specific interview prep notes.
   - Logged **5 applications shipped** (`Ourgreenfish`, `Oh Mango`, `S39 Digital Agency`, `LH Bank`, `OUTLIER`) and **2 direct speculative outreach emails** in `job_application_pipeline.md` with `+5d` follow-up dates.
3. **Reusable `github-portfolio-sync` Skill (`.agents/skills/github-portfolio-sync/SKILL.md`):**
   - Codified the SSOT mapping and verification gates for all 4 Flagship Public Repos (`ApecSinlapachai`, `fast-lead-automation`, `agentic-media-pipeline`, `ai-automation-til`): diff against `gh repo list`, isolate staging in `$env:TEMP\gh-portfolio-sync` (never nesting `.git` inside the vault), run `npm test` (`45/45` + `28/28`) & `py_compile`, and run PDPA/secret scans before pushing.

---

## 3. Verified Proof of Work & Tech Stack
- **Tech Stack:** `HTML/CSS (@media print)`, `Chrome Headless`, `GitHub CLI (gh)`, `PowerShell`, `Markdown / Obsidian SSOT`, `Agent Skills`
- **Verified Outcomes:**
  - **5 applications + 2 direct outreach emails** logged on `2026-11-20` with `+5d` follow-up tracking (`job_application_pipeline.md`).
  - **Daily Job Radar (Test Run #1)** live with Track A & Track B shortlists and automatic pipeline de-duplication.
  - **`github-portfolio-sync` skill** created to enforce 100% English, test-verified, PDPA-clean sync across all 4 Flagship Repos.
- **Public Links & Internal References:**
  - GitHub: [github.com/ApecSinlapachai](https://github.com/ApecSinlapachai)
  - Internal Files: `Daily Job Radar` • `Application Pipeline` • `GitHub Portfolio Sync Skill`

---

## 4. Learn in Public Copy Pack (Ready to Post)

### 💼 LinkedIn / 🐦 X Post (EN — Value-First & Authority)
```text
I stopped treating job hunting as random scrolling and rebuilt it as a lightweight GTM pipeline.

Today's build inside my Personal AI OS:
1. Daily Job Radar (10:00 schedule): splits local Thailand MNC/JobsDB roles (Track A) from global remote GTM/Automation boards (Track B), and auto-excludes companies already in my pipeline.
2. HTML/CSS -> PDF Tailored Resumes: single-page #FFFFFF ATS layouts with role-specific keyword density instead of one generic PDF.
3. GitHub Portfolio Sync Skill: a repeatable checklist that diffs my 4 flagship public repos, runs automated tests (45/45 & 28/28 + py_compile), and scans for PDPA/secrets before pushing.

Result on Day 1: 5 targeted applications + 2 direct outreach emails shipped with +5d follow-up dates locked in.

If you're applying for AI/Automation or GTM roles, treating your own job search like an ops system is the fastest proof of work you can build.

#AIAutomation #GTMEngineering #CareerOps #BuildInPublic
```

### 📘 Facebook Page Summary (TH — Optional)
```text
วันนี้เปลี่ยนการสมัครงานจากการไถหาแบบสุ่ม ให้กลายเป็นระบบ Pipeline เหมือนงาน GTM จริงๆ ครับ:
1. ทำ Daily Job Radar คัดงานสาย AI & Automation / GTM ทุกวัน 10:00 น. แยกงานไทย/MNC กับงาน Remote ต่างประเทศ พร้อมตัดบริษัทที่สมัครแล้วออกอัตโนมัติ
2. ทำ Resume แบบ HTML/CSS -> PDF ที่ปรับคีย์เวิร์ดเฉพาะตำแหน่งได้ไว + ทำแบนเนอร์ LinkedIn ด้วยโค้ด
3. เขียนสกิล github-portfolio-sync ไว้เช็กความเรียบร้อย รันเทส และตรวจ PDPA ก่อนซิงค์ 4 Flagship Repos บน GitHub ทุกครั้ง

ผลลัพธ์วันแรก: ยิงใบสมัครตรงสายไป 5 บริษัท + ส่ง Direct Outreach อีก 2 แห่ง พร้อมตั้งวัน Follow-up ครบในระบบเดียวครับ
```

