---
title: "TIL #011: อัปเกรด Personal AI OS — แยก Human-Only Zone, คิว Triage และ Priority Gatekeeper"
date: 2026-11-16
last_updated: 2026-11-16
domain: learning/til
category: ai-systems
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, ai-os, adhd, triage, human-in-the-loop]
---

# 🚀 TIL #011: อัปเกรด Personal AI OS — แยก Human-Only Zone, คิว Triage และ Priority Gatekeeper

> **🎯 เป้าหมายของการบันทึกนี้:** แสดงสถาปัตยกรรมจริงของการออกแบบ **AI Workspace ที่เคารพพื้นที่ส่วนตัวของผู้ใช้และช่วยคุมโฟกัส (Anti-Distraction)** สำหรับคนที่มีภาวะ ADHD และกำลังวิ่ง Sprint ธุรกิจ

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่ทำ/เรียนรู้:** แยก `00-Notes/` เป็น **Human-Only Zone**, สร้างกระดาน `Triage.md` แบบ **Single-Threaded Focus (`P0 → P1.1 → P1.2`)**, ตั้งมาตรฐานคำตอบ `1 → 2 → 3`, และฝังกฎ **Proactive Priority Gatekeeper** ให้ AI ช่วยเบรกงานนอกโฟกัส
- **ทำไมเรื่องนี้ถึงสำคัญ:** AI ที่เก่งเกินไปมัก "รับทำทุกอย่างที่ผู้ใช้โยนเข้ามา" จนทำให้ผู้ใช้หลุดจากงานสำคัญ — การตั้งกฎให้ AI **กล้าห้าม** และเคารพพื้นที่ส่วนตัวคือจุดเปลี่ยนจาก Chatbot เป็น Co-Founder จริงๆ

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point / Bottleneck):**
  1. โน้ตกันลืมส่วนตัว (เช่น To-do จิปาถะ / ของเข้าบ้าน) ปนอยู่ใน `00-Inbox/` ทำให้ AI เข้าไป Triage หรือสร้างไฟล์ Bridge โดยไม่จำเป็น
  2. เวลาไอเดียใหม่โผล่เข้ามาเยอะ (โดยเฉพาะบริบท ADHD) AI มักกระโดดไปช่วยทำไอเดียใหม่ทันที แทนที่จะช่วยรักษาโฟกัสที่โปรเจกต์ `P1.1`
  3. คำตอบที่ยาวปะปนกันระหว่างไฟล์อ้างอิงกับข้อความพร้อมใช้ ทำให้ต้องเปิดสลับหลายหน้าจอ
- **แนวทาง/สถาปัตยกรรมที่เลือกใช้ (Solution & Trade-offs):**
  1. **Human-Only Zone (`00-Notes/`):** ล็อกกฎ Default No-Touch (AI ห้ามอ่าน/แก้/ย้าย/ซิงค์เข้า Log หรือ Triage ยกเว้นผู้ใช้สั่งเจาะจง) ทั้งใน `AGENTS.md`, `MASTER_PROMPT.md`, Rules และ Skills
  2. **Single-Threaded Triage + North Star Filter:** จัดลำดับงานตาม `End Goal` และฝังกฎ **Priority Gatekeeper** ใน `.agents/rules/proactive-ai-os.md` ให้ AI ทักท้วงทันทีเมื่อมีงานใหม่ที่ไม่ขยับเข้าใกล้เป้าหมายหลักใน 45 วันนี้
  3. **Response Standard (`1 → 2 → 3`):** บังคับโครงสร้างคำตอบเป็น 1) ไฟล์ที่เกี่ยวข้อง → 2) ข้อความ Ready-to-Paste ใน Code Block → 3) BLUF Gaps & Next Steps 1–3 ข้อ
- **ขั้นตอนที่ลงมือทำจริง (What I Built / Tested):**
  1. ปรับโครงสร้าง `AGENTS.md`, `00-System/MASTER_PROMPT.md`, `.agents/rules/proactive-ai-os.md`, และ Skills (`summary-bridge-sync`, `daily-wrap-up`)
  2. สร้าง `Triage.md` + `north_star.md` + ปรับ `.gitignore` รองรับ `00-Notes/.obsidian/`
  3. ทดสอบใช้งานจริงกับคิว `00-Inbox/` โดยปัดไอเดียใหม่ที่ยังไม่เร่งด่วนลง **Parked Backlog** แทนการลงมือทำแทรก

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `Obsidian Markdown`, `Agent Rules & Skills (.agents/)`, `2-Tier Summary Bridge`, `Git`
- **ผลลัพธ์จริงที่วัดได้ (เฉพาะที่ทดสอบจริง):**
  - แยกโน้ตส่วนตัว (`00-Notes/`) ออกจากระบบอัตโนมัติของ AI ได้สมบูรณ์ 100%
  - ขนาดไฟล์ `AGENTS.md` ทั้ง Root และ 4 Sub-Projects ยังคงต่ำกว่าเกณฑ์ `24 KB` (`4.9–16.1 KB`) ไม่เกิด Rule Truncation
  - มีระบบกรองไอเดียเข้า Backlog อัตโนมัติ ลด Cognitive Load ในการตัดสินใจ
