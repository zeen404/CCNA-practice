# CCNA Homework LAB-X30: EIGRP Authentication

**รูปที่ 1:** ตรวจสอบสถานะการเชื่อมต่อ EIGRP Neighbor ก่อนเริ่มทำ Authentication
![](Image/Pasted%20image%2020260512152830.png)

> [!note] ⚙️ การเตรียมตัว
> การทำ Authentication ช่วยเพิ่มความปลอดภัยให้ระบบ Routing โดยการบังคับให้ Router ต้องยืนยันตัวตนก่อนแลกเปลี่ยน Routing Table เพื่อป้องกันการโจมตีหรือการนำ Router ปลอมมาเชื่อมต่อในระบบ

**รูปที่ 2:** การสร้าง Key Chain และกำหนดรหัสผ่าน (Key String)
![](Image/Pasted%20image%2020260512152845.png)

> [!note] ⚙️ ขั้นตอนที่ 1: Key Chain
> เราสร้าง `key chain [name]` เพื่อเก็บกลุ่มของกุญแจ โดยระบุ `key [number]` และ `key-string [password]` ซึ่งรหัสนี้จะต้องตรงกันทั้งสองฝั่งของ Router

**รูปที่ 3:** การเปิดใช้งาน EIGRP Authentication บนอินเทอร์เฟซ
![](Image/Pasted%20image%2020260512152855.png)

> [!note] ⚙️ ขั้นตอนที่ 2: Interface Config
> ต้องระบุสองส่วนคือ 1. โหมดการเข้ารหัส (MD5) และ 2. กุญแจที่จะนำมาใช้ โดยใช้คำสั่ง `ip authentication mode eigrp [AS] md5` และ `ip authentication key-chain eigrp [AS] [chain-name]`

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Routing Security (MD5 Authentication)
- **Why:** หากไม่มี Authentication, ใครก็ตามที่เชื่อมต่อ Router เข้ามาในเครือข่ายและตั้งค่า AS Number ตรงกัน จะสามารถส่งข้อมูลเส้นทางปลอม (Route Injection) เพื่อดักจับข้อมูลหรือทำให้ระบบล่มได้
- **How:** EIGRP รองรับการทำ MD5 Hashing ซึ่งจะส่งค่า Hash ของรหัสผ่านไปพร้อมกับ Routing Update แทนการส่งรหัสผ่านแบบ Clear Text ทำให้มีความปลอดภัยสูงขึ้น

### 2. Key Chain Concept
- **Flexibility:** การใช้ Key Chain ช่วยให้เราสามารถกำหนดช่วงเวลาการใช้งานของแต่ละกุญแจได้ (Key Rotation) เช่น กำหนดให้ Key 1 หมดอายุในสิ้นเดือน และเริ่มใช้ Key 2 ทันที เพื่อความปลอดภัยที่เหนือกว่าการใช้รหัสผ่านคงที่เพียงชุดเดียว

---

## 🛠️ Troubleshooting & Verification

- **Common Problem:** Neighbor ขาดการเชื่อมต่อ (Neighbor Down) ทันทีหลังตั้งค่าเสร็จ สาเหตุมักมาจาก Key String ไม่ตรงกัน หรือตั้งค่า Authentication เพียงฝั่งเดียว
- **Solution:** ตรวจสอบรหัสผ่านใน `key-string` และ `key id` ให้ตรงกันทั้งสองฝั่ง และตรวจสอบว่าพิมพ์ชื่อ `key chain` ถูกต้อง (Case-sensitive)
- **Verification Commands:**
    - `show ip eigrp neighbors`: ตรวจสอบว่ายังมี Neighbor อยู่ครบถ้วนหรือไม่
    - `show key chain`: ดูสถานะของกุญแจที่สร้างขึ้นและวันหมดอายุ
    - `debug eigrp packets`: (ใช้ด้วยความระมัดระวัง) เพื่อดูการแจ้งเตือน Authentication Mismatch ในกรณีที่เชื่อมต่อไม่สำเร็จ
