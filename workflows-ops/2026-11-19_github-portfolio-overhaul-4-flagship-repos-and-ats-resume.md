---
title: "TIL #019: GitHub Portfolio Overhaul — Archiving 30 Legacy Repos, Shipping 4 Flagship Repos & a 4-Role ATS Resume"
date: 2026-11-19
last_updated: 2026-11-19
domain: learning/til
category: workflows-ops
status: ready-to-post
public_channels: [linkedin, x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, github, ats-resume, gtm-engineering, ai-automation, career-ops]
---

# 🚀 TIL #019: GitHub Portfolio Overhaul — Archiving 30 Legacy Repos, Shipping 4 Flagship Repos & a 4-Role ATS Resume

> **🎯 Goal:** Reposition an early-career technical profile from mixed university coursework and creative media into a focused **AI & Automation / GTM Engineering** portfolio backed by verifiable public repositories, automated tests, and an ATS-clean 1-page resume.

---

## 1. TL;DR (1–3 Lines)
- **What I shipped:** Switched 30 legacy 2025 coursework repos on GitHub to `Private` (preserving commit history), launched **4 Flagship Public Repos** with passing tests, built a single-page A4 HTML/CSS ATS resume with a 4-role switcher, and audited my LinkedIn PDF export to fix 5 rendering and completeness bugs.
- **Why it matters:** Hiring managers and ATS filters spend seconds scanning a profile. Removing visual/coursework noise and leading with tested, production-hardened automation systems eliminates role ambiguity immediately.

---

## 2. Problem → Architecture & Solution

### Pain Points
1. **Low Signal-to-Noise on GitHub:** 30 older 2025 college repositories overshadowed real production automation work.
2. **ATS & Recruiter Pigeonholing:** Listing design tools (`Figma`, `UX/UI`, Adobe Creative Suite) in Technical Skills caused automated screeners to bucket the profile into graphic/social-admin roles instead of systems and automation engineering.
3. **Broken LinkedIn PDF Formatting:** Auditing an exported LinkedIn profile PDF surfaced 5 concrete bugs: a surname typo, an outdated email address, raw `&amp;` / `&lt;` HTML entity artifacts in the `About` section, an under-documented Experience section, and missing Education/TOEIC/Honors proof.

### Architecture & Execution
1. **GitHub Profile Overhaul (`github.com/ApecSinlapachai`):**
   - Converted **30 legacy coursework repos** from `Public` to `Private` without deleting commit history.
   - Published and verified **4 Flagship Public Repos**:
     1. `ApecSinlapachai/ApecSinlapachai` — English Profile README aligned to AI & Automation / GTM Engineering.
     2. `ApecSinlapachai/fast-lead-automation` — Google Apps Script (`v2.0` / `v3.0`) + n8n + LINE Flex CRM alert pipeline (`npm test` passing **45/45** and **28/28** mock test cases).
     3. `ApecSinlapachai/ai-automation-til` — Sanitized public engineering log synced from `03-Learning/TIL/` via `sync_til_public.ps1`.
     4. `ApecSinlapachai/agentic-media-pipeline` — `auto_cut_video.py` (Python + FFmpeg `-16 LUFS` loudness normalization), verified with `py_compile` and `--help`.
2. **Single-Page HTML/CSS ATS Resume (`resume_apec_ai_automation.html`):**
   - Designed on a `#FFFFFF` pure-white printable A4 layout with a `.no-print` interactive switcher across 4 target roles (`AI & Automation Specialist`, `GTM Engineer`, `Agentic Content Automation Specialist`, `AI Marketing Systems Architect`).
   - Removed creative-suite tool keywords from Technical Skills while keeping factual internship history intact, focusing ATS density on `Google Apps Script`, `n8n`, `REST Webhooks`, `LINE Messaging API`, `Python`, `FFmpeg`, `PowerShell`, and `Context Engineering`.
3. **LinkedIn PDF Audit & Outreach Kit (`linkedin_and_outreach_kit.md`):**
   - Replaced `&` and `<` symbols in LinkedIn `About` copy with plain words (`and`, `under`) so LinkedIn's PDF generator no longer renders `&amp;` / `&lt;` entity glitches.
   - Built structured outreach templates across 3 target tiers (Tier 1 MNCs, Tier 2 MarTech/Automation teams, Tier 3 Remote/Global).

