---
title: "TIL #016: ปั้น YouTube Branding + 3-Stage Media Workspace และปล่อย EP #1 จริงพร้อม Repurpose"
date: 2026-11-18
last_updated: 2026-11-18
domain: learning/til
category: workflows-ops
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, youtube, branding, ffmpeg, obs, workflow]
---

# 🚀 TIL #016: ปั้น YouTube Branding + 3-Stage Media Workspace และปล่อย EP #1 จริงพร้อม Repurpose

> **🎯 เป้าหมายของการบันทึกนี้:** สรุปการวางระบบผลิตวิดีโอตั้งแต่ต้นน้ำ (OBS + Branding) จนถึงปลายน้ำ (YouTube Publish + Social Repurpose) ให้ Solo Founder ปล่อยคลิปได้ไวและปลอดภัยกับ Git Repository

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่ทำ/เรียนรู้:** ออกแบบชุดภาพ Channel Branding (`800×800` Profile + `2560×1440` Safe-Area Banner), วางโครง **3-Stage YouTube Media Workspace** (`01-raw` → `02-autocut` → `03-ready`), อัปเกรดสคริปต์ `auto_cut_video.py`, และปล่อยคลิป **EP #1 (`07:05`)** ขึ้น YouTube พร้อมโพสต์ **FB Page & X Thread** ได้จริงในวันเดียว
- **ทำไมเรื่องนี้ถึงสำคัญ:** เปลี่ยนจาก "มีไอเดียคลิปค้างในไฟล์" ให้กลายเป็น **Published Asset ที่มีทราฟฟิกจริง** และได้ Pipeline ถ่าย-ตัด-อัปโหลดที่พร้อมทำซ้ำใน EP ถัดไปโดยไม่กินเวลาหรือดันไฟล์วิดีโอหนักๆ ขึ้น Git

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point / Bottleneck):**
  1. ไฟล์วิดีโอดิบจาก OBS และโปรเจกต์ตัดต่อกระจัดกระจาย เสี่ยงเผลอ Backup ไฟล์ขนาดใหญ่เข้า GitHub และเสียเวลาพิมพ์ Path ยาวๆ เวลาสั่งสคริปต์ FFmpeg Auto-Cut
  2. ภาพโปรไฟล์และ Banner ช่องต้องดูเข้าชุดกันและไม่โดน Crop บนมือถือ/ทีวี (Safe Area `1546×423 px`)
  3. ในคลิป EP #1 ที่ตัดเสร็จ (`Ep1Edited.mp4`, `07:05`) ช่วงกลางเล่าเฉพาะ Architecture (`02:56–04:55`) ยังไม่มี Live Demo จริง — ถ้าฝืนรออัดใหม่ทั้งคลิปจะเสีย Momentum
- **แนวทาง/สถาปัตยกรรมที่เลือกใช้ (Solution & Trade-offs):**
  1. **Channel Branding ผ่าน HTML/CSS + Chrome Headless:** สร้างแผ่น QA มัลติดีไวซ์ (`channel_final_preview.png`) เทียบให้เห็นระยะ Crop จริง แล้วล็อก Vector Profile (`profile-vector-d2-v1-800x800.png`) คู่ Light-Grid Banner (`2560×1440 px`)
  2. **3-Stage Media Workspace + Git Safety:** แยกโฟลเดอร์ `01-raw/` (OBS Output) → `02-autocut/` (FFmpeg Output `-16 LUFS`) → `03-ready/` (Final Export) + `_scratch/` และบล็อกทุกนามสกุลวิดีโอใน `.gitignore` พร้อมเพิ่ม Flag `--latest` และ `--inspect` ให้ `auto_cut_video.py`
  3. **Ship First, Spin-off Demo Later:** สแกน Timestamps จริงจาก `Ep1Edited.mp4` ให้ตรงเป๊ะ แล้วปล่อย **EP #1** ทันที ส่วน Live Demo จริงแยกไปทำเป็นคลิป Spin-off / Short #3 แทน
