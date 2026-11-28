---
title: "TIL #025: All-In AI Engineer Portfolio Architecture — Sprint 0 Deterministic Framing & Thai-English Hybrid RAG Blueprint"
date: 2026-11-22
last_updated: 2026-11-22
domain: learning/til
category: ai-systems
status: ready-to-post
public_channels: [linkedin, x, facebook-page]
tags: [til, learn-in-public, portfolio, ai-engineer, rag, pgvector, deterministic-automation, eval]
---

# 🚀 TIL #025: All-In AI Engineer Portfolio Architecture — Sprint 0 Deterministic Framing & Thai-English Hybrid RAG Blueprint

> **🎯 Goal:** Document how auditing my live GitHub portfolio against 2027 `AI / AI Automation Engineer` hiring standards led to an honest Sprint 0 front-door rebrand and a validated `PostgreSQL 16 + pgvector` Hybrid RAG blueprint (`enterprise-rag-ops-copilot`).

---

## 1. TL;DR
- **What I built & shipped:** Audited `github.com/ApecSinlapachai` from a Hiring Manager's 15-second perspective, executed a **Sprint 0 rebrand** across 3 public repos (`ApecSinlapachai`, `fast-lead-automation`, `agentic-media-pipeline`), and designed + syntax-verified the **Project #1 kickoff blueprint** (`enterprise-rag-ops-copilot`: Thai-English Hybrid RAG + Dual-Stage Hallucination Refusal + Pydantic v2 Ticket Triage + 25-case Golden Eval Set).
- **Why it matters:** In 2026–2027, generic ChatGPT wrappers get filtered out immediately. Hiring teams look for engineers who know **when *not* to use AI** (deterministic reliability) and can prove **RAG accuracy, refusal discipline, latency, and token cost** with automated evaluation harnesses.

---

## 2. Problem → Architecture & Solution

- **Pain Point (Portfolio Gap Audit):**
  - My live GitHub had strong deterministic automation (`fast-lead-automation`, `85/85` offline tests) and 24 engineering notes (`ai-automation-til`), but:
    1. Profile README still said `GTM Engineer | AI Marketing Systems` instead of `AI & AI Automation Engineer`.
    2. `agentic-media-pipeline` had the word `agentic` in its slug while being a 421-line deterministic Python stdlib + FFmpeg CLI — a credibility risk when a Tech Lead reads the source code.
    3. Zero public repos demonstrated **Production RAG (`pgvector`)**, **LLM Evaluation**, **Docker**, or a **No-Login Live Web Demo**.
- **Architecture & Key Decisions:**
  1. **Sprint 0 Honest Framing (Pushed Live):**
     - Positioned `fast-lead-automation` around the thesis **"Knowing When *Not* to Use AI"** (`LockService` mutex, regex normalization, formula-injection sanitization, `85/85` offline tests, `$0/mo` SaaS overhead).
     - Added an explicit **Scope Transparency Note** to `agentic-media-pipeline` confirming zero LLM calls and exact verified benchmarks (`27:00 → 10:37`, `-41.8 dB → -20.1 dB`, `0` external Python dependencies).
     - Aligned `ApecSinlapachai/README.md` to **`AI & AI Automation Engineer`** with a clear pinning lifecycle.
  2. **Project #1 Blueprint (`enterprise-rag-ops-copilot`) — Ready for Implementation:**
     - **Thai-English Hybrid Search:** `PostgreSQL 16 + pgvector` (HNSW cosine index) combined with Full-Text Search (`tsvector` + `PyThaiNLP newmm` tokenization) merged via **Reciprocal Rank Fusion (RRF)** in SQL.
     - **Dual-Stage Hallucination Refusal:** Retrieval threshold gate + prompt-level refusal contract, exposed on the No-Login Next.js/Vercel demo via a 1-click **"Try Unanswerable Query"** button.
     - **Hardened Ticket Triage (`Pydantic v2`):** Strict schema (`StrictInt`, `allow_inf_nan=False`) extracting `intent`, `urgency_1_to_5`, `department`, `suggested_reply`, and `confidence`.
     - **25-Case Golden Eval Set:** Automated `pytest` harness measuring `Citation Accuracy`, `Refusal Accuracy`, `JSON Schema Pass Rate`, `P95 Latency`, and `Cost / 1,000 Requests` (including Thai, out-of-scope, and prompt-injection cases).

---

