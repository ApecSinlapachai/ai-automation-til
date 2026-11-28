---
title: "TIL #017: Fine-Tuning vs. RAG vs. Context Engineering — Map เข้า Personal AI OS & Discovery Call Playbook"
date: 2026-11-18
last_updated: 2026-11-18
domain: learning/til
category: ai-systems
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, context-engineering, rag, fine-tuning, ai-os]
---

# 🚀 TIL #017: Fine-Tuning vs. RAG vs. Context Engineering — Map เข้า `Personal AI OS` & Client Discovery Playbook

> **🎯 เป้าหมายของการบันทึกนี้:** จัดกรอบตัดสินใจทางสถาปัตยกรรม AI ให้ชัดเจน ทั้งสำหรับพัฒนาระบบ `Personal AI OS` ของตัวเอง และใช้อธิบายลูกค้า SME ใน Discovery Call ไม่ให้ลงทุนผิดจุด

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่ทำ/เรียนรู้:** สรุปความแตกต่างและ Decision Matrix ระหว่าง **Context Engineering**, **RAG**, และ **Fine-Tuning** พร้อม Map เข้าสถาปัตยกรรม `Personal AI OS` (`2-Tier Summary Bridge` = Context Engineering) และวาง Playbook 3 ระยะสำหรับตอบคำถามลูกค้า AI Agency
- **ทำไมเรื่องนี้ถึงสำคัญ:** ลูกค้า SME จำนวนมากเข้าใจผิดว่าต้อง "Fine-tune โมเดล" เพื่อให้ AI รู้ข้อมูลบริษัท ทั้งที่จริงๆ แล้วความต้องการ 90% แก้ได้เร็วกว่าและถูกกว่าด้วย Context Engineering + RAG

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point / Bottleneck):**
  1. คำศัพท์ 3 ตัว (Fine-Tuning, RAG, Context Engineering) มักถูกใช้ปนกันจนเลือกสถาปัตยกรรมผิด — เช่น เอาข้อมูลที่เปลี่ยนรายวัน (ราคา/สต็อก/โน้ตส่วนตัว) ไปคิดทำ Fine-tuning ซึ่งจะล้าสมัยทันทีและดูแลยาก
  2. เวลาลูกค้าถามว่า *"อยากเทรน AI ด้วยข้อมูลบริษัท ทำได้ไหม?"* ต้องมีกรอบอธิบายที่เข้าใจง่ายและช่วยลูกค้าประหยัดงบ
- **แนวทาง/สถาปัตยกรรมที่เลือกใช้ (Solution & Trade-offs):**
  1. **แยกตาม Layer คอมพิวเตอร์:**
     - **Context Engineering** = RAM / หน่วยความจำทำงาน (เลือกไฟล์ที่โหลด, สรุป Rollup, Isolate Subagent, เคลียร์ Output ใหญ่, รักษา Prefix ให้นิ่งเพื่อ KV-Cache)
     - **RAG** = Hard Disk / Database ค้นหาสด (ใช้กับข้อเท็จจริงที่เปลี่ยนบ่อยและต้องมี Citation; ภาษาไทยต้องระวัง Chunking ตัดกลางคำ ควรใช้ Hybrid Search + Re-ranking)
     - **Fine-Tuning** = Firmware / ปรับ Weights (ใช้เฉพาะเมื่อต้องการ Strict Schema/โทนเฉพาะทาง หรือย่อโมเดลเล็กเพื่อลด Latency/Cost ที่ Volume สูงมาก)
  2. **Mapping เข้า AI OS:** `AGENTS.md` + `2-Tier Summary Bridge` + `.agents/skills/` คือ **Context Engineering** อยู่แล้ว (ไม่ต้อง Fine-tune AI OS) และใน P2.2 ค่อยต่อยอดเป็น **RAG** สำหรับ Website Portfolio AI Q&A
  3. **เพิ่ม Rule `/learn` ด้าน Visual:** อัปเดตกฎ `Visual & Printable Deliverable Standard` ใน `.agents/rules/proactive-ai-os.md` (แนบภาพ Preview เสมอ, ใช้พื้นขาว `#FFFFFF` เป็นค่าเริ่มต้นสำหรับเอกสารปริ้น)
