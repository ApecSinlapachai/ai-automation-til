---
title: "TIL #023: YouTube Repurpose Pipeline MVP (Gemini Interactions API + FFmpeg/libass), Client Project Bridge & 3-Track NOC STEM Roadmap"
date: 2026-11-21
last_updated: 2026-11-21
domain: learning/til
category: workflows-ops
status: ready-to-post
public_channels: [linkedin, x, facebook-page]
tags: [til, python, ffmpeg, gemini-api, ai-os, skills, career-roadmap, noc-stem]
---

# 🚀 TIL #023: YouTube Repurpose Pipeline MVP, Client Project Bridge & 3-Track NOC STEM Roadmap

> **🎯 Goal:** Ship a zero-SaaS-cost Python + Gemini + FFmpeg video repurposing pipeline (`14/14` tests), formalize PDPA-safe client project ingestion into Personal AI OS, and align technical upskilling with Canada's Express Entry STEM / Global Talent Stream NOC requirements.

---

## 1. TL;DR — What I Built & Learned Today
- **Built:**
  1. **YouTube Repurpose Pipeline MVP (`yt-repurpose-pipeline`):** Python + `google-genai 3.6.1` (`gemini-3.8-flash` Interactions API) + FFmpeg/libass Thai-aware `.ass` 9:16 Shorts Clipper + Auto-Highlight + `2x2` Contact Sheet visual QA (`14/14` unit tests passing).
  2. **Unified AI OS Skills (`client-project-bridge` & `daily-wrap-up`):** Created an anonymized, PDPA/NDA-safe bridge (`00-Inbox/client-drops/`) for ingesting standalone client projects into AI OS, and consolidated `summary-bridge-sync`, `client-project-bridge`, and `github-portfolio-sync` into one `daily-wrap-up` workflow.
  3. **3-Track Canada NOC STEM Roadmap & Job Sprint:** Expanded job targeting beyond narrow `"Automation"` titles into 3 high-volume NOC STEM tracks (`Business Systems / IT BA [NOC 21221/21222]`, `Full-Stack Web & AI Dev [NOC 21234/21232]`, `Data & BI Analyst [NOC 21223/21211]`) while shipping 3 more Plan B applications today (`8` total in Week 47).

---

## 2. Problem → Architecture & Solutions

- **Pain Point 1 — Manual Short-Form Repurposing & Thai Subtitle Clipping:**
  - Manually cutting 16:9 YouTube videos into 9:16 vertical Shorts with readable Thai captions takes hours per episode.
  - **Solution:** Built a local Python pipeline using the Gemini Interactions API for highlight extraction and FFmpeg + `libass` Thai-aware `.ass` subtitle rendering, plus a `2x2` contact sheet generator for deterministic visual QA before posting.
- **Pain Point 2 — Siloed External Client Work & Fragmented Wrap-Up Skills:**
  - Working on standalone client repos outside Personal AI OS risked losing architectural lessons or leaking client PII if copied naively.
  - **Solution:** Created the `client-project-bridge` skill enforcing strict PDPA/NDA anonymization before writing to `00-Inbox/client-drops/`, and unified end-of-day triage, TIL generation, Summary Bridge sync, and conditional GitHub syncing into a single `daily-wrap-up` skill.
- **Pain Point 3 — Narrow Job Search Funnel vs. Enterprise Tech Requirements:**
  - Searching only `"Automation"` titles narrows local openings, while enterprise recruiters also check for `PostgreSQL/SQL`, live `Next.js/FastAPI` web apps, and `pgvector` RAG proof.
  - **Solution:** Codified the **3-Track NOC STEM & Learn-by-Building Guardrail**—keeping Block 1 mornings locked to daily job applications (`2–3/day`) while dedicating Block 2 afternoons to building production `SQL + FastAPI/Next.js + RAG` flagship repos.

---

## 3. Verified Proof of Work & Metrics
- **Tech Stack:** `Python`, `google-genai 3.6.1`, `FFmpeg / libass`, `PowerShell`, `Agent Skills Architecture`
- **Verified Results:**
  - `yt-repurpose-pipeline`: **`14/14` unit tests passed** + Thai `.ass` smoke tests verified
  - Job Search Velocity (`2026-W47`): **`8` applications sent** (`+3` today: Hy Digital, Deloitte Thailand Tier 1, OxygenAI)
  - AI OS Hygiene: `health_check.ps1` passing with `0` blockers (`113` active markdown files scanned)

---

## 4. Learn in Public Pack (Ready-to-Paste)

### 💼 LinkedIn / 🐦 X Post (EN — Value-First & Authority)
```text
Built 2 developer-ops upgrades today while keeping my morning job sprint moving:

1. Local YouTube-to-Shorts Repurpose Pipeline (Python + Gemini Interactions API + FFmpeg/libass): extracts highlights, renders Thai-aware 9:16 `.ass` captions, and outputs a 2x2 contact sheet for fast visual QA (14/14 unit tests passing).
2. PDPA-Safe Client Project Bridge: an agent skill that anonymizes lessons learned from standalone client repos and syncs sanitized technical insights directly into my Personal AI OS and public TIL log.

Shipping small, tested tools daily compounds fast.

Full TIL log: https://github.com/ApecSinlapachai/ai-automation-til
#AIEngineering #Python #FFmpeg #GeminiAPI #BuildInPublic
```

