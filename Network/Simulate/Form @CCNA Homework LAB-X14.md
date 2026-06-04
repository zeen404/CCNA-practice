# CCNA Homework LAB-X14: L2 Loop Test

**รูปที่ 1:** แผนผังเครือข่ายและการสร้าง Loop ทางกายภาพ
![](Image/Pasted%20image%2020260511173508.png)

**รูปที่ 2:** การสังเกตพฤติกรรมของข้อมูล (Broadcast Storm)
![](Image/Pasted%20image%2020260511173542.png)

**รูปที่ 3:** ผลกระทบต่อประสิทธิภาพและการทดสอบการเชื่อมต่อ
![](Image/Pasted%20image%2020260511173555.png)

---

## 🧠 Technical Deep Dive: Why & How?

### 1. The Anatomy of a Loop
- **Why:** ใน Layer 2 เฟรมของ Ethernet ไม่มีค่า **TTL (Time to Live)** เหมือนใน Layer 3 (IP) หมายความว่าถ้าเฟรมวิ่งวนเป็นวงกลม มันจะวิ่งไปตลอดกาลจนกว่าสายจะขาดหรือเครื่องจะดับ
- **How:** เมื่อเกิด Broadcast Frame (เช่น ARP Request) วิ่งเข้าสู่ Loop มันจะถูกส่งต่อ (Flood) ไปมาแบบทวีคูณ จนกินแบนด์วิธทั้งหมดของสายแลน เรียกว่า **Broadcast Storm**

### 2. MAC Table Instability
- **Mechanism:** Switch จะเกิดอาการสับสน (MAC Flapping) เพราะเห็น MAC Address เดียวกันโผล่มาจากหลายพอร์ตพร้อมกัน ทำให้ตาราง MAC Address อัปเดตไปมาอย่างรวดเร็วและไม่สามารถส่งข้อมูลได้ถูกต้อง

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** ไฟสถานะบน Switch กระพริบถี่ผิดปกติ (ทุกพอร์ตพร้อมกัน) และไม่สามารถ Ping ไปหาอุปกรณ์ใดๆ ได้เลย แม้แต่อุปกรณ์ที่อยู่ข้างๆ
- **Solution:** 
    1. ตรวจสอบสถานะ STP ด้วยคำสั่ง `show spanning-tree` ว่ามีพอร์ตไหนถูก Block หรือไม่
    2. หากตั้งใจทดสอบโดยปิด STP ให้รีบตัดสายเส้นที่เกินมาทิ้งก่อนระบบจะล่มทั้งหมด
- **Verification:**
    - ใช้คำสั่ง `show interfaces [interface-id]` เพื่อดูปริมาณ Input/Output Traffic ว่าสูงผิดปกติหรือไม่
    - ตรวจสอบ CPU Usage ของ Switch (ถ้าเป็นอุปกรณ์จริง)