- **ขั้นตอนที่ลงมือทำจริง (What I Built / Tested):**
  1. เขียนเอกสารวิจัย `03-Learning/Research/fine_tuning_vs_rag_vs_context_engineering.md` พร้อม Decision Matrix และ Client Discovery Call Playbook 3 Stage
  2. ซิงค์ขึ้น `03-Learning/Research/topics.md` และ `03-Learning/index.md`
  3. บันทึกกฎ `Visual & Printable Deliverable Standard` ผ่าน `/learn` ลงใน `.agents/rules/proactive-ai-os.md`

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `Markdown 2-Tier Summary Bridge`, `.agents/skills/`, `Hybrid RAG Architecture (Vector + Lexical/BM25)`
- **ผลลัพธ์จริงที่วัดได้ (เฉพาะที่ทดสอบจริง):** ได้คู่มือ Reference + Playbook พร้อมใช้คุยงานลูกค้า Discovery Call และกรอบออกแบบระบบ P2.2 Portfolio RAG
- **ลิงก์ / ไฟล์อ้างอิงภายใน:** `Fine-Tuning vs. RAG vs. Context Engineering` • `Portfolio RAG & SOT`

> [!CAUTION] PDPA & Security Check ก่อนเผยแพร่สาธารณะ
> - เมื่อทำระบบ RAG ให้ลูกค้า ต้องแยกข้อมูลภายในออกจากชุดข้อมูลสาธารณะ และตั้ง Access Control / Metadata Filtering ให้รัดกุมก่อนเปิดใช้งาน

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post / Thread Hook
```text
TIL: 90% ของงาน "อยากให้ AI รู้ข้อมูลบริษัท" ไม่ต้องเสียเงินทำ Fine-tuning เลยครับ

วิธีจำแบบคอมพิวเตอร์ 10 วินาที:
• Context Engineering = RAM (จัดการสิ่งที่ AI เห็นในรอบนั้นๆ — เร็วสุดและคุ้มสุด)
• RAG = Hard Disk (ดึงข้อมูลที่อัปเดตบ่อยแบบเรียลไทม์ + อ้างอิงแหล่งที่มาได้)
• Fine-Tuning = แก้ Firmware (เปลี่ยนนิสัย/Format เฉพาะทาง หรือลดต้นทุนตอน Volume มหาศาล)

ห้ามใช้ Fine-tuning เพื่อจำราคาสินค้าหรือข้อมูลที่เปลี่ยนทุกวันเด็ดขาด!
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
เวลาลูกค้าถามว่า "อยากเอาข้อมูลสินค้ากับคู่มือบริษัทไปเทรน AI (Fine-tune) ให้ตอบแชทลูกค้า" — ผมมักจะช่วยเบรกและอธิบายภาพนี้ก่อนเสมอครับ 👇

ทั้ง Context Engineering, RAG และ Fine-Tuning ไม่ได้แข่งกัน แต่ทำหน้าที่คนละชั้น:
1️⃣ Context Engineering (เหมือน RAM): จัดการกติกา บริบท และข้อมูลสำคัญที่ส่งให้ AI ในจังหวะนั้นๆ — ปรับแก้ได้ใน 1 วินาที
2️⃣ RAG (เหมือนตู้เอกสารค้นหาสด): เชื่อมฐานความรู้ที่อัปเดตตลอดเวลา เช่น ราคาสินค้า โปรโมชั่น SOP — แก้ข้อมูลปุ๊บ AI ตอบตรงทันทีและอ้างอิงที่มาได้
3️⃣ Fine-Tuning (เหมือนฝึกทักษะเฉพาะทาง): เหมาะกับงานที่ต้องการรูปแบบเฉพาะมากๆ หรือปริมาณงานวันละหลายหมื่นครั้งเพื่อลดต้นทุนต่อข้อความ

💡 สรุปให้เจ้าของธุรกิจเข้าใจง่ายสุด:
ถ้าข้อมูลบริษัทคุณยังเปลี่ยนทุกเดือน → ใช้ Context Engineering + RAG จะถูกกว่า เร็วกว่า และไม่เสี่ยง AI จำราคาเก่ามาตอบผิดครับ
```

### 🎬 YouTube / Short Video Hook *(ถ้าต่อยอดเป็นคลิปได้)*
- **ไอเดียชื่อคลิป:** `อย่าเพิ่งเทรน AI! เข้าใจความต่าง Context Engineering vs RAG vs Fine-Tuning ใน 3 นาที`
- **Hook 5 วินาทีแรก:** `ถ้ามีคนเสนอให้คุณเสียเงินหลักหมื่นเพื่อ Fine-tune AI ให้จำราคาสินค้า... หยุดฟังคลิปนี้ 3 นาทีก่อนจ่ายเงินครับ`

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md`
- [x] บันทึกลงบนสุดของ `log.md`

