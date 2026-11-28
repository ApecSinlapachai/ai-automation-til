---
title: "TIL #028: Production AI Agent Ops — LangGraph State Machine, PDPA Redaction, HITL/DLQ & Same-Origin Proxy"
date: 2026-11-25
last_updated: 2026-11-25
domain: learning/til
category: ai-systems
status: ready-to-post
public_channels: [linkedin, x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, ai-agents, langgraph, pdpa, hitl, dlq, nextjs, fastapi, postgresql, gemini]
---

# 🚀 TIL #028: Production AI Agent Ops — LangGraph State Machine, PDPA Redaction, HITL/DLQ & Same-Origin Proxy

> **🎯 Goal:** Document the architecture and production verification of [`production-ai-agent-ops`](https://github.com/ApecSinlapachai/production-ai-agent-ops) — an enterprise-grade State-Machine AI Operations Agent with pre-LLM PDPA PII redaction, PostgreSQL `FOR UPDATE`-locked HITL approval and DLQ replay, tiered Gemini routing (`62.78%` cost reduction), and a Next.js `rewrites()` same-origin proxy that prevents Brave Shields/AdBlocker/firewall failures on `$0` Vercel + Render deployments.

---

## 1. TL;DR (English-First)
- **What I built:** Shipped and verified [`production-ai-agent-ops`](https://github.com/ApecSinlapachai/production-ai-agent-ops) live on a `$0/month` cloud stack (Vercel Hobby + Render Free + Supabase PostgreSQL 16) with a SQL-first `LangGraph` state machine, deterministic TH/EN PDPA redaction, `FOR UPDATE`-locked HITL/DLQ gates, tiered Gemini 3.x routing, `n8n` webhook workflows, and a Next.js same-origin `/backend/:path*` proxy.
- **Why it matters:** Most AI agent demos leak PII into LLM logs, double-execute side effects on retries, overspend on frontier models, and fail silently in recruiter/client browsers when cross-origin requests to `*.onrender.com` get blocked by Brave Shields, AdBlockers, or corporate firewalls. Solving both backend reliability and browser-side proxy resilience makes public `$0` AI demos production-grade.

---

## 2. Problem → Architecture & Implementation Patterns

### Engineering Bottlenecks Solved
1. **Agent Reliability & Compliance:** Opaque prompt chains lack deterministic state persistence, leak Thai/EN PII into LLM/DB logs, duplicate side effects on webhook retries, and lack bounded retry + Dead-Letter Queue (DLQ) recovery.
2. **Free-Tier Split-Domain Friction:** Cross-origin browser calls from Vercel to `*.onrender.com` are frequently blocked by Brave Shields, AdBlockers, and corporate firewalls; sending `Content-Type: application/json` on `GET /health` triggers unnecessary CORS preflights; and Render Free services sleep after inactivity (30–50s cold starts).

### Production Patterns Shipped
1. **SQL-First State Machine (`LangGraph` + PostgreSQL 16):**
   - Explicit `StateGraph` transitions (`RECEIVED` → `PII_REDACTED` → `INTENT_ROUTED` → `CONTEXT_FETCHED` → `DRAFT_GENERATED` → `AWAITING_APPROVAL` / `EXECUTING_ACTION` → `COMPLETED` / `FAILED_RETRYING` → `DLQ`) persisted to `agent_runs` and `agent_state_transitions` via PgBouncer-safe `psycopg_pool` (`prepare_threshold=None`).
2. **Pre-LLM PDPA Redaction Boundary:**
   - Deterministic regex + structured redactor strips Thai National IDs, Thai phone numbers, emails, bank accounts, and personal names before any LLM call or DB insert (persisting `redacted_input` + `redaction_summary` only).
3. **HITL Gate, Idempotency & DLQ Replay:**
   - High-impact or low-confidence actions pause at `AWAITING_APPROVAL` with a structured `action_preview` under PostgreSQL `FOR UPDATE` row locking; exhausted retries move to `dead_letter_jobs` for `FOR UPDATE`-locked manual replay (`POST /api/v1/dlq/{job_id}/replay`).
   - `Idempotency-Key` deduplication (`INSERT ... ON CONFLICT`, `X-Idempotency-Replayed: true`) prevents duplicate side effects.
4. **Tiered Gemini 3.x Routing & Public Sandbox Security:**
   - Routes low-complexity tasks to `gemini-3.5-flash-lite` (`FAST`) and complex reasoning to `gemini-3.8-flash` (`COMPLEX`), with `429` fallback to `gemini-3.1-flash-lite` and deterministic offline fallback.
   - `AUTH_MODE=demo_sandbox` (zero secrets in browser), SHA-256 IP-hashed sliding-window rate limiting (`429` + `Retry-After`), and `X-Webhook-Secret`-protected `/webhooks/n8n`.
5. **Same-Origin Next.js `rewrites()` Proxy + Conditional `Content-Type` + Cold-Start Polling:**
   - Proxies `/backend/:path*` to `API_PROXY_TARGET` so all browser traffic stays same-origin (`0` direct `*.onrender.com` calls), sets `Content-Type: application/json` only when a request body exists, and polls `/backend/health` (10×5s ~50s) with `useEffect` cleanup.

```ts
// Sanitized, reusable production pattern (Zero client secrets/PII)
// 1) next.config.ts — Same-origin proxy to upstream API
const nextConfig = {
  reactStrictMode: true,
  async rewrites() {
    const target = (
      process.env.API_PROXY_TARGET ||
      process.env.NEXT_PUBLIC_API_BASE_URL ||
      "http://localhost:8000"
    ).replace(/\/+$/, "");

    return [{ source: "/backend/:path*", destination: `${target}/:path*` }];
  },
};

// 2) lib/api.ts — Same-origin fetch with conditional JSON Content-Type
export const API_PROXY_PREFIX = (process.env.NEXT_PUBLIC_API_PROXY_PREFIX ?? "/backend").replace(/\/+$/, "");

async function requestJson<T>(path: string, init?: RequestInit): Promise<T> {
  const headers = new Headers(init?.headers);
  if (init?.body !== undefined && init?.body !== null && !headers.has("Content-Type")) {
    headers.set("Content-Type", "application/json");
  }
  const response = await fetch(`${API_PROXY_PREFIX}${path}`, { ...init, headers, cache: "no-store" });
  if (!response.ok) throw new Error(`Request failed (${response.status})`);
  return (await response.json()) as T;
}
```

---

## 3. Verified Results & Proof of Work
- **Tech Stack:** `Next.js 15 (App Router, TypeScript, Tailwind CSS v4)`, `FastAPI`, `Pydantic v2`, `LangGraph`, `PostgreSQL 16 (psycopg3 / Supabase)`, `Google GenAI (Gemini 3.x)`, `n8n`, `Docker`, `GitHub Actions`, `Vercel`, `Render`
- **Benchmark Evaluation (`eval/run_benchmark.py` across 24 synthetic TH/EN cases — 16 `FAST`, 8 `COMPLEX`, 32 PII entities):**
  - **Tiered Model Routing Cost Reduction:** `62.78%` (`$0.1211` vs. `$0.3254` estimated USD per 1,000 runs)
  - **PDPA PII Leak Rate:** `0.00` (`100%` masked before LLM/DB)
  - **HITL Policy Enforcement Rate:** `1.00`
  - **Intent & Routing Accuracy:** `100.00%`
- **Automated Test & CI Verification (`2026-11-25`):**
  - `ruff`: `0` errors across `app`, `tests`, `eval`
  - `pytest`: `40 passed` (`31` unit + `7` live PostgreSQL 16 integration + `2` eval gates)
  - Frontend & Infra: `npx tsc --noEmit && npm run build` exit `0`; `docker compose config -q` exit `0`; GitHub Actions CI (`backend-ci` + `frontend-ci`) green.
- **Live Cloud E2E Verification (`https://production-ai-agent-ops.vercel.app/`):**
  - Verified `ONLINE • PG16 CONNECTED` on Supabase Free + Render Free + Vercel Hobby (`AUTH_MODE=demo_sandbox`), all 6 E2E flows passing, `docs/assets/demo-60s.gif` (`1280x720`) generated, and `0` direct browser calls to `*.onrender.com` (all `/backend/*` endpoints returned `200`).
- **Live Demo:** [production-ai-agent-ops.vercel.app](https://production-ai-agent-ops.vercel.app/)
- **Public Repository:** [github.com/ApecSinlapachai/production-ai-agent-ops](https://github.com/ApecSinlapachai/production-ai-agent-ops)
- **Internal References:** 🚀 All-In AI Engineer Roadmap • 📦 Archived Project Drop

---

## 4. Learn in Public Kit (Ready-to-Paste)

### 💼 LinkedIn / 🐦 X Post (EN — Authority & Value-First)
```text
Most AI agent demos fail the moment they touch real operations—leaking PII into LLM logs, double-firing webhooks on retries, burning token budgets, or breaking in corporate browsers.

I just shipped `production-ai-agent-ops` on a $0 stack (Next.js 15 + FastAPI + LangGraph + Supabase PG16 + n8n) to solve all four:

1. SQL-first LangGraph state machine + `FOR UPDATE` row locking: Explicit state transitions persisted in PostgreSQL 16, pausing high-risk actions at a Human-in-the-Loop (HITL) approval gate and moving exhausted retries to a replayable Dead-Letter Queue (DLQ).
2. Pre-LLM PDPA PII redaction: Strips Thai National IDs, phone numbers, emails, bank accounts, and names before any LLM call or DB insert (`0.00` PII leak rate across 32 entities).
3. Tiered Gemini 3.x routing: Routes routine tasks to `gemini-3.5-flash-lite` and complex reasoning to `gemini-3.8-flash` — cutting estimated cost by `62.78%` ($0.1211 vs. $0.3254 per 1,000 runs) at `100%` routing accuracy.
4. Browser-resilient $0 demo proxy: A 10-line Next.js `rewrites()` same-origin `/backend/:path*` proxy eliminates Brave Shields/AdBlocker blocks on `*.onrender.com` and unnecessary CORS preflights.

Verified: 40/40 pytest passed, 24/24 benchmark cases passed, live E2E green.
Live Demo (no login): https://production-ai-agent-ops.vercel.app/
```

### 📘 Facebook Page Summary (TH — Conversational Tech Builder)
```text
ทำ AI Agent เดโมให้ตอบได้ไม่ยาก แต่ทำให้ "เอาไปรันงานองค์กรจริงแล้วไม่พัง" มี 4 จุดที่คนมักพลาด: ข้อมูลลูกค้าหลุดเข้า LLM, Webhook ยิงซ้ำแล้วทำรายการเบิ้ล, ค่า Token บาน และหน้าเว็บเดโมพังบน Brave/เน็ตออฟฟิศ

วันนี้เพิ่งปล่อยโปรเจกต์ Flagship ชิ้นที่ 2 `production-ai-agent-ops` (Next.js 15 + FastAPI + LangGraph + PostgreSQL 16 + n8n) บนงบคลาวด์ $0/เดือน เก็บครบทั้ง 4 จุด:
1. State Machine บน PostgreSQL 16 + `FOR UPDATE`: บังคับทุกงานความเสี่ยงสูงหยุดรอด่านอนุมัติคน (HITL) และถ้ายิงพังเกินโควตาจะโยนเข้า Dead-Letter Queue (DLQ) ให้กด Replay ซ้ำได้ไม่หล่น
2. ด่านเซ็นเซอร์ PDPA ก่อนถึง LLM: ตัดเลขบัตรประชาชน เบอร์โทร อีเมล เลขบัญชี ออกก่อนส่งเข้าโมเดลและฐานข้อมูล (ผลเทสต์ PII Leak Rate = 0.00)
3. Tiered Model Routing ลดค่า AI ลง 62.78%: งานง่ายส่ง `gemini-3.5-flash-lite` งานซับซ้อนส่ง `gemini-3.8-flash` ($0.1211 vs $0.3254 ต่อ 1,000 รัน ที่ความแม่นยำ 100%)
4. Same-Origin Proxy ด้วย Next.js `rewrites()`: แก้ปัญหา Brave Shields / AdBlock / Firewall บล็อกโดเมน `*.onrender.com` ทำให้หน้าเดโมยิง `/backend/*` ผ่านโดเมนเดียวกัน 100% พร้อมระบบรอ Cold Start อัตโนมัติ

ผ่านเทสต์อัตโนมัติ 40/40 (`pytest`) + CI เขียว + ลองกดเล่น Live Demo แบบไม่ต้อง Login ได้เลยครับ:
https://production-ai-agent-ops.vercel.app/
```

### 🎬 YouTube / Short Video Hook
- **Title Idea:** *"Why Your Vercel + Render AI Demo Breaks on Brave & Corporate Networks (and How a 10-Line Next.js Proxy Fixes It)"*
- **5-Second Hook:** *"Your AI agent demo works on your laptop, then breaks the second a recruiter opens it on Brave or an office network. Here's how I built a $0 State-Machine AI Agent with 62.78% lower LLM cost, 0 PDPA leaks, and a 10-line Next.js proxy that never gets blocked."*

