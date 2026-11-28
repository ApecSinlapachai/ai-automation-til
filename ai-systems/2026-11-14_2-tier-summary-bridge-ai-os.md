---
title: "TIL #001: ออกแบบสถาปัตยกรรม 2-Tier Summary Bridge สำหรับ Personal AI OS"
date: 2026-11-14
last_updated: 2026-11-14
domain: learning/til
category: portfolio
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, ai-os, summary-bridge, obsidian, antigravity]
---

# 🚀 TIL #001: ออกแบบสถาปัตยกรรม 2-Tier Summary Bridge สำหรับ Personal AI OS

> **🎯 เป้าหมายของการบันทึกนี้:** ใช้เป็น Portfolio ชิ้นแรกสำหรับ Learn in Public แสดงวิธีวางโครงสร้างความรู้และ Rule ให้ AI Agent ทำงานข้าม 4 เสาหลักของชีวิตและธุรกิจได้โดยไม่เกิด Silo

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่สร้าง:** วางโครงสร้าง **Personal AI OS** ที่รวม 4 เสาหลัก (`01-Personal`, `02-Business`, `03-Learning`, `04-Competitive-Analysis`) ด้วยสถาปัตยกรรม **2-Tier Summary Bridge**
- **ทำไมเรื่องนี้ถึงสำคัญ:** ช่วยแก้ปัญหาเวลาใช้งาน AI Agent ในโปรเจกต์ที่มีไฟล์เยอะๆ แล้ว Agent มักจะลืมบริบทข้ามโฟลเดอร์ (Silo) หรือโหลดไฟล์ใหญ่เกินจำเป็นจนเปลือง Token

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point):**
  1. ถ้าโยนไฟล์ทั้งหมด (แผนธุรกิจ, สเปกงานลูกค้า, โน้ตหนังสือ, ตารางสุขภาพ) ไว้รวมกันแล้วให้ AI อ่านหมด Context จะบวมเร็วและตอบหลุดโฟกัส
  2. ถ้าแยกโฟลเดอร์เป็นโปรเจกต์ย่อย (`Sub-Projects`) โดยไม่มีจุดเชื่อม ข้อมูลสำคัญจะติดอยู่ใน Silo (เช่น เป้าหมายการนอน/เวลาว่างไม่ถูกส่งไปคุมขอบเขตงานธุรกิจ)
- **แนวทางที่ออกแบบ (2-Tier Summary Bridge):**
  - **Tier 1 — Executive Summary & Rollup Layer:** ทุกเสาหลักมี `index.md` และตารางสรุปย่อย (1–3 บรรทัดต่อเรื่อง) พร้อมส่วน **`Cross-Pillar Sync Signals`** สำหรับส่งสัญญาณข้ามเสาหลัก
  - **Tier 2 — Deep Spec Layer:** เก็บสเปกละเอียด (เช่น Blueprint งานลูกค้า, รายงานวิจัยเต็ม, สคริปต์) แยกไว้เฉพาะในโฟลเดอร์ย่อย อ่านเฉพาะตอนลงมือทำเรื่องนั้น
  - **Rule File Budget (`AGENTS.md < 24 KB`):** แยกคู่มือยาวๆ ออกเป็นไฟล์ Handbook เพื่อไม่ให้ไฟล์ Rule ของ Sub-Project ถูกตัดทอน

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `Markdown` + `Obsidian Wikilinks` + `Google Antigravity` (`AGENTS.md` + Custom Rules/Skills)
- **สิ่งที่ทำงานได้จริงในระบบตอนนี้:**
  - เปิด Workspace ระดับ Root (`AI OS`) → Agent อ่านเฉพาะ `index.md` และ Rollup เพื่อเชื่อมโยงภาพรวมทั้ง 4 เสาหลัก
  - เปิดโฟลเดอร์ย่อย (เช่น `02-Business` หรือ `03-Learning`) → Agent โหลดบทบาทเฉพาะทางจาก `AGENTS.md` ประจำเสาหลักทันที และมี Skill (`daily-wrap-up`) คอยซิงค์ผลสรุปกลับขึ้นตารางหลัก
- **ไฟล์อ้างอิงภายใน:** `Command Center`, `Master Prompt`, `Daily Wrap-Up & Bridge Sync Skill`

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post
```text
TIL: วิธีจัดการไฟล์ให้ AI Agent ไม่ลืมบริบทข้ามโปรเจกต์ และไม่กิน Token ทิ้ง

ผมลองจัด Personal AI OS ด้วยโครงสร้าง "2-Tier Summary Bridge":
1. แยกไฟล์สรุป (1-3 บรรทัด) ออกจากไฟล์สเปกยาวๆ
2. ทุกโฟลเดอร์มี index.md + "Cross-Pillar Signals" คุยกัน
3. คุม AGENTS.md ให้เล็กกว่า 24 KB

เปิด Root ก็เห็นภาพรวม เปิดโฟลเดอร์ย่อยก็ลงลึกได้ทันทีครับ
```

### 📘 Facebook Page — Story Post
```text
แชร์ไอเดียจัดระบบความรู้ให้ AI Agent ทำงานร่วมกันข้ามโปรเจกต์แบบไม่เปลือง Token 🧠⚡

ปัญหาหนึ่งที่เจอบ่อยเวลาใช้ AI Agent กับงานจริงหลายอย่างพร้อมกัน (ทั้งงานธุรกิจ, เนื้อหาเรียน, และตารางส่วนตัว) คือ:
❌ ถ้ารวมทุกไฟล์ไว้ที่เดียว AI จะอ่านหนักและเปลือง Context
❌ ถ้าแยกโฟลเดอร์เด็ดขาด ข้อมูลก็คุยกันไม่ได้ (เกิด Silo)

วันนี้เลยลองออกแบบสถาปัตยกรรม "2-Tier Summary Bridge" บน Obsidian + Antigravity:
1️⃣ Tier 1 (Summary Bridge): ใช้ index.md และตารางสรุปสั้นๆ 1-3 บรรทัด + ส่วน "Cross-Pillar Signals" ส่งต่อข้อมูลสำคัญข้ามโฟลเดอร์
2️⃣ Tier 2 (Deep Files): เก็บรายละเอียดเชิงลึก/โค้ด/สเปกงานลูกค้าแยกไว้ อ่านเฉพาะตอนลงมือทำเรื่องนั้น
3️⃣ คุมไฟล์ Rule (AGENTS.md) ของแต่ละโปรเจกต์ย่อยให้กระชับต่ำกว่า 24 KB

ผลคือเวลาเปิดระดับบนสุด Agent เห็นภาพรวมทั้งชีวิตและงานได้ไวมาก และพอเปิดโฟลเดอร์ย่อยก็เปลี่ยนหมวกเป็นผู้เชี่ยวชาญเฉพาะทางได้ทันที

ใครใช้ Markdown + AI Agent จัดระบบงานอยู่ แชร์ท่าที่ใช้กันในคอมเมนต์ได้นะครับ!
```

### 🎬 YouTube Hook (เชื่อมกับคลิป #1 ใน Content Calendar)
- **ไอเดียชื่อคลิป:** *"วิธีสร้าง Personal AI OS คุมทั้งชีวิตและธุรกิจด้วย Markdown + Obsidian (ไม่เกิด Silo + ประหยัด Token)"*
- **เชื่อมโยง:** `YouTube Content Calendar #1`

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md`
- [x] เชื่อมต่อกับ `content_calendar.md` (คลิป #1)
- [x] บันทึกลงบนสุดของ `log.md`

