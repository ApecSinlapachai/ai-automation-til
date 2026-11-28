---
title: "TIL #015: Production-Hardening Google Apps Script Fast-Lead (v2.0/v3.0): แก้ Race Condition, Formula Injection & LINE Validate API"
date: 2026-11-18
last_updated: 2026-11-18
domain: learning/til
category: automation-agency
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, apps-script, line-flex, security, testing]
---

# 🚀 TIL #015: Production-Hardening Google Apps Script Fast-Lead (`v2.0` → `v3.0`): แก้ Race Condition, Formula Injection & LINE Validate API

> **🎯 เป้าหมายของการบันทึกนี้:** เปลี่ยนงาน Code Audit & Hardening ระบบรวม Lead ของ Fastwork Gig #1 จากระดับ "ใช้งานได้" ให้ทนทานระดับ "Production ส่งมอบลูกค้าจริง" พร้อมชุด Mock Test ตรวจจับ Edge Case ได้อัตโนมัติ

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่ทำ/เรียนรู้:** ตรวจและอัปเกรดสคริปต์ Google Apps Script รับ Lead จากฟอร์ม (`apps_script_lead_alert.gs` v2.0 และ `apps_script_lead_alert_v3.gs`) เพื่ออุดช่องโหว่ Race Condition, Formula Injection, บั๊กตัดคำ LINE ID (`lineman_shop` → `man_shop`), และยกระดับการ์ด LINE Flex พร้อมระบบ Fallback Email
- **ทำไมเรื่องนี้ถึงสำคัญ:** เวลาลูกค้าใช้งานจริงมักเจอเคสแปลกๆ ที่การเทส 1–2 ครั้งมองไม่เห็น (เช่น กดส่งพร้อมกัน, ลบแถวทดสอบแล้ว ID ชน, หรือ LINE ล่มชั่วคราว) การ Hardening + ทำ Offline Mock Test ช่วยลดความเสี่ยงงานพังหลังส่งมอบได้แทบเป็นศูนย์

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point / Bottleneck):**
  1. **Lead ID ชน & Concurrency:** เมื่อลบแถว `[TEST]` ทิ้ง เลข `LD-xxx` เดิมอาจถูกสร้างซ้ำ และหากฟอร์มส่งเข้ามาพร้อมกัน 2 คนอาจเกิด Race Condition เขียนทับหรือข้ามแถว
  2. **Parsing Edge Cases:** Regex เดิมตัดคำว่า `line` หน้า ID จริง (เช่น `lineman_shop` กลายเป็น `man_shop`), เบอร์โทรจากช่องรายละเอียดไปทับ LINE ID, และการเช็ก Checkbox ล่วงหน้าใน Google Sheets ทำให้ `getLastRow()` อ่านแถวผิด
  3. **Security & Resilience:** ข้อมูลจากฟอร์มที่ขึ้นต้นด้วย `=`, `+`, `-`, `@` เสี่ยง **Formula Injection** ลงใน Google Sheets, LINE ล่ม 5xx ไม่มี Retry, และเมื่อ LINE ส่งไม่ผ่านไม่มีแจ้งเตือนสำรอง
- **แนวทาง/สถาปัตยกรรมที่เลือกใช้ (Solution & Trade-offs):**
  1. **Lock + Monotonic Counter + Checkbox-Safe Scanner:** ใช้ `LockService.getScriptLock()` (`waitLock(25000)` + `finally` ปลดล็อกก่อนยิง API ภายนอก), เก็บ `LAST_LEAD_SEQ` ใน Properties ไม่ให้เลข ID ย้อนหลัง, และสแกนแถวสุดท้ายเฉพาะคอลัมน์ข้อมูลหลัก `A–E`
  2. **Sanitization & Strict Validation:** เพิ่ม `sanitizeSheetCell_()` กัน Formula Injection, ปรับ `extractPhone_` / `isValidThaiPhone_` / `extractLineId_` / `isValidLineId_` ให้แยกฟิลด์เด็ดขาด, และตัดลิงก์แปลกปลอมที่ผู้กรอกแนบมาใน Auto-Reply Email
  3. **Resilient Delivery & Zero-Quota Design QA:** เพิ่ม Retry + Backoff พร้อม `X-Line-Retry-Key` กันการ์ดเด้งซ้ำ, ส่งอีเมลแจ้งเตือนสำรองเมื่อ LINE ล้มเหลว, และเพิ่ม `testValidateFlexDesign()` ยิงเช็ก JSON ผ่าน LINE Validate API โดยไม่เสียโควตาข้อความ
- **ขั้นตอนที่ลงมือทำจริง (What I Built / Tested):**
  1. อัปเกรด `apps_script_lead_alert.gs` เป็น **v2.0** (Header Gradient ตาม Tier, Consent Meter, ปุ่มคัดลอกเบอร์/LINE ID, ลิงก์ตรงไปแถวใน CRM) และรวมจุดแข็งเข้า `apps_script_lead_alert_v3.gs`
  2. รัน Offline Mock Test จำลองสภาพแวดล้อม Google Apps Script ด้วย Node.js: ชุดตรวจ `v2` ผ่าน **`51/51` เคส** (เทียบกับ `v1` ที่ตก `26/45` เคส) และชุดตรวจความเสถียร 12 จุดผ่าน **`28/28` เคส**
  3. เพิ่มฟังก์ชัน Handover 1 คลิก (`installTrigger`, `checkSetup` / `checkSystemReadiness`, `testValidateFlexDesign`) เพื่อใช้ตรวจความพร้อมก่อนส่งมอบลูกค้า

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `Google Apps Script (LockService, PropertiesService, MailApp, UrlFetchApp)`, `LINE Messaging API (Flex + Validate API)`, `Node.js (Offline Mock Test Suite)`
- **ผลลัพธ์จริงที่วัดได้ (เฉพาะที่ทดสอบจริง):** Offline Mock Tests ผ่านครบ **`51/51` เคส** (Audit Suite) และ **`28/28` เคส** (Production Concurrency/Parsing Suite) โดยต้นทุนซอฟต์แวร์ยังคง **`฿0/เดือน`**
- **ลิงก์ / ไฟล์อ้างอิงภายใน:** `Fastwork Gig #1` • `Setup & Handover SOP`

