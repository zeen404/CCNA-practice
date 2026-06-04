# CCNA Homework LAB-X20: Static Route and Default Route

**รูปที่ 1:** แผนผังเครือข่าย (Topology)
![](Image/Pasted%20image%2020260512145226.png)

> [!note] ⚙️ การเตรียมตัว
> ใน Lab นี้เราจะเรียนรู้การสร้างเส้นทางในตาราง Routing Table ด้วยตัวเอง (Manual) โดยการใช้ Static Route สำหรับเจาะจงวงเครือข่าย และ Default Route สำหรับเส้นทางทางออกสุดท้าย

**รูปที่ 2:** การกำหนด Static Route เจาะจงปลายทาง
![](Image/Pasted%20image%2020260512150217.png)

> [!note] ⚙️ คำสั่ง Static Route
> ใช้คำสั่ง `ip route [Network_ID] [Subnet_mask] [Next_hop_IP / Exit_Interface]` เพื่อบอก Router ว่าถ้าจะไปเครือข่ายนั้นให้ส่งข้อมูลไปที่ไหน

**รูปที่ 3:** การกำหนด Default Route (Gateway of Last Resort)
![](Image/Pasted%20image%2020260512150225.png)

> [!note] ⚙️ เส้นทางเริ่มต้น
> ใช้คำสั่ง `ip route 0.0.0.0 0.0.0.0 [Next_hop]` เพื่อสร้างเส้นทางที่ใช้เมื่อ Router ไม่พบเส้นทางที่เจาะจงใน Routing Table (เหมาะสำหรับทางออกสู่อินเทอร์เน็ต)

**รูปที่ 4:** ตรวจสอบ Routing Table
![](Image/Pasted%20image%2020260512150234.png)

**รูปที่ 5:** ทดสอบการเชื่อมต่อด้วยการ Ping
![](Image/Pasted%20image%2020260512150242.png)

> [!note] ⚙️ การยืนยันผล
> เมื่อตั้งค่าถูกต้อง Router จะต้องมีสัญลักษณ์ `S` (Static) หรือ `S*` (Static Default Route) ในตารางเส้นทาง และสามารถสื่อสารข้ามเครือข่ายได้

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Static Route vs Dynamic Route
- **Why:** Static Route ใช้ทรัพยากรเครื่อง (CPU/RAM) น้อยมากและไม่มีการส่งข้อมูล Routing Update บนสายสัญญาณ ทำให้ประหยัด Bandwidth และมีความปลอดภัยสูงเพราะเราควบคุมเส้นทางได้เองทั้งหมด
- **How:** ผู้ดูแลระบบต้องทราบ Topology ทั้งหมดและระบุเส้นทางไป-กลับให้ครบถ้วน (Static Route ทำงานแบบ One-way เสมอ)

### 2. Default Route (0.0.0.0/0)
- **Concept:** เป็น Static Route ประเภทพิเศษที่ครอบคลุมทุกที่หมายในโลก (Quad-zero route)
- **Use Case:** ใช้ใน Stub Network หรือ Router ที่มีทางออกทางเดียว เพื่อลดขนาดของ Routing Table ให้เล็กลง

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** ลืมตั้งเส้นทางขากลับ (Return path missing) ทำให้ Ping ไปถึงแต่ตอบกลับไม่ได้ หรือใส่ Next-hop IP ผิดพลาด
- **Solution:** ตรวจสอบตาราง Routing Table ของ Router ทุกตัวในเส้นทางว่ามีข้อมูลครบทั้งขาไปและขากลับ
- **Verification Commands:**
    - `show ip route`: ดูตารางเส้นทางทั้งหมด (มองหาตัวอักษร S)
    - `show ip route static`: ดูเฉพาะเส้นทางที่ตั้งค่าแบบ Static
    - `ping [Destination_IP]`: ทดสอบการเข้าถึงปลายทาง
