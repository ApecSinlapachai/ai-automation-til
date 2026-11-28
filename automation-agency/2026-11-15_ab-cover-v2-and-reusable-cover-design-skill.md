---
title: "TIL #009: ปั้นปก Fastwork A/B v2 (3D Device + Safe Zone) พร้อมแปลงเป็น Agent Skill ใช้ซ้ำ (.agents/skills/fastwork-cover-design)"
date: 2026-11-15
last_updated: 2026-11-15
domain: learning/til
category: automation-agency
status: ready-to-post
public_channels: [x, facebook-page]
tags: [til, learn-in-public, agent-skills, html-css, fastwork, design-system]
---

# 🚀 TIL #009: ปั้นปก Fastwork A/B v2 (3D Device + Safe Zone) พร้อมแปลงเป็น Agent Skill ใช้ซ้ำ (.agents/skills/fastwork-cover-design)

> **🎯 เป้าหมายของการบันทึกนี้:** สรุปบทเรียนการปรับปก Gig #1 สู่เวอร์ชัน A/B v2 เพื่อแก้จุดอ่อนเวลาถูกย่อบนมือถือ และการสกัดกระบวนการที่ทำซ้ำได้ให้เป็น Workspace Skill สำหรับ AI Agent

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่ทำ/เรียนรู้:** ออกแบบและ Render ปก Fastwork Draft 2 (`cover_gig01_v2`) ขนาด `2400×1600 px` แบบมี Mockup มือถือ 3D เอียงดึงสายตา พร้อมหน้า Preview เทียบความกว้าง `185px` และแปลงสูตรทั้งหมดเป็น Agent Skill ถาวรที่ `.agents/skills/fastwork-cover-design/SKILL.md`
- **ทำไมเรื่องนี้ถึงสำคัญ:** รูปปกในหน้าค้นหามือถือมีขนาดเล็กมาก การลด Text รกแล้วเน้น 1 พาดหัว + 1 Hero UI พร้อมแพ็กเป็น Skill ช่วยให้ทำปก Gig ถัดๆ ไปได้ไวและคุมมาตรฐานเดิมได้ทันที

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point / Bottleneck):**
  - ปกเวอร์ชันแรกมีกล่องข้อความอธิบายหลายจุดด้านล่าง เมื่อย่อลงเหลือ `185px` บนจอมือถือตัวหนังสือเล็กจะอ่านแทบไม่ออก
  - หากไม่บันทึกกฎและขั้นตอนไว้ในระบบ ทุกครั้งที่เริ่มทำ Gig ใหม่จะต้องมานั่งอธิบายขนาด, Safe Zone 15%, และคำสั่ง Render Chrome Headless ใหม่ทั้งหมด
- **แนวทาง/สถาปัตยกรรมที่เลือกใช้ (Solution & Trade-offs):**
  - แยกทำไฟล์ `cover_gig01_v2.*` เพื่อทำ A/B Test โดยไม่เขียนทับ Draft 1
  - ใช้กฎ **Grill-me Discovery ก่อนเขียนโค้ด** ใน `SKILL.md` (ห้าม Lock ธีมเป็นสีขาวโดยอัตโนมัติ และบังคับถาม ICP/โทนสี/Layout ก่อน)
