---
title: "TIL #013: อัปเกรด Gig #1 ครบ 100% + โลโก้ LINE OA 400×400 และ Apps Script Auto-Reply Email < 1 นาที"
date: 2026-11-17
last_updated: 2026-11-17
domain: learning/til
category: automation-agency
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, fastwork, apps-script, line-oa, auto-reply, security]
---

# 🚀 TIL #013: อัปเกรด Gig #1 ครบ 100% + โลโก้ LINE OA 400×400 และ Apps Script Auto-Reply Email < 1 นาที

> **🎯 เป้าหมายของการบันทึกนี้:** สรุปการปั้นระบบหลังบ้านและหน้าร้าน Fastwork Gig #1 ให้พร้อมรับทราฟฟิกจาก YouTube แบบ **Dogfooding (ใช้ระบบของตัวเองเป็น Live Proof)** โดยไม่หลุดข้อมูลลับและไม่ต้องเสียค่ารายเดือน SaaS

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่ทำ/เรียนรู้:** อัปโหลด Gallery ครบ **100% Profile Score**, สร้างรูปโปรไฟล์ LINE OA (`400×400 px` Light & Dark Safe Zone), และอัปเกรด `apps_script_lead_alert.gs` ให้ส่ง **Auto-Reply Email `< 1 นาที`** แยกตามเจตนา Lead (`Hot` / `Warm` / `Standard Client`) ผ่าน `Script Properties`
- **ทำไมเรื่องนี้ถึงสำคัญ:** ลูกค้าที่กรอกฟอร์มใต้คลิปจะได้สัมผัส **"Speed to Lead < 1 นาที"** จริงตั้งแต่วินาทีแรกที่กรอก โดยสคริปต์ยังปลอดภัย 100% ไม่มีลิงก์หรือ Token ส่วนตัวฮาร์ดโค้ดอยู่ในโค้ดเมื่อนำไปส่งมอบลูกค้า

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point / Bottleneck):**
  1. แพ็กเกจ Pro ระบุฟีเจอร์ *Auto-Welcome Email* แต่ใน `apps_script_lead_alert.gs` เดิมมีเฉพาะการยิง LINE Flex
  2. เมื่อนำโค้ดเดียวมาใช้ทั้งรับ Lead แคมเปญ YouTube และส่งมอบลูกค้า ถ้าฮาร์ดโค้ดลิงก์จองงานหรือชื่อแบรนด์ลงในโค้ด จะเสี่ยงหลุดข้อมูลส่วนตัวเวลาส่งมอบ (Pre-Delivery Secret Audit)
  3. หลังเพิ่ม `MailApp.sendEmail` ใน Google Apps Script หากผู้ใช้ไม่กด Run ฟังก์ชันใน Editor เพื่อขอสิทธิ์ `MailApp` ก่อน Trigger `onFormSubmit` จะติด Error สิทธิ์เงียบๆ ใน `try/catch` (LINE เด้งปกติแต่เมลไม่ส่ง)
- **แนวทาง/สถาปัตยกรรมที่เลือกใช้ (Solution & Trade-offs):**
  1. **Zero-Secret Configuration ผ่าน `Script Properties`:** แยกค่า `AUTO_REPLY_ENABLED`, `BOOKING_URL`, `BRAND_NAME`, `STARTING_PRICE`, `DEFAULT_CAMPAIGN` ออกจากโค้ดทั้งหมด และครอบ `sendAutoReplyEmail_` ด้วย `try/catch` + `MailApp.getRemainingDailyQuota()` เพื่อไม่ให้ปัญหาเมลไปขัดจังหวะ LINE Flex
  2. **1-Click Auth & Safe Mock Test:** แยก `testWithMockLead()` (ข้าม `@example.com` เพื่อไม่ยิงเมลปลอม) และเพิ่ม `testAutoReplyToOwner()` เพื่อกด Run ขอสิทธิ์ `MailApp` และทดสอบเข้า Gmail จริงได้ในคลิกเดียว
  3. **Positioning Guardrail ฝั่งลูกค้า:** เก็บโครงสร้าง BYOB `0% GP` ไว้เป็นกลไกหลังบ้าน ส่วนในอีเมล/หน้าคลิปฝั่งผู้ซื้อให้สื่อสารเป็น **ระบบคุ้มครองเงิน (Escrow) + ฟรีประเมิน Flow & ดูแล 30 วัน**