- **ขั้นตอนที่ลงมือทำจริง (What I Built / Tested):**
  1. เรนเดอร์ชุด Channel Branding (`800×800` และ `2560×1440`) และล็อกเวอร์ชันใช้งานจริง
  2. วางโครง `02-Business/YouTube/media/` + อัปเดต `auto_cut_video.py` และ `.agents/skills/youtube-ep-packaging/SKILL.md` (Step 4B)
  3. เติม Google Form + Invite Code `HALO-WISH-KING`, อัปโหลด **EP #1** (`https://youtu.be/V7EboxNpv0Y`) และโพสต์ Repurpose **Post A (Facebook Page)** + **Post B (X Thread)** สำเร็จ

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `OBS Studio`, `Python + FFmpeg 9.0.2 (auto_cut_video.py)`, `HTML/CSS + Chrome Headless`, `YouTube Studio`, `Git (.gitignore)`
- **ผลลัพธ์จริงที่วัดได้ (เฉพาะที่ทดสอบจริง):** คลิป EP #1 (`07:05`) เผยแพร่จริงบน YouTube (`https://youtu.be/V7EboxNpv0Y`) พร้อม Channel Branding ครบชุด และกระจายต่อบน Facebook Page + X Thread เรียบร้อย
- **ลิงก์ / ไฟล์อ้างอิงภายใน:** `3-Stage Media Workspace` • `EP #1 Blueprint` • `Channel Branding`
- **ลิงก์สาธารณะ:** https://youtu.be/V7EboxNpv0Y

> [!CAUTION] PDPA & Security Check ก่อนเผยแพร่สาธารณะ
> - ไฟล์สื่อใน `02-Business/YouTube/media/` ถูกกันใน `.gitignore` แล้ว ไม่ถูก Sync ขึ้น GitHub สาธารณะ
> - เมื่อแยกถ่ายคลิป Live Demo (Short #3) ให้ใช้ Google Sheet และกลุ่ม LINE สำหรับ Demo เท่านั้น (ปิดการแจ้งเตือนส่วนตัวทั้งหมด)

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post / Thread Hook
```text
TIL: วิธีจัดโฟลเดอร์ทำงานวิดีโอสำหรับสาย Solo Founder ไม่ให้ไฟล์ดิบรกเครื่องและไม่เผลอหลุดเข้า Git

วันนี้วางโครง 3-Stage Media Workspace ร่วมกับสคริปต์ Python + FFmpeg:
01-raw/ → ให้ OBS อัดลงตรงนี้
02-autocut/ → รัน --latest ตัดช่วงเงียบ + ปรับเสียง -16 LUFS อัตโนมัติ
03-ready/ → คัด Take เสร็จ Export ลงช่องนี้ (และ Ignore ไฟล์สื่อทั้งหมดใน .gitignore)

ผลลัพธ์: วันนี้ปล่อย EP #1 ขึ้นช่องจริงได้เรียบร้อยครับ 🎬
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
เบื้องหลังการปล่อยคลิป EP #1 วันนี้ — สิ่งที่ช่วยให้ "ทำเสร็จและได้ปล่อยจริง" ไม่ใช่ความสมบูรณ์แบบ แต่คือ Workflow ที่ลดแรงเสียดทานครับ

นอกจากทำ Branding ช่อง (Profile + Banner Safe Area ทีวี/มือถือ) ด้วย HTML/CSS วันนี้ผมจัดโครงสร้างโฟลเดอร์ตัดคลิปใหม่เป็น 3 ด่าน:
1️⃣ 01-raw/ — OBS อัดเสร็จลงโฟลเดอร์นี้ตรงๆ
2️⃣ 02-autocut/ — รันสคริปต์ FFmpeg (--latest) ตัดช่วงเงียบและปรับความดังเสียง -16 LUFS รอไว้ให้
3️⃣ 03-ready/ — ตัด Take ที่พูดพลาดออกแล้ว Export พร้อมอัปโหลด

💡 จุดเปลี่ยนสำคัญวันนี้:
ช่วง Live Demo ในคลิปยังมีจังหวะที่ต้องปรับเพิ่ม แทนที่จะรื้อถ่ายใหม่ทั้งหมดแล้วเลื่อนปล่อยคลิปออกไปอีก เราเลือกแยก Demo จริงไปเป็นคลิปสั้นตัวถัดไป แล้วส่ง EP #1 ออกสู่สายตาคนดูก่อนทันที

ดูคลิป EP #1 เต็มๆ ได้ที่นี่ครับ: https://youtu.be/V7EboxNpv0Y
```

### 🎬 YouTube / Short Video Hook *(ถ้าต่อยอดเป็นคลิปได้)*
- **ไอเดียชื่อคลิป:** `จัดระบบโฟลเดอร์ตัดต่อคลิปด้วย OBS + Python FFmpeg 3 ขั้นตอน (สายทำคนเดียว)`
- **Hook 5 วินาทีแรก:** `ทำไมคนทำช่องคนเดียวถึงชอบดองไฟล์ดิบไว้จนไม่ได้ตัดสักที? นี่คือระบบโฟลเดอร์ 3 ด่านที่ช่วยส่งคลิปจาก OBS ไป YouTube ได้ไวขึ้นหลายเท่าครับ`

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md`
- [x] บันทึกลงบนสุดของ `log.md`

