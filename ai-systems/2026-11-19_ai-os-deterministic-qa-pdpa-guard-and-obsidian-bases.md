---
title: "TIL #018: Deterministic QA Gates, PDPA Guardrails, SSOT Deduplication & Obsidian Bases for Personal AI OS"
date: 2026-11-19
last_updated: 2026-11-19
domain: learning/til
category: ai-systems
status: ready-to-post
public_channels: [linkedin, x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, ai-os, powershell, devsecops, obsidian-bases, context-engineering]
---

# 🚀 TIL #018: Deterministic QA Gates, PDPA Guardrails, SSOT Deduplication & Obsidian Bases for Personal AI OS

> **🎯 Goal:** Harden a multi-pillar Markdown `Personal AI OS` with deterministic PowerShell validation, automated secret/PDPA scanning, Single Source of Truth (SSOT) deduplication, and dynamic Obsidian Bases so agent workflows stay fast, private, and reliable as the vault grows.

---

## 1. TL;DR (1–3 Lines)
- **What I built:** Implemented `health_check.ps1` (pre-backup QA & secret gate), upgraded `sync_til_public.ps1` with PDPA/PII regex guards, rotated `log.md` (`129.7 KB` → `~33 KB`), consolidated duplicated goals into SSOT files, and added Obsidian Bases (`AI_OS_Dashboard.base`, `TIL_Catalog.base`) alongside new skills (`morning-launch`, `weekly-mastermind`).
- **Why it matters:** Prompt rules alone are probabilistic—agents and humans can still leave broken wikilinks, oversized context files, or accidental PII. Pairing Markdown architecture with deterministic PowerShell gates makes system hygiene enforceable before every Git backup or public push.

---

## 2. Problem → Architecture & Solution

### Pain Points
1. **Goal & Rule Drift:** Copying milestones and rules across multiple pillar indexes created stale duplicates that required manual cleanup whenever strategy changed.
2. **Context Bloat & Silent Link Decay:** `log.md` grew to `129.7 KB`, and manual file moves risked breaking Obsidian wikilinks or missing YAML frontmatter.
3. **Public Export Privacy Risk:** Syncing TIL engineering notes to a public GitHub repo (`ai-automation-til`) without deterministic regex checks risked exposing API keys, personal contact details, or internal Google Sheet IDs.

### Architecture & Implementation
1. **SSOT Consolidation & Weekly Log Rotation:**
   - Designated `north_star.md`, `Triage.md`, and `user_profile.md` as Single Sources of Truth for goals, execution priority (`P0 → P1.1 → P1.2 → P2`), and constraints.
   - Archived 3 legacy singular `AGENT.md` files into `00-System/Archive/legacy-agents/` and rotated older activity logs into `2026-W46.md` and `2026-W47-part1.md`, trimming `log.md` from `129.7 KB` down to `~33 KB`.
2. **Deterministic Pre-Backup QA (`00-System/health_check.ps1`):**
   - Integrated with `backup.bat` (`-PreBackup`, plus `-Strict` mode) while strictly excluding `00-Notes/` (Human-Only Zone).
   - Enforces 7 automated checks:
     1. **`AGENTS.md` 24 KB Cap:** Fails if any `AGENTS.md` hits `>= 24,000` bytes.
     2. **UTF-8 Broken Wikilink Check:** Strips fenced/inline code blocks and verifies target resolution across active `.md` files.
     3. **YAML Frontmatter Schema:** Checks `title`, `last_updated`, `domain`, `status`, and `tags` on active content files.
     4. **Secret & PDPA Regex Guard + Lab Privacy Check:** Flags Google API keys, OpenAI/LLM keys, GitHub PATs, private key blocks, Thai National ID patterns, and verifies raw medical files under `lab_results/` are never tracked by Git.
     5. **`log.md` 40 KB Threshold:** Warns when `log.md` exceeds `40 KB` so rotation happens on time.
     6. **Root Cleanliness & `00-Inbox/` Pending Check.**
3. **PDPA-Safe Public GitHub Sync (`00-System/sync_til_public.ps1`):**
   - Filters strictly by frontmatter `status` (`ready-to-post`, `published`, `public`), supports `-DryRun` staging preview, aborts on sensitive patterns (API keys, Bearer tokens, National IDs, Thai mobile numbers, personal Gmail, Google Sheet IDs), and converts private Obsidian `Label` links into plain display labels.
