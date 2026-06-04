# CCNA Homework LAB-X12: Per-VLAN STP

**รูปที่ 1:** แผนผังเครือข่ายที่มี Loop (Redundant Links)
![](Image/Pasted%20image%2020260511164143.png)

**รูปที่ 2:** สถานะของ Spanning Tree ในสภาวะเริ่มต้น
![](Image/Pasted%20image%2020260511164209.png)

**รูปที่ 3:** การกำหนด Root Bridge สำหรับแต่ละ VLAN
![](Image/Pasted%20image%2020260511164245.png)

**รูปที่ 4:** การตรวจสอบ Priority และ MAC ของ Root Bridge
![](Image/Pasted%20image%2020260511164257.png)

**รูปที่ 5:** พอร์ตที่ถูก Block (Alternate Port) เพื่อกัน Loop
![](Image/Pasted%20image%2020260511164312.png)

**รูปที่ 6:** การทดสอบความถูกต้องของเส้นทาง
![](Image/Pasted%20image%2020260511164322.png)

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Loop Prevention
- **Why:** การต่อสายสำรอง (Redundancy) เป็นเรื่องดี แต่ถ้าไม่มีระบบจัดการ ข้อมูล Broadcast จะวิ่งวนเป็นวงกลม (Broadcast Storm) จนกินทรัพยากรเครื่องเต็ม 100% และทำให้เน็ตล่มทั้งระบบ
- **How:** STP จะทำการ "เลือกตั้ง" (Election) เพื่อหาผู้ดูแลระบบหลัก (**Root Bridge**) แล้วทำการปิดพอร์ตที่ทำให้เกิด Loop ชั่วคราว (Blocking State)

### 2. Per-VLAN STP (PVST)
- **Mechanism:** มาตรฐานของ Cisco ช่วยให้เราสามารถเลือกให้ Switch ตัวหนึ่งเป็นหัวหน้าของ VLAN 10 และอีกตัวเป็นหัวหน้าของ VLAN 20 ได้ เพื่อกระจายภาระงาน (Load Balancing)

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** ระบบใช้เวลานานเกินไป (30-50 วินาที) กว่าพอร์ตจะเปลี่ยนจากสีส้มเป็นสีเขียว
- **Solution:** หากพอร์ตนั้นต่อกับคอมพิวเตอร์ (ไม่ใช่ Switch) ให้ใช้คำสั่ง `spanning-tree portfast` เพื่อให้พอร์ตใช้งานได้ทันที
- **Verification:**
    - ใช้คำสั่ง `show spanning-tree` เพื่อดูว่าตัวไหนเป็น Root และพอร์ตไหนถูก Block (Alternate Port)
