---
title: "TIL #031: Fixing a 15,520px Mermaid v11 SVG Void in Tailwind v4 — Deterministic Flowchart Parser, Unified Drag-and-Drop & FOUC-Free Dark Mode"
date: 2026-11-28
last_updated: 2026-11-28
domain: learning/til
category: ai-systems
status: ready-to-post
public_channels: [linkedin, x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, nextjs, tailwind-v4, mermaid, ux-engineering, screen-to-sop, multimodal-ai]
---

# 🚀 TIL #031: Fixing a 15,520px Mermaid v11 SVG Void in Tailwind v4 — Deterministic Flowchart Parser, Unified Drag-and-Drop & FOUC-Free Dark Mode

> **🎯 Goal:** Document the production UX/UI overhaul of the **Screen-to-SOP Automation Workbench** (`Next.js 15` + `React 19` + `Tailwind CSS v4` + `FastAPI SSE`) — replacing a broken runtime Mermaid SVG layout with a deterministic 60-line flowchart parser, unifying screen capture + drag-and-drop upload, and shipping a FOUC-free Light/Dark theme verified at desktop and `375px` mobile viewports.

---

## 1. TL;DR — What I Built & Learned Today
- **What I built:** Overhauled the `screen-to-sop-automation` frontend workbench by replacing runtime `mermaid@11.4.1` (`dagre`) SVG rendering with a deterministic `flowchart TD` parser (`parseMermaidFlowchart`) driving accessible Tailwind flowchart cards, stripping duplicate Mermaid fences non-destructively from the Markdown preview, unifying `Record & Upload` with HTML5 drag-and-drop, and adding FOUC-free Tailwind v4 Light/Dark mode.
- **Why it matters:** Passing `tsc`, `eslint`, and `next build` does not guarantee visual correctness of third-party SVG diagram libraries under Tailwind CSS v4. Live browser bounding-box inspection caught a `2140 × 15520 px` SVG void and horizontal table overflow before public demo release.

---

## 2. Problem & Engineering Solution (Problem → Architecture → Fix)

### 2.1 Five Usability Blockers Found in Live Visual Inspection
1. **Oversized Mermaid SVG Void:** Under Next.js 15 + Tailwind CSS v4, `mermaid@11.4.1` (`dagre` layout engine) inflated bounding boxes (`~2,089` SVG units between nodes), stretching the flowchart card to `4,378 px` tall on desktop (`~2140 × 15520` SVG `viewBox`) with microscopic unreadable node labels (`~63 × 17 px`) — even after setting `htmlLabels: false` and explicit font strings.
2. **Duplicate Raw Mermaid Block:** `<ReactMarkdown>` rendered a duplicate raw ` ```mermaid ` code block directly beneath the interactive flowchart card.
3. **Horizontal Table Overflow:** Wide 7-column Markdown SOP and `Steps` tables (`845 px` inside a `622 px` grid column) clipped the `Risk` column and triggered page-level horizontal scroll.
4. **Fragmented Capture Tabs & Missing Drag-and-Drop:** `Record` and `Upload` were split across separate sidebar tabs with ~500 px of empty vertical space below each control, and upload lacked real HTML5 drag-and-drop handlers.
5. **Cold-Start Status Flash & No Dark Mode:** Initial API health checks briefly flashed a red `API offline` badge during backend wakeup, and no Dark Mode toggle existed.

### 2.2 Architecture & Key Implementation Patterns
1. **Deterministic `flowchart TD` Parser (`parseMermaidFlowchart`):** Parses terminal pills (`(["..."])`), step nodes (`["..."]`), step numbers, action badges (`NAVIGATE`, `SELECT`, `CLICK`, etc.), and `(ENV)` / `[ENV]` markers into structured Tailwind cards with compact `24 px` SVG connectors — while preserving Zoom (`60%–160%`), raw Mermaid `Copy`, and `View source / View diagram` toggles.
2. **Non-Destructive Markdown Cleanup (`stripMermaidFences`):** Removes the duplicate `## Workflow Flowchart` + ` ```mermaid ` block only in the `<ReactMarkdown>` preview layer while keeping `sop.markdown` untouched for `Copy Markdown` and `Download .md`.
3. **Unified Capture Panel + Flicker-Free Drag-and-Drop:** Merged capture into 2 clean tabs (`Samples` and `Record & Upload`) with `onDragEnter`, `onDragOver`, `onDragLeave` (guarded by `e.currentTarget.contains(e.relatedTarget)` to prevent child-element flicker), `onDrop`, and an explicit `Browse file` button accepting `.mp4`, `.webm`, and `.mov`.
4. **FOUC-Free Tailwind v4 Dark Mode:** Configured `@custom-variant dark (&:where(.dark, .dark *));` in `globals.css`, persisted `screen-to-sop-theme` in `localStorage` via `ThemeToggle.tsx`, and injected a tiny synchronous `<head>` script in `layout.tsx` to apply `.dark` before first paint.
5. **Containment & Mobile Polish:** Wrapped `<ReactMarkdown>` `<table>` components in `overflow-x-auto`, enforced `overflow-x-clip` / `min-w-0` on grid tracks, added a `Connecting...` initial health state, clarified `redacted` vs `.env vars` badges, and added mobile auto-scroll to output on run.