- **ลิงก์ / ไฟล์อ้างอิงภายใน:** `Root AGENTS.md`, `Triage.md`, `north_star.md`, `proactive-ai-os.md`

> [!CAUTION] PDPA & Security Check ก่อนเผยแพร่สาธารณะ
> - ห้ามเปิดเผยเนื้อหาหรือตัวอย่างบันทึกจริงจากโซน `00-Notes/` และข้อมูลส่วนตัว/สุขภาพเชิงลึก
> - เล่าเฉพาะแนวคิดเชิงสถาปัตยกรรมและโครงสร้างไฟล์เชิงระบบ

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post / Thread Hook
```text
TIL: ข้อเสียของ AI ที่เชื่อฟังเกินไป คือมันจะช่วยคุณทำ "งานที่ไม่สำคัญ" ได้เร็วมาก

วันนี้เลยอัปเกรด Personal AI OS ให้มีกฎใหม่ 3 ข้อ:
1. Human-Only Zone (`00-Notes/`) — AI ห้ามยุ่งโน้ตส่วนตัวเด็ดขาด
2. Single-Threaded `Triage.md` — โฟกัสจบทีละโปรเจกต์
3. Priority Gatekeeper — ถ้าโยนไอเดียใหม่ที่ไม่ขยับเป้าหมายหลัก AI ต้อง "ช่วยเบรก" ก่อนทำ

AI ที่ดีที่สุดไม่ใช่ตัวที่ตอบ Yes ทุกเรื่อง แต่คือตัวที่กล้าเตือนให้คุณกลับมาโฟกัสครับ
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
ทำไมใช้ AI ทำงานแล้วรู้สึกยุ่งกว่าเดิม? เพราะ AI ยุคนี้ "ใจดีเกินไป" ครับ — คุณโยนไอเดียอะไรไป มันทำให้หมด จนลืมโปรเจกต์หลักที่หาเงินได้จริง

วันนี้เลยแก้กติกาในระบบ Personal AI OS (Obsidian + Markdown) ของตัวเองใหม่ 3 จุด:
🔎 1. แยก Human-Only Zone: กันโฟลเดอร์โน้ตส่วนตัวออกจากการอ่าน/จัดระเบียบของ AI 100%
🛠️ 2. บังคับทำจบทีละลำดับ (Triage Board): ทุกโปรเจกต์ต้องตอบให้ได้ว่าสนับสนุน End Goal ยังไง ถ้าไม่ใช่คิวปัจจุบัน ให้ปัดลง Backlog ทันที
💡 3. ตั้งกฎ "Priority Gatekeeper": สั่งใน System Rule เลยว่า ถ้าผมเริ่มวอกแวกไปทำสิ่งที่ไม่ขยับเป้าหมายรายได้ใน 45 วันนี้ ให้ AI ทักท้วงและช่วยเบรกก่อนเลย

สำหรับคนที่ไอเดียผุดตลอดเวลา การมีระบบที่ช่วย "ตัดสิ่งที่ไม่จำเป็นออก" สำคัญพอๆ กับการคิดสิ่งใหม่เลยครับ
```

### 🎬 YouTube / Short Video Hook *(ถ้าต่อยอดเป็นคลิปได้)*
- **ไอเดียชื่อคลิป:** `วิธีตั้งค่า AI ให้ "กล้าห้ามเรา" เวลาหลุดโฟกัส (ระบบ Triage สำหรับคนไอเดียเยอะ)`
- **Hook 5 วินาทีแรก:** `ยิ่งใช้ AI เก่ง เรายิ่งวอกแวกง่ายขึ้น... นี่คือวิธีตั้งกฎให้ AI ช่วยเบรกงานที่ไม่ทำเงินใน 30 วินาที`

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md`
- [x] บันทึกลงบนสุดของ `log.md`

