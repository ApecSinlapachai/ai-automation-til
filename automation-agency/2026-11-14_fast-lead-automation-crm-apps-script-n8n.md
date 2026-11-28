---
title: "TIL #003: สร้างระบบรวม Lead อัตโนมัติ Dual-Mode (Mini-CRM + Apps Script ฿0/ด. + n8n + LINE Flex)"
date: 2026-11-14
last_updated: 2026-11-14
domain: learning/til
category: portfolio
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, lead-automation, google-sheets, apps-script, n8n, line-flex]
---

# 🚀 TIL #003: สร้างระบบรวม Lead อัตโนมัติ Dual-Mode (Mini-CRM + Apps Script ฿0/ด. + n8n + LINE Flex)

> **🎯 เป้าหมายของการบันทึกนี้:** ใช้เป็น **Portfolio Proof of Work หลักของ Fastwork Gig #1** และวัตถุดิบ Learn in Public แสดงการสร้างระบบรับ Lead ที่พร้อมใช้งานจริงทั้งแบบฟรี ฿0/เดือน และแบบ Multi-Channel

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่สร้าง:** ประกอบระบบ **Fast-Lead Automation** ครบชุดสำหรับ Gig #1 ประกอบด้วย **Google Sheet Mini-CRM** (3 แท็บ: `Dashboard`, `All_Leads`, `Setup_Guide`), โค้ด **Google Apps Script** (รันฟรี ฿0/เดือน), ไฟล์ **n8n Workflow JSON** (Multi-Channel), และ **LINE Flex Message JSON** พร้อมปุ่มกดโทรออก
- **ทำไมเรื่องนี้ถึงสำคัญ:** ออกแบบตามหลัก **Build Once, Use Twice** — ใช้ตัวเดียวกันเป็นทั้ง Demo เปิดขายบน `fastwork.co` และระบบรับสมัครหลังบ้านสำหรับแคมเปญ YouTube ของตัวเอง

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point / Bottleneck):**
  1. ลูกค้า SME ที่ยิงแอดหรือรับฟอร์มมักเจอปัญหาตอบ Lead ช้า (หรือตกหล่นระหว่างช่องทาง) และไม่อยากเสียค่าบริการ SaaS รายเดือนแพงๆ ตั้งแต่วันแรก
  2. เบอร์โทรศัพท์ที่ลูกค้ากรอกมามักรูปแบบไม่เหมือนกัน (เช่น `089-999-8888`, `089 999 8888`, `+66899998888`) ทำให้เช็กเบอร์ซ้ำด้วยสูตรตรงๆ พลาดได้ง่าย
  3. LINE Notify ยุติการให้บริการแล้ว จึงต้องขยับไปใช้ **LINE Messaging API (Flex Message)** แทน พร้อมคุมความปลอดภัยไม่ให้ Token หลุดในโค้ด
- **แนวทาง/สถาปัตยกรรมที่เลือกใช้ (Dual-Mode Deterministic Workflow):**
  - **Mode A (`apps_script_lead_alert.gs` — ฟรี ฿0/เดือน):** สำหรับ Google Form → ใช้ Trigger `onFormSubmit` รันตรงใน Google Sheets ไม่ต้องเช่า Server หรือเสียค่ารายเดือน
  - **Mode B (`n8n_fast_lead_workflow.json` — Multi-Channel):** สำหรับรับ Webhook จาก Facebook Lead Ads / TikTok / Website Form รวมศูนย์เข้า Google Sheet เดียวกัน
  - **Deterministic > Agentic สำหรับงานส่ง Lead:** งานดึงและแจ้งเตือน Lead ต้องการความแม่นยำ 100% และ Latency ต่ำ จึงใช้ Workflow แบบกำหนดขั้นตอนตายตัว ไม่เอา LLM มาคั่นให้ช้าและเสีย Token
- **ขั้นตอนที่ลงมือทำจริง (What I Built):**
  1. สร้าง Google Sheet จริง `Fast-Lead CRM & YouTube Campaign (Gig #1 Demo)` แบ่ง 3 แท็บ:
     - `Dashboard`: สูตร KPI อัตโนมัติ + 2 กราฟสรุปผล
     - `All_Leads` (12 คอลัมน์ A–L): ใส่ Data Validation, Conditional Formatting แยกสถานะ `🟢 ลูกค้าใหม่` / `⚠️ เบอร์ซ้ำ` และข้อมูล `[DEMO]` 6 แถว
     - `Setup_Guide`: คู่มือตั้งค่าสำหรับทีมขาย
  2. เขียน `apps_script_lead_alert.gs`:
     - ดึงค่า Token จาก `PropertiesService.getScriptProperties()` (`LINE_CHANNEL_ACCESS_TOKEN`, `LINE_TARGET_ID`) ไม่ Hardcode ลงโค้ด
     - ทำ **Phone Normalization** ตัดอักขระที่ไม่ใช่ตัวเลขและแปลงรหัส `66...` เป็น `0...` ก่อนเช็กเบอร์ซ้ำในคอลัมน์ Phone
     - ทำ **Lead Scoring** เบื้องต้น (`🔥 Hot Lead` / `🟡 Warm Lead`) แล้วสร้างการ์ด **LINE Flex** สี Header ต่างกันระหว่างลูกค้าใหม่ (`#0F172A`) กับเบอร์ซ้ำ (`#92400E`) พร้อมปุ่ม `tel:` กดโทรออกทันที
     - ใส่ฟังก์ชัน `testWithMockLead()` ให้กดทดสอบยิง Flex ได้จากหน้า Apps Script ทันที
  3. สร้าง `n8n_fast_lead_workflow.json` และ `line_flex_lead_alert.json` แยกเป็น Asset พร้อม Import สำหรับงาน Multi-Channel

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `Google Sheets` • `Google Apps Script` • `n8n` • `LINE Messaging API (Flex Message)`
- **สถานะจริง ณ วันนี้:** สร้าง Google Sheet Mini-CRM, Apps Script (`271` บรรทัด), n8n Workflow JSON, และ LINE Flex JSON ครบพร้อมใช้งานแล้ว *(เหลือขั้นตอนผูก Google Form ใน UI + ใส่ LINE Channel Access Token เพื่อยิงทดสอบ Live และแคปภาพแกลเลอรีเปิดขาย)*
- **ลิงก์ / ไฟล์อ้างอิงภายใน:**
  - `Gig #1 Overview`
  - `Gig #1 Execution Blueprint`
  - `02-Business/AI-Agency/gigs/gig-01-lead-automation/assets/apps_script_lead_alert.gs`
  - `02-Business/AI-Agency/gigs/gig-01-lead-automation/assets/n8n_fast_lead_workflow.json`
  - `02-Business/AI-Agency/gigs/gig-01-lead-automation/assets/line_flex_lead_alert.json`

