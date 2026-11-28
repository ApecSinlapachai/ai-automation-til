---
title: "TIL #007: ทดสอบระบบ Fast-Lead Automation E2E 7 เคส — แก้บั๊ก Google Sheets ตัดเลข 0, Fuzzy Form Parser และ Dynamic LINE Flex"
date: 2026-11-15
last_updated: 2026-11-15
domain: learning/til
category: automation-agency
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, apps-script, google-sheets, line-flex, e2e-testing]
---

# 🚀 TIL #007: ทดสอบระบบ Fast-Lead Automation E2E 7 เคส — แก้บั๊ก Google Sheets ตัดเลข 0, Fuzzy Form Parser และ Dynamic LINE Flex

> **🎯 เป้าหมายของการบันทึกนี้:** บันทึกบทเรียนจากการทดสอบระบบรับ Lead จริงแบบ End-to-End (Google Form → Google Sheets Mini-CRM → LINE Messaging API) ที่ช่วยเปลี่ยนโค้ดตัวอย่างให้ทนทานต่อข้อมูลลูกค้าจริงในหน้างาน

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่ทำ/เรียนรู้:** ทดสอบระบบ Fast-Lead Automation แบบ E2E ครบ 7 เคสบน Google Form + Google Sheets + LINE Official Account จริง และอัปเกรด `apps_script_lead_alert.gs` ให้รับมือ Edge Case หน้างานได้ครบ
- **ทำไมเรื่องนี้ถึงสำคัญ:** การทดสอบจริงเผยจุดตายที่โค้ด Prototype มักมองข้าม เช่น Google Sheets ตัดเลข `0` นำหน้าเบอร์จนเช็กข้อมูลซ้ำพลาด, หัวข้อฟอร์มไม่ตรงตัวอักษรเป๊ะ, และลูกค้ากรอก LINE ID แทนเบอร์โทร

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point / Bottleneck):**
  1. หัวข้อคำถามใน Google Form ที่สร้างหรือปรับแก้เองอาจไม่ตรงตัวอักษรเป๊ะกับ Key ในโค้ด (`e.namedValues`)
  2. เมื่อกรอกเบอร์โทรลง Google Sheets ระบบจะมองเป็นตัวเลขและตัดเลข `0` นำหน้าออก ทำให้ Logic ตรวจซ้ำเทียบค่าไม่ตรงและส่งต่อเป็น `tel:` ผิดรูป
  3. ลูกค้าบางส่วนกรอกเป็น LINE ID แทนเบอร์โทร ทำให้ปุ่ม `📞 โทรหาลูกค้าทันที` บนการ์ด LINE Flex ใช้งานไม่ได้
  4. การคัดกรอง Lead ต้องแยกเจตนาชัดระหว่างผู้ที่สนใจจ้างงานจริงกับผู้ที่ขอลุ้นสิทธิ์ฟรีอย่างเดียว
- **แนวทาง/สถาปัตยกรรมที่เลือกใช้ (Solution & Trade-offs):**
  - ใช้ระบบจับคู่แบบ 3 ชั้น: Exact Header → Fuzzy Keyword Match → Regex Scanner (`e.values`) แทนการพึ่งชื่อคอลัมน์ตายตัว
  - บังคับ Format คอลัมน์ติดต่อ (`D`) เป็น Plain Text (`@`) พร้อม Normalize เบอร์โทร/LINE ID ก่อนเช็กซ้ำและก่อนส่งเข้า LINE Flex
