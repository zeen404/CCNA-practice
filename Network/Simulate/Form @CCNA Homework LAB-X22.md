# CCNA Homework LAB-X22: OSPF Multi-Area

**รูปที่ 1:** แผนผังเครือข่าย Multi-Area OSPF
![](Image/Pasted%20image%2020260512150633.png)

> [!note] ⚙️ แนวคิดลำดับชั้น
> OSPF Multi-Area ช่วยในการแบ่งเครือข่ายขนาดใหญ่ออกเป็นส่วนๆ เพื่อลดภาระการคำนวณของ Router และจำกัดขอบเขตของ Link State Update (LSA)

**รูปที่ 2:** การตั้งค่าบน ABR (Area Border Router)
![](Image/Pasted%20image%2020260512150646.png)

> [!note] ⚙️ บทบาทของ ABR
> Router ที่มีขาเชื่อมต่อมากกว่า 1 Area (เช่น อยู่ทั้ง Area 0 และ Area 1) จะเรียกว่า ABR ทำหน้าที่ส่งผ่านข้อมูล Routing ระหว่างพื้นที่

**รูปที่ 3-5:** การประกาศเครือข่ายแยกตาม Area
![](Image/Pasted%20image%2020260512150655.png)
![](Image/Pasted%20image%2020260512150703.png)
![](Image/Pasted%20image%2020260512150710.png)

**รูปที่ 6:** ตรวจสอบตาราง Routing (Inter-Area Routes)
![](Image/Pasted%20image%2020260512150718.png)

> [!note] ⚙️ สัญลักษณ์ O IA
> เส้นทางที่เรียนรู้จาก Area อื่นจะปรากฏใน Routing Table ด้วยสัญลักษณ์ `O IA` (OSPF Inter-Area)

**รูปที่ 7-8:** การตรวจสอบสถานะ Neighbor และ Topology
![](Image/Pasted%20image%2020260512150724.png)
![](Image/Pasted%20image%2020260512150731.png)

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Hierarchy Design (Backbone & Non-Backbone)
- **Why:** เพื่อความยืดหยุ่น (Scalability) ในเครือข่ายขนาดใหญ่ หากเกิดการเปลี่ยนแปลงใน Area หนึ่ง จะไม่ส่งผลให้ Router ใน Area อื่นๆ ต้องคำนวณ SPF ใหม่ทั้งหมด
- **Rule:** Non-backbone area ทุกตัวจะต้องเชื่อมต่อโดยตรงกับ Backbone Area (Area 0) เสมอ (ยกเว้นกรณีทำ Virtual-link)

### 2. LSA Types (Simplified)
- **Type 1 (Router LSA):** ประกาศภายใน Area เดียวกัน
- **Type 3 (Summary LSA):** ถูกสร้างโดย ABR เพื่อประกาศเส้นทางจาก Area หนึ่งไปยังอีก Area หนึ่ง

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** ลืมเชื่อมต่อ Non-backbone area เข้ากับ Area 0 ทำให้ข้อมูล Routing ไม่สามารถไหลผ่านไปยังพื้นที่อื่นได้
- **Solution:** ตรวจสอบตำแหน่งของ ABR ว่ามีการตั้งค่า interface ถูกต้องตาม Area ที่ออกแบบไว้หรือไม่
- **Verification Commands:**
    - `show ip ospf`: ดูบทบาทของ Router (เช่น "It is an area border router")
    - `show ip ospf database`: ตรวจสอบ Link State Database แยกตามประเภท LSA
    - `show ip route ospf`: สังเกตเส้นทางประเภท O IA
