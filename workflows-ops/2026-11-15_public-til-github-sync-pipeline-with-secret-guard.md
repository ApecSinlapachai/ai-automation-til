---
title: "TIL #008: แยกคลัง TIL ขึ้น Public GitHub จาก Private AI OS ด้วย PowerShell Export Script (Status Gate + Wikilink Sanitizer + Secret Scanner)"
date: 2026-11-15
last_updated: 2026-11-15
domain: learning/til
category: workflows-ops
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, github, powershell, obsidian, devsecops]
---

# 🚀 TIL #008: แยกคลัง TIL ขึ้น Public GitHub จาก Private AI OS ด้วย PowerShell Export Script (Status Gate + Wikilink Sanitizer + Secret Scanner)

> **🎯 เป้าหมายของการบันทึกนี้:** บันทึกสถาปัตยกรรมและสคริปต์จริงสำหรับเผยแพร่ความรู้ (Learn in Public) ขึ้น GitHub สาธารณะ โดยยังเขียนบันทึกทุกอย่างรวมศูนย์ไว้ใน Personal AI OS ส่วนตัวได้แบบ Zero Data Leak

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่ทำ/เรียนรู้:** จัดหมวดหมู่ `03-Learning/TIL/` เป็น 4 โฟลเดอร์ (`ai-systems/`, `automation-agency/`, `books/`, `workflows-ops/`) พร้อมสร้างสคริปต์ `00-System/sync_til_public.ps1` เพื่อคัดกรองและส่งออกเฉพาะไฟล์พร้อมเผยแพร่ขึ้น Public GitHub
- **ทำไมเรื่องนี้ถึงสำคัญ:** ลดภาระการสลับ Workspace และป้องกันความเสี่ยงที่ไฟล์ `draft`, ข้อมูลส่วนตัว, หรือ API Token ใน Private Vault จะหลุดขึ้น Repo สาธารณะโดยไม่ตั้งใจ

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point / Bottleneck):**
  - อยากทำ Public GitHub TIL Repository เพื่อโชว์ Proof of Work แต่ไม่อยากแยกโฟลเดอร์เขียน 2 ที่
  - หากใช้ `git submodule` มักลืม Commit 2 ชั้นและเสี่ยงเผลอ Push ไฟล์ `draft` ที่มีลิงก์เอกสารภายในหรือข้อมูลส่วนตัว
  - ลิงก์แบบ Obsidian `Label` จะกลายเป็น Broken Link เมื่อเปิดอ่านบน GitHub Web UI
- **แนวทาง/สถาปัตยกรรมที่เลือกใช้ (Solution & Trade-offs):**
  - ใช้ **Single Source of Truth ใน Private AI OS + PowerShell Staging Export Script** แทน `git submodule` หรือการสั่ง `git subtree push` ดิบๆ โดยตรง
  - ข้อดีคือสามารถตรวจ YAML `status` และสแกน Secret ก่อน Push 100% แลกกับการใช้ `--force` push ไปยัง Repo ปลายทางที่เป็น Mirror สำหรับแสดงผลอย่างเดียว
- **ขั้นตอนที่ลงมือทำจริง (What I Built / Tested):**
  1. จัดโครงสร้างโฟลเดอร์ย่อย 4 หมวด (`ai-systems/`, `automation-agency/`, `books/`, `workflows-ops/`) พร้อม `README.md` หน้าแรก
  2. เขียน `00-System/sync_til_public.ps1` ที่มีระบบตรวจ 3 ด่าน:
     - **Status Gate:** ส่งออกเฉพาะไฟล์ที่มี `status: ready-to-post`, `published`, หรือ `public` (ข้าม `draft` และไฟล์ระบบอัตโนมัติ)
     - **Secret Scanner:** ตรวจ Pattern ของ Key/Token (`AIza...`, `sk-...`, `ghp_...`, `Bearer ...`) หากพบให้ Abort ทันที
     - **Wikilink Sanitizer:** แปลง `Display Label` เป็นข้อความปกติเพื่อไม่ให้เกิด Broken Link ชี้เข้าไฟล์ Private
  3. รองรับโหมด `-DryRun` เพื่อตรวจสอบรายชื่อไฟล์ใน Staging Directory ก่อนสั่ง Push จริง

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `PowerShell`, `Git`, `Obsidian Markdown`, `YAML Frontmatter`, `Regex`
- **ผลลัพธ์จริงที่วัดได้ (เฉพาะที่ทดสอบจริง):** ย้าย TIL #001–#006 เข้าโครงสร้าง 4 หมวดสำเร็จ และสร้างสคริปต์ Export 122 บรรทัดพร้อมโหมด `-DryRun` แยก Private AI OS กับ Public TIL ได้สมบูรณ์
- **ลิงก์ / ไฟล์อ้างอิงภายใน:** `til_architecture_and_github_sync.md` • `03-Learning/TIL/README.md` • `00-System/sync_til_public.ps1`
- **ลิงก์สาธารณะ (ถ้ามี):** *(รอสร้าง Public GitHub Repo แล้วรัน `-DryRun` + Push จริง)*