> [!CAUTION] PDPA & Security Check ก่อนเผยแพร่สาธารณะ
> - ห้ามโชว์ `Spreadsheet ID` จริงของลูกค้า, `LINE_CHANNEL_ACCESS_TOKEN`, `LINE_TARGET_ID`, หรือเบอร์โทร/ชื่อจริงใน Screenshot
> - ใช้ข้อมูล `[DEMO]` / `[TEST]` เท่านั้นเวลาแคปภาพหน้า `Dashboard`, `All_Leads`, และการ์ด LINE Flex

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post / Thread Hook
```text
TIL: วิธีทำระบบแจ้งเตือน Lead เข้า LINE ทันทีแบบไม่ต้องเสียค่ารายเดือน (฿0/เดือน) ⚡

วันนี้ประกอบระบบ Fast-Lead Mini-CRM เตรียมไว้ 2 โหมด:
1. Google Form + Apps Script (฿0/ด.) -> ตรวจเบอร์ซ้ำ + ยิงการ์ด LINE Flex
2. n8n Webhook -> รวม Lead จาก FB / TikTok / Web เข้า Google Sheet เดียว

ทริคเล็กๆ ที่ช่วยกันพลาด:
ต้อง Normalize เบอร์โทร (ตัดขีด/เว้นวรรค และแปลง +66 -> 0) ก่อนเช็กซ้ำ ไม่งั้นระบบจะมองว่า 089-999-8888 กับ +66899998888 เป็นคนละคนครับ!
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
แชร์เบื้องหลังการออกแบบ "ระบบรวม Lead + แจ้งเตือนเข้า LINE แบบมีปุ่มกดโทรออก" ในงบ ฿0/เดือน ⚡📱

เวลาธุรกิจยิงแอดหลายช่องทาง หรือรับข้อมูลจากฟอร์ม ปัญหาคลาสสิกที่เจอคือ:
❌ ข้อมูลกระจัดกระจาย ไม่รู้ว่าลูกค้าคนไหนทักซ้ำ
❌ เซลส์เห็นช้า ต้องก๊อปปี้เบอร์จาก Sheet ไปกดโทรเอง

วันนี้เลยลงมือประกอบ Mini-CRM บน Google Sheets + ระบบอัตโนมัติ 2 แบบ:
1️⃣ โหมดประหยัด ฿0/เดือน (Google Apps Script): จับ Trigger ตอนส่งฟอร์ม -> เช็กเบอร์ซ้ำอัตโนมัติ -> ส่งการ์ด LINE Flex พร้อมปุ่ม "📞 กดโทรหาลูกค้าทันที"
2️⃣ โหมด Multi-Channel (n8n JSON): รับ Webhook จาก Facebook / TikTok / หน้าเว็บ มารวมศูนย์ที่ Google Sheets เดียว

💡 จุดที่ต้องใส่ใจตอนทำจริง:
- อย่า Hardcode LINE Token ในโค้ด ให้เก็บใน Script Properties / Credentials
- ต้อง Normalize เบอร์โทร (ตัดขีด และแปลง +66 เป็น 0) ก่อนเช็กเบอร์ซ้ำเสมอ
- แยกสีการ์ดแจ้งเตือนระหว่าง "ลูกค้าใหม่" กับ "เบอร์ซ้ำ" ให้ทีมขายเห็นแล้วรู้ทันที

ใครทำระบบแจ้งเตือนลูกค้าเข้า LINE อยู่ ลองเช็ก 3 จุดนี้ดูนะครับ ช่วยลดงานแอดมินได้เยอะมาก!
```

### 🎬 YouTube / Short Video Hook
- **เชื่อมโยงกับ:** `YouTube Content Calendar #2 & #3`
- **Hook 5 วินาทีแรก:** *"ถ้ารอตอบลูกค้าเกิน 5 นาที โอกาสปิดการขายหายไปกว่า 80% — คลิปนี้มาดูวิธีทำระบบดึง Lead เข้าการ์ด LINE พร้อมปุ่มกดโทรออก ในงบ 0 บาทต่อเดือนครับ"*

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md`
- [x] เชื่อมต่อกับ `content_calendar.md` (คลิป #2 & #3)
- [x] บันทึกลงบนสุดของ `log.md`

