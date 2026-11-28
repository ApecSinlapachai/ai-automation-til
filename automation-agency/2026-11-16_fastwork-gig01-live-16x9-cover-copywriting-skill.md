---
title: "TIL #010: เปิดขาย Gig #1 บน Fastwork พร้อมปก 16:9 Safe Zone, สกิล Copywriting และ BYOB 0% GP"
date: 2026-11-16
last_updated: 2026-11-16
domain: learning/til
category: automation-agency
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, fastwork, cover-design, copywriting, agent-skills]
---

# 🚀 TIL #010: เปิดขาย Gig #1 บน Fastwork พร้อมปก 16:9 Safe Zone, สกิล Copywriting และ BYOB 0% GP

> **🎯 เป้าหมายของการบันทึกนี้:** สรุปบทเรียนจากการนำ **Fastwork Gig #1 (ระบบรวม Lead อัตโนมัติ)** ขึ้นขายจริงบน `fastwork.co` ตั้งแต่แก้บั๊กฟอนต์/ลูกศรบนมือถือ, ปรับสเปกภาพ `16:9` Safe Zone, สกัด Agent Skill ถาวร ไปจนถึงพบโอกาส **BYOB 0% GP**

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่ทำ/เรียนรู้:** ปรับปกและ Gallery ของ Gig #1 เป็นสเปก `2400×1350 (16:9)` ที่ผ่าน Visual QA บนการ์ด `3:2` จริง, ถอดบทเรียนหน้าขายเป็นสกิล `fastwork-copywriting`, เปิดขาย **Gig #1 Approved Live** บน `fastwork.co`, และวางระบบ **Basic Handover SOP** + **BYOB 0% GP**
- **ทำไมเรื่องนี้ถึงสำคัญ:** เปลี่ยนงานที่เคยเป็น Spec บนกระดาษให้กลายเป็นบริการที่เปิดรับออเดอร์ได้จริง พร้อมชุด Reusable Agent Skills สำหรับใช้ซ้ำใน Gig ถัดไป

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point / Bottleneck):**
  1. หน้าอัปโหลดจริงบน `fastwork.co` ใช้สัดส่วน **`16:9` (Gallery)** และครอปซ้าย-ขวาเป็นการ์ด **`3:2` (`298 × 198.66 px`)** ทำให้สเปก `3:2` เดิมโดนตัดขอบหรือสัดส่วนคลาดเคลื่อน และภาพแนวตั้งหน้าจอมือถือถูกครอปปุ่มโทรออกด้านล่าง
  2. ข้อความบนหน้าขาย Fastwork เมื่อใช้ `->` ระบบจะ Escape เป็น `-&gt;` และตัวหัวข้อบนมือถือตัดที่ ~2 บรรทัด (~60–75 ตัวอักษร)
  3. การรับงานผ่านหน้าเว็บปกติมีค่าธรรมเนียมแพลตฟอร์ม (~12–15%)
- **แนวทาง/สถาปัตยกรรมที่เลือกใช้ (Solution & Trade-offs):**
  1. ปรับ Canvas เป็น **`2400×1350 px (16:9)`** โดยล็อก Safe Margin ใหม่ที่ **`270 px` ซ้าย-ขวา / `82 px` บน-ล่าง** (เหลือขอบหายใจ `12 px` รอบการ์ด `3:2` ทุกด้าน) และหุ้มภาพหน้าจอมือถือจริงไว้ในกรอบ 16:9 เพื่อไม่ให้โดนครอปปุ่มสำคัญ
  2. สกัดกฎหน้าขายจริงเป็นสกิล `.agents/skills/fastwork-copywriting/SKILL.md` (ใช้ Emoji `➡️` แทน `->`, เขียนภาษาไทยสไตล์ Human-Write, วางโครงสร้าง 9 ส่วน + 3 แพ็กเกจ)
  3. ใช้ลิงก์ **BYOB (Bring Your Own Buyer)** ของ Fastwork สำหรับ Traffic นอกแอป (YouTube / Outreach) เพื่อรับสิทธิ์ **0% ค่าบริการ** และยังได้นับยอดขาย+รีวิวดันอันดับค้นหาบนแพลตฟอร์ม
