# CCNA Homework LAB-X31: EIGRP Path Optimization

**รูปที่ 1:** แผนผังเครือข่ายที่มีเส้นทางสำรอง (Redundant Paths)
![](Image/Pasted%20image%2020260512153034.png)

> [!note] ⚙️ การเตรียมตัว
> ในเครือข่ายที่มีหลายเส้นทางไปยังปลายทางเดียวกัน EIGRP จะเลือกเส้นทางที่ดีที่สุดตามค่า Metric แต่บางครั้งเราต้องการกำหนดให้ข้อมูลวิ่งไปในเส้นทางที่เราต้องการ (Path Control) เพื่อบริหารจัดการ Bandwidth ให้คุ้มค่าที่สุด

**รูปที่ 2:** ตรวจสอบ EIGRP Topology Table เพื่อดูค่า FD และ RD
![](Image/Pasted%20image%2020260512153044.png)

> [!note] ⚙️ การตรวจสอบเบื้องต้น
> ใช้คำสั่ง `show ip eigrp topology` เพื่อดูว่ามีเส้นทางไหนบ้างที่เป็น Successor (เส้นทางหลัก) และ Feasible Successor (เส้นทางสำรอง)

**รูปที่ 3:** การปรับค่า Delay บนอินเทอร์เฟซเพื่อเปลี่ยนเส้นทาง
![](Image/Pasted%20image%2020260512153052.png)

> [!note] ⚙️ การตั้งค่า Optimization
> เราสามารถปรับแต่ง Metric ได้โดยการเปลี่ยนค่า `delay` หรือ `bandwidth` บนอินเทอร์เฟซ (แนะนำให้เปลี่ยนค่า Delay เพราะ Bandwidth มักถูกใช้ในการคำนวณ QoS ด้วย)

**รูปที่ 4:** ตรวจสอบตารางเส้นทางหลังการปรับแต่ง
![](Image/Pasted%20image%2020260512153100.png)

> [!note] ⚙️ ผลลัพธ์
> หลังจากปรับค่า Delay แล้ว EIGRP จะคำนวณ Metric ใหม่และอาจสลับไปใช้เส้นทางที่เราต้องการเป็นเส้นทางหลัก (Successor) แทน

---

## 🧠 Technical Deep Dive: Why & How?

### 1. EIGRP Metric Calculation
- **Why:** EIGRP ไม่ได้ใช้แค่ Hop Count เหมือน RIP แต่ใช้ "Composite Metric" เพื่อคำนวณความเร็วและความล่าช้าจริงของเส้นทาง
- **How:** สูตรคำนวณมาตรฐานคือ `[(10^7 / Minimum Bandwidth) + (Sum of Delay)] * 256` โดยจะเลือกเส้นทางที่มีค่า Metric ต่ำที่สุดเป็นเส้นทางหลัก

### 2. Successor vs Feasible Successor
- **Successor:** คือเส้นทางที่ดีที่สุดที่ถูกเลือกไปใส่ใน Routing Table
- **Feasible Successor (FS):** คือเส้นทางสำรองที่มีคุณสมบัติตรงตาม `Feasibility Condition` (ค่า RD < FD ของ Successor) หากเส้นทางหลักล่ม FS จะถูกนำมาใช้งานทันทีโดยไม่ต้องคำนวณใหม่ (Fast Convergence)

---

## 🛠️ Troubleshooting & Verification

- **Common Problem:** หลังปรับค่า Metric แล้ว เส้นทางไม่เปลี่ยนตามที่คาดหวัง หรือไม่มีเส้นทางสำรอง (FS) ปรากฏใน Topology Table
- **Solution:** ตรวจสอบ `Feasibility Condition` ว่าผ่านเงื่อนไขหรือไม่ และระวังอย่าปรับค่า Bandwidth ต่ำเกินไปจนส่งผลต่อโปรโตคอลอื่น
- **Verification Commands:**
    - `show ip route eigrp`: ตรวจสอบเส้นทางที่ถูกเลือกใช้งานจริง
    - `show ip eigrp topology all-links`: ดูเส้นทางทั้งหมดรวมถึงเส้นทางที่ไม่ผ่านเงื่อนไข FS
    - `show interface [type/number]`: ตรวจสอบค่า Bandwidth และ Delay ที่ตั้งไว้บนอินเทอร์เฟซ
