---
title: "TIL #022: Building & Testing Fast-Lead v2.0 Multi-Platform Router via n8n Workflow SDK & Live MCP"
date: 2026-11-21
last_updated: 2026-11-24
domain: learning/til
category: automation-agency
status: published
public_channels: [linkedin, x, facebook-page, youtube]
tags: [til, n8n, mcp, workflow-sdk, multi-platform, automation, line-flex, gtm]
---

# 🚀 TIL #022: Building & Testing Fast-Lead v2.0 Multi-Platform Router via n8n Workflow SDK & Live MCP

> **🎯 Goal:** Upgrade `fast-lead-automation` from V1 (`Google Forms -> Sheets`) to V2 (`X`, `Facebook Page`, `Facebook Group`, `LinkedIn`, `TikTok`, `YouTube`, `Web`) using the official **n8n Workflow SDK** and **Instance-Level MCP**, verified end-to-end with live `test_workflow` runs and `85/85` automated regression tests.

---

## 1. TL;DR — What I Built & Learned Today
- **Built:** A 2-pipeline, 11-node **n8n v2.0 Multi-Platform Lead Router & Social Content Distributor** directly on a live Dockerized n8n instance via `n8n-mcp` (`validate_workflow` → `create_workflow_from_code` → `get_workflow_details` → `test_workflow`), then synced to GitHub (`ApecSinlapachai/fast-lead-automation`, commit `a787688`, **`85/85` tests passing**).
- **Why it matters:** Treating n8n workflows like production software—using native OAuth credentials (zero `$env` secrets), explicit Node Groups, deterministic unit tests, and live MCP pin-data execution—eliminates fragile manual drag-and-drop wiring and prevents platform API bans.

---

## 2. Problem → Architecture & Engineering Fixes

- **Pain Point / Bottleneck:**
  1. Distributing proof-of-work content across **X, Facebook Page, Facebook Group, and LinkedIn** without unified UTM attribution makes it impossible to know which channel drives qualified leads.
  2. **Meta permanently deprecated the Facebook Groups API (`publish_to_groups`)**—attempting to auto-post to Facebook Groups with browser bots risks instant account suspension.
  3. Calling `test_workflow` via an MCP client with spaces in `pinData` node-name keys caused quote-wrapping mismatches (`"\"Node Name\""`), rejecting valid node names at runtime.

- **Architecture & Solutions Implemented:**
  1. **Pipeline 1 — Inbound Multi-Platform Lead Router (`/webhook/fast-lead-intake`):**
     - `Read_Existing_CRM_Leads` (`Google Sheets v4.7`, `executeOnce: true`, `retryOnFail: 3`) → `Normalize_Lead_and_Check_Duplicates` (`Code v2` multi-source aggregation) → parallel fan-out to `Append_Lead_to_Google_Sheets_CRM` (14 columns including `LINE_ID` & `Sub_Channel`) and `Send_LINE_Flex_Lead_Alert` (`HTTP Request v4.5` with `httpTemplatedCustomAuth` credential).
     - Enforces spreadsheet formula-injection sanitization (`=`, `+`, `-`, `@`), Thai phone normalization (`+66`/9-digit), safe LINE ID extraction (preserving `lineman_shop`), monotonic `LD-XXX` IDs, and phone/email deduplication.
  2. **Pipeline 2 — Outbound Multi-Platform Content Distributor (`/webhook/distribute-social-content`):**
     - `Build_Platform_Copy_and_UTM_Links` (`Edit Fields / Set v3.5` with inline IIFE expressions—avoiding unnecessary Code node overhead) generates deterministic `?utm_source=x|fb_page|fb_group|linkedin` links and platform-native copy (`<= 280` chars for X).
     - Fans out to native `X (Twitter) v2`, `Facebook Graph API v1 (v22.0)`, `LinkedIn v1`, and a policy-safe **LINE Ready-to-Paste Dispatch** for **Facebook Groups** so community posts take 10 seconds with zero ban risk.
  3. **Live MCP Debugging Fix:**
     - Renamed all 11 nodes to underscore identifiers (`Receive_Multi_Platform_Lead_Webhook`, etc.) and updated cross-node expressions (`$('Node_Name').item.json`), resolving the MCP `pinData` key-serialization edge case.

---