- **ขั้นตอนที่ลงมือทำจริง (What I Built / Tested):**
  1. สร้าง `cover_gig01_v4_2400x1350.png` + Gallery เสริม 3 ภาพ (`packages`, `workflow`, `line_flex` Safe-Zone)
  2. สร้าง `.agents/skills/fastwork-copywriting/SKILL.md` และอัปเดต `.agents/skills/fastwork-cover-design/SKILL.md` (§6–7)
  3. เผยแพร่ **Gig #1** จนผ่านการอนุมัติ (`Live & Approved`) พร้อมจัดทำ **Basic Handover SOP** และสคริปต์เสนองานในบอร์ด/Outreach

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `HTML/CSS`, `Chrome Headless`, `Agent Skills (SKILL.md)`, `Fastwork BYOB`, `Google Sheets + Apps Script + LINE Flex`
- **ผลลัพธ์จริงที่วัดได้ (เฉพาะที่ทดสอบจริง):**
  - Gig #1 ผ่านการอนุมัติและขึ้นหน้าขายจริง (`Live & Approved`)
  - ผลิตปก `v4` + Gallery เสริม 3 ภาพขนาด `2400×1350` ผ่าน 2-Pass Visual QA ครบทุกภาพ พร้อมคำอธิบายภาพ ≤ 100 ตัวอักษรไม่ซ้ำกัน
  - ได้ Reusable Skills 2 ตัว (`fastwork-cover-design` + `fastwork-copywriting`) สำหรับสเกลบริการถัดไป
- **ลิงก์ / ไฟล์อ้างอิงภายใน:** `Gig #1 Overview`, `Basic Handover SOP`, `Fastwork Gigs Summary`

> [!CAUTION] PDPA & Security Check ก่อนเผยแพร่สาธารณะ
> - ไม่มี API Key, LINE Token, Webhook URL, หรือลิงก์บัญชี/ชีตส่วนตัวในโพสต์สาธารณะนี้
> - กรณีนำภาพหน้าจอมือถือ LINE Flex ไปใช้เป็น Showcase สาธารณะ ให้ใช้เฉพาะข้อมูล Mock/Demo เท่านั้น

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post / Thread Hook
```text
TIL: กว่าจะมี 1 Gig ที่เปิดขายได้จริงบน Fastwork ต้องแก้เรื่องเล็กๆ ที่คนมองข้ามเยอะมาก

วันนี้เอา Gig #1 (ระบบรวม Lead อัตโนมัติ) ขึ้นขายจริงจน Approved แล้ว สิ่งที่ได้เรียนรู้:

1. ภาพ 16:9 ต้องเว้น Safe Zone ซ้าย-ขวา 270px ไม่งั้นโดนครอปเป็น 3:2 แล้วข้อความขาด
2. ห้ามพิมพ์ "->" ในรายละเอียดงาน เพราะระบบแปลงเป็น "-&gt;" ให้ใช้ Emoji "➡️" แทน
3. ทุกการแก้ปัญหาที่ทำซ้ำได้ ให้สกัดเก็บเป็น Agent Skill ทันที — รอบหน้าเร็วขึ้น 5x

เปลี่ยนงานลงมือทำหนึ่งครั้ง ให้กลายเป็น Asset ที่ใช้ซ้ำได้ทั้งระบบครับ
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
เปิดตัวบริการ Automation แรกบน Fastwork สำเร็จแล้ว — แต่วันนี้ไม่ได้มาแค่อวดงาน แต่เอา "3 บทเรียนหน้างานจริง" มาฝากครับ

กว่าจะกด Publish แล้วผ่าน Approve มีรายละเอียดจุกจิกที่ถ้าไม่ลองทำเองจะไม่เจอเลย:
🔎 1. กับดักสัดส่วนภาพ: หน้าอัปโหลดเป็น 16:9 แต่การ์ดหน้ารวมครอปเป็น 3:2 ถ้าไม่คำนวณ Safe Margin ไว้ล่วงหน้า ปุ่มหรือตัวเลขสำคัญโดนตัดทิ้งหมด
🛠️ 2. รายละเอียดเล็กน้อยบนมือถือ: สัญลักษณ์ "->" โดนระบบ Escape เป็น "-&gt;" และหัวข้อบนมือถือตัดที่ ~2 บรรทัด ถ้าคำสำคัญไม่อยู่ข้างหน้า คนเลื่อนผ่านทันที
💡 3. แปลงทุกบทเรียนเป็น System: ผมจับกฎทั้งหมดที่แก้หน้างานจริง (ทั้งสัดส่วนภาพ + โครงสร้าง Copy ภาษาไทย) สกัดเก็บเป็น Agent Skill ไว้ใช้ซ้ำในโปรเจกต์ถัดไปเรียบร้อย

ใครกำลังทำ Freelance หรือ AI Agency อยู่ ลองเก็บ SOP จากงานแรกของตัวเองตั้งแต่วันนี้เลยครับ งานต่อไปจะไวขึ้นคนละเรื่อง!
```

### 🎬 YouTube / Short Video Hook *(ถ้าต่อยอดเป็นคลิปได้)*
- **ไอเดียชื่อคลิป:** `เบื้องหลังเปิดขายระบบ AI Automation ชิ้นแรกบน Fastwork (แก้จุดพลาดที่คนส่วนใหญ่ไม่รู้)`
- **Hook 5 วินาทีแรก:** `ทำระบบเสร็จ 100% แต่คนไม่คลิกซื้อบน Fastwork เพราะพลาดแค่ 2 จุดนี้บนหน้าจอมือถือ...`

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md`
- [x] บันทึกลงบนสุดของ `log.md`

