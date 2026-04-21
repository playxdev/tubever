# 🚀 TubeVer - Concept Specification (Beta 0.1)

**Slogan:** WATCH, PLAY, REWARD  
**Vision:** เปลี่ยนทุกการติดตามให้เป็นรางวัล เปลี่ยนผู้ชมให้เป็นส่วนหนึ่งของกิจกรรม  

---

## 1. ภาพรวมโปรเจกต์ (Overview)

**TubeVer (tubever.com)** คือแพลตฟอร์ม SaaS สำหรับสร้างกิจกรรม (Gamification) ระหว่าง YouTuber และ Fandom  

แพลตฟอร์มนี้แก้ปัญหาความยุ่งยากในการจับรางวัลแบบแมนนวล โดยการเชื่อมต่อกับ YouTube Data API เพื่อยืนยันสถานะการเป็นสมาชิก (Subscriber, Membership Tier, Duration) และนำข้อมูลเหล่านั้นมาคำนวณเป็น **"ตัวคูณโชค" (Weighted Probability)** เพื่อให้สิทธิพิเศษแก่กลุ่มผู้ติดตามที่สนับสนุนช่องได้อย่างแม่นยำและยุติธรรม  

---

## 2. ฟีเจอร์หลัก (Core Features)

### 🎯 ฝั่งผู้ชม (Viewer Experience)

- **YouTube OAuth Login**  
  เข้าสู่ระบบอย่างปลอดภัยเพื่อยืนยันตัวตนผ่านบัญชี Google/YouTube  

- **Automated Eligibility Check**  
  ตรวจสอบสิทธิ์การเข้าร่วมกิจกรรมและระดับตัวคูณอัตโนมัติ (ไม่ต้องส่งหลักฐานการสมัครสมาชิก)  

- **The Gacha Wheel**  
  หน้า UI มินิเกมกงล้อสุ่มรางวัลที่แสดงผลสิทธิ์ตามจริง พร้อมแอนิเมชันที่ลื่นไหล  

- **Dynamic UI**  
  รองรับ Dark / Light Mode เน้นดีไซน์ Modern Retro ที่มีความ Playful เข้าถึงกลุ่มเกมเมอร์และสตรีมเมอร์  

---

### 🛠 ฝั่งครีเอเตอร์ (Creator Dashboard)

- **Campaign Management**  
  ระบบสร้างและตั้งค่าแคมเปญกิจกรรมแจกรางวัล  

- **Rule Engine**  
  ระบบตั้งเงื่อนไขสิทธิ์พื้นฐาน เช่น  
  - ผู้ติดตามทั่วไป = 1 สิทธิ์  
  - สมาชิกระดับ Tier 1 ที่เป็นมานานกว่า 3 เดือน = 2 สิทธิ์  

- **Smart Link Generator**  
  ระบบสร้างลิงก์กิจกรรม (Unique Link) สำหรับนำไป Pinned ในช่องแชทสดหรือใต้คลิปวิดีโอ  

---

## 3. สถาปัตยกรรมและเทคโนโลยี (Tech Stack)

- **Frontend:** Next.js (React) + TypeScript  
  จัดการ State ฝั่งไคลเอนต์และรองรับการขยายสเกล  

- **Animation:** Framer Motion  
  ควบคุมแอนิเมชันกงล้อให้สมูทระดับ 60fps  

- **Backend API:** Golang  
  โครงสร้าง API 8 ฟังก์ชัน รองรับ Concurrency สูงและประมวลผลรวดเร็ว  

- **Primary Database:** PostgreSQL  
  จัดการ Transaction, เก็บข้อมูลการสุ่ม และ Audit logs อย่างปลอดภัย  

- **Cache & Queue:** Redis  
  รับมือกับ Thundering Herd Problem และจัดการคิว  

- **Integration:** YouTube Data API v3  
  ตรวจสอบสถานะการติดตามและข้อมูลการเป็นสมาชิก  

---

## 4. ลำดับการทำงานของระบบ (System Flow)

1. **Authentication**  
   ผู้ชมกดเข้าลิงก์กิจกรรม → Login ผ่าน YouTube → ระบบรับ Authorization Code ไปแลก JWT  

2. **Verification**  
   Backend API นำ Channel ID ของผู้ชมไปตรวจสอบกับข้อมูลหลังบ้านของ Creator ผ่าน API  
   เพื่อหาค่า Membership Tier และระยะเวลา  

3. **Calculation**  
   ระบบคำนวณน้ำหนัก (Weight) ตาม Rule Engine ที่ Creator ตั้งไว้  

4. **Execution**  
   ผู้ชมกด "หมุนกงล้อ" → Backend (Golang) สุ่มรางวัลตามน้ำหนักสิทธิ์ →  
   บันทึกผลลง Database → ส่งข้อมูลกลับให้ Frontend (Next.js)  
   แสดงแอนิเมชันกงล้อไปหยุดที่รางวัลนั้น  

---

## 📌 หมายเหตุการนำไปใช้

ข้อมูลชุดนี้ครอบคลุมทั้งมิติทางธุรกิจ (Business Logic) และมิติทางวิศวกรรม (Engineering)  
สามารถใช้เป็นเอกสารอ้างอิงหลักสำหรับทีมพัฒนาในการอิง Concept ของ Beta 0.1 ได้อย่างสมบูรณ์