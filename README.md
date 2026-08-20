# 💸 bills-line — บันทึกบิล + แจ้งเตือนรอบจ่าย ผ่าน LINE

งานส่วนตัว · บันทึกค่าใช้จ่าย/บิล ส่งรูปเข้า LINE → OCR → 9arm AI ดึงข้อมูล → เก็บ → cron แจ้งเตือนก่อนรอบจ่าย
(แผน/ดีไซน์เต็มอยู่ใน Knowledge-Base: `02_Projects/Bill-Tracker-LINE/`)

## โครงสร้าง
```
index.html   ← LIFF page (Phase 0 = Hello World แสดงโปรไฟล์)
config.js    ← LIFF_ID (ไม่ลับ)
server/      ← webhook (Phase 1 ขึ้นไป — ยังว่าง)
```

## Phase 0 — LIFF Hello World (ทำอยู่)

### 1. Deploy หน้าเว็บ (เลือกทางใดทางหนึ่ง — ได้ HTTPS ฟรี)
- **GitHub Pages** (ง่ายสุด · repo ต้อง public): Settings → Pages → Deploy from branch `main` / root
  → ได้ `https://xerooneplus.github.io/bills-line/`
- **Cloudflare Pages / Netlify**: connect repo → auto HTTPS (รองรับ repo private)

### 2. สร้าง LIFF app
LINE Developers → channel **MyBill** → แท็บ **LIFF** → **Add**
- Endpoint URL = URL จากขั้นที่ 1 (เช่น `https://xerooneplus.github.io/bills-line/`)
- Size = **Full** · Scopes = `profile`, `openid`
- กด Add → คัดลอก **LIFF ID**

### 3. ใส่ LIFF ID
แก้ `config.js` → เปลี่ยน `PUT_LIFF_ID_HERE` เป็น LIFF ID จริง → commit + push (redeploy)

### 4. ทดสอบ
เปิดลิงก์ LIFF (`https://liff.line.me/<LIFF_ID>`) ในแอป LINE → ต้องเห็นชื่อ+รูปโปรไฟล์ตัวเอง = ✅

## Phase ถัดไป
- Phase 1: webhook (`server/`) รับรูปบิล → OCR → 9arm → บันทึก (Google Sheets)
- Phase 2: cron แจ้งเตือนรอบจ่าย (LINE push)
- Phase 3: LIFF ดู/แก้/สรุปยอด

## Secrets
Channel secret / access token / 9arm key → เก็บใน `server/.env` (gitignored) เท่านั้น ห้าม commit
