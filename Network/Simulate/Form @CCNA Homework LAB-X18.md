# CCNA Homework LAB-X18: PPP PAP

**รูปที่ 1:** แผนผังการต่อเชื่อม (Topology) และการตั้งค่าพื้นฐาน
![](Image/Pasted%20image%2020260512145047.png)

> [!note] ⚙️ การเตรียมตัว
> ใน Lab นี้เราจะตั้งค่าการเชื่อมต่อแบบ Serial ระหว่าง Router 2 ตัว โดยใช้โปรโตคอล PPP และเพิ่มความปลอดภัยด้วยการทำ Authentication แบบ PAP (Password Authentication Protocol)

**รูปที่ 2:** การตั้งค่า Username และ Password บน R1
![](Image/Pasted%20image%2020260512145102.png)

> [!note] ⚙️ การกำหนดสิทธิ์
> ใช้คำสั่ง `username [Remote_Router_Name] password [Password]` เพื่อสร้างฐานข้อมูลผู้ใช้สำหรับการตรวจสอบสิทธิ์

**รูปที่ 3:** การเปิดใช้งาน PPP และ PAP บน Interface
![](Image/Pasted%20image%2020260512145111.png)

> [!note] ⚙️ คำสั่งเปิดใช้งาน
> เข้าไปยัง interface serial แล้วใช้คำสั่ง `encapsulation ppp` ตามด้วย `ppp authentication pap` และส่งรหัสผ่านด้วย `ppp pap sent-username ...`

**รูปที่ 4:** การตรวจสอบสถานะการเชื่อมต่อ
![](Image/Pasted%20image%2020260512145122.png)

**รูปที่ 5:** ผลลัพธ์การตรวจสอบสิทธิ์สำเร็จ
![](Image/Pasted%20image%2020260512145130.png)

> [!note] ⚙️ การตรวจสอบ
> ใช้คำสั่ง `show interface serial` เพื่อดูสถานะ LCP ว่าเป็น Open หรือไม่ และตรวจสอบว่าการส่งข้อมูลสามารถทำได้ปกติ

---

## 🧠 Technical Deep Dive: Why & How?

### 1. PPP (Point-to-Point Protocol)
- **Why:** เป็นโปรโตคอลมาตรฐานที่ใช้เชื่อมต่อ Link แบบ Point-to-Point (WAN) แทนที่ HDLC เพราะ PPP รองรับการทำ Authentication และรองรับหลายโปรโตคอลในระดับ Network Layer
- **How:** PPP ใช้ LCP (Link Control Protocol) ในการสร้างและทดสอบการเชื่อมต่อ และใช้ NCP (Network Control Protocol) ในการตกลงเรื่องโปรโตคอลระดับบน

### 2. PAP (Password Authentication Protocol)
- **Concept:** เป็นการตรวจสอบสิทธิ์แบบ 2-way handshake โดยฝั่ง Client จะส่ง Username และ Password ไปยังฝั่ง Server แบบ Clear Text (ไม่ปลอดภัยเท่า CHAP)
- **Security Note:** เนื่องจากส่งรหัสผ่านแบบไม่เข้ารหัส จึงควรใช้ในระบบที่มั่นใจว่าปลอดภัยจากการดักฟัง หรือใช้ร่วมกับการเข้ารหัสระดับอื่น

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** Username หรือ Password ไม่ตรงกันทั้งสองฝั่ง หรือลืมตั้งค่า `ppp pap sent-username` ทำให้การเจรจา LCP ไม่สำเร็จ (Link Down)
- **Solution:** ตรวจสอบ Username/Password ให้ตรงกัน (Case-sensitive) และตรวจสอบว่าได้เปิดใช้งาน `encapsulation ppp` แล้วทั้งสองฝั่ง
- **Verification Commands:**
    - `show interface serial [number]`: ตรวจสอบว่า encapsulation เป็น PPP และสถานะเป็น up/up
    - `debug ppp authentication`: ใช้เพื่อดูขั้นตอนการส่งรหัสผ่านและการตอบรับจากฝั่งตรงข้าม