## 3. Verified Proof of Work & Tech Stack
- **Tech Stack:** `PostgreSQL 16 + pgvector` · `PyThaiNLP (newmm)` · `FastAPI` · `Pydantic v2` · `Pytest` · `Docker` · `Next.js (Vercel)` · `GitHub CLI (gh)` · `pglast` / `sqlglot`
- **Verified Results:**
  - **SQL DDL Validation:** `20` PostgreSQL 16 DDL statements + `1` Hybrid RRF query parsed cleanly with `pglast` (`libpg_query` PG16) and `sqlglot` (`EXIT:0`).
  - **Pydantic v2 Boundary Tests:** Passed strict boundary validation (`StrictInt`, `allow_inf_nan=False`).
  - **Live GitHub Verification:** `fast-lead-automation` verified `85/85` passing offline tests (`45 + 28 + 12`); `agentic-media-pipeline` verified `421` lines (`py_compile` & `--help` `EXIT:0`); Sprint 0 READMEs & repo metadata pushed live.
- **Internal References:** 🚀 All-In AI Engineer Roadmap • 📐 Project #1 Blueprint
- **Public Links:** `https://github.com/ApecSinlapachai`

> [!CAUTION] PDPA & Security Check
> - No API keys, tokens, personal phone numbers, or client PII included. All metrics verified against real scripts and parsers.

---

## 4. Learn in Public Pack (Ready-to-Paste)

### 💼 LinkedIn / 🐦 X (English — Authority & Proof-of-Work)
```text
Honest GitHub audit lesson for anyone targeting AI / AI Automation Engineer roles in 2027:

Calling a Python + FFmpeg CLI "agentic" when there's zero LLM loop inside doesn't impress Senior Engineers — it hurts credibility the second they open `scripts/`.

Today I ran a Sprint 0 overhaul on my public GitHub before building my next flagship repo:
1. Reframed `fast-lead-automation` around "Knowing When Not to Use AI" (LockService mutex, formula-injection guards, 85/85 offline tests, $0/mo overhead).
2. Added a Scope Transparency Note to `agentic-media-pipeline` (421-line 0-dep Python + FFmpeg CLI, 27:00 → 10:37, zero LLM hype).
3. Finalized & syntax-verified the blueprint for my next build (`enterprise-rag-ops-copilot`): PostgreSQL 16 + pgvector Hybrid Search (RRF + PyThaiNLP), a 1-click "Try Unanswerable Query" refusal test, Pydantic v2 triage, and a 25-case Golden Eval harness.

Building in public over the next 14 days — shipping the backend & pgvector schema tomorrow.
```

### 📘 Facebook Page (Thai — Optional Summary)
```text
บทเรียนจากการผ่าตัด GitHub ตัวเองก่อนสมัครงาน AI Engineer ปี 2027

สิ่งที่ทำให้ Tech Lead ปัดตกเร็วที่สุดไม่ใช่การเขียนโค้ดไม่เก่ง แต่คือการ "ตั้งชื่อเกินจริง" (เช่น ใส่คำว่า Agentic แต่ข้างในไม่มี LLM) กับการทำแชตบอตที่วัดความแม่นยำและคุมต้นทุนไม่ได้

วันนี้เลยเคลียร์หน้าบ้าน GitHub ใหม่หมดก่อนลุยโปรเจกต์ใหญ่:
1. ชูจุดขาย `fast-lead-automation` ว่ารู้ว่าเมื่อไหร่ *ไม่ต้อง* ใช้ AI (รันเทสต์ผ่าน 85/85 เคส ต้นทุน $0/เดือน)
2. แก้ README ของ `agentic-media-pipeline` ให้ตรงความจริง 100% ว่าเป็น Python + FFmpeg CLI
3. วาง Blueprint + ตรวจ Syntax SQL (`PostgreSQL 16 + pgvector`) ของโปรเจกต์ถัดไป `enterprise-rag-ops-copilot` ที่มีทั้ง Hybrid Search ไทย-อังกฤษ, ระบบปฏิเสธคำถามนอกเอกสาร (กัน AI มั่ว) และชุดเทสต์วัดผล 25 เคส

พรุ่งนี้เริ่มขึ้นโค้ดจริงวันแรกครับ!
```

---

## 5. Sync Checklist
- [x] Updated `til_summary.md`
- [x] Updated `03-Learning/index.md`
- [x] Logged in `log.md`

