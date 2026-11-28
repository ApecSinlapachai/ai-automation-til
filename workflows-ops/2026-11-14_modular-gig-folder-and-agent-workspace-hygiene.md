---
title: "TIL #006: Refactoring สเปก 54 KB เป็น Modular Folder + .agents Rules/Skills & กฎข้อมูลจริง"
date: 2026-11-14
last_updated: 2026-11-14
domain: learning/til
category: business-workflow
status: ready-to-post
public_channels: [x, facebook-page]
tags: [til, learn-in-public, refactoring, modular-docs, agent-skills, solo-founder, scope-control]
---

# 🚀 TIL #006: Refactoring สเปก 54 KB เป็น Modular Folder + `.agents` Rules/Skills & กฎข้อมูลจริง

> **🎯 เป้าหมายของการบันทึกนี้:** สรุปบทเรียนสำคัญด้านการออกแบบ Workspace ให้ AI Agent ทำงานได้แม่นยำขึ้น และวินัยการคัดกรองข้อมูลจริงของ Solo Founder จากการลงมือสร้างระบบตลอดทั้งวัน

---

## 1. TL;DR — วันนี้ได้เรียนอะไรอีกบ้าง? (1–3 บรรทัด)
- **สิ่งที่ทำเพิ่มจากระบบหลัก:**
  1. แตกไฟล์สเปกขนาดใหญ่ (`54 KB`) ออกเป็นโฟลเดอร์ย่อยตามบริบทใช้งาน + แยก Technical Assets (`.json`, `.gs`, `.html`) ให้รันได้จริง
  2. ฝังพฤติกรรม Agent ผ่าน `.agents/rules/` และ `.agents/skills/` แทนการพิมพ์สั่งซ้ำในแชต
  3. ตรวจเช็กข้อมูลจริงในตลาด/ชีวิตจริงทันที (เช่น Fastwork.com 0% GP เลื่อนเปิดตัว, ล้างข้อมูลตัวอย่าง LDL, และล็อกลำดับทำทีละ Demo)

---

## 2. ปัญหาที่เจอ & บทเรียนที่ได้ (Problem → Lesson & Fix)
- **บทเรียนที่ 1: ไฟล์สเปกใหญ่ไฟล์เดียว (54 KB) ทำให้ AI เปลืองบริบทและหยิบโค้ดไปใช้ยาก**
  - **สิ่งที่แก้:** แตก `fastwork_gig_1_lead_automation.md` (`54 KB`) ออกเป็นโฟลเดอร์ `gigs/gig-01-lead-automation/` แยกตามจังหวะงานจริง:
    - `index.md` (ภาพรวม & ราคา)
    - `copywriting_fastwork.md` (ตอนอัปเดตหน้าขาย)
    - `sales_script_and_scope.md` (ตอนตอบแชตลูกค้า)
    - `execution_blueprint.md` (ตอนประกอบระบบ)
    - `assets/` (แยกไฟล์โค้ด `.json`, `.gs`, `.html` ออกมาเป็นไฟล์เดี่ยวพร้อมใช้งาน ไม่ต้องก๊อปปี้จากบล็อกโค้ดใน Markdown)
- **บทเรียนที่ 2: กฎที่อยากให้ AI ทำซ้ำทุกครั้ง ต้องฝังเป็น Workspace Rule & Skill (ไม่ใช่แค่บอกในแชต)**
  - **สิ่งที่แก้:** สร้าง `proactive-ai-os.md` (คุม 2-Tier Summary Bridge, เพดาน `AGENTS.md < 24 KB`, Proactive Risk Spotting) และ `daily-wrap-up/SKILL.md` เพื่อให้ Agent ใน Session ใหม่ๆ เดินตามมาตรฐานเดียวกันทันที
