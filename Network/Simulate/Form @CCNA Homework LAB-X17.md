# CCNA Homework LAB-X17: HDLC and PPP

**รูปที่ 1:** แผนผังการเชื่อมต่อ Serial Link ระหว่าง Router
![](Image/Pasted%20image%2020260512143912.png)

> [!note] ⚙️ WAN Encapsulation
> ในการเชื่อมต่อผ่านพอร์ต Serial เราสามารถเลือกใช้ Protocol ได้หลายชนิด โดยพื้นฐานของ Cisco จะใช้ HDLC เป็นค่าเริ่มต้น แต่ใน Lab นี้เราจะทดสอบการเปลี่ยนเป็น PPP

**รูปที่ 2:** การเปลี่ยน Encapsulation เป็น PPP
![](Image/Pasted%20image%2020260512144144.png)

> [!note] ⚙️ คำสั่ง Encapsulation
> เข้าไปที่ Interface Serial และใช้คำสั่ง `encapsulation ppp` เพื่อเปลี่ยนโปรโตคอลในการห่อหุ้มข้อมูล (ต้องตั้งค่าให้ตรงกันทั้งสองฝั่ง)

**รูปที่ 3:** การตรวจสอบสถานะด้วยคำสั่ง Show Interface
![](Image/Pasted%20image%2020260512144200.png)

> [!note] ⚙️ การตรวจสอบ
> สังเกตบรรทัด "Encapsulation PPP, LCP Open" หมายถึงการเจรจาเชื่อมต่อระดับ Layer 2 สำเร็จ และพร้อมสำหรับการส่งข้อมูล

---

## 🧠 Technical Deep Dive: Why & How?

### 1. HDLC (High-Level Data Link Control)
- **What:** เป็นโปรโตคอล Layer 2 มาตรฐานสำหรับการเชื่อมต่อแบบ Point-to-Point
- **Cisco HDLC:** Cisco มีการเพิ่มฟิลด์ "Type" เข้าไปในมาตรฐาน HDLC ปกติเพื่อให้รองรับการส่งข้อมูลหลายโปรโตคอล (Multi-protocol) ทำให้ HDLC ของ Cisco ไม่สามารถคุยกับยี่ห้ออื่นได้

### 2. PPP (Point-to-Point Protocol)
- **Why PPP:** เป็นโปรโตคอลที่เป็นมาตรฐานสากล (Standard) และมีฟีเจอร์ที่เหนือกว่า HDLC เช่น:
    - **Authentication:** รองรับการยืนยันตัวตน (PAP/CHAP)
    - **Multilink:** รวมสาย Serial หลายเส้นเข้าด้วยกัน
    - **Compression:** บีบอัดข้อมูลก่อนส่ง
    - **Error Detection:** การตรวจสอบความผิดพลาดของข้อมูล

### 3. LCP (Link Control Protocol)
- ใน PPP จะมี LCP ทำหน้าที่สร้าง (Establish), ตั้งค่า (Configure) และทดสอบการเชื่อมต่อ ถ้า LCP ไม่ "Open" การสื่อสารจะไม่เกิดขึ้น

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** สถานะอินเทอร์เฟซเป็น "up" แต่ Protocol เป็น "down"
- **Solution:** ตรวจสอบว่า Encapsulation ทั้งสองฝั่งตรงกันหรือไม่ (เช่น ฝั่งหนึ่งเป็น HDLC อีกฝั่งเป็น PPP จะคุยกันไม่ได้)
- **Verification Commands:**
    - `show interface serial <number>`: ตรวจสอบสถานะและโปรโตคอลที่ใช้งาน
    - `debug ppp negotiation`: ใช้ตรวจสอบขั้นตอนการเจรจาของ PPP (ใช้เฉพาะเมื่อเกิดปัญหา)