- **ขั้นตอนที่ลงมือทำจริง (What I Built / Tested):**
  1. เพิ่ม `pickFieldByKeywords_()` และ `findPhoneInValues_()` / `findEmailInValues_()` ใน `apps_script_lead_alert.gs` และเพิ่ม `.trim().replace(/^Bearer\s+/i, '')` ให้ Token จาก `Script Properties` เพื่อป้องกัน Error `401`
  2. แก้ฟังก์ชัน `processAndNotifyLead_()` ให้บังคับ `setNumberFormat('@')` ที่คอลัมน์ `D`, รองรับตรวจซ้ำทั้งเบอร์โทรและ LINE ID (`🟢 ลูกค้าใหม่` vs `⚠️ ข้อมูลซ้ำ` พร้อมเปลี่ยนสีหัวการ์ดเป็นส้ม `#EA580C`)
  3. เพิ่มระบบ Lead Scoring 3 ระดับ (`🔥 Hot Lead` / `🟡 Warm Lead` / `🔵 Cold Lead`) พร้อมต่อท้ายสถานะ `สนใจ Fastwork` / `ไม่สนใจ Fastwork` ในคอลัมน์ `Notes` อัตโนมัติ และปรับปุ่ม LINE Flex แบบ Dynamic (สีฟ้า `tel:` เมื่อเป็นเบอร์ / สีเขียว `line.me/ti/p/...` เมื่อเป็น LINE ID)
  4. จัดทำ `setup_checklist.md` (SOP ตั้งค่าแบบสั้น) และซิงค์สถานะ `⚠️ ข้อมูลซ้ำ` ให้ตรงกันใน `n8n_fast_lead_workflow.json`

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `Google Forms`, `Google Sheets`, `Google Apps Script (฿0/เดือน)`, `LINE Messaging API (Flex Message)`, `n8n JSON`
- **ผลลัพธ์จริงที่วัดได้ (เฉพาะที่ทดสอบจริง):** ผ่านการทดสอบ E2E จริง 7 เคสครอบคลุม Hot/Warm/Cold Lead, การตรวจเบอร์/LINE ซ้ำ, และปุ่ม Dynamic CTA บน LINE Flex (พร้อมใช้งานจริงที่ต้นทุนค่า Script ฿0/เดือน)
- **ลิงก์ / ไฟล์อ้างอิงภายใน:** `apps_script_lead_alert.gs` • `setup_checklist.md`
- **ลิงก์สาธารณะ (ถ้ามี):** *(รออัปโหลดตัวอย่างพร้อมเปิดขาย Fastwork Gig #1)*

> [!CAUTION] PDPA & Security Check ก่อนเผยแพร่สาธารณะ
> - ห้ามเผยแพร่ Google Sheet ID, LINE Channel Access Token, User/Group ID, หรือข้อมูลติดต่อผู้ทดสอบจริงในโพสต์และภาพหน้าจอ
> - ตัวอย่างข้อมูลในภาพ Gallery ต้องใช้แถว `[DEMO]` หรือเบอร์/ชื่อสมมติเท่านั้น

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post / Thread Hook
```text
TIL: ทดสอบระบบรับ Lead อัตโนมัติ E2E แค่ 7 เคส เจอบั๊กหน้างานที่โค้ด Prototype มักมองข้าม 👇

1. Google Sheets ตัดเลข 0 นำหน้าเบอร์โทรอัตโนมัติ → ทำระบบเช็กเบอร์ซ้ำพัง (ต้องบังคับ Text Format '@' + Normalize ก่อนเทียบ)
2. ลูกค้ากรอก LINE ID แทนเบอร์ → ปุ่มโทรออกบน LINE Flex กดไม่ได้ (ต้องทำปุ่ม Dynamic แยกสีฟ้า tel: vs สีเขียว แอด LINE)
3. หัวข้อ Form เพี้ยนนิดเดียว Parser พัง → เพิ่ม Fuzzy Keyword + Regex Fallback จาก e.values

ระบบราคา ฿0/เดือนบน Apps Script ถ้าเทสหน้างานไม่ครบ ส่งงานลูกค้ามีพังแน่นอนครับ
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
🧪 เบื้องหลังการเทสระบบแจ้งเตือน Lead 7 เคส ก่อนเปิดให้บริการจริง (เจอบั๊กเพียบที่ Demo ทั่วไปไม่เคยบอก!)

วันนี้ลองทดสอบระบบ Google Form → Google Sheets Mini-CRM → LINE Flex Message (รันฟรี ฿0/เดือนบน Google Apps Script) แบบ End-to-End 7 เคสจริง

🔎 ปัญหาที่เจอตอนเทสจริง:
• Google Sheets แปลงเบอร์มือถือเป็นตัวเลขแล้ว "ตัดเลข 0 ข้างหน้าทิ้ง" ทำให้เช็กเบอร์ซ้ำพลาด
• ลูกค้าบางคนใส่ LINE ID แทนเบอร์โทร ปุ่ม "โทรหาลูกค้าทันที" เลยใช้ไม่ได้
• แค่ Copy Token มาติดช่องว่างหรือคำว่า Bearer ก็ยิง LINE API ติด 401 ได้ง่ายๆ

🛠️ สิ่งที่แก้เพิ่มเข้าไปในระบบ:
✅ บังคับคอลัมน์เบอร์/LINE เป็น Plain Text อัตโนมัติ + Normalize ก่อนตรวจซ้ำ (เจอซ้ำเปลี่ยนหัวการ์ดเป็นสีส้มทันที)
✅ ปรับปุ่ม LINE Flex แบบ Dynamic: กรอกเบอร์ขึ้นปุ่มสีฟ้า "โทรทันที" / กรอก LINE ID ขึ้นปุ่มสีเขียว "แอด LINE"
✅ จัดลำดับ Hot / Warm / Cold Lead อัตโนมัติจากคำตอบคัดกรองบนฟอร์ม

💡 บทเรียนที่ได้:
ความต่างของ "โค้ดเดโม" กับ "ระบบที่ลูกค้าเอาไปใช้หาเงินได้จริง" อยู่ที่การไล่เก็บ Edge Case หน้างานนี่แหละครับ
```

### 🎬 YouTube / Short Video Hook *(ถ้าต่อยอดเป็นคลิปได้)*
- **ไอเดียชื่อคลิป:** `3 บั๊กปราบเซียนตอนทำระบบรวม Lead เข้า Google Sheets + แจ้งเตือน LINE (ที่คนทำ Automation มักพลาด)`
- **Hook 5 วินาทีแรก:** `"ใครเคยทำระบบเก็บเบอร์ลูกค้าลง Google Sheets แล้วเลข 0 ข้างหน้าหาย แถมเช็กเบอร์ซ้ำพังบ้างครับ? วันนี้ผมเทสจริง 7 เคสแล้วเจอวิธีแก้จบในโค้ดเดียว"`

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md` (รวมถึง `Cross-Pillar Sync Signals` หากส่งต่อเข้า `02-Business`)
- [x] หากเป็นไอเดียคลิปใหม่ ให้เพิ่มเข้า `content_calendar.md`
- [x] บันทึกลงบนสุดของ `log.md`

