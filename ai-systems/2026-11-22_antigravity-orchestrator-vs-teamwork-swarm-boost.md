---
title: "TIL #024: Antigravity Orchestrator Mode vs. /teamwork, /swarm & /boost — เลือกใช้ Multi-Agent ให้ถูกสเกล"
date: 2026-11-22
last_updated: 2026-11-22
domain: learning/til
category: ai-systems
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, antigravity, multi-agent, orchestration]
---

# 🚀 TIL #024: Antigravity Orchestrator Mode vs. `/teamwork`, `/swarm` & `/boost` — เลือกใช้ Multi-Agent ให้ถูกสเกล

> **🎯 เป้าหมายของการบันทึกนี้:** เปลี่ยนสิ่งที่เรียนในวันนี้ให้เป็น **Playbook เลือกรูปแบบ Multi-Agent Orchestration** ใน Google Antigravity เพื่อไม่ให้เปลือง Token/Quota เกินความจำเป็น และเป็น **Proof of Work (Portfolio)** ด้านการออกแบบสถาปัตยกรรม AI Systems

---

## 1. TL;DR — วันนี้ได้เรียนอะไร? (1–3 บรรทัด)
- **สิ่งที่เรียนรู้:** การเปิด **Orchestrator Mode** ใน Antigravity (จับคู่ Planner Model + Executor Model) เพียงพอสำหรับ 90% ของงานรายวันแล้ว **ไม่จำเป็นต้องเรียก `/teamwork` ทุกครั้ง**
- **ทำไมเรื่องนี้ถึงสำคัญ:** การเข้าใจขอบเขตของ `Orchestrator Mode` vs `/teamwork` vs `/swarm` vs `/boost` ช่วยคุมต้นทุน Token, ลด Latency และป้องกันการเปิดระบบ Multi-Agent ใหญ่เกินสเกลงาน (Over-engineering)

---

## 2. ปัญหาที่เจอ & แนวทางเลือกใช้ให้ถูกสเกล (Problem → Decision Framework)
- **ปัญหาตั้งต้น (Pain Point):** เมื่อตั้งค่า `orchestratorModel` คู่กับ `orchestratorExecutorModel` ใน Antigravity แล้ว อาจสับสนว่ายังต้องเรียก `/teamwork` อีกไหม หรือสองอย่างนี้ทำงานซ้ำซ้อนกันหรือไม่
- **ความจริงเชิงสถาปัตยกรรม (Architecture & Trade-offs):**
  1. **Orchestrator Mode (Single-Session Planner + Executor):** ทำงานในแชทเดียว จับคู่โมเดลวางแผน (ตัวฉลาด) กับโมเดลลงมือทำ (ตัวเร็ว/ประหยัด) เหมาะกับงานจบในเซสชันเดียว เช่น สร้างฟีเจอร์ 1 ชิ้น, แก้บั๊กหลายไฟล์, รีแฟกเตอร์, หรือจัดการไฟล์ใน AI OS
  2. **`/teamwork` (Long-Horizon Autonomous Multi-Agent):** เป็น Slash Command สำหรับโปรเจกต์ใหญ่ระยะยาวระดับหลายชั่วโมงหรือข้าม Milestone มักเริ่มด้วย **Scoping Interview** เพื่อล็อกสเปก แล้วปล่อยทีม Agent เฉพาะทาง (เช่น Sentinel, Implementer, Verifier) แยก Workspace/Git worktree และมีด่านตรวจ Milestone ก่อนส่งมอบ — กินเวลาและโควตาสูงกว่ามาก
  3. **`/swarm` (Parallel Shared-Board Workers):** เหมาะกับงานที่แตกเป็นชิ้นย่อยขนานกันได้ (เช่น แก้ไฟล์ 10–20 ไฟล์ หรือวิจัยหลายหัวข้อพร้อมกัน) โดยประสานงานผ่าน Message Board กลาง
  4. **`/boost` (Deep-Reasoning Challenge Loop):** เหมาะกับปัญหาที่ติดล็อกเชิงตรรกะ/อัลกอริทึมยากๆ หรือ Race Condition ที่ต้องให้ Agent ถกเถียงและทดสอบซ้ำหลายรอบ

| เกณฑ์ตัดสินใจ | **Orchestrator Mode** | **`/swarm`** | **`/boost`** | **`/teamwork`** |
| :--- | :--- | :--- | :--- | :--- |
| **สเกลงาน** | งานรายวัน / จบในแชทเดียว | งานแตกไฟล์ทำขนานได้ | งานแก้โจทย์ยากเฉพาะจุด | โปรเจกต์ใหญ่หลาย Milestone |
| **ความเร็ว & ต้นทุน** | เร็วสุด / ประหยัดสุด | ปานกลาง | สูงเฉพาะจุด | สูงสุด / รันยาว |
| **จังหวะที่ควรใช้** | 90% ของงานทั่วไป | Batch งานหลายไฟล์พร้อมกัน | ติดบั๊ก/อัลกอริทึมลึก | สร้างระบบใหม่ทั้งก้อน / Migration ใหญ่ |

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** Google Antigravity (`Orchestrator Mode`, `/teamwork`, `/swarm`, `/boost`)
- **ลิงก์อ้างอิงภายใน:** TIL #002: เลือกโมเดล Argon vs. Hydrogen + Orchestrator • TIL #001: 2-Tier Summary Bridge