## 3. Verified Proof of Work & Metrics
- **Tech Stack:** `n8n (Docker v2.2+)`, `n8n Workflow SDK`, `Instance-Level MCP`, `Google Sheets API`, `LINE Messaging API (Flex v2)`, `X API v2`, `Facebook Graph API v22.0`, `LinkedIn API`, `Node.js`
- **Verified Live MCP Execution Results:**
  - `validate_workflow`: `valid: true` (`11` nodes, `2` Node Groups, `0` warnings)
  - `test_workflow` Execution #4 (Inbound Lead Router): `status: "success"` (`Normalize_Lead_and_Check_Duplicates` ran in **`21 ms`**, produced `LD-020`, `👥 Facebook Group`, `🔥 Hot Lead`)
  - `test_workflow` Execution #5 (Outbound Content Distributor): `status: "success"` (`Build_Platform_Copy_and_UTM_Links` ran in **`97 ms`**, generated all 4 UTM URLs & platform copy)
  - Offline Regression Suite (`npm test`): **`85/85` passing** (`45/45` Apps Script v3 + `28/28` Apps Script v2 + `12/12` n8n Multi-Platform v2)
- **Public GitHub Repo:** [ApecSinlapachai/fast-lead-automation](https://github.com/ApecSinlapachai/fast-lead-automation)

---

## 4. Learn in Public Pack (Ready-to-Paste — `2026-11-23` Conversational Tech Builder Style)
> **Style Reference:** ✍️ Content Voice & Style Guide (Conversational Tech Builder • Zero AI Tells • Link in First Comment / Reply)

### 💼 1. LinkedIn Post #2 (EN — Value-First Follow-Up to Post #1)
*แนบภาพ Architecture Diagram หรือ Screenshot ผลเทสต์ 85/85 (ไม่แปะลิงก์ในตัวโพสต์)*
```text
Right after I shared Fast-Lead v1 (Google Forms to Sheets + LINE alerts), I ran into the next obvious problem. Leads rarely come from just one form. Mine come from X, LinkedIn, Facebook Pages, Facebook Groups, YouTube, and web forms, and without clean UTM tracking, you are basically guessing which channel actually works.

So over the weekend I built v2.0 in n8n. Instead of clicking around the canvas manually, I wrote and tested an 11-node workflow directly against my local Docker n8n instance using the n8n Workflow SDK and Instance-Level MCP.

Two practical snags came up while wiring it together:

1. Meta permanently deprecated the Facebook Groups API. A lot of automation tutorials suggest browser bots as a workaround, which is a fast way to get an account banned. Instead of forcing full automation there, my workflow auto-posts natively to X, Facebook Page, and LinkedIn, and sends a ready-to-paste package with the UTM link straight to LINE for Facebook Groups. Takes 10 seconds to post by hand with zero account risk.

2. Running live `test_workflow` calls through MCP tripped over spaces in node names (quote-wrapping mismatches in `pinData`). Switching all 11 nodes to clean underscore names fixed it right away.

Once those were sorted, both pipelines passed live MCP test runs (21 ms for the inbound lead router, 97 ms for the outbound distributor) along with 85/85 automated offline tests covering formula-injection guards, Thai phone normalization, and duplicate checks.

If you have been building with n8n's MCP or Workflow SDK lately, how are you handling automated testing before pushing workflows live?

#n8n #MCP #WorkflowAutomation #GTMEngineering #BuildInPublic
```
**First Comment (LinkedIn):**
```text
Here is the full repo, architecture diagram, and test suite if you want to poke around:
https://github.com/ApecSinlapachai/fast-lead-automation
```

---

### 🐦 2. X / Twitter Post (EN — 266 chars + Reply with Repo)
*แนบภาพ Architecture Diagram ใน Main Post และอยู่ตอบคอมเมนต์ 30 นาทีแรก*
```text
Built an 11-node n8n v2.0 lead router via Workflow SDK + live MCP (21ms/97ms runs, 85/85 tests).

Two gotchas worth knowing:
1. Use underscore node names so MCP pinData won't break
2. Meta killed the Groups API. Send ready-to-paste copy to LINE instead of using bots
```
**Reply 1 (X):**
```text
Inbound side handles X, LinkedIn, FB, YouTube, and Web leads with UTM tracking, formula-injection guards, and LINE Flex alerts.

Full repo + architecture diagram:
https://github.com/ApecSinlapachai/fast-lead-automation
```

---

### 📘 3. Facebook Page Post (TH — เจ้าของธุรกิจ SME / Fastwork)
*แนบภาพผังระบบหรือภาพการ์ด LINE Flex (ใส่ลิงก์ในคอมเมนต์แรก)*
```text
เพิ่งอัปเกรดระบบเก็บรายชื่อลูกค้า (Fast-Lead) จากเวอร์ชันแรกที่รับแค่ Google Form มาเป็นเวอร์ชัน 2 บน n8n เพื่อแก้ปัญหาที่คนทำเพจและเจ้าของธุรกิจเจอบ่อย 2 เรื่องครับ

เรื่องแรกคือเวลาลงคอนเทนต์หลายช่องทาง ทั้ง Facebook, LinkedIn, X, YouTube พอลูกค้าทักหรือกรอกฟอร์มเข้ามา เรามักไม่รู้ว่าคนนี้มาจากโพสต์ไหนหรือช่องทางไหน เลยเพิ่มระบบติดป้าย UTM อัตโนมัติตั้งแต่ตอนโพสต์ พอลูกค้ากรอกเข้ามา ระบบจะดึงลง Google Sheets เช็กเบอร์ซ้ำให้เรียบร้อย แล้วเด้งการ์ดแจ้งเตือนเข้า LINE ทันทีภายในไม่กี่วินาที

เรื่องที่สองอันนี้สำคัญมากสำหรับคนทำกลุ่ม Facebook ครับ ตอนนี้ Meta ปิดระบบ API สำหรับโพสต์เข้ากลุ่มถาวรแล้ว ถ้าไปฝืนใช้บอทคลิกอัตโนมัติมีสิทธิ์บัญชีปลิวสูงมาก ผมเลยแก้แบบปลอดภัยแทน คือให้ระบบโพสต์อัตโนมัติเฉพาะช่องทางที่เปิด API ถูกต้อง (X, Facebook Page, LinkedIn) ส่วนของ Facebook Group ให้ระบบจัดข้อความพร้อมลิงก์ส่งมาให้ใน LINE แทน เราก็กดก๊อปวางเองใช้เวลาแค่ 10 วินาที สบายใจเรื่องบัญชีไม่โดนแบนด้วย

รอบนี้เทสต์ระบบจริงผ่านครบทั้ง 85 เคส (กันเบอร์ซ้ำ กันข้อมูลแปลกๆ ที่ทำให้สูตรใน Sheet พัง) ใครอยากดูโครงสร้างระบบหรือเอาไปต่อยอด แปะลิงก์ GitHub ไว้ให้ในคอมเมนต์แรกนะครับ
```
**First Comment (FB Page):**
```text
ลิงก์โค้ดและแผนผังระบบเต็มๆ ดูได้ที่นี่ครับ:
https://github.com/ApecSinlapachai/fast-lead-automation

ส่วนใครทำธุรกิจแล้วอยากให้ช่วยดูเรื่องระบบรับ Lead / แจ้งเตือนเข้า LINE ทักแชทมาคุยเล่นหรือสอบถามก่อนได้เลยครับ
```

---

### 👥 4. Facebook Group Post (TH — คอมมูนิตี้สาย n8n / AI Automation, 0% ขายของ)
*แนบภาพผัง 11-Node Workflow ใน n8n (ไม่แปะลิงก์ขายของในโพสต์)*
```text
แวะมาแชร์ประสบการณ์หลังเพิ่งลองอัปเกรดระบบ Fast-Lead v2 บน n8n โดยลองสั่งสร้างและเทสต์ Workflow ผ่าน n8n Workflow SDK + Instance-Level MCP แบบไม่ลากเมาส์ต่อ Node เองครับ

ระหว่างทำไปเจอ 2 จุดที่น่าจะเป็นประโยชน์กับคนที่กำลังทำระบบ Multi-Platform บน n8n เหมือนกัน:

1. บั๊กชื่อ Node ตอนสั่งเทสต์ผ่าน MCP: ตอนเรียก `test_workflow` พร้อม `pinData` ถ้าชื่อ Node มีเว้นวรรค (เช่น `Read Existing Leads`) ตัว MCP client จะแปลงเครื่องหมายคำพูดเพี้ยนจนรันไม่ผ่าน พอเปลี่ยนชื่อทั้ง 11 Node เป็นแบบขีดล่าง (`Read_Existing_CRM_Leads`) ปุ๊บ ผ่านฉลุยเลยครับ รันจริงฝั่งรับ Lead ใช้เวลา 21 ms ฝั่งกระจายคอนเทนต์ 97 ms

2. เรื่องโพสต์ลง Facebook Group: เนื่องจาก Meta ปิด `publish_to_groups` ถาวรแล้ว แทนที่จะฝืนเอาบอทมาช่วยคลิกให้เสี่ยงโดนแบน ผมเลยออกแบบให้ n8n ยิง API ปกติไปแค่ X, Facebook Page กับ LinkedIn ส่วนของกลุ่มให้แพ็กข้อความพร้อมลิงก์ UTM ส่งเข้า LINE ตัวเองแทน กดก๊อปวางมือใช้เวลา 10 วิ ปลอดภัยกว่าเยอะครับ

ใครในกลุ่มกำลังลองเล่น n8n MCP หรือมีท่าเทสต์ Workflow ก่อนขึ้นงานจริงแบบไหน มาแชร์กันได้นะครับ อยากรู้ว่าปกติทุกคนเขียนเทสต์แยกกันยังไงบ้าง
```