> [!CAUTION] PDPA & Security Check ก่อนเผยแพร่สาธารณะ
> - ห้ามเปิดเผย `LINE_CHANNEL_ACCESS_TOKEN`, `LINE_TARGET_ID`, อีเมลจริง, หรือ Spreadsheet ID ในโค้ดตัวอย่าง/ภาพหน้าจอ
> - เตือนลูกค้าเสมอว่าทุกคนที่มีสิทธิ์ **Editor** ใน Google Sheet สามารถเปิดดู Script Properties ได้ จึงต้องจำกัดสิทธิ์เฉพาะผู้เกี่ยวข้อง

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post / Thread Hook
```text
TIL: ความต่างระหว่าง "สคริปต์รับ Lead ที่พอใช้ได้" กับ "ระดับส่งมอบลูกค้าจริง" อยู่ที่ Edge Cases เหล่านี้ครับ:

วันนี้ลองทำ Code Audit ระบบ Google Apps Script (฿0/ด.) แล้วเจอจุดพังที่คนมักมองข้าม:
1. ลบแถวทดสอบแล้ว Lead ID รันเลขซ้ำ
2. ชื่อ LINE ID ขึ้นต้นด้วย "line..." (เช่น lineman_shop) โดน Regex ตัดคำทิ้ง!
3. เตรียม Checkbox ไว้ล่วงหน้าในชีต ทำให้ getLastRow() ชี้ไปท้ายตารางแทนที่จะต่อแถวบน
4. คนกรอก "=IMPORTXML..." เข้ามา เสี่ยง Formula Injection ใน Google Sheets

แก้ครบทุกจุดด้วย LockService + Monotonic ID + Strict Parser + Retry-Key พร้อมทำ Offline Mock Test ผ่าน 51/51 เคสครับ
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
เบื้องหลังการตรวจโค้ดระบบรวม Lead (฿0/เดือน) ก่อนส่งมอบลูกค้าจริง — ทำไมเทสผ่านแค่ 1–2 รอบถึงยังไม่พอ?

หลายคนคิดว่าทำ Google Form ➡️ Google Sheets ➡️ แจ้งเตือน LINE แค่นี้ก็จบแล้ว แต่พอลองทำ Stress Test จำลองสถานการณ์หน้างานจริง วันนี้เจอเคสที่พร้อมจะพังได้ตลอดเวลาหลายจุดเลยครับ:

🔎 ตัวอย่างบั๊กจริงที่ดักเจอจากการ Audit:
• ลูกค้ากรอก LINE ID ที่มีคำว่า "line" นำหน้า → ฟังก์ชันตัดคำเก่าเผลอตัดหัวทิ้งจนแอดไม่ได้
• ส่งฟอร์มพร้อมกัน 2 คน → มีโอกาสเกิด Race Condition แถวชนกัน (ต้องแก้ด้วย LockService)
• ชีตมี Checkbox เตรียมไว้ 100 แถว → ข้อมูลใหม่กระโดดไปโผล่แถวที่ 101
• LINE API ล่มชั่วคราว → ถ้าไม่มี Retry + Fallback Email ลูกค้าหายเงียบ

🛠️ สิ่งที่อัปเกรดเพิ่มเข้าไปในเวอร์ชันใหม่:
✅ กัน Formula Injection ใน Google Sheets + ตัดลิงก์แปลกปลอมใน Auto-Reply
✅ เพิ่ม LINE Validate API Test เช็กการ์ด Flex 4 สถานะโดยไม่เสียโควตาข้อความ
✅ แปลงเป็น Offline Mock Test ด้วย Node.js รันเช็กซ้ำ 51 เคสผ่าน 100%

💡 บทเรียนที่ได้:
ความน่าเชื่อถือของ AI Agency ไม่ได้วัดกันที่ "ทำให้มันวิ่งได้" แต่อยู่ที่ "เรารู้ได้ยังไงว่ามันจะไม่พังตอนลูกค้าใช้งานจริงครับ"
```

### 🎬 YouTube / Short Video Hook *(ถ้าต่อยอดเป็นคลิปได้)*
- **ไอเดียชื่อคลิป:** `5 บั๊กอันตรายเวลาทำระบบรวม Lead เข้า Google Sheets + LINE (และวิธีแก้ให้ไม่พัง)`
- **Hook 5 วินาทีแรก:** `รู้ไหมครับว่าแค่คนกรอก LINE ID คำว่า lineman_shop สคริปต์ที่คุณเขียนไว้อาจตัดชื่อลูกค้าพังทันที... นี่คือ 5 จุดพังที่ผมเจอตอน Audit โค้ดส่งลูกค้าจริง`

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md`
- [x] บันทึกลงบนสุดของ `log.md`