> [!CAUTION] PDPA & Security Check ก่อนเผยแพร่สาธารณะ
> - ไม่มีข้อมูลลูกค้า, Token, API Key หรือ Path ส่วนตัวของระบบภายใน

---

## 4. 🎯 เอาไปใช้อะไรได้ทันทีใน AI OS หรือ AI Agency ของเรา?
- **ใน Personal AI OS:** ใช้ **Orchestrator Mode** เป็นค่าเริ่มต้นสำหรับงานประจำวัน (จัดการคิว, เขียนสคริปต์, สรุป TIL, อัปเดต Repo) เพื่อเซฟโควตาและได้ความเร็วสูงสุด ส่วน `/teamwork` เก็บไว้ใช้เฉพาะตอนขึ้นโปรเจกต์ใหม่ระดับ Full-Stack หรือ Refactor โครงสร้างใหญ่ข้ามหลายโฟลเดอร์
- **ใน AI Agency (`02-Business/AI-Agency`):** ใช้กรอบตัดสินใจนี้เป็น **Architecture Selling Point** เวลาอธิบายลูกค้า SME — ว่าเราไม่ได้ยัด Multi-Agent หนักๆ ให้ทุกงานจนค่า API บานปลาย แต่เลือก Pattern ให้ตรงกับ ROI และสเกลของงานจริง

---

## 5. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post
```text
TIL: ใช้ Orchestrator Mode อยู่แล้ว ยังต้องเรียก /teamwork อีกไหม?

คำตอบสั้นๆ คือ "90% ของงาน ไม่ต้องครับ"

สรุปการเลือกใช้ Multi-Agent ให้ถูกสเกลใน Antigravity:
1. Orchestrator Mode ➡️ งานรายวัน จบในแชทเดียว (Planner + Executor) เร็วและคุ้มโควตาสุด
2. /swarm ➡️ งานที่แตกทำขนานหลายไฟล์พร้อมกันได้
3. /boost ➡️ ติดบั๊กหรืออัลกอริทึมยากๆ ที่ต้องคิดลึกหลายรอบ
4. /teamwork ➡️ โปรเจกต์ใหญ่ข้ามหลาย Milestone ที่ต้องมีทีมตรวจทานกันเอง

อย่ายิงปืนใหญ่ใส่นกกระจอก เลือกสถาปัตยกรรมให้พอดีสเกลงาน จะคุมทั้งเวลาและค่า Token ได้อยู่หมัดครับ
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
ใช้โหมด Orchestrator อยู่แล้ว ยังจำเป็นต้องใช้ /teamwork อีกไหม? (บทเรียนการคุมสเกล Multi-Agent)

ช่วงนี้หลายคนเริ่มใช้ระบบ Multi-Agent แล้วมักสงสัยว่า ถ้าเปิด Orchestrator Mode (ที่จับคู่โมเดลวางแผนกับโมเดลลงมือทำ) อยู่แล้ว ยังต้องเรียกทีม Agent ใหญ่อย่าง /teamwork อีกหรือเปล่า?

🔎 คำตอบคือ: ไม่จำเป็นต้องใช้ทุกงานครับ เพราะมันออกแบบมาคนละสเกล

🛠️ วิธีเลือกใช้ให้คุ้มเวลาและโควตาที่สุด:
- Orchestrator Mode: ใช้กับ 90% ของงานประจำวัน (สร้างฟีเจอร์ 1 ชิ้น, แก้บั๊ก, จัดการไฟล์) จบไวในแชทเดียว
- /swarm: ใช้ตอนมีงานที่แตกทำขนานหลายไฟล์หรือหลายหัวข้อพร้อมกันได้
- /boost: ใช้ตอนเจอบั๊กหรือโจทย์ตรรกะยากๆ ที่ต้องคิดลึกและเทสซ้ำหลายชั้น
- /teamwork: เก็บไว้ใช้กับโปรเจกต์ใหญ่ระยะยาวระดับหลาย Milestone ที่ต้องเริ่มจาก Scoping, แยกทีมเขียน-ตรวจ, และรันยาวอัตโนมัติ

💡 บทเรียนที่ได้:
การทำระบบ AI ให้เก่งไม่ได้แปลว่าต้องเปิด Agent เยอะที่สุดเสมอไป แต่คือการเลือก "สถาปัตยกรรมที่พอดีกับขนาดปัญหา" ครับ

ใครใช้ Multi-Agent แบบไหนเป็นประจำ แชร์มาแลกเปลี่ยนกันได้นะครับ!
```

### 🎬 YouTube / Short Video Hook
- **ไอเดียชื่อคลิป:** `อย่าใช้ Multi-Agent ผิดสเกล! เทียบ Orchestrator vs Teamwork vs Swarm ใน 60 วิ`
- **Hook 5 วินาทีแรก:** `เปิด Orchestrator Mode อยู่แล้ว ยังต้องเรียก /teamwork อีกไหม? ระวังเสีย Token ฟรีถ้าใช้ผิดสเกลครับ!`

---

## 6. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **TIL Log** ใน `til_summary.md`
- [ ] อัปเดต `03-Learning/index.md` (รอรวบยอดตอน `/chat-handoff` หรือ `/daily-wrap-up`)
- [ ] บันทึกลงบนสุดของ `log.md` (รอรวบยอดตอน `/chat-handoff` หรือ `/daily-wrap-up`)