---

## 3. Verified Proof of Work & Tech Stack
- **Tech Stack:** `GitHub`, `HTML/CSS Print (@media print)`, `Node Mock Tests`, `Python`, `FFmpeg`, `PowerShell`
- **Verified Outcomes:**
  - `30` legacy 2025 repos archived to `Private`; `4` Flagship Public Repos live.
  - `fast-lead-automation` test suites verified (`45/45` and `28/28` passing before push); `agentic-media-pipeline` verified via `py_compile` and `--help`.
  - `5` LinkedIn PDF defects identified and patched in reusable copy templates.
- **Public Links & Internal References:**
  - GitHub: [github.com/ApecSinlapachai](https://github.com/ApecSinlapachai)
  - Internal Files: `HTML Resume` • `LinkedIn & Outreach Kit` • `Application Pipeline`

---

## 4. Learn in Public Copy Pack (Ready to Post)

### 💼 LinkedIn / 🐦 X Post (EN — Value-First & Authority)
```text
If your GitHub and Resume show everything you've ever touched, recruiters only see noise.

Today I overhauled my engineering portfolio for AI & Automation / GTM roles:

1. GitHub Signal Reset:
- Moved 30 legacy 2025 coursework repos to Private (kept commit history)
- Pinned 4 flagship public repos with real verification (45/45 & 28/28 automated tests, Python/FFmpeg checks, and sanitized engineering TIL logs)

2. Role-Tailored 1-Page HTML Resume:
- Built a pure-white A4 printable HTML resume with a .no-print role switcher for 4 adjacent titles
- Removed unrelated design-tool clutter from Technical Skills so ATS sees systems engineering, not graphic admin

3. LinkedIn PDF Export Audit:
- Exported my own profile to PDF and caught 5 hidden bugs—including raw &amp; and &lt; entity glitches in the About section.

Tip: Always export your LinkedIn profile to PDF and inspect it like an external recruiter.
```

### 📘 Facebook Page Summary (TH — Optional)
```text
บทเรียนจากการยกเครื่อง GitHub และ Resume สาย AI & Automation วันนี้: "ลด Noise ให้เหลือแต่ Proof of Work ที่พิสูจน์ได้"

1. ล้างหน้า GitHub ใหม่: ย้าย Repo งานเรียนเก่าปี 2025 ทั้ง 30 ตัวเป็น Private (ไม่ลบประวัติ) แล้วปักหมุดแค่ 4 Flagship Repos ที่รันเทสผ่านจริง (45/45 และ 28/28 เคส)
2. ทำ HTML Resume 1 หน้า A4: ใส่ปุ่ม .no-print สลับ Title ตาม 4 ตำแหน่งเป้าหมาย และตัดคีย์เวิร์ดสายกราฟิกออกจาก Technical Skills เพื่อไม่ให้ ATS จัดหมวดผิดสาย
3. Export LinkedIn เป็น PDF เช็กตัวเอง: เจอ 5 จุดพังที่ไม่เคยสังเกต รวมถึงตัวอักษร & กับ < ที่ LinkedIn แปลงเพี้ยนเป็น &amp; / &lt; ในไฟล์ PDF

ใครกำลังเตรียมสมัครงานสาย Tech ลองกด Save to PDF บนโปรไฟล์ LinkedIn ตัวเองดูครับ เจอบั๊กเพียบแน่นอน
```

### 🎬 YouTube / Short Video Hook
- **Title Idea:** `I Made 30 GitHub Repos Private and Built a 4-Role HTML Resume (Here's Why)`
- **5-Second Hook:** *"Your old college GitHub repos and your LinkedIn PDF export might be silently disqualifying you from tech interviews."*

---

## 5. Rollup Sync Checklist
- [x] Updated `til_summary.md` (Portfolio Highlights & TIL Log)
- [x] Updated `README.md`
- [x] Updated `03-Learning/index.md` & `log.md`

