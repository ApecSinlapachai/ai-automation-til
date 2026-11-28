---
title: "TIL #029: Unified AI Engineering Portfolio Hub — File-Driven Markdown + Zod SSOT, Hydration-Safe EN/TH SSG & Guardrailed Copilot"
date: 2026-11-26
last_updated: 2026-11-26
domain: learning/til
category: ai-systems
status: ready-to-post
public_channels: [linkedin, x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, nextjs, zod, rag, i18n, guardrails, gemini, vercel]
---

# 🚀 TIL #029: Unified AI Engineering Portfolio Hub — File-Driven Markdown + Zod SSOT, Hydration-Safe EN/TH SSG & Guardrailed Copilot

> **🎯 Goal:** Document the architecture and production verification of [`apec-ai-portfolio`](https://apec-ai-portfolio.vercel.app/) — a `$0/month` Next.js 15 + Vercel Hobby unified AI Engineering Portfolio Hub powered by a single Zod-validated Markdown knowledge base that drives both bilingual (`EN/TH`) SSG case studies and a Zero-Cold-Start AI Portfolio Copilot, while proxying live RAG and Agent Ops demos same-origin.

---

## 1. TL;DR (English-First)
- **What I built:** Shipped [`apec-ai-portfolio`](https://apec-ai-portfolio.vercel.app/) on Next.js 15 (App Router, React 19, TypeScript 5.8, Tailwind CSS v4) + Vercel Hobby (`$0/mo`), unifying 2 live production AI backends (`/demos/rag-copilot`, `/demos/agent-ops`), bilingual (`EN/TH`) case studies, and a Zero-Cold-Start AI Portfolio Copilot (`/api/chat`) backed by a single Zod-validated Markdown Single Source of Truth (`content/`).
- **Why it matters:** Hosting separate AI demos across free-tier backends creates fragmented entry points, CORS friction, cold-start latency, and metric drift between UI copy and RAG knowledge bases. Combining a 1-file Markdown SSOT, hydration-safe bilingual SSG, Stage 0 `1 ms` prompt-injection blocking (`$0` LLM spend), and same-origin API proxies gives both global technical reviewers (EN) and local Thai HR/hiring managers (TH) one reliable front door.

---

## 2. Problem → Architecture & Implementation Patterns

### Engineering Bottlenecks Solved
1. **Fragmented Demos & Metric Drift:** Separate free-tier AI backends cause CORS issues, cold-start stalls, and stale numbers when UI case studies and chatbot knowledge bases are maintained separately.
2. **Next.js App Router SSG + Client i18n Hydration Mismatch:** Naively reading `localStorage` during initial render breaks SSR/client hydration on statically generated routes.
3. **Unintended Free-Tier Token Burn:** Auto-submitting preset prompt chips fires `/api/chat` requests before visitors read or customize their query, wasting free-tier LLM quotas.

### Production Patterns Shipped
1. **File-Driven Markdown + Zod Single Source of Truth (`content/`):**
   - Validated frontmatter (`ProjectFrontmatterSchema` with `LocalizedTextSchema: { en, th }`) using `gray-matter` + `Zod`. The exact same Markdown files power SSG pages (`/`, `/projects/[slug]`) and chunked retrieval for `/api/chat`, giving 1-file extensibility (`content/projects/<slug>.md`) for future projects.
2. **Hydration-Safe SSG + Client `EN/TH` i18n:**
   - Kept `page.tsx` routes as SSG Server Components and passed serialized bilingual props into thin client shells (`HomeView`, `ProjectDetailView`) bound to `LanguageContext`.
   - Initial SSR and first client render default to `"en"`, synchronized post-mount from `localStorage` alongside an inline `<head>` bootstrap script (`document.documentElement.lang` & `data-lang`) — achieving `0` hydration warnings with full reload persistence.
3. **Defense-in-Depth Copilot Guardrails (`/api/chat`):**
   - **Stage 0:** Deterministic prompt-injection gate blocks adversarial payloads in `~1 ms` with `$0` LLM spend.
   - **Stage 1:** Enforces PDPA and scope relevance, paired with IP rate limiting and tiered Gemini 3.x routing (`@google/genai`) that falls back cleanly to deterministic grounded citations when `GEMINI_API_KEY` is absent. Raised `maxOutputTokens` to `1200` to eliminate mid-sentence truncation on Thai multi-project summaries.
4. **Same-Origin Demo Proxy, Auto-Fill Chips & Theme Parity:**
   - Routed 100% of embedded demo traffic through Next.js rewrites (`/api/proxy/rag/*`, `/api/proxy/agent/*`) with `0` direct `*.onrender.com` browser calls.
   - Standardized all preset chips across `/`, `/chat`, `/projects/[slug]`, and both live demos to **Auto-Fill Only** (populate + focus input without auto-submitting).
   - Added `.demo-theme-scope` tokens for Dark/Light parity with `0` horizontal overflow at `390px` mobile width.

```typescript
// Sanitized, reusable production pattern: Hydration-safe Next.js App Router Language Context + Zod Localized Schema
import { z } from "zod";

export const LocalizedTextSchema = z.object({
  en: z.string().min(1),
  th: z.string().min(1),
});

// In LanguageProvider ("use client"):
// 1. Always initialize useState<Lang>("en") so SSR HTML matches first client render.
// 2. Sync saved preference from localStorage inside useEffect and update documentElement.lang.
// 3. Keep page.tsx as a Server Component that loads Zod-validated Markdown at build time,
//    then pass serialized bilingual props into a thin Client View component.
```

---

## 3. Verified Results & Proof of Work
- **Tech Stack:** `Next.js 15 (App Router)`, `React 19`, `TypeScript 5.8`, `Tailwind CSS v4`, `@google/genai`, `gray-matter`, `Zod`, `Vitest`, `Vercel Hobby`
- **Automated Verification (`npm run verify` — 4/4 Gates Passed):**
  - `eslint .`: `0` errors
  - `tsc --noEmit`: `0` errors
  - `vitest run`: `9/9` tests passed (Zod schema validation, Markdown chunking, guardrail checks)
  - `next build`: `12/12` routes generated cleanly (including `/icon.svg`)
- **Live Cloud & Browser QA (`https://apec-ai-portfolio.vercel.app/`):**
  - Verified across Desktop (`1440px`) and Mobile (`390px`), Dark & Light modes, and `EN`/`TH` reload persistence with `0` console/hydration errors, `0` failed network requests, and `0` direct `*.onrender.com` browser calls.
  - Verified Stage 0 `BLOCKED_INJECTION` live at `1 ms` latency and `$0` LLM spend; verified Auto-Fill Only chips trigger `0` unintended `/api/chat` POSTs.
- **Live Portfolio Hub:** [apec-ai-portfolio.vercel.app](https://apec-ai-portfolio.vercel.app/)
- **Internal References:** 🚀 All-In AI Engineer Roadmap • 🌐 Portfolio RAG & SOT Architecture • 📦 Archived Project Drop

---

## 4. Learn in Public Kit (Ready-to-Paste — Conversational Tech Builder Style)

### 💼 LinkedIn (EN — ใส่ลิงก์ใน First Comment)
```text
After shipping two live AI demos on free-tier backends (Hybrid RAG and a LangGraph Agent Ops control plane), I ran into an annoying recruiter UX problem. Sending three separate links meant people hit cold starts, CORS friction, and stale metrics whenever I updated one README but forgot another.

So I built a single Next.js 15 portfolio hub on Vercel Hobby ($0/mo) that wraps both live demos and adds a bilingual (EN/TH) AI Copilot. Two snags came up fast while building it:

1. Adding client-side EN/TH switching with localStorage broke SSR hydration on statically generated Next.js App Router pages.
2. Preset starter chips that auto-submit queries burned free-tier Gemini quota before visitors even finished reading the prompt.

Here is how I fixed both:
- Kept every page route as a Server Component loading one Zod-validated Markdown folder (content/). The same Markdown files render the SSG case studies and feed chunked context to /api/chat, so metrics stay in sync from a single file.
- Initialized LanguageContext to "en" on SSR and first client paint, then synced localStorage post-mount alongside an inline <head> bootstrap script. Reload persistence works with zero hydration warnings.
- Put a deterministic Stage 0 prompt-injection gate in front of /api/chat (blocks adversarial inputs in ~1 ms at $0 LLM spend), routed both live demos through same-origin /api/proxy/* rewrites (zero direct *.onrender.com browser calls), and changed all preset chips to auto-fill and focus only.

Final verification: 0 eslint errors, 0 TypeScript errors, 9/9 Vitest passed, 12/12 Next.js routes built, and clean Dark/Light QA on both 1440px desktop and 390px mobile.

Link to try the live hub and bilingual AI Copilot is in the first comment. If you host multiple free-tier AI demos, how are you handling cold starts and shared content updates?
```

**LinkedIn First Comment:**
```text
Live Portfolio Hub + Bilingual AI Copilot (no login): https://apec-ai-portfolio.vercel.app/
Live RAG Demo: https://apec-ai-portfolio.vercel.app/demos/rag-copilot
Live Agent Ops Demo: https://apec-ai-portfolio.vercel.app/demos/agent-ops
```

### 🐦 X (Twitter — EN · ใส่ลิงก์ใน Reply #1)
```text
Shipped a $0/mo Next.js 15 + Zod AI portfolio hub on Vercel Hobby to replace 3 separate free-tier demo links.

One Markdown folder (content/) drives both bilingual (EN/TH) SSG case studies and a Zero-Cold-Start /api/chat Copilot.

3 small engineering details that saved headaches:
1. Hydration-safe EN/TH toggle: default "en" on SSR, sync localStorage in useEffect + <head> script (0 hydration warnings).
2. Stage 0 prompt-injection gate blocks bad payloads in 1 ms at $0 LLM cost.
3. Same-origin /api/proxy/* rewrites (0 direct *.onrender.com calls) + Auto-Fill Only chips so visitors don't burn free-tier quota by accident.

Verified: eslint 0, tsc 0, 9/9 Vitest, 12/12 routes, 1440px + 390px Dark/Light QA.

Live link in reply.
```

**X Reply #1:**
```text
Try the live hub + bilingual AI Copilot (no login): https://apec-ai-portfolio.vercel.app/
```

### 📘 Facebook Page (TH — เจ้าของธุรกิจ / คนทำเว็บ · ใส่ลิงก์ในคอมเมนต์แรก)
```text
เวลาทำระบบ AI Demo หลายตัวบน Free Tier ปัญหาที่เจอบ่อยคือส่งให้ลูกค้าหรือ HR ดูแล้วต้องคลิกหลายลิงก์ บางตัวติดโหลดช้า (Cold Start) หรือข้อมูลหน้าเว็บกับข้อมูลที่แชตบอตตอบอัปเดตไม่ตรงกัน

วันนี้เลยรวมทุกอย่างไว้ในเว็บ Portfolio กลางตัวเดียวบน Next.js 15 + Vercel Hobby ($0/เดือน) ที่มีทั้งหน้า Case Study 2 ภาษา (EN/TH), Live Demo 2 ระบบ และ AI Copilot คอยตอบคำถามเรื่องผลงานได้ทันที

จุดที่ตั้งใจออกแบบเพื่อให้ใช้งานจริงได้ลื่นๆ มี 3 เรื่องครับ:
1. ใช้ไฟล์ Markdown + Zod ชุดเดียวเป็นศูนย์กลาง (Single Source of Truth) แก้ข้อมูลจุดเดียวอัปเดตทั้งหน้าเว็บและฐานความรู้ของ AI Copilot พร้อมกัน
2. ใส่ด่านกัน Prompt Injection ด่านแรกก่อนส่งเข้าโมเดล AI ตัดคำถามป่วนออกใน ~1 ms โดยไม่เสียค่า Token และปรับปุ่มคำถามตัวอย่างให้กดแล้วเติมข้อความลงช่องก่อน (ไม่กดส่งอัตโนมัติ) เพื่อช่วยคุมโควตา AI
3. ยิง API ไปยัง Demo เบื้องหลังผ่าน Proxy ในโดเมนเดียวกัน ลดปัญหาโดน Browser หรือเน็ตออฟฟิศบล็อก และเทสต์หน้าจอทั้งคอมและมือถือ (390px) ทั้ง Dark/Light Mode เรียบร้อย

ผ่านเทสต์อัตโนมัติครบ (9/9 Vitest, 12/12 routes, 0 console errors) แปะลิงก์ให้ลองกดเล่นแบบไม่ต้อง Login ไว้ในคอมเมนต์แรกครับ
```

**Facebook Page First Comment:**
```text
ลองกดเล่นเว็บ Portfolio Hub + AI Copilot 2 ภาษาได้ที่นี่ครับ: https://apec-ai-portfolio.vercel.app/
```

### 👥 Facebook Group (TH — สาย Dev / AI / Automation · แชร์ประสบการณ์ล้วน ไม่มีขายของ)
```text
แวะมาแชร์ 2 จุดที่เพิ่งเจอกับ Next.js 15 App Router ตอนทำเว็บ Portfolio 2 ภาษา (EN/TH) + AI Copilot บน Vercel Hobby ($0/เดือน) เผื่อใครกำลังทำเว็บ SSG ผสม AI อยู่ครับ

จุดแรกคือเรื่องสลับภาษา EN/TH ด้วย localStorage บนหน้า SSG ถ้าอ่านค่าตอน render รอบแรกเลยจะโดน React Hydration Mismatch ทันที วิธีที่จบสะอาดสุดคือคง page.tsx เป็น Server Component โหลด Markdown ที่ validate ด้วย Zod ไว้ตอน build ส่ง props สองภาษาเข้า Client View ที่ default เป็น "en" เหมือน SSR แล้วค่อย sync localStorage ใน useEffect คู่กับ inline script ใน <head> พอลองรีโหลดหน้าเว็บได้ 0 hydration warning เลย

จุดที่สองคือเรื่องกันโควตา Free Tier รั่ว ผมใส่ Stage 0 prompt-injection filter ดักก่อนถึง Gemini API (ตัดเคสป่วนใน ~1 ms, $0) เปลี่ยนปุ่มคำถามตัวอย่างทั้งหมดเป็น Auto-Fill Only ไม่ให้ยิง POST ทันทีที่คลิก และใช้ Next.js rewrites ทำ /api/proxy/* ยิงไปหา Render backend แทนการยิงข้ามโดเมนจาก browser ตรงๆ

เพื่อนๆ ที่ทำเว็บ AI บน Free Tier มีทริคคุมโควตากับจัดการ Cold Start กันยังไงบ้างครับ มาแลกเปลี่ยนกันได้
```

### 🎬 YouTube / Short Video Hook
- **Title Idea:** *"How I Built a $0/mo Bilingual AI Portfolio with One Markdown SSOT Powering Both SSG Pages and a Guardrailed AI Copilot (Next.js 15 + Zod + Gemini)"*
- **5-Second Hook:** *"Instead of sending recruiters to 3 slow free-tier links, I built one $0/mo Next.js 15 AI Portfolio Hub where a single Markdown file powers both bilingual case studies and a guardrailed AI Copilot that blocks prompt injections in 1 millisecond."*