```typescript
// Reusable deterministic parser for Mermaid flowchart TD syntax
interface ParsedNode {
  id: string;
  kind: "start" | "step" | "end";
  stepNumber?: string;
  actionType?: string;
  label: string;
  hasEnv: boolean;
}

export function parseMermaidFlowchart(chart: string): ParsedNode[] {
  const nodes: ParsedNode[] = [];
  const seenIds = new Set<string>();

  for (const rawLine of chart.split("\n")) {
    const line = rawLine.trim();
    if (!line || line.startsWith("flowchart") || line.includes("-->")) continue;

    const terminalMatch = line.match(/^([A-Za-z0-9_]+)\(\["(.+?)"\]\)$/);
    if (terminalMatch) {
      const [, id, rawLabel] = terminalMatch;
      if (seenIds.has(id)) continue;
      seenIds.add(id);
      nodes.push({
        id,
        kind: id.toLowerCase() === "start" ? "start" : "end",
        label: rawLabel.trim(),
        hasEnv: false,
      });
      continue;
    }

    const stepMatch = line.match(/^([A-Za-z0-9_]+)\["(.+?)"\]$/);
    if (stepMatch) {
      const [, id, rawLabel] = stepMatch;
      if (seenIds.has(id)) continue;
      seenIds.add(id);

      const hasEnv = /\s*(?:\(ENV\)|\[ENV\])\s*$/i.test(rawLabel);
      let label = rawLabel.replace(/\s*(?:\(ENV\)|\[ENV\])\s*$/i, "").trim();
      const numMatch = label.match(/^(\d+)\.\s*(.+)$/);
      const stepNumber = numMatch?.[1];
      if (numMatch) label = numMatch[2].trim();

      const actionMatch = label.match(/^([A-Z_]+):\s*(.+)$/);
      const actionType = actionMatch?.[1];
      if (actionMatch) label = actionMatch[2].trim();

      nodes.push({ id, kind: "step", stepNumber, actionType, label, hasEnv });
    }
  }
  return nodes;
}
```

---

## 3. Portfolio Proof of Work & Verified Metrics
- **Tech Stack:** `Next.js 15`, `React 19`, `Tailwind CSS v4`, `TypeScript`, `FastAPI SSE`, `Pydantic v2`, `PostgreSQL 16 (Supabase Free)`, `Render Free`, `Vercel Hobby`
- **Verified Checks & Metrics:**
  - `npm run typecheck`, `npm run lint` (`0` ESLint warnings/errors), and `npm run build` all passed cleanly.
  - Live browser verification on `http://localhost:3000` (with FastAPI backend on `:8000`) passed at `1440×900` (Light & Dark Mode) and `375×812` (Mobile) with **`0` console errors** and **`0` failed network requests**.
  - Sample 1 flowchart bounding box shrank from an oversized **`2140 × 15520 px` SVG void** (`4,378 px` stretched card) to a crisp **`512 × 504 px`** 8-node flowchart inside a `540 px` scrollable container.
  - Horizontal overflow check confirmed **`horizontalScrollDetected: false`** (`scrollWidth <= innerWidth`) on both desktop (`1440×900`) and mobile (`375×812`).
- **Internal References:** 📐 Project #3 Blueprint • 📦 Archived Client Drop

