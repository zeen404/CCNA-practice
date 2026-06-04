# CCNA Homework LAB-X25: OSPF Authentication

**รูปที่ 1:** แผนผังเครือข่ายสำหรับการทำ Authentication
![](Image/Pasted%20image%2020260512151122.png)

**รูปที่ 2:** การตั้งค่า MD5 Authentication บน Interface
![](Image/Pasted%20image%2020260512151302.png)

> [!note] ⚙️ การเพิ่มความปลอดภัย
> OSPF Authentication ช่วยป้องกันการนำ Router แปลกปลอมเข้ามาเชื่อมต่อในระบบ (Rogue Router) เพื่อดักจับข้อมูลหรือทำ Routing Attack

**รูปที่ 3:** ข้อความ Error เมื่อรหัสผ่านไม่ตรงกัน
![](Image/Pasted%20image%2020260512151316.png)

> [!note] ⚙️ การแจ้งเตือน
> หากรหัสผ่าน (Key) หรือประเภทการตรวจสอบสิทธิ์ไม่ตรงกัน Router จะแจ้งเตือน "Authentication failed" และจะตัดความสัมพันธ์กับ Neighbor ทันที

**รูปที่ 4:** การตั้งค่าที่ถูกต้องทั้งสองฝั่ง
![](Image/Pasted%20image%2020260512151327.png)

**รูปที่ 5:** ตรวจสอบสถานะการเชื่อมต่อหลังทำ Authentication
![](Image/Pasted%20image%2020260512151346.png)

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Authentication Types in OSPF
- **Null Authentication (Type 0):** ไม่มีการตรวจสอบสิทธิ์ (ค่าเริ่มต้น)
- **Simple Password (Type 1):** ส่งรหัสผ่านแบบ Clear Text (ไม่ปลอดภัย)
- **MD5 Authentication (Type 2):** ใช้การทำ Hash ข้อมูลด้วยอัลกอริทึม MD5 ก่อนส่ง (แนะนำให้ใช้)

### 2. Implementation Level
- **Interface Level:** ตั้งค่าเจาะจงที่แต่ละ Interface (ยืดหยุ่นกว่า)
- **Area Level:** ตั้งค่าให้ Router ทุกตัวใน Area นั้นๆ ต้องทำ Authentication เหมือนกันหมด

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** ใส่ Key ID ไม่ตรงกัน (เช่น ฝั่งหนึ่งใช้ Key 1 อีกฝั่งใช้ Key 2) หรือรหัสผ่านไม่ตรงกัน ทำให้ Adjacency หลุด
- **Solution:** ต้องตรวจสอบทั้ง `ip ospf message-digest-key [ID] md5 [Password]` และ `ip ospf authentication message-digest` ให้ตรงกันทั้งสองฝั่ง
- **Verification Commands:**
    - `show ip ospf interface`: ตรวจสอบว่า Interface นั้นมีการเปิดใช้งาน Authentication หรือไม่
    - `debug ip ospf adj`: ดูขั้นตอนการสร้างความสัมพันธ์และจุดที่เกิด Error ในการตรวจสอบสิทธิ์
