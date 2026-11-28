---
title: "TIL #026: Enterprise RAG Ops Copilot — Thai-English Hybrid Search (pgvector + PyThaiNLP + pg_trgm), 3-Stage $0 Guardrails & 25-Case CI Eval Gate"
date: 2026-11-23
last_updated: 2026-11-23
domain: learning/til
category: ai-systems
status: ready-to-post
public_channels: [linkedin, x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, rag, pgvector, pythainlp, fastapi, guardrails, llm-eval]
---

# 🚀 TIL #026: Enterprise RAG Ops Copilot — Thai-English Hybrid Search (`pgvector` + `PyThaiNLP` + `pg_trgm`), 3-Stage `$0` Guardrails & 25-Case CI Eval Gate

> **🎯 Goal:** Turn the completed initial build of [`enterprise-rag-ops-copilot`](https://github.com/ApecSinlapachai/enterprise-rag-ops-copilot) into an English-first engineering Proof of Work demonstrating bilingual Hybrid RAG, deterministic anti-hallucination guardrails, Gemini-safe Pydantic v2 structured outputs, and automated CI evaluation on a `$0` free-tier stack.

---

## 1. TL;DR (English-First)
- **What I built:** Shipped the full-stack initial build of [`enterprise-rag-ops-copilot`](https://github.com/ApecSinlapachai/enterprise-rag-ops-copilot) — a bilingual Thai–English Hybrid RAG Ops Copilot and Guarded Ticket Triage API (`Next.js 15` on Vercel → `FastAPI` on Render → `PostgreSQL 16` with `pgvector` + `pg_trgm` on Supabase/Docker) backed by a 25-case Golden Eval Gate in GitHub Actions.
- **Why it matters:** Vector-only RAG misses exact alphanumeric support identifiers (`INV-2026-0091`, `ERR-AUTH-403`), while standard Postgres FTS breaks on unspaced Thai text. Combining `PyThaiNLP` (`newmm`) + `pg_trgm` + `pgvector` RRF with `$0` pre-LLM refusal/injection gates solves both retrieval precision and hallucination/cost control.

---

## 2. Problem → Architecture & Implementation

### Pain Points & Engineering Constraints
1. **Unspaced Thai FTS & Parameter Safety:** Standard PostgreSQL `simple` FTS cannot tokenize Thai strings; strict `AND` `tsquery` collapses on multi-word Thai queries; and literal `%` wildcards in `ILIKE` collide with `psycopg3` parameter placeholders.
2. **Matryoshka Vector & RRF Similarity Pitfalls:** Truncating `gemini-embedding-001` to `1536` dims breaks unit L2 norm; zero-norm vectors cause `NaN` in `<=>` cosine distance; and FTS-only RRF winners outside the top-20 vector CTE falsely report `vec_similarity = 0.0` unless recomputed in the final projection.
3. **LLM Structured Output & Free-Tier Demo Exposure:** Pydantic `dict[str, str]` generates JSON Schema `additionalProperties`, which `google-genai` structured output rejects; meanwhile, a public no-login demo requires `$0` short-circuiting and strict IP rate limiting to protect quota.

### What I Built & Hardened
1. **Hybrid RRF + Exact Code Boost (`sql/002_hybrid_search_query.sql`, `tokenizer.py`, `embeddings.py`):**
   - Truncates `gemini-embedding-001` to `1536` dims and enforces finiteness + non-zero L2 norm in Python (`l2_normalize_vector`) before `<=>`.
   - Segments Thai text with pre-warmed `PyThaiNLP` (`newmm`), strips bilingual + B2B SaaS domain stopwords, builds `OR`-joined `tsquery`, and passes regex-escaped SKU/error tokens as `$3` for `pg_trgm` (`~*` GIN trigram) `+0.25` boost without `%` literals.
   - Fuses top-20 vector and top-20 FTS candidates via RRF (`0.6 / (60.0 + vec_rank) + 0.4 / (60.0 + fts_rank_pos)`) and recomputes exact `vec_similarity` for FTS-only winners in the final `LIMIT 5` projection.
2. **3-Stage Anti-Hallucination & Security Guardrails (`guardrails.py`):**
   - *Stage 0 (Injection Gate, `$0` LLM spend):* Bilingual regex screening blocks EN/TH prompt-injection and secret-exfiltration attempts (`BLOCKED_INJECTION`).
   - *Stage 1 (Pre-LLM Retrieval Gate, `$0` LLM spend):* Short-circuits before calling the LLM if `chunks_found == 0` or (`max_vec_similarity < 0.58` and `max_fts_rank < 0.08`) (`REFUSED_LOW_EVIDENCE`).
   - *Stage 2 (Post-LLM Groundedness Gate):* Filters `cited_chunk_ids` against retrieved PostgreSQL UUIDs and enforces `confidence_score >= 0.55`; overrides ungrounded answers with a localized deterministic refusal and `citations = []`.
3. **Gemini-Safe Pydantic v2 Triage & Public Demo Rate Limiter (`triage.py`, `rate_limiter.py`):**
   - Replaced `dict[str, str]` with an explicit `ExtractedEntitiesLLM` model (`invoice_id`, `org_id`, `error_code`, `sku`, `incident_id`) to eliminate `additionalProperties`, then mapped back to SQL-aligned `TriageTicketExtraction` with `tenacity` retry + deterministic fallback.
   - Added a thread-safe SHA-256 IP-hashed sliding-window rate limiter (`15 req/min`, `60 req/day`, bounded `10,000` keys with stale eviction) exposing `Retry-After` via CORS, plus `/health` cold-start warm-up pings.

```python
# Production patterns: Matryoshka L2 guard & Gemini-safe structured output schema
def l2_normalize_vector(values: Sequence[float], expected_dim: int = 1536) -> list[float]:
    if len(values) != expected_dim:
        raise ValueError(f"Expected {expected_dim} dims, got {len(values)}")
    floats = [float(v) for v in values]
    if not all(math.isfinite(v) for v in floats):
        raise ValueError("Non-finite embedding value encountered")
    norm = math.sqrt(sum(v * v for v in floats))
    if norm <= 1e-12:
        raise ValueError("Zero-norm embedding vector cannot be normalized for cosine similarity")
    return [v / norm for v in floats]

class ExtractedEntitiesLLM(BaseModel):
    """Explicit entity fields for structured output (avoids dict/additionalProperties)."""
    invoice_id: str | None = None
    org_id: str | None = None
    error_code: str | None = None
    sku: str | None = None
    incident_id: str | None = None
```

---

## 3. Verified Results & Proof of Work
- **Tech Stack:** `FastAPI`, `Pydantic v2`, `PostgreSQL 16 (pgvector, pg_trgm, tsvector)`, `psycopg3`, `PyThaiNLP (newmm)`, `google-genai`, `Next.js 15`, `Tailwind CSS`, `Docker`, `GitHub Actions`
- **Verified Metrics (Synthetic 12-Doc / 52-Chunk B2B SaaS Corpus):**
  - **Static & Test Suite:** `ruff check` **0 errors**; `pytest -v` **37 / 37 passed** (32 unit tests + 3 live PostgreSQL 16 `pgvector`/RRF/RAG integration tests + 2 Golden Eval Gate tests).
  - **25-Case Golden Eval Gate (6 TH, 5 EN, 4 Mixed, 4 Out-of-Scope, 3 Injection, 3 Triage):**
    - `ci_mock`: **25/25 passed** — Citation Hit Rate `1.0000`, Refusal Precision `1.0000`, Injection Block Rate `1.0000`, JSON Schema Pass Rate `1.0000`, Avg/P95 Latency `36 ms / 48 ms`.
    - `live_gemini` baseline (Supabase live-embedded corpus): **25/25 passed**, Avg/P95 Latency `548 ms / 834 ms`.
  - **Live Stack Smoke Check:** Vercel (`Next.js 15`) + Render (`FastAPI`) + Supabase (`pgvector`) verified HTTP `200` across `/health`, `/api/v1/rag/query`, `/api/v1/triage`, `/api/v1/eval/latest`, and `/api/v1/documents`.
  - **Open Follow-Ups (Bug-Fix & Polish Pass):** Rotate/verify `GEMINI_API_KEY` quota on Render (`429` fallback verified graceful) and polish minor frontend markdown/placeholder formatting.
- **Public Repository:** [github.com/ApecSinlapachai/enterprise-rag-ops-copilot](https://github.com/ApecSinlapachai/enterprise-rag-ops-copilot)
- **Internal References:** 📐 Project #1 Blueprint • 📦 Archived Project Drop

---

## 4. Learn in Public Kit (Ready-to-Paste)

### 💼 LinkedIn / 🐦 X Post (EN — Authority & Value-First)
```text
Vector search alone breaks on Thai-English support tickets.

When a user asks about "INV-2026-0091" or "ERR-AUTH-403" in code-mixed Thai/English, embeddings blur exact IDs and standard Postgres full-text search fails on unspaced Thai text.

I just shipped the initial full-stack build of `enterprise-rag-ops-copilot` on a $0 free-tier stack (Next.js 15 + FastAPI + PostgreSQL 16 pgvector/pg_trgm) to fix this:

1. Thai-aware Hybrid RRF: Pre-warmed PyThaiNLP (`newmm`) + domain stopword `OR` tsquery + `pg_trgm` exact-code boost (+0.25) fused with L2-normalized 1536-dim embeddings.
2. 3-Stage Anti-Hallucination Guardrails: Bilingual prompt-injection gate ($0 LLM spend) -> Pre-LLM similarity/FTS refusal gate ($0 LLM spend) -> Post-LLM UUID citation & confidence verification.
3. Gemini-safe Pydantic v2 Triage: Replaced `dict[str, str]` with an explicit flat schema wrapper so structured outputs never fail on `additionalProperties`.
4. 25-Case Golden Eval Gate in CI: 37/37 pytest passed, 25/25 eval passed (100% refusal precision & injection block rate, 548ms avg / 834ms P95 on live Gemini).

Next up: rotating free-tier API quota, polishing minor UI formatting, and recording the 60s demo GIF.

Repo: https://github.com/ApecSinlapachai/enterprise-rag-ops-copilot
```

### 📘 Facebook Page Summary (TH — Conversational Tech Builder)
```text
ทำไม RAG ที่ใช้แค่ Vector Search ถึงพังเวลาเจอเคสลูกค้าไทย-อังกฤษ?

ถ้าลูกค้าถามหารหัสเคสเป๊ะๆ อย่าง `INV-2026-0091` หรือ `ERR-AUTH-403` ผสมภาษาไทยแบบไม่มีเว้นวรรค Vector อย่างเดียวมักหาไม่แม่น ส่วน Full-Text Search ปกติของ Postgres ก็ตัดคำไทยไม่ได้

วันนี้เพิ่งขึ้นโครงตัวเต็มของ `enterprise-rag-ops-copilot` (Next.js 15 + FastAPI + PostgreSQL 16 `pgvector` บน Free Tier งบ $0) เสร็จรอบแรกครับ จุดหลักที่ใส่เข้าไปกันพังในงานจริงมี 4 อย่าง:
1. Hybrid Search ไทย-อังกฤษ: ใช้ `PyThaiNLP` ตัดคำ + `pg_trgm` ดันคะแนนรหัสเป๊ะๆ ผสมกับ Vector (`pgvector`) ด้วย RRF
2. Guardrail 3 ชั้นกัน AI มั่ว: ดัก Prompt Injection และคำถามนอกขอบเขตตั้งแต่ก่อนยิง LLM (ประหยัดค่า Token $0) + เช็ก UUID อ้างอิงหลังตอบอีกชั้น
3. Pydantic v2 Structured Output: แยก Schema ให้ชัดเจนไม่ใช้ `dict` ลอยๆ เพื่อไม่ให้ Gemini ปฏิเสธ JSON Schema
4. ชุดทดสอบ 25 เคสใน CI (`pytest` ผ่าน 37/37, Golden Eval ผ่าน 25/25)

รอบถัดไปเหลือไล่เก็บงานบั๊กเล็กน้อย + เช็กโควตา Key บน Render แล้วจะอัด Demo สั้นๆ มาแชร์ครับ
GitHub: https://github.com/ApecSinlapachai/enterprise-rag-ops-copilot
```

### 🎬 YouTube / Short Video Hook
- **Title Idea:** *"Why Vector-Only RAG Fails Thai-English Support Tickets (Hybrid `pgvector` + `PyThaiNLP` + 3-Stage `$0` Guardrails)"*
- **5-Second Hook:** *"If you throw Thai support tickets with invoice numbers into a basic vector database, your RAG bot will either miss the ticket or hallucinate. Here is the `$0` Postgres Hybrid RAG + 3-stage guardrail architecture that passed 25/25 eval cases."*

---

## 5. Rollup Sync Checklist
- [x] Updated **Portfolio Highlights** & **TIL Log** in til_summary.md
- [x] Updated 03-Learning/index.md
- [x] Added YouTube Track B hook to content_calendar.md
- [x] Logged compact entry in log.md