- **ขั้นตอนที่ลงมือทำจริง (What I Built / Tested):**
  1. สร้าง `cover_gig01_v2.html` ด้วยแนวคิด Editorial Signal Pipeline + โทรศัพท์ 3D (`perspective(2200px)`) ที่ขยาย LINE Flex Card และโลโก้แอปให้เด่นขึ้น พร้อมโหมด `?guides=1` เช็กขอบซ้าย-ขวา `15%` (`360px`)
  2. สร้าง `cover_gig01_v2_preview.html` และ Render ภาพตรวจขนาด `2400×1600` (`cover_gig01_v2_2400x1600.png`) + ภาพ Preview Check (`cover_gig01_v2_preview_check.png`) เพื่อเทียบ A/B ที่ความกว้าง `185px`
  3. ติดตั้ง Workspace Skill `.agents/skills/fastwork-cover-design/SKILL.md` ครอบคลุม 4 ขั้นตอน: Grill-me Discovery → Technical & Safe Zone Standards → File Naming & Preview Inspector → Windows Chrome Headless Render Pattern

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `HTML/CSS`, `Inline SVG`, `CSS 3D Transform`, `Chrome Headless`, `Antigravity Workspace Skill (SKILL.md)`
- **ผลลัพธ์จริงที่วัดได้ (เฉพาะที่ทดสอบจริง):** ได้ภาพปก A/B v2 ขนาด `2400×1600 px` พร้อมภาพ Preview ตรวจ Safe Zone 15% และ Mobile `185px` รวมถึง Skill มาตรฐานที่เรียกใช้งานซ้ำได้ทันทีใน Workspace
- **ลิงก์ / ไฟล์อ้างอิงภายใน:** `Fastwork Gig #1` • `fastwork-cover-design/SKILL.md`
- **ลิงก์สาธารณะ (ถ้ามี):** *(รอเผยแพร่พร้อมหน้างาน Fastwork Gig #1)*

> [!CAUTION] PDPA & Security Check ก่อนเผยแพร่สาธารณะ
> - ปก Fastwork ห้ามใส่ LINE ID, QR Code, หรือเบอร์ติดต่อส่วนตัวเด็ดขาด
> - ข้อมูลใน Mockup บนภาพปกต้องเป็นข้อมูลตัวอย่างที่สังเกตได้ชัดเจน (เช่น `08X-XXX-XXXX`)

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post / Thread Hook
```text
TIL: ทำรูปปก Service 1 ครั้งแล้วไม่จบแค่รูปเดียว แต่แพ็กเป็น "AI Agent Skill" ไว้ใช้ซ้ำได้ตลอด 🔁

สิ่งที่ปรับใน Cover v2 (2400×1600 px):
1. ตัดกล่องข้อความรกด้านล่างออก ยึดกฎ 3-Second Mobile Readability (เทสย่อเหลือ 185px)
2. ใส่ Hero Mockup มือถือ 3D ด้วย CSS perspective + คุม Safe Zone ซ้ายขวา 15% (360px)
3. สกัด Prompt, Layout Rules และคำสั่ง Chrome Headless ลง SKILL.md พร้อมบังคับ Grill-me ถามธีมสีก่อนทุกครั้ง

รอบหน้าทำปกบริการใหม่ แค่เรียกสกิลเดียวจบ
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
🎨 จาก "ทำรูปปกด้วย HTML/CSS" สู่การสร้าง "Agent Skill ส่วนตัว" ที่ AI เรียกใช้ซ้ำได้ทุกโปรเจกต์

เวลาทำภาพปกบริการบนแพลตฟอร์ม Freelance ปัญหาใหญ่สุดคือพอโดนย่อเป็น Thumbnail บนมือถือ (~185px) ตัวหนังสือเล็กๆ จะอ่านไม่ออก แถมขอบซ้าย-ขวาอาจโดน Crop หาย

🛠️ วันนี้ลองปรับปกเวอร์ชัน 2 (A/B Test) และถอดบทเรียนออกมาเป็น Skill มาตรฐานให้ AI Agent ประจำตัว:
• ตัด Text รองที่ไม่จำเป็นออก ให้เหลือเฉพาะพาดหัวใหญ่ + โลโก้ + หน้าจอมือถือ 3D ที่เห็นผลลัพธ์ชัดใน 3 วินาที
• ฝังเส้นประ Safe Zone 15% (360px ซ้าย-ขวา) และหน้า Preview เทียบความกว้าง 185px ไว้ตรวจทุกครั้งก่อนส่งออก
• บันทึกกฎทั้งหมด + คำสั่ง Render ผ่าน Chrome Headless ลงไฟล์ SKILL.md (พร้อมเงื่อนไขให้ AI สัมภาษณ์เลือกโทนสีก่อนทำ ไม่ Lock ธีมเดิม)

💡 บทเรียนที่ได้:
ทุก Workflow ที่เราทำซ้ำเกิน 2 ครั้ง ถ้าแปลงเป็น Markdown Skill เก็บไว้ใน Workspace จะช่วยประหยัดเวลาและคุมคุณภาพงานรอบถัดไปได้มหาศาลครับ
```

### 🎬 YouTube / Short Video Hook *(ถ้าต่อยอดเป็นคลิปได้)*
- **ไอเดียชื่อคลิป:** `วิธีเปลี่ยนงานออกแบบที่ทำซ้ำๆ ให้เป็น AI Agent Skill ใช้ซ้ำได้ตลอดชีวิต`
- **Hook 5 วินาทีแรก:** `"อย่าเสียเวลาอธิบายสเปกงานเดิมๆ ให้ AI ฟังซ้ำทุกรอบครับ นี่คือวิธีแปลง Workflow ออกแบบภาพปก 2400x1600 ให้กลายเป็น Skill 1 ไฟล์"`

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md` (รวมถึง `Cross-Pillar Sync Signals` หากส่งต่อเข้า `02-Business`)
- [x] หากเป็นไอเดียคลิปใหม่ ให้เพิ่มเข้า `content_calendar.md`
- [x] บันทึกลงบนสุดของ `log.md`