> [!CAUTION] PDPA & Security Check ก่อนเผยแพร่สาธารณะ
> - Public Repo ต้องเป็นเพียง Output Mirror ที่ผ่าน `sync_til_public.ps1` เท่านั้น ห้ามเพิ่ม Remote ชี้ Repo หลักของ AI OS แบบ Public
> - ตรวจสอบทุกครั้งหลังรัน `-DryRun` ว่าไม่มีไฟล์ที่มีข้อมูลส่วนตัวหรือ Sheet ID หลุดเข้าไปใน Staging

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post / Thread Hook
```text
TIL: อยากทำ Public TIL บน GitHub แต่ไม่อยากแยกเขียน 2 ที่จาก Private Obsidian Vault ทำยังไงให้ไม่ข้อมูลรั่ว?

วิธีที่ผมเลือกใช้คือเขียนสคริปต์ Export (sync_til_public.ps1) คั่นกลาง 3 ด่านก่อน Push:
1. Status Gate: เอาเฉพาะไฟล์ status: ready-to-post / published
2. Secret Scanner: ดักจับ pattern ของ API key / Bearer token ถ้าเจอให้หยุดทันที
3. Wikilink Sanitizer: แปลง Label เป็นข้อความธรรมดาไม่ให้ลิงก์พังบน GitHub

เขียนที่เดียวใน AI OS แต่แยก Public Portfolio ออกมาได้แบบปลอดภัยครับ
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
🗂️ วิธีเปิดคลัง TIL ขึ้น Public GitHub จากโน้ตส่วนตัว (Obsidian) โดยไม่เสี่ยงข้อมูลลับรั่ว

หลายคนอยากทำ Learn in Public บน GitHub แต่ติดปัญหาว่าโน้ตทั้งหมดอยู่ใน Vault ส่วนตัว จะก๊อปปี้แยก 2 ที่ก็ขี้เกียจ แต่จะ Push ขึ้นทั้งโฟลเดอร์ก็กลัวไฟล์ Draft หรือ API Key หลุด

🛠️ วันนี้เลยออกแบบระบบ Export คั่นกลาง 3 ชั้นด้วย PowerShell Script ง่ายๆ:
1️⃣ Status Filter — อ่าน YAML Frontmatter ส่งออกเฉพาะโน้ตที่ตั้งสถานะ ready-to-post หรือ published เท่านั้น (Draft ยังอยู่ในเครื่อง)
2️⃣ Secret Guardrail — สแกน Regex หา Pattern ของ API Key และ Token ก่อนสร้าง Commit ถ้าเจอสั่ง Abort ทันที
3️⃣ Wikilink Sanitizer — แปลง ชื่อแสดง ให้เป็นข้อความอ่านง่ายบน GitHub ไม่ทิ้งลิงก์ตาย

💡 บทเรียนที่ได้:
ระบบ Portfolio ที่ยั่งยืนต้องไม่เพิ่มงานซ้ำซ้อนครับ แค่เพิ่มขั้นตอนคัดกรองอัตโนมัติ 1 สคริปต์ เราก็เปลี่ยนบันทึกงานประจำวันให้เป็น Public Proof of Work ได้ทันที
```

### 🎬 YouTube / Short Video Hook *(ถ้าต่อยอดเป็นคลิปได้)*
- **ไอเดียชื่อคลิป:** `วิธีดูดโน้ตจาก Obsidian ส่วนตัวขึ้น GitHub สาธารณะอัตโนมัติ (แบบกันข้อมูลลับหลุด 100%)`
- **Hook 5 วินาทีแรก:** `"ถ้าคุณจดโน้ตทุกอย่างไว้ใน Obsidian ส่วนตัว แล้วอยากแชร์บางเรื่องขึ้น GitHub เป็น Portfolio โดยไม่ต้องก๊อปปี้ทีละไฟล์ นี่คือสคริปต์กรอง 3 ชั้นที่ผมใช้ครับ"`

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md` (รวมถึง `Cross-Pillar Sync Signals` หากส่งต่อเข้า `02-Business`)
- [x] หากเป็นไอเดียคลิปใหม่ ให้เพิ่มเข้า `content_calendar.md`
- [x] บันทึกลงบนสุดของ `log.md`