4. **Skills & Obsidian Bases (`.base`):**
   - Created `morning-launch` and `weekly-mastermind` skills, upgraded `daily-wrap-up` to **English-First Portfolio Mode**, and built `00-System/AI_OS_Dashboard.base` and `03-Learning/TIL/TIL_Catalog.base` for dynamic vault querying.

---

## 3. Verified Proof of Work & Tech Stack
- **Tech Stack:** `PowerShell`, `Git`, `Obsidian Bases (.base)`, `Markdown YAML Frontmatter`, `Regex Security Guardrails`
- **Verified Outcomes:**
  - `log.md` compressed from `129.7 KB` to `~33 KB` via weekly archival (`2026-W46`, `2026-W47-part1`).
  - `health_check.ps1` runs deterministically in `backup.bat` before Git commit/push, protecting `00-Notes/` and blocking any secret/PII or broken wikilink regressions.
  - `sync_til_public.ps1` enforces status gating + secret/PDPA abort checks before pushing sanitized Markdown to public GitHub.
- **Internal References:** `health_check.ps1` • `sync_til_public.ps1` • `AI_OS_Dashboard.base` • `TIL_Catalog.base`

---

## 4. Learn in Public Copy Pack (Ready to Post)

### 💼 LinkedIn / 🐦 X Post (EN — Value-First & Authority)
```text
Prompts are probabilistic. Your workspace guardrails shouldn't be.

Today I upgraded my Markdown-based Personal AI OS with deterministic PowerShell QA and privacy gates before every backup and public GitHub sync:

1. Pre-backup validator (health_check.ps1):
- Enforces a <24 KB cap on every AGENTS.md entrypoint
- Resolves UTF-8 Obsidian wikilinks & YAML frontmatter schemas
- Scans for API keys, private keys, and PII patterns (while excluding my private notes folder)
- Flags log.md whenever it crosses 40 KB (just rotated mine from 129.7 KB -> ~33 KB)

2. Public TIL export gate (sync_til_public.ps1):
- Exports only status: ready-to-post / published notes
- Aborts on accidental emails, phone numbers, tokens, or Google Sheet IDs
- Sanitizes private wikilinks into clean Markdown labels

3. Obsidian Bases (.base):
- Replaced manual index maintenance with dynamic dashboard queries.

AI workflows scale much better when Markdown memory meets deterministic engineering checks.
```

### 📘 Facebook Page Summary (TH — Optional)
```text
กฎใน Prompt อย่างเดียวไม่พอ — ถ้าจะใช้ AI OS ทำงานจริงจัง ต้องมี Script คอยตรวจแบบ Deterministic ก่อน Backup และก่อน Publish เสมอ!

วันนี้ยกเครื่อง Personal AI OS เพิ่มด่านตรวจอัตโนมัติด้วย PowerShell:
1. health_check.ps1 (รันก่อน git backup ทุกครั้ง): เช็กขนาด AGENTS.md (< 24 KB), ตรวจ Wikilinks ขาด, เช็ก Frontmatter, สแกน API Key / PII และเตือนหมุนไฟล์ log.md เมื่อเกิน 40 KB (เพิ่งย่อจาก 129.7 KB เหลือ ~33 KB)
2. sync_til_public.ps1: กรองเฉพาะโน้ตสถานะพร้อมเผยแพร่ สแกนกันเบอร์/อีเมล/Token หลุด แล้วค่อยส่งขึ้น GitHub สาธารณะ
3. Obsidian Bases (.base): ดึงตาราง Dashboard แบบ Dynamic ไม่ต้องอัปเดตมือซ้ำหลายที่

สรุป: ให้ AI ช่วยคิดและลงมือทำ แล้วให้ Script เป็นคนเฝ้าประตูเรื่องความปลอดภัยและความสะอาดของระบบครับ
```

### 🎬 YouTube / Short Video Hook
- **Title Idea:** `Why Prompt Rules Aren't Enough: Adding Deterministic QA & PDPA Gates to My AI OS`
- **5-Second Hook:** *"Your AI agent will eventually leak a secret or break a link—unless you put a deterministic gate in front of Git."*

---

## 5. Rollup Sync Checklist
- [x] Updated `til_summary.md` (Portfolio Highlights & TIL Log)
- [x] Updated `README.md`
- [x] Updated `03-Learning/index.md` & `log.md`

