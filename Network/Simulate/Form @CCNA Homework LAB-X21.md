# CCNA Homework LAB-X21: OSPF Single Area

**รูปที่ 1:** แผนผังเครือข่าย OSPF (Area 0)
![](Image/Pasted%20image%2020260512150355.png)

**รูปที่ 2:** การเริ่มต้นกระบวนการ OSPF (Process ID)
![](Image/Pasted%20image%2020260512150452.png)

> [!note] ⚙️ การเปิดใช้งาน
> ใช้คำสั่ง `router ospf [Process_ID]` เพื่อเริ่มทำงาน OSPF โดยที่ Process ID นี้มีความสำคัญเฉพาะภายในตัว Router เองเท่านั้น (ไม่จำเป็นต้องตรงกับตัวอื่น)

**รูปที่ 3:** การประกาศเครือข่าย (Network Command)
![](Image/Pasted%20image%2020260512150458.png)

> [!note] ⚙️ การใช้ Wildcard Mask
> OSPF ใช้ Wildcard Mask ในการระบุช่วงของ IP Address ที่ต้องการประกาศ เช่น `network 192.168.1.0 0.0.0.255 area 0`

**รูปที่ 4:** ข้อความแจ้งเตือนสถานะ Adjacency
![](Image/Pasted%20image%2020260512150506.png)

**รูปที่ 5:** ตรวจสอบรายชื่อเพื่อนบ้าน (Neighbors)
![](Image/Pasted%20image%2020260512150513.png)

> [!note] ⚙️ สถานะ Full
> เมื่อ OSPF ทำงานสมบูรณ์ สถานะ Neighbor ควรเป็น `FULL` ซึ่งหมายถึงมีการแลกเปลี่ยนฐานข้อมูล (LSDB) กันเรียบร้อยแล้ว

**รูปที่ 6:** ตรวจสอบเส้นทางที่เรียนรู้ผ่าน OSPF
![](Image/Pasted%20image%2020260512150519.png)

---

## 🧠 Technical Deep Dive: Why & How?

### 1. OSPF Concept (Link State Protocol)
- **Why:** OSPF เป็น Link State Protocol ที่ทำงานได้รวดเร็ว (Fast Convergence) และไม่มีข้อจำกัดเรื่อง Hop Count เหมือน RIP เหมาะสำหรับเครือข่ายขนาดกลางถึงใหญ่
- **How:** Routerแต่ละตัวจะสร้างแผนที่ของเครือข่ายทั้งหมด (LSDB) และใช้ Dijkstra's Algorithm (SPF) ในการคำนวณเส้นทางที่สั้นที่สุดตามค่า Cost (Bandwidth)

### 2. Single Area (Area 0)
- **Concept:** ในระบบขนาดเล็ก เราจะให้ Router ทุกตัวอยู่ใน Area เดียวกัน ซึ่งเรียกว่า Backbone Area (Area 0)
- **Benefit:** ลดความซับซ้อนในการจัดการและการออกแบบเครือข่าย

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** Area ID ไม่ตรงกัน, Subnet/Wildcard mask ผิดพลาด หรือค่า Hello/Dead Interval ไม่ตรงกัน ทำให้ไม่สามารถสร้าง Adjacency ได้
- **Solution:** ตรวจสอบด้วยคำสั่ง `show ip ospf interface` เพื่อดูค่าต่างๆ ว่าตรงกับเพื่อนบ้านหรือไม่
- **Verification Commands:**
    - `show ip ospf neighbor`: ตรวจสอบสถานะความสัมพันธ์กับเพื่อนบ้าน
    - `show ip route ospf`: ดูเส้นทางที่ได้รับมาจาก OSPF (สัญลักษณ์ O)
    - `show ip protocols`: ตรวจสอบค่าคอนฟิกภาพรวมของโปรโตคอล OSPF