> [!CAUTION] PDPA & Security Check
> - Anonymized and verified at source: `0` API keys, `0` tokens, `0` private URLs, and `0` PII included.
> - All metrics above were directly measured during local full-stack build and live headless-browser verification.

---

## 4. Learn in Public Package (Ready-to-Paste)

### 💼 LinkedIn / 🐦 X (EN — Authority & Practical Engineering)
```text
Passing `tsc`, `eslint`, and `next build` does NOT guarantee your UI isn't broken.

While inspecting my Screen-to-SOP AI Workbench in a live browser, Mermaid v11 (`dagre`) under Next.js 15 + Tailwind CSS v4 rendered an 8-node flowchart into a 2140 × 15520 px SVG void — stretching the card to 4,378 px tall with microscopic 63×17 px labels.

Switching `htmlLabels: false` and explicit font strings didn't fix it.

So I replaced runtime SVG layout with a ~55-line deterministic `flowchart TD` parser rendering accessible Tailwind cards + compact 24 px connectors:
1. Flowchart bounding box: 2140 × 15520 px → 512 × 504 px (inside a 540 px container), keeping Zoom, Copy, and View Source intact.
2. Stripped duplicate `mermaid` fences non-destructively from `<ReactMarkdown>` preview while preserving full `.md` export.
3. Wrapped Markdown tables in `overflow-x-auto` + `min-w-0` grid tracks → 0 horizontal overflow on 1440×900 desktop and 375×812 mobile.
4. Added FOUC-free Tailwind v4 Dark Mode (`@custom-variant dark` + inline `<head>` script) and unified Screen Record + flicker-free HTML5 Drag-and-Drop upload.

Lesson: Always verify real rendered bounding boxes across desktop and 375px mobile before calling a frontend "done".
```

### 📘 Facebook Page (TH — Builder Field Note)
```text
รัน TypeScript ผ่าน + Build ผ่าน ไม่ได้แปลว่าหน้าเว็บไม่พังครับ 😅

วันนี้เช็กหน้าเว็บโปรเจกต์ AI แปลงคลิปอัดหน้าจอเป็นเอกสาร SOP + สคริปต์ Automation (`Next.js 15 + Tailwind v4`) บนเบราว์เซอร์จริง เจอ Mermaid v11 สร้างกราฟ Flowchart ยืดยาวไปถึง 15,520 px จนตัวหนังสือเหลือจิ๋วเดียว แถมตาราง 7 คอลัมน์ล้นจอมือถือ

วิธีแก้ที่จบขาด:
1. เขียน Parser ~55 บรรทัดแปลง `flowchart TD` เป็นการ์ด Tailwind ตรงๆ → กราฟหดจาก 15,520 px เหลือ 512×504 px อ่านชัดเป๊ะทั้งปุ่ม Zoom / Copy / ดูโค้ดต้นฉบับ
2. ตัดโค้ด Mermaid ที่โผล่ซ้ำใน Markdown Preview ออกโดยไม่กระทบปุ่ม Download `.md`
3. รวมแท็บ Record + Drag-and-Drop Upload ไว้ที่เดียว พร้อมเพิ่ม Light/Dark Mode แบบไม่กะพริบ (FOUC-free)
4. เทสจริงทั้งจอ Desktop (1440×900) และมือถือ (375×812) → 0 Horizontal Overflow และ 0 Console Errors

บทเรียนสำคัญ: งาน Frontend ที่ใช้ไลบรารีกราฟ/SVG ต้องเทส Bounding Box บนเบราว์เซอร์จริงเสมอครับ
```

### 🎬 YouTube / Short Video Hook
- **Title Idea:** *"Why Mermaid v11 Stretched My Flowchart into a 15,000px Void in Tailwind v4 — and How a 55-Line Parser Fixed It"*
- **First 5s Hook:** *"My Next.js build passed with zero errors — until I scrolled down and found a 15,000-pixel blank void where my AI flowchart was supposed to be."*

---

## 5. Rollup Sync Checklist
- [x] Updated **Portfolio Highlights** & **TIL Log** in til_summary.md
- [x] Updated 03-Learning/index.md
- [x] Added Track B Hook #14 to content_calendar.md
- [x] Logged compact summary in log.md