- **บทเรียนที่ 3: Reality Check & Modular Execution Discipline (Solo Founder Guardrail)**
  - **ตลาดเปลี่ยนต้อง Pivot ทันที:** เมื่อตรวจสอบว่า `Fastwork.com` (0% GP) เลื่อนเปิดตัวแบบไม่มีกำหนด จึงอัปเดตทุกไฟล์ที่เกี่ยวข้อง (`market_intel.md`, Playbook แคมเปญ YouTube, Content Calendar) ให้โฟกัส `fastwork.co` ปัจจุบันก่อน ไม่รอแพลตฟอร์มใหม่
  - **ห้ามปล่อย Placeholder/Sample Data ปนในระบบจริง:** ลบข้อมูลตัวอย่าง "LDL สูง" ออกจากทุกไฟล์และยึดเงื่อนไขจริง (Zone 2 Cardio 95+ นาที/สัปดาห์, นอน 8 ชม./วัน) เพื่อไม่ให้ Agent วางแผนธุรกิจผิดบริบท
  - **ทำทีละ Demo กัน Context Switching:** สำหรับระบบ AI Marketing วาง Roadmap ให้จบ **Gig #1** ก่อน → ค่อยทำ Demo 1 (Copywriting Page/X) → Demo 2 (AI Video Pipeline) ทีละชิ้น

---

## 3. หลักฐานอ้างอิง (Workspace Artifacts)
- **Modular Gig Folder:** `gigs/gig-01-lead-automation/`
- **Rules & Skills:** `.agents/rules/proactive-ai-os.md` • `daily-wrap-up/SKILL.md`
- **Roadmap & Market Pivot:** `Modular AI Marketing Roadmap` • `Market Intel`

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post
```text
TIL: 3 นิสัยจัดระเบียบไฟล์ที่ช่วยให้ AI Coding Agent เก่งขึ้นแบบเห็นได้ชัด:

1. อย่าเก็บสเปก 50+ KB ไว้ในไฟล์เดียว — แยกตามโหมดใช้งาน (หน้าขาย / สคริปต์คุยลูกค้า / Blueprint)
2. แยกโค้ด (.json / .gs / .html) ออกจากไฟล์ Markdown มาไว้ใน assets/ เพื่อให้รันหรือ Import ได้ทันที
3. กฎไหนที่ต้องสั่ง AI ซ้ำเกิน 2 รอบ ให้ย้ายไปเป็น Workspace Rule / Skill ถาวร!
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
บทเรียนเล็กๆ จากการ Refactor โปรเจกต์เอกสาร 54 KB ให้ AI Agent ทำงานแม่นยำขึ้น ⚡📂

ตอนแรกเราชอบรวมทุกอย่างของโปรเจกต์ไว้ในไฟล์ Markdown เดียว (ทั้ง Copywriting หน้าขาย, สคริปต์ตอบแชตลูกค้า, แผนลงมือทำ, และโค้ด JSON) จนไฟล์โตทะลุ 54 KB

พอใช้งานจริงเจอปัญหา 2 อย่าง:
❌ เวลาแค่จะแก้คำโปรยหน้าขาย Agent ก็ต้องอ่านสคริปต์และโค้ดทั้งหมดไปด้วย เปลือง Context
❌ เวลาจะเอาโค้ดไป Import จริง ต้องมานั่งก๊อปปี้จากใน Markdown ทีละก้อน

วิธีแก้ที่เวิร์กมากคือเปลี่ยนเป็น "Modular Folder + Assets":
✅ แยกไฟล์ตามจังหวะการใช้งาน (Overview / Copywriting / Sales Script / Blueprint)
✅ แยกไฟล์เทคนิค (.json / .gs / .html) ออกมาเป็นไฟล์จริงในโฟลเดอร์ assets/
✅ กฎการทำงานที่ใช้บ่อย (เช่น Checklist ซิงค์ไฟล์สรุป) ฝังไว้ใน `.agents/rules` และ `SKILL.md` แทนการพิมพ์บอกซ้ำทุกครั้ง

จัดบ้านให้ดีตั้งแต่ต้น ช่วยให้ทั้งคนและ AI ทำงานไวขึ้นเยอะเลยครับ!
```

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md`
- [x] บันทึกลงบนสุดของ `log.md`

