# CCNA Homework LAB-X19: PPP CHAP

**รูปที่ 1:** แผนผังการต่อเชื่อม (Topology) สำหรับการทำ CHAP
![](Image/Pasted%20image%2020260512150126.png)

**รูปที่ 2:** การตั้งค่า Username และ encapsulation PPP
![](Image/Pasted%20image%2020260512145241.png)

> [!note] ⚙️ ความปลอดภัยที่เหนือกว่า
> CHAP (Challenge Handshake Authentication Protocol) ใช้การตรวจสอบสิทธิ์แบบ 3-way handshake ซึ่งมีความปลอดภัยสูงกว่า PAP เพราะไม่มีการส่งรหัสผ่านจริงไปบนสาย

**รูปที่ 3:** การเปิดใช้งาน PPP Authentication CHAP
![](Image/Pasted%20image%2020260512145311.png)

> [!note] ⚙️ ขั้นตอนการทำงาน
> เมื่อตั้งค่า `ppp authentication chap` แล้ว Router จะทำการท้าทาย (Challenge) ฝั่งตรงข้ามเพื่อยืนยันตัวตนโดยใช้ Hash Algorithm (MD5)

**รูปที่ 4:** การตรวจสอบสถานะอินเทอร์เฟซและการเชื่อมต่อ
![](Image/Pasted%20image%2020260512145326.png)

> [!note] ⚙️ ผลลัพธ์
> สถานะของ Serial Interface ควรเป็น `up` และ `line protocol up` โดยมีการระบุว่าใช้ encapsulation PPP และ LCP state เป็น Open

---

## 🧠 Technical Deep Dive: Why & How?

### 1. CHAP Mechanism (3-Way Handshake)
- **Step 1 (Challenge):** ตัวตรวจสอบ (Authenticator) ส่ง Challenge Message ไปยังฝั่งตรงข้าม
- **Step 2 (Response):** ฝั่งตรงข้ามคำนวณค่า Hash จาก Password และ Challenge ที่ได้รับ แล้วส่งค่า Hash กลับมา
- **Step 3 (Accept/Reject):** ตัวตรวจสอบเปรียบเทียบค่า Hash ที่คำนวณเองกับที่ได้รับ ถ้าตรงกันจะอนุญาตให้เชื่อมต่อ

### 2. Why CHAP is better than PAP?
- **Periodic Verification:** CHAP สามารถทำการตรวจสอบสิทธิ์ซ้ำได้เรื่อยๆ ตลอดระยะเวลาการเชื่อมต่อ
- **No Password in Wire:** ไม่มีการส่งรหัสผ่านแบบ Clear Text ทำให้ป้องกันการดักจับรหัสผ่าน (Eavesdropping) ได้ดีเยี่ยม

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** การตั้งชื่อ Username ไม่ตรงกับ Hostname ของฝั่งตรงข้าม หรือใช้ Password ต่างกัน ทำให้ค่า Hash ไม่ตรงกัน
- **Solution:** ตรวจสอบว่า `username` ที่ตั้งไว้ต้องตรงกับ `hostname` ของ Router อีกฝั่งหนึ่งพอดี และรหัสผ่านต้องเหมือนกันทุกตัวอักษร
- **Verification Commands:**
    - `show ppp multilink` (ถ้ามีการใช้งาน): ตรวจสอบการรวมลิงก์
    - `debug ppp negotiation`: ดูขั้นตอนการเจรจา LCP และ NCP
    - `debug ppp authentication`: ตรวจสอบว่าขั้นตอน CHAP ติดขัดที่ตรงไหน
