# CCNA Homework LAB-X28: EIGRP (Enhanced Interior Gateway Routing Protocol)

**รูปที่ 1:** แผนผังเครือข่ายสำหรับ EIGRP
![](Image/Pasted%20image%2020260512151749.png)

**รูปที่ 2:** การเปิดใช้งาน EIGRP และ Autonomous System (AS)
![](Image/Pasted%20image%2020260512151806.png)

> [!note] ⚙️ การตั้งค่าเริ่มต้น
> ใช้คำสั่ง `router eigrp [AS_Number]` เพื่อเริ่มทำงาน โดยเลข AS นี้ต้องตรงกันใน Router ทุกตัวที่ต้องการแลกเปลี่ยนข้อมูลกัน (ต่างจาก OSPF Process ID)

**รูปที่ 3:** การประกาศเครือข่ายและความสัมพันธ์เพื่อนบ้าน
![](Image/Pasted%20image%2020260512151826.png)

> [!note] ⚙️ ความเร็วในการเชื่อมต่อ
> EIGRP มีจุดเด่นที่ความรวดเร็วในการสร้างความสัมพันธ์ (Neighbor Adjacency) และการแจ้งเตือนสถานะ "Dual-5-Adjchange" เมื่อพบเพื่อนบ้านใหม่

**รูปที่ 4:** ตรวจสอบตาราง Routing และสัญลักษณ์ D
![](Image/Pasted%20image%2020260512151850.png)

> [!note] ⚙️ สัญลักษณ์ D (DUAL)
> เส้นทางที่เรียนรู้ผ่าน EIGRP จะใช้สัญลักษณ์ `D` ซึ่งย่อมาจากอัลกอริทึม DUAL (Diffusing Update Algorithm) ที่ใช้ในการคำนวณเส้นทาง

---

## 🧠 Technical Deep Dive: Why & How?

### 1. EIGRP Concept (Advanced Distance Vector)
- **Why:** EIGRP เป็นโปรโตคอลที่รวมข้อดีของทั้ง Distance Vector และ Link State เข้าด้วยกัน (Hybrid) ให้ความเร็วในการ Convergence สูงมากและใช้ทรัพยากรน้อย
- **How:** ใช้ค่า Metric ที่ซับซ้อน (Composite Metric) โดยคำนวณจาก Bandwidth, Delay, Reliability และ Load (แต่ค่าเริ่มต้นจะใช้แค่ Bandwidth และ Delay)

### 2. DUAL Algorithm & Successor
- **Successor:** เส้นทางที่ดีที่สุดที่ถูกเลือกไปใส่ใน Routing Table
- **Feasible Successor (FS):** เส้นทางสำรองที่ผ่านเงื่อนไขความปลอดภัย (Loop-free) พร้อมใช้งานทันทีหากเส้นทางหลักขาด ทำให้ไม่ต้องรอคำนวณใหม่

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** AS Number ไม่ตรงกัน หรือค่า K-Values (Metric Weights) ไม่ตรงกัน ทำให้ไม่สามารถสร้าง Adjacency ได้
- **Solution:** ตรวจสอบ AS Number ให้ตรงกันทุกตัว และหลีกเลี่ยงการปรับแต่ง K-Values หากไม่จำเป็น
- **Verification Commands:**
    - `show ip eigrp neighbors`: ตรวจสอบรายชื่อเพื่อนบ้านที่เชื่อมต่ออยู่
    - `show ip eigrp topology`: ดูตาราง Topology (รวมเส้นทางสำรอง FS)
    - `show ip route eigrp`: ตรวจสอบเส้นทางที่ได้รับมาในตารางหลัก