- **ขั้นตอนที่ลงมือทำจริง (What I Built / Tested):**
  1. อัปโหลดรูป Gallery ครบ 5+ ภาพดันคะแนน Gig #1 บน `fastwork.co` ขึ้น **`100%`** + ทดสอบลิงก์ BYOB ใน Incognito/Mobile
  2. สร้างรูปโปรไฟล์ LINE OA `400×400 px` ทั้งธีมสว่างและเข้ม (`line_oa_profile_fast_lead_400x400.png` / `_dark_400x400.png`)
  3. เพิ่ม `sendAutoReplyEmail_` และ `testAutoReplyToOwner()` ลงใน `apps_script_lead_alert.gs` (`node --check` ผ่าน และทดสอบส่งเข้า Inbox จริงสำเร็จ)

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `Google Apps Script (MailApp, UrlFetchApp, PropertiesService)`, `Google Sheets`, `LINE Messaging API (Flex)`, `HTML/CSS + Chrome Headless`
- **ผลลัพธ์จริงที่วัดได้ (เฉพาะที่ทดสอบจริง):** Gig #1 Profile Score `100%`, Auto-Reply Email ส่งเข้า Inbox จริงภายใน `< 1 นาที` จาก Apps Script (ต้นทุนซอฟต์แวร์ `฿0/เดือน`)
- **ลิงก์ / ไฟล์อ้างอิงภายใน:** `Fastwork Gig #1` • `Setup Checklist`

> [!CAUTION] PDPA & Security Check ก่อนเผยแพร่สาธารณะ
> - ห้ามเผยแพร่ภาพหน้าจอที่มี `LINE_CHANNEL_ACCESS_TOKEN`, `LINE_TARGET_ID`, Webhook URL หรือ Spreadsheet ID ส่วนตัวเด็ดขาด
> - ตัวอย่างอีเมลและเบอร์โทรในโพสต์/คลิปใช้ข้อมูลจำลองเท่านั้น (`@example.com`, `08X-XXX-XXXX`)

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post / Thread Hook
```text
TIL: ทำไมระบบแจ้งเตือน Lead ต้องมี "Auto-Reply ใน 1 นาที" พ่วงไปด้วย?

วันนี้ผมอัปเกรด Google Apps Script (฿0/ด.) ให้ทำ 3 อย่างพร้อมกันทันทีที่ลูกค้ากรอกฟอร์ม:
1. บันทึกลง Google Sheets + เช็กเบอร์ซ้ำ
2. เด้งการ์ด LINE Flex ปุ่มกดโทรออกหาเซลส์
3. ส่ง Email ตอบรับลูกค้าที่สนใจทันที (แยกเทมเพลต Hot vs Warm Lead)

ทริคสำคัญ: อย่า Hardcode ลิงก์/Token ลงในโค้ด ให้ดึงจาก Script Properties 100% เวลาเอา Template ไปติดให้ลูกค้าจะได้ไม่เสี่ยงข้อมูลส่วนตัวรั่วครับ
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
อัปเดตหน้าร้าน Fastwork ครบ 100% แล้ว — สิ่งที่ทำต่อทันทีคือ "ใช้ระบบของตัวเองรับลูกค้าตัวเอง" (Dogfooding) ครับ

ไหนๆ ก็ทำระบบรับ Lead แจ้งเตือนเข้า LINE ใน 1 วินาทีขายลูกค้า วันนี้เลยต่อยอดเอาโค้ด Google Apps Script เดิมมาเพิ่มระบบ Auto-Reply Email (< 1 นาที) สำหรับคัดกรองคนที่กรอกฟอร์มจากคลิป YouTube:

🔎 จุดที่เจอตอนเทสจริงแล้วคนมักพลาด:
พอเพิ่มคำสั่งส่ง Email (MailApp) ลงในโค้ดเดิม ถ้าไม่กด Run ฟังก์ชันใน Editor เพื่อขอสิทธิ์ใหม่ก่อน Trigger อัตโนมัติจะติด Permission Error เงียบๆ (LINE เด้งปกติ แต่เมลไม่ออก!)

🛠️ วิธีแก้ที่วางเพิ่มเข้าไป:
1. แยก try/catch ให้ส่งอีเมลล้มเหลวได้โดยไม่ทำให้การ์ด LINE Flex พังตาม
2. เก็บชื่อแบรนด์/ลิงก์จองงานไว้ใน Script Properties ทั้งหมด ไม่มีค่าส่วนตัวฝังในโค้ด
3. ทำฟังก์ชันเทสส่งเข้าอีเมลตัวเอง 1 คลิก เพื่อขอสิทธิ์และเช็ก Inbox ก่อนเปิดรับจริง

💡 บทเรียนที่ได้:
ระบบที่ดีที่สุดที่ใช้ขายลูกค้าได้ คือระบบที่เรากล้าเอามาใช้ปิดลูกค้าของตัวเองครับ
```

### 🎬 YouTube / Short Video Hook *(ถ้าต่อยอดเป็นคลิปได้)*
- **ไอเดียชื่อคลิป:** `วิธีทำให้ Google Form ส่ง LINE + ตอบ Email ลูกค้าอัตโนมัติใน 1 นาที (ฟรี ฿0/ด.)`
- **Hook 5 วินาทีแรก:** `รู้ไหมครับว่าแค่เพิ่มคำสั่งส่งเมลไม่กี่บรรทัดใน Google Sheets ลูกค้ากรอกฟอร์มปุ๊บ ได้อีเมลตอบกลับใน 1 นาทีแบบไม่ต้องเสียค่า SaaS สักบาท`

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md`
- [x] บันทึกลงบนสุดของ `log.md`

