---
title: "TIL #030: Next.js App Router Open Graph Shallow-Merge Gotcha, Localized Metric SSOT & Bilingual Demo Explainers"
date: 2026-11-27
last_updated: 2026-11-27
domain: learning/til
category: ai-systems
status: ready-to-post
public_channels: [linkedin, x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, nextjs, opengraph, seo, zod, i18n, ux, vercel]
---

# 🚀 TIL #030: Next.js App Router Open Graph Shallow-Merge Gotcha, Localized Metric SSOT & Bilingual Demo Explainers

> **🎯 Goal:** Document the Phase 8 engineering upgrades to [`apec-ai-portfolio`](https://apec-ai-portfolio.vercel.app/) — fixing the Next.js App Router shallow-merge `og:image` drop on child demo routes with `next/og` `ImageResponse` (`1200×630`), extending the Zod Markdown SSOT with backward-compatible Thai metric fields, and shipping accessible bilingual 3-pillar demo explainers for non-technical recruiters and SME buyers.

---

## 1. TL;DR (English-First)
- **What I built:** Upgraded [`apec-ai-portfolio`](https://apec-ai-portfolio.vercel.app/) across 3 parallel tracks merged cleanly into `main` (20 files, `+678 / -111` lines): (1) static `1200×630` Open Graph image (`src/app/opengraph-image.tsx`) + explicit child-route `openGraph`/`twitter` image declarations & Red Maple SVG brand mark, (2) backward-compatible Zod `labelTh`/`valueTh` metric localization + plain-language EN/TH project summaries, and (3) a reusable, theme-safe `DemoProjectExplainer` collapsible banner for both live AI demos.
- **Why it matters:** In Next.js App Router, overriding `openGraph` or `twitter` in a child `layout.tsx` shallow-merges and silently drops the parent `og:image`, leaving blank preview cards on LinkedIn/X. Pairing reliable social previews with plain-language bilingual explainers and localized metrics bridges the comprehension gap for non-technical Thai/English recruiters and business decision-makers in under 10 seconds at `$0/mo`.

---

## 2. Problem → Architecture & Implementation Patterns

### Engineering Bottlenecks Solved
1. **Silent `og:image` Drop on Child Routes (Next.js App Router Shallow Merge):** Defining `openGraph` or `twitter` metadata in a child route `layout.tsx` overrides the entire parent object rather than deep-merging, causing shared demo links (`/demos/rag-copilot`, `/demos/agent-ops`) to render blank preview cards on LinkedIn and X unless `images` are explicitly re-declared alongside `metadataBase`.
2. **Recruiter & Non-Tech Comprehension Friction:** Jargon-heavy project prose that duplicated benchmark numbers already shown in metric cards caused cognitive overload, while untranslated metric badges and unguided live demos left non-technical HR reviewers unsure who each system serves or how to test it.
3. **Parallel Delivery Risk:** Executing schema/content updates, brand/OG SEO metadata, and interactive demo UI changes simultaneously risked merge conflicts and Dark/Light theme or hydration regressions without strict partitioning and deterministic gates.

### Production Patterns Shipped
1. **Explicit Child-Route Open Graph Metadata + Static `next/og` Card (`brand-og-seo`):**
   - Added `metadataBase` + `openGraph`/`twitter` (`summary_large_image`) in `src/app/layout.tsx` and generated a static `1200×630` PNG card via `next/og` `ImageResponse` (`src/app/opengraph-image.tsx`).
   - Explicitly declared `openGraph.images` and `twitter.images: ["/opengraph-image"]` in both child demo layouts (`src/app/demos/{rag-copilot,agent-ops}/layout.tsx`) to prevent Next.js shallow-merge image loss.
   - Replaced the brand mark with a custom Red Maple Leaf SVG favicon (`src/app/icon.svg`) & `BrandMapleMark` (`Navbar.tsx`) and standardized the language toggle to `EN / TH`.
2. **Backward-Compatible Localized Metric SSOT (`content-metrics`):**
   - Extended `MetricSchema` in `src/lib/content/schema.ts` with optional `labelTh` and `valueTh` fields, preserving 1-file Markdown extensibility with graceful English fallback (`lang === "th" ? (m.labelTh ?? m.label) : m.label`).
   - Rewrote all 3 project summaries (`enterprise-rag-ops-copilot`, `production-ai-agent-ops`, `fast-lead-automation`) and `_TEMPLATE_project.md` into plain-language 2–3 sentence EN/TH value summaries without duplicating structured telemetry numbers.
3. **Semantic Theme-Safe Bilingual Demo Explainers (`demo-explainers`):**
   - Built a reusable `DemoProjectExplainer` (`src/components/demos/DemoProjectExplainer.tsx`) backed by bilingual `explainer` copy in `src/lib/i18n/translations.ts`, presenting 3 plain-language pillars (*Who it's for*, *What problem it solves*, *How it works*) plus a *Try it* guide.
   - Styled purely with CSS custom properties (`var(--surface)`, `var(--background)`, `var(--border)`, `var(--accent)`) and accessible `aria-expanded`/`aria-controls` while keeping `/api/proxy/*` rewrites and Auto-Fill Only chips untouched.

```typescript
// Sanitized, reusable production pattern:
// Next.js App Router child route metadata that preserves og:image / twitter:image
// under shallow merge, paired with a static 1200x630 app/opengraph-image.tsx
import type { Metadata } from "next";
import { z } from "zod";

export const MetricSchema = z.object({
  label: z.string().min(1),
  labelTh: z.string().min(1).optional(),
  value: z.string().min(1),
  valueTh: z.string().min(1).optional(),
});

// In child route layout.tsx (e.g., src/app/demos/<demo-slug>/layout.tsx):
// Root layout.tsx must define `metadataBase: new URL("https://<your-domain>")`.
export const metadata: Metadata = {
  title: "Live AI Demo | Portfolio",
  description: "Plain-language interactive demo summary for social link previews.",
  openGraph: {
    type: "website",
    url: "https://<your-domain>/demos/<demo-slug>",
    title: "Live AI Demo | Portfolio",
    description: "Plain-language interactive demo summary for social link previews.",
    images: [{ url: "/opengraph-image", width: 1200, height: 630, alt: "Live AI Demo" }],
  },
  twitter: {
    card: "summary_large_image",
    title: "Live AI Demo | Portfolio",
    description: "Plain-language interactive demo summary for social link previews.",
    images: ["/opengraph-image"],
  },
};
```

---

## 3. Verified Results & Proof of Work
- **Tech Stack:** `Next.js 15 (App Router, next/og ImageResponse)`, `React 19`, `TypeScript 5.8`, `Tailwind CSS v4`, `Zod`, `Vitest`, `Vercel Hobby`
- **Automated Verification (`npm run verify` — 4/4 Gates Passed):**
  - `eslint .`: `0` errors
  - `tsc --noEmit`: `0` errors
  - `vitest run`: `9/9` tests passed (including localized metric schema assertions)
  - `next build`: `13/13` routes generated cleanly (including `/icon.svg` and `/opengraph-image`)
- **Browser QA (`http://localhost:3006`) & Git Delivery:**
  - Verified across `EN`/`TH`, Dark & Light modes, Desktop (`1440px`), and Mobile (`390px`) with `0` console errors, `0` React hydration errors, `0` horizontal overflow, and `0` direct `*.onrender.com` browser calls.
  - All Phase 8 commits (20 files changed, `+678 / -111` lines, plus `aa6b249` SEO image fix) merged and pushed cleanly to `origin/main`.
- **Live Portfolio Hub:** [apec-ai-portfolio.vercel.app](https://apec-ai-portfolio.vercel.app/)
- **Internal References:** TIL #029 • 🌐 Portfolio RAG & SOT Architecture • 🚀 All-In AI Engineer Roadmap • 📦 Archived Project Drop

---

## 4. Learn in Public Kit (Ready-to-Paste — Conversational Tech Builder Style)

### 💼 LinkedIn (EN — ใส่ลิงก์ใน First Comment)
```text
Small Next.js App Router gotcha that just saved me from shipping blank LinkedIn preview cards on my live AI demos.

When you define a root opengraph-image.tsx (1200x630 via next/og ImageResponse) and set metadataBase in app/layout.tsx, the homepage preview works great. But the moment you add a custom openGraph or twitter title/description inside a child route's layout.tsx, Next.js shallow-merges that object and silently drops the parent og:image.

Your root URL shows a clean card, while your subpage demo links show up blank.

The fix is simple once you catch it:
1. Keep metadataBase in root app/layout.tsx and generate a static 1200x630 PNG via app/opengraph-image.tsx.
2. In every child layout.tsx that overrides openGraph or twitter, explicitly re-declare images: ["/opengraph-image"].

While fixing SEO previews on my portfolio hub (Phase 8), I also cleaned up two recruiter UX bottlenecks:
- Extended my Zod MetricSchema with optional labelTh and valueTh fields so Thai metric badges render natively while keeping 1-file Markdown fallback intact.
- Added a collapsible 3-pillar DemoProjectExplainer (Who it's for, What problem it solves, How it works + Try it guide) on both live AI demos using CSS custom properties for Dark/Light parity.

Verified with eslint (0), tsc (0), Vitest (9/9), Next.js build (13/13 routes including /opengraph-image), and Desktop/Mobile browser QA (0 console or hydration errors).

Live hub link is in the first comment. Have you ever been bitten by Next.js metadata shallow-merging on nested routes?
```

**LinkedIn First Comment:**
```text
Live Portfolio Hub + Bilingual Demo Explainers (no login): https://apec-ai-portfolio.vercel.app/
Live RAG Demo: https://apec-ai-portfolio.vercel.app/demos/rag-copilot
Live Agent Ops Demo: https://apec-ai-portfolio.vercel.app/demos/agent-ops
```

### 🐦 X (Twitter — EN · ใส่ลิงก์ใน Reply #1)
```text
Next.js App Router gotcha that breaks LinkedIn/X previews on subpages:

If a child layout.tsx overrides `openGraph` or `twitter` for a custom title/description, Next.js shallow-merges the object and silently drops your root `og:image`.

Fix:
1. Set `metadataBase` in root `app/layout.tsx`
2. Add `app/opengraph-image.tsx` (1200x630 `next/og` `ImageResponse`)
3. Explicitly pass `images: ["/opengraph-image"]` in every child `layout.tsx`

Shipped this in Phase 8 of my $0/mo AI portfolio hub along with optional Zod `labelTh`/`valueTh` localized metrics and collapsible 3-pillar bilingual demo explainers.

Verified: eslint 0, tsc 0, 9/9 Vitest, 13/13 routes, 0 hydration errors.

Live link in reply.
```

**X Reply #1:**
```text
Try the updated Portfolio Hub + Live AI Demos (no login): https://apec-ai-portfolio.vercel.app/
```

### 📘 Facebook Page (TH — เจ้าของธุรกิจ / คนทำเว็บ · ใส่ลิงก์ในคอมเมนต์แรก)
```text
เวลาแชร์ลิงก์เว็บหรือหน้า Demo ลง LinkedIn / Facebook แล้วรูปปกไม่ขึ้น ทั้งที่หน้าแรกขึ้นปกติ — หลายครั้งไม่ได้เป็นเพราะแคช แต่เป็นพฤติกรรมของ Next.js App Router ครับ

ตอนทำอัปเกรด Portfolio Hub เฟส 8 รอบนี้ เจอจุดนี้เต็มๆ คือถ้าหน้าย่อย (อย่างหน้า Live Demo) มีการตั้งค่า title/description ใน openGraph ของตัวเอง Next.js จะทับ object เดิมแบบ shallow-merge ทำให้รูป og:image จากหน้าหลักหายไปเงียบๆ วิธีแก้คือตั้ง metadataBase ที่ root, สร้างการ์ด 1200×630 ด้วย next/og ImageResponse และระบุ images: ["/opengraph-image"] ซ้ำให้ชัดใน layout ของทุกหน้าย่อยครับ

นอกจากแก้เรื่องการ์ดแชร์ลิงก์แล้ว รอบนี้ยังปรับ UX อีก 2 จุดเพื่อให้ HR และเจ้าของธุรกิจที่ไม่ได้เป็นสาย Dev เข้าใจระบบได้ใน 10 วินาที:
1. แยกสรุปโปรเจกต์ให้เป็นภาษาคน (EN/TH) และเพิ่ม field labelTh / valueTh แบบ optional ใน Zod Schema ให้ตัวเลข Metric แสดงเป็นภาษาไทยได้โดยไม่พังโครงสร้างไฟล์เดิม
2. เพิ่มกล่องอธิบายแบบพับเก็บได้บนหน้า Live Demo ทั้ง 2 ตัว บอกชัดๆ 3 ข้อ (เหมาะกับใคร / แก้ปัญหาอะไร / ทำงานยังไง + วิธีลองกดเล่น) รองรับทั้ง Dark และ Light Mode

ผ่านเทสต์ครบ (eslint 0, tsc 0, Vitest 9/9, Next.js 13/13 routes) แปะลิงก์ให้ลองเข้าไปกดดูในคอมเมนต์แรกครับ
```

**Facebook Page First Comment:**
```text
ลองเข้าไปดูหน้า Portfolio Hub + กล่องอธิบาย Live Demo 2 ภาษาได้ที่นี่ครับ: https://apec-ai-portfolio.vercel.app/
```

### 👥 Facebook Group (TH — สาย Dev / Next.js / AI · แชร์ประสบการณ์ล้วน ไม่มีขายของ)
```text
แวะมาแชร์ Gotcha เล็กๆ ของ Next.js 15 App Router เรื่อง Open Graph ที่เพิ่งเจอตอนทำหน้า Live Demo หลาย Route ครับ

ผมสร้าง `src/app/opengraph-image.tsx` (1200×630 ด้วย `next/og` `ImageResponse`) + ตั้ง `metadataBase` ใน root `layout.tsx` ไว้แล้ว หน้า `/` แชร์แล้วรูปขึ้นปกติ แต่พอสร้าง `layout.tsx` ในหน้าย่อย (`/demos/*`) เพื่อใส่ `openGraph.title` กับ `description` เฉพาะหน้า ปรากฏว่ารูป `og:image` หายไปเลย เพราะ Next.js ทำ shallow-merge ที่ระดับ `openGraph` / `twitter` object ไม่ได้ deep-merge `images` ลงมาให้

วิธีแก้จบสะอาดคือใส่ `images: [{ url: "/opengraph-image", width: 1200, height: 630 }]` และ `twitter.images: ["/opengraph-image"]` ให้ชัดในทุก child `layout.tsx` ที่ override metadata ครับ

รอบเดียวกันนี้ยังขยาย Zod `MetricSchema` เพิ่ม `labelTh` / `valueTh` แบบ `.optional()` เพื่อทำ fallback ภาษาอังกฤษอัตโนมัติเวลาไฟล์ Markdown เก่ายังไม่ได้แปล และทำ `DemoProjectExplainer` ใช้ CSS variables ล้วนให้รอดทั้ง Dark/Light Mode ไม่มี hydration error

ใครใช้ Next.js App Router แชร์ลิงก์ลง LinkedIn อย่าลืมเช็ก subpage ด้วยนะครับ เผลอแป๊บเดียวการ์ดโล่งเลย
```

### 🎬 YouTube / Short Video Hook
- **Title Idea:** *"The Next.js App Router Open Graph Gotcha That Breaks Your LinkedIn Previews (And How to Fix It with `next/og` + Explicit Child Metadata)"*
- **5-Second Hook:** *"Your Next.js homepage has a crisp LinkedIn preview card, so why do your subpages show up completely blank? It's a silent shallow-merge gotcha in App Router—and here's the 15-line fix."*

