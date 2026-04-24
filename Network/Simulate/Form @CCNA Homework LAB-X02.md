# CCNA Homework LAB-X02: Password Recovery

**รูปที่ 1:** กระบวนการ Boot ของ Router
![[Pasted image 20260424103654.png]]

**รูปที่ 2:** การเข้าโหมด ROMMON และข้ามการโหลด Startup Config
![[Pasted image 20260424103713.png]]

**รูปที่ 3:** การดึงการตั้งค่าเดิมกลับมา
![[Pasted image 20260424103735.png]]

**รูปที่ 4:** การตั้งรหัสผ่านใหม่
![[Pasted image 20260424103754.png]]

**รูปที่ 5:** การคืนค่า Configuration Register และบันทึกการตั้งค่า
![[Pasted image 20260424103802.png]]

---

## 🧠 Technical Deep Dive: Why & How?

### 1. The Configuration Register Logic
- **Why:** เมื่อลืมรหัสผ่าน เราไม่สามารถเข้าโหมด `enable` ได้ วิธีเดียวคือต้องสั่งให้ Router "แกล้งทำเป็นเครื่องเปล่า" โดยไม่โหลดไฟล์คอนฟิกที่มีรหัสผ่านเดิมติดมา
- **How (0x2142):** 
    - ค่าปกติคือ `0x2102` (บิตที่ 6 เป็น 0) -> สั่งให้โหลดคอนฟิกจาก NVRAM
    - ค่ากู้คืนคือ `0x2142` (บิตที่ 6 เป็น 1) -> สั่งให้ข้าม (Ignore) NVRAM ในการ Boot
- **The Recovery Process:** หลังจากเข้าเครื่องได้ เราต้องสั่ง `copy startup-config running-config` เพื่อดึงคอนฟิกเก่ามาแก้ (ถ้าไม่ทำแบบนี้แล้วบันทึกทับ คอนฟิกเก่าจะหายหมด)

### 2. ROMMON Mode
- **Mechanism:** คือมินิระบบปฏิบัติการที่อยู่ใน ROM ของ Router ทำหน้าที่เหมือน BIOS/UEFI ใน PC ใช้จัดการฮาร์ดแวร์เบื้องต้นก่อนที่ IOS (Cisco OS) จะถูกโหลดขึ้นมา

### 📨 Packet/Signal Flow
- **Break Signal:** ระหว่าง Boot การส่งสัญญาณ **Break** (Ctrl+Pause/Break) ผ่านสาย Console จะเป็นการส่งสัญญาณขัดจังหวะ (Interrupt) ไปที่ CPU ของ Router เพื่อสั่งให้หยุดโหลด IOS และกระโดดเข้าสู่โหมด ROMMON ทันที
- **Bootstrap Process:** ROM -> ROMMON -> Check Config Register -> Load IOS from Flash (or skip) -> Load Config from NVRAM (or skip).
