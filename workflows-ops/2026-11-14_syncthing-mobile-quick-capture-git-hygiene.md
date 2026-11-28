---
title: "TIL #005: เชื่อม Syncthing ซิงค์ 00-Inbox มือถือ ↔ PC โดยไม่ชน Git และ Obsidian"
date: 2026-11-14
last_updated: 2026-11-14
domain: learning/til
category: tech-ai
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, syncthing, obsidian, git, quick-capture, mobile-workflow]
---

# 🚀 TIL #005: เชื่อม Syncthing ซิงค์ `00-Inbox` มือถือ ↔ PC โดยไม่ชน Git และ Obsidian

> **🎯 เป้าหมายของการบันทึกนี้:** สรุปสถาปัตยกรรมการทำ Mobile Quick Capture ลง Personal AI OS แบบฟรี ปลอดภัย และไม่สร้างไฟล์ขยะหรือ Merge Conflict ให้ Git Repository

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่ทำ:** ติดตั้ง **Syncthing (`v2.1.7`)** บน Windows คู่กับ **Syncthing-Fork** บน Android โดยซิงค์แบบเจาะจงพื้นที่ (**Scoped Sync**) เฉพาะโฟลเดอร์ `00-Inbox/` (`Folder ID: ai-os-inbox`)
- **ทำไมเรื่องนี้ถึงสำคัญ:** จดไอเดียหรือโยนภาพจากมือถือเข้า AI OS ได้ทันทีโดยไม่ซิงค์โฟลเดอร์ `.git` หรือไฟล์ระบบทั้งหมดให้เกิด Conflict พังกลางทาง

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point / Bottleneck):**
  1. ถ้าซิงค์ทั้ง Vault (`AI OS/`) ระหว่างมือถือกับ PC ไฟล์ `.git/` และสถานะ Workspace ของ Obsidian (`.obsidian/workspace.json`) จะชนกันและสร้าง `*.sync-conflict-*` รก Repository
  2. เวลาอยู่ข้างนอกแล้วนึกไอเดียออก หรือแคปหน้าจอไว้บนมือถือ อยากโยนเข้า `00-Inbox/` ของ AI OS ทันทีโดยไม่ต้องพึ่งคลาวด์เสียเงิน
- **แนวทาง/สถาปัตยกรรมที่เลือกใช้ (Scoped Inbox Sync + 2-Layer Ignore):**
  - **ซิงค์เฉพาะ `00-Inbox/` (`ai-os-inbox`):** ตั้งโหมด `sendreceive` พร้อมเปิด **Simple File Versioning (`keep=5`)** เผื่อกู้ไฟล์กรณีเผลอลบ, ส่วนเสาหลัก `01`–`04` และ `.git` ให้ PC เป็น Source of Truth เพียงจุดเดียว
  - **Layer 1 — `00-Inbox/.stignore` (กันไม่ให้ Syncthing ซิงค์ขยะข้ามเครื่อง):** บล็อก `.obsidian`, `.trash`, `.git`, `.vscode`, `*.tmp`, `~$*`, `.~*`, `Thumbs.db`, `Desktop.ini`, `.DS_Store`
  - **Layer 2 — Root `.gitignore` (กันไม่ให้ไฟล์ Syncthing ปนเข้า Git Backup):** เพิ่ม `.stfolder/`, `.stfolder`, `.stversions/`, `*.sync-conflict-*`, `00-Inbox/.obsidian/`, `00-Inbox/.trash/`
  - **Quick Capture Template:** สร้าง `00-Inbox/_TEMPLATE_quick_note.md` (ตั้งกฎไม่ย้ายไฟล์ที่มีคำนำหน้า `_TEMPLATE_` และ `index.md` ลง Archive)
- **การทดสอบจริง:** เชื่อมต่อ Android `Syncthing-Fork` สำเร็จและทดสอบซิงค์ไฟล์ภาพหน้าจอและโน้ตเข้า `00-Inbox/` บน Windows เรียบร้อย (ก่อนลบไฟล์ทดสอบออกหลังยืนยันการทำงาน)

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `Syncthing v2.1.7 (BillStewart.SyncthingWindowsSetup)` • `Syncthing-Fork (Android)` • `Obsidian` • `Git`
- **ไฟล์อ้างอิงภายใน:**
  - `00-Inbox/index.md`
  - `00-Inbox/_TEMPLATE_quick_note.md`
  - `00-Inbox/.stignore`
  - `.gitignore`

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post
```text
TIL: ท่าซิงค์ Obsidian มือถือ ↔ PC ด้วย Syncthing แบบไม่ให้ Git พัง 📱🔄💻

แทนที่จะซิงค์ทั้ง Vault (ซึ่งเสี่ยง .git กับ .obsidian ชนกัน):
1. ซิงค์เฉพาะโฟลเดอร์ "00-Inbox/" เป็น Scoped Capture Zone
2. ใส่ .stignore กัน .obsidian / .trash / ไฟล์ temp ของมือถือ
3. ใส่ .gitignore กัน .stfolder / .stversions / *.sync-conflict-* เข้า Git

จดจากมือถือปุ๊บ เด้งเข้า Inbox ให้ AI Agent คัดแยกต่อบน PC ได้ทันทีครับ
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
ทริคเชื่อมต่อ "มือถือ ↔ PC" ให้จดไอเดียลง Second Brain (Obsidian + AI OS) ได้ทันทีแบบไม่ชน Git 📱⚡

หลายคนอยากซิงค์โฟลเดอร์จดบันทึกจากมือถือเข้าคอมด้วย Syncthing แต่พอซิงค์ทั้งโปรเจกต์ทีไร มักเจอปัญหาไฟล์ .git ชนกัน หรือโฟลเดอร์ .obsidian ของมือถือไปตีกับคอมจนเกิดไฟล์ conflict เต็มไปหมด

วันนี้เลยลองวางโครงสร้างแบบ "Scoped Inbox Sync":
1️⃣ เปิด Syncthing ซิงค์เฉพาะโฟลเดอร์ `00-Inbox/` อย่างเดียว (เหมือนตู้รับจดหมายหน้าบ้าน) ไม่ต้องซิงค์ทั้ง Vault
2️⃣ ใส่ `.stignore` ใน Inbox เพื่อบล็อก `.obsidian`, `.trash`, และไฟล์ชั่วคราวไม่ให้ข้ามเครื่องมาปน
3️⃣ ใส่ `.stfolder/`, `.stversions/`, และ `*.sync-conflict-*` ลงใน `.gitignore` เพื่อไม่ให้หลุดเข้า Git Backup
4️⃣ เตรียมไฟล์ `_TEMPLATE_quick_note.md` ไว้ใน Inbox รอไว้เลย

ผลคือเวลาอยู่ข้างนอกแค่จดหรือโยนไฟล์เข้า Inbox บนมือถือ กลับมาเปิดคอมก็ให้ AI Agent ช่วย Triage กระจายเข้าโปรเจกต์ที่เกี่ยวข้องได้ทันทีครับ!
```

### 🎬 YouTube / Short Video Hook
- **ไอเดียชื่อคลิป / Short:** *"วิธีซิงค์โน้ตมือถือเข้า Obsidian + AI OS ด้วย Syncthing แบบฟรี 100% และไม่ชน Git"*
- **Hook 5 วินาทีแรก:** *"อย่าเพิ่งซิงค์ Obsidian ทั้งก้อนถ้าคุณใช้ Git เก็บโค้ด! นี่คือวิธีซิงค์เฉพาะ Inbox จากมือถือเข้า AI OS แบบปลอดภัยครับ"*

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md`
- [x] บันทึกลงบนสุดของ `log.md`

