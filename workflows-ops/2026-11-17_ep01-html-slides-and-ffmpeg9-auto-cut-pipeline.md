---
title: "TIL #014: ผลิตคลิป YouTube ด้วย HTML Slide Deck + FFmpeg 9.0.2 Auto-Cut & ปรับเสียงมาตรฐาน (-16 LUFS)"
date: 2026-11-17
last_updated: 2026-11-17
domain: learning/til
category: workflows-ops
status: ready-to-post
public_channels: [x, facebook-page, youtube]
tags: [til, learn-in-public, portfolio, youtube, ffmpeg, automation, html-slides, agent-skills]
---

# 🚀 TIL #014: ผลิตคลิป YouTube ด้วย HTML Slide Deck + FFmpeg 9.0.2 Auto-Cut & ปรับเสียงมาตรฐาน (-16 LUFS)

> **🎯 เป้าหมายของการบันทึกนี้:** สรุปบทเรียนจากการเปลี่ยนวิธีอัดคลิป EP #1 เป็น **Slide-Led Storytelling (HTML/CSS `1920×1080`)** และสร้าง Pipeline ตัดต่ออัตโนมัติด้วย Python + FFmpeg 9.0.2 จนได้ไฟล์พร้อมอัปโหลดในวันเดียว

---

## 1. TL;DR — วันนี้ได้เรียน / ได้สร้างอะไร? (1–3 บรรทัด)
- **สิ่งที่ทำ/เรียนรู้:** สร้าง Thumbnail `1280×720` + สไลด์นำเสนอ `1920×1080` ด้วย HTML/CSS, อัดคลิป Raw `27:00.55`, และเขียนสคริปต์ `auto_cut_video.py` (Python stdlib + FFmpeg 9.0.2) ตัดช่วงที่พัง + ช่วงเงียบออกเหลือ **`10:37.82`** (`152` ช่วง) พร้อมดันเสียงจาก `-41.8 dB` ขึ้นมาเป็น `-20.1 dB` แล้วสกัดเป็น Skill `/learn`
- **ทำไมเรื่องนี้ถึงสำคัญ:** ลดแรงเสียดทานตอนตั้งกล้องและลดเวลาตัดต่อคลิปยาวลงได้เกินครึ่งด้วยต้นทุน `฿0` และได้มาตรฐานสไลด์/ตัดต่อถาวรไว้ใช้ซ้ำตั้งแต่ EP #2 เป็นต้นไป

---

## 2. ปัญหาที่เจอ & วิธีลงมือแก้ (Problem → Build / Solution)
- **ปัญหาตั้งต้น (Pain Point / Bottleneck):**
  1. **FFmpeg 9.0.2 Compatibility & RAM:** Flag `-filter_complex_script` ถูกถอดออกใน FFmpeg 9.0.2 และการใช้กราฟ `split=152` + `concat` ในคำสั่งเดียวเสี่ยงกิน RAM มหาศาล
  2. **Silence Threshold Trap:** ไมค์ที่อัดจาก OBS มี `mean_volume -41.8 dB` หากใช้ค่า Default `-35 dB` จะตัดโดนช่วงพูดเบาหายไป
  3. **Keyboard/Mouse Click Leaks:** เมื่อใส่ Padding หัว-ท้าย `0.2s` เสียงคลิกเมาส์สั้นๆ `0.01s` จะกลายเป็นช่วงความยาว `0.41s` ที่รอดผ่านการกรองปกติ
  4. **Slide Canvas Hygiene:** ป้ายกำกับโครงเรื่องมุมซ้ายล่าง (`- Hook`) และคำแนะนำคีย์ลัดมุมขวาล่างไม่ควรติดเข้าไปในภาพวิดีโอที่อัดจริง
- **แนวทาง/สถาปัตยกรรมที่เลือกใช้ (Solution & Trade-offs):**
  1. **Chunked `-ss` Extraction + `-f concat` Demuxer:** แยกตัดทีละช่วงแล้วรวมไฟล์ด้วย `-c:v copy` รอบเดียว ทำให้ใช้ RAM ต่ำคงที่และรันผ่านบน FFmpeg 9.0.2 ได้เสถียร
  2. **Calibrate ก่อนตัด (`volumedetect` + Contact Sheet):** ใช้ `ffmpeg -vf "fps=1/30,scale=320:180,tile=9x6"` (และซูมช่วงรอยต่อ `fps=1`) หาพิกัด `11:20–22:10` ที่ต้องตัดออก, ตั้ง `--noise-db -50 --min-silence 0.9 --pad 0.2 --min-segment 0.55`, แล้วปิดท้ายด้วย `loudnorm=I=-16:TP=-1.5:LRA=11`
  3. **Skill Persistence (`/learn`):** อัปเดตมาตรฐาน HTML Slide Deck (ซ้ายล่างเลขหน้าล้วน, ขวาล่างว่าง 100%) และ Step 4B Auto-Cut เข้าไปใน `.agents/skills/youtube-ep-packaging/SKILL.md`
- **ขั้นตอนที่ลงมือทำจริง (What I Built / Tested):**
  1. ออกแบบ Thumbnail `thumb_ep01_v1_1280x720.png` (Chrome Headless + QA Preview `168px` และ Timestamp Safe Zone)
  2. สร้าง Deck 10 สไลด์ `slides_ep01.html` (`translate(-50%, -50%) scale(...)` Auto-Fit `F11`)
  3. เขียนและรัน `02-Business/YouTube/scripts/auto_cut_video.py` ตัดไฟล์ Raw `27:00.55` (`107 MB`) ➡️ `10:37.82` (`~37.1 MB`), `mean_volume: -20.1 dB`, `max_volume: -1.3 dB`

