---
title: "TIL #027: Production-Hardening a $0 Thai-English RAG Stack — Tiered Gemini 3.x Routing, Sync psycopg3 & 25/25 Live Eval"
date: 2026-11-24
last_updated: 2026-11-24
domain: learning/til
category: ai-systems
status: ready-to-post
public_channels: [linkedin, x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, rag, gemini, fastapi, supabase, pgvector, llm-routing, production]
---

# 🚀 TIL #027: Production-Hardening a `$0` Thai-English RAG Stack — Tiered Gemini 3.x Routing, Sync `psycopg3` & `25/25` Live Eval

> **🎯 Goal:** Document the production-hardening pass on [`enterprise-rag-ops-copilot`](https://github.com/ApecSinlapachai/enterprise-rag-ops-copilot) — turning an initial full-stack build into a quota-resilient, security-audited `$0` cloud deployment (Vercel + Render + Supabase) verified by `47/47` tests and a `25/25` live production Golden Eval Gate.

---

## 1. TL;DR (English-First)
- **What I built:** Hardened [`enterprise-rag-ops-copilot`](https://github.com/ApecSinlapachai/enterprise-rag-ops-copilot) for public no-login `$0` cloud traffic by adding **tiered Gemini 3.x model routing** (`Flash-Lite` default → automatic `3.8-Flash` escalation + deterministic `429` fallbacks), **sync `def` FastAPI routes** for `psycopg_pool`, and **Supabase Postgres `SET search_path = ''`** security fixes.
- **Why it matters:** Free-tier LLM quotas (`429 RESOURCE_EXHAUSTED`) and subtle backend pitfalls (blocking FastAPI's event loop with sync DB pools or mixing mock vs. live embedding spaces) break public AI demos in production. Separating fast-path vs. complex-path routing and verifying against a live cloud eval gate keeps latency, safety, and uptime predictable at `$0/month`.

---

## 2. Problem → Architecture & Hardening Patterns

### Production Bottlenecks Found
1. **Free-Tier Quota Walls (`429 RESOURCE_EXHAUSTED`):** Sending every routine FAQ query to the heaviest flagship model burns through daily RPD caps rapidly on a public no-login demo.
2. **Event-Loop Blocking & DB Security Linter Warnings:** Declaring FastAPI route handlers `async def` while calling synchronous `psycopg_pool` connections blocks the ASGI event loop; meanwhile, Supabase trigger functions without an explicit `SET search_path = ''` trigger `function_search_path_mutable` security warnings.
3. **Structured Output & Vector-Space Traps:** Open `dict[str, str]` fields in Pydantic models generate `additionalProperties`, which Gemini's `response_schema` rejects. Separately, running `ci_mock` embeddings against a live-embedded database creates vector-space mismatches.

### Hardening Patterns Shipped
1. **Tiered Gemini 3.x Routing & Deterministic Fallbacks:**
   - **Default Fast Path:** Routes standard queries to `gemini-3.5-flash-lite` → `gemini-3.1-flash-lite` → `gemini-3.8-flash`.
   - **Complexity Escalation:** Automatically escalates to `gemini-3.8-flash` first for complex multi-document, comparison, or gray-zone RAG queries and PDPA/multi-entity ticket triage.
   - **Graceful `429` Survival:** Falls back to deterministic grounded extractive RAG / rule-based triage so public demo UX never crashes even if daily quotas saturate.
2. **Gemini-Compatible Pydantic v2 Schema Wrapper:**
   - Replaced open `dict[str, str]` in `TriageTicketLLMOutput` with an explicit typed submodel (`TriageExtractedEntitiesLLM`) and mapped it back to `TriageTicketExtraction` after validation.
3. **FastAPI + Supabase + Public Demo Security Hardening:**
   - Converted DB-backed FastAPI routes to sync `def` so Starlette runs `psycopg_pool` calls in its worker threadpool without blocking the event loop.
   - Added `SET search_path = ''` to PostgreSQL trigger functions (`0` Supabase `function_search_path_mutable` warnings).
   - Paired a SHA-256 IP-hashed sliding-window rate limiter (`15 req/min`, `60 req/day`, bounded memory, CORS-exposed `Retry-After`) with a `/health` cold-start warm-up pattern for Render Free + Vercel Hobby.
   - Separated offline `ci_mock` CI checks from live production eval (`--mode live_gemini --no-ingest`) to prevent embedding vector-space drift.

```python
# Sanitized pattern: Gemini-compatible structured output without additionalProperties
class TriageExtractedEntitiesLLM(BaseModel):
    invoice_id: str | None = None
    org_id: str | None = None
    error_code: str | None = None
    sku: str | None = None
    incident_id: str | None = None

    def to_dict(self) -> dict[str, str]:
        raw = {
            "invoice_id": self.invoice_id,
            "org_id": self.org_id,
            "error_code": self.error_code,
            "sku": self.sku,
            "incident_id": self.incident_id,
        }
        return {k: v.strip() for k, v in raw.items() if isinstance(v, str) and v.strip()}
```

---

## 3. Verified Results & Proof of Work
- **Tech Stack:** `FastAPI`, `Pydantic v2`, `psycopg3 (psycopg_pool)`, `PostgreSQL 16 (pgvector + pg_trgm + tsvector)`, `PyThaiNLP (newmm)`, `Google Gemini 3.x`, `Next.js 15`, `Tailwind CSS`, `Vercel`, `Render`, `Supabase`
- **Verified Production Metrics (`2026-11-24`):**
  - **Static & Automated Tests:** `ruff` **0 lint errors**; `pytest` **47/47 passing** (42 unit + 3 live PG16 integration + 2 Golden Eval Gate tests); Next.js `tsc --noEmit` & `npm run build` clean.
  - **25-Case Golden Eval Gate:**
    - `ci_mock`: **25/25** (`1.0000` Citation Hit Rate, Refusal Precision, Injection Block Rate, JSON Schema Pass Rate; Avg `36 ms` / P95 `48 ms`).
    - **Live Production (`production-live-38`):** **25/25** (`1.0000` across all 4 quality/safety rates; Avg `767 ms` / P95 `1516 ms`).
  - **Live Cloud Smoke Test:** Vercel (`200 OK`), Render `/health` (`database_connected: true`), Supabase (`12` active docs / `52` chunks, `0` `function_search_path_mutable` warnings), and Ticket Triage returning `pydantic_passed` (`P0`, `technical_incident`).
- **Public Repository:** [github.com/ApecSinlapachai/enterprise-rag-ops-copilot](https://github.com/ApecSinlapachai/enterprise-rag-ops-copilot)
- **Internal References:** 📘 Project #1 Quick Rundown • 📐 Project #1 Blueprint • 📦 Archived Project Drop

---

## 4. Learn in Public Kit (Ready-to-Paste)

### 💼 LinkedIn / 🐦 X Post (EN — Authority & Value-First)
```text
Shipping a RAG demo on a $0 stack is easy. Keeping it alive under public traffic without hitting quota walls or leaking hallucinations is the real engineering work.

I just finished the production-hardening pass on `enterprise-rag-ops-copilot` (Vercel Next.js 15 + Render FastAPI + Supabase PG16 pgvector):

1. Tiered Gemini 3.x routing: Fast-path routine queries go to Flash-Lite first (`3.5-flash-lite` -> `3.1-flash-lite` -> `3.8-flash`), while complex multi-doc / PDPA triage escalates straight to `3.8-flash` — with deterministic grounded fallbacks on `429`.
2. $0 pre-LLM guardrails: Bilingual prompt-injection and low-evidence refusal gates block bad queries before touching the LLM.
3. Backend & DB hygiene: Sync `def` FastAPI routes so `psycopg_pool` never blocks the event loop, explicit Pydantic entity submodels (no `additionalProperties` rejections), and `SET search_path = ''` on Postgres trigger functions (0 Supabase linter warnings).
4. Live Cloud Verification: 47/47 pytest passed, 25/25 Live Production Golden Eval passed (100% citation hit, refusal precision, injection block & JSON schema pass; 767ms avg / 1516ms P95).

Repo: https://github.com/ApecSinlapachai/enterprise-rag-ops-copilot
```

### 📘 Facebook Page Summary (TH — Conversational Tech Builder)
```text
ทำเดโม RAG บน Free Tier งบ $0 ให้รันขึ้นไม่ยาก แต่ทำให้ "รอดจริงตอนคนนอกกดเล่น" โดยไม่ติดโควตา 429 และไม่ตอบมั่ว คืองานวิศวกรรมของจริงครับ

วันนี้เพิ่งเก็บงาน Production Hardening ให้โปรเจกต์ `enterprise-rag-ops-copilot` (Vercel + Render + Supabase PostgreSQL 16 `pgvector`) เสร็จครบชุด:
1. Tiered Gemini 3.x Routing: คำถามทั่วไปยิงเข้า Flash-Lite ก่อนเพื่อประหยัดโควต้า ส่วนเคสเปรียบเทียบหลายเอกสารหรือเคส PDPA ซับซ้อนค่อยส่งขึ้น 3.8-Flash พร้อม Fallback แบบ Deterministic กันระบบล่มเวลาเจอ 429
2. Guardrail 3 ด่าน: ดัก Prompt Injection และคำถามนอกเอกสารตั้งแต่ก่อนยิง LLM (เสียค่า Token $0)
3. อุดรูรั่วหลังบ้าน: เปลี่ยน Route ที่เรียก `psycopg_pool` เป็น sync `def` ไม่ให้บล็อก Event Loop, แยก Pydantic submodel กัน Gemini ตีกลับ `additionalProperties`, และใส่ `SET search_path = ''` ให้ฟังก์ชันใน Supabase จน Security Warning เป็น 0
4. ผลเทสต์จริงบน Cloud: `pytest` ผ่าน 47/47 เคส และชุดข้อสอบ Golden Eval บน Production ผ่าน 25/25 (Citation/Refusal/Injection/JSON 100% ที่ความเร็วเฉลี่ย 767 ms)

GitHub: https://github.com/ApecSinlapachai/enterprise-rag-ops-copilot
```

### 🎬 YouTube / Short Video Hook
- **Title Idea:** *"How I Built a Zero-Hallucination Thai–English Hybrid RAG Copilot on a $0 Stack (with a 25-Case Eval Gate)"*
- **5-Second Hook:** *"Most portfolio RAG apps break the moment a recruiter tests them—either they hallucinate, fail on Thai error codes, or hit a 429 quota wall. Here's how I hardened a $0 Postgres Hybrid RAG stack to pass 25/25 live production eval cases."*