---

## 3. หลักฐานผลงาน & Tech Stack (Portfolio Proof of Work)
- **Tech Stack / เครื่องมือ:** `Python 3.15 (stdlib only)`, `FFmpeg 9.0.2 (Gyan.FFmpeg)`, `HTML/CSS + Chrome Headless`, `OBS Studio`
- **ผลลัพธ์จริงที่วัดได้ (เฉพาะที่ทดสอบจริง):** ลดความยาว Raw `27:00.55` เหลือ `10:37.82` (`152` ช่วง, ขนาด `38,896,982` bytes), ปรับเสียงจาก `-41.8 dB` เป็น `-20.1 dB` (Peak `-1.3 dB`), คง `1920×1080 @ 60fps`
- **ลิงก์ / ไฟล์อ้างอิงภายใน:** `EP #1 Blueprint` • `YouTube Calendar`

> [!CAUTION] PDPA & Security Check ก่อนเผยแพร่สาธารณะ
> - ระบุเฉพาะชื่อไฟล์และผลลัพธ์ทางเทคนิค ไม่เปิดเผยข้อมูลส่วนตัวหรือ Path บัญชีที่มี Token/Credential

---

## 4. แพ็กเกจโพสต์ Learn in Public (พร้อมก๊อปปี้ใช้งานทันที)

### 🐦 X (Twitter) — Short Post / Thread Hook
```text
TIL: วิธีลดเวลาตัดคลิป YouTube จาก 27 นาทีเหลือ 10 นาทีครึ่ง ด้วย Python + FFmpeg (ไม่เสียเงินสักบาท)

3 ทริคที่เพิ่งเจอหน้างานจริงวันนี้:
1. เช็ก volumedetect ก่อนตัดความเงียบ! (ไมค์พูดเบา -41.8 dB ถ้าใช้เกณฑ์ -35 dB เสียงพูดจะโดนหั่นหาย)
2. ตั้ง min-segment 0.55s กันเสียงคลิกเมาส์ที่ถูก pad ยืดเป็น 0.4s
3. FFmpeg 9.0 ถอด -filter_complex_script แล้ว — ให้ตัด chunk ด้วย -ss แล้วต่อด้วย -f concat แทน ประหยัด RAM กว่าเยอะ + ดันเสียง loudnorm -16 LUFS จบในรอบเดียว
```

### 📘 Facebook Page / LinkedIn — Story Post
```text
อยากทำคลิป YouTube แต่ติดตรง "ขี้เกียจตั้งกล้อง + ขี้เกียจตัดต่อ" — วันนี้เลยลองเปลี่ยนมาทำเป็น HTML Slide Deck + ระบบ Auto-Cut ด้วย FFmpeg ครับ

ผลลัพธ์จากคลิปแรกที่อัดสดวันนี้ (ความยาวดิบ 27 นาที):
🔎 ปัญหาที่เจอระหว่างทาง:
- มีช่วงทดสอบ Live Demo ที่ต้องตัดออกยาว ~10 นาที
- เสียงไมค์ต้นฉบับค่อนข้างเบา (เฉลี่ย -41.8 dB)
- พอลองใช้ Flag เก่าอย่าง -filter_complex_script บน FFmpeg 9.0.2 ระบบไม่รองรับแล้ว

🛠️ ระบบที่เขียนขึ้นมาแก้จบในครั้งเดียว:
1. สกัดเฟรม Contact Sheet ทุก 30 วิ และซูมดูทีละ 1 วิ เพื่อหาจุดตัดเป๊ะๆ โดยไม่ต้องนั่งไถ Timeline เอง
2. ตัดความเงียบที่ -50 dB + กรองเสียงคลิกสั้นกว่า 0.55 วิ ออกทั้งหมด
3. ปรับเสียงมาตรฐาน YouTube (-16 LUFS) จนความดังขยับจาก -41.8 dB มาอยู่ที่ -20.1 dB (เหลือคลิปพร้อมอัปโหลด 10:37 นาที ขนาด 37 MB)

💡 บทเรียนที่ได้:
พอทำจบปุ๊บ ผมเอาบทเรียนทั้งหมดไปอัปเดตเป็น Agent Skill ถาวรทันที คลิปหน้าแค่ส่งหัวข้อก็ได้ทั้งสไลด์และสคริปต์ตัดต่ออัตโนมัติในรอบเดียวครับ
```

### 🎬 YouTube / Short Video Hook *(ถ้าต่อยอดเป็นคลิปได้)*
- **ไอเดียชื่อคลิป:** `วิธีให้ AI ทำสไลด์ + เขียนสคริปต์ FFmpeg ตัดคลิป 27 นาทีเหลือ 10 นาทีอัตโนมัติ`
- **Hook 5 วินาทีแรก:** `คลิปดิบยาว 27 นาทีที่มีช่วงพูดผิดกับช่วงเงียบเต็มไปหมด... ผมใช้สคริปต์ Python กับ FFmpeg ตัดเหลือ 10 นาทีครึ่งพร้อมปรับเสียงเสร็จในคำสั่งเดียว`

---

## 5. เช็กลิสต์ซิงค์ระบบ (Mandatory Rollup Sync)
- [x] อัปเดตตาราง **Portfolio Highlights** และ **TIL Log** ใน `til_summary.md`
- [x] อัปเดต `03-Learning/index.md`
- [x] อัปเดต `content_calendar.md`
- [x] บันทึกลงบนสุดของ `log.md`

