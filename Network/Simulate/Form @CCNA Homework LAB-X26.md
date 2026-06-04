# CCNA Homework LAB-X26: OSPF Path Optimization

**รูปที่ 1:** แผนผังเครือข่ายและการคำนวณค่า Cost เริ่มต้น
![](Image/Pasted%20image%2020260512151441.png)

> [!note] ⚙️ การเลือกเส้นทาง
> OSPF ใช้ค่า Cost เป็น Metric ในการตัดสินใจเลือกเส้นทางที่ดีที่สุด โดยค่า Cost คำนวณมาจากสูตร `Reference Bandwidth / Interface Bandwidth` (ค่าเริ่มต้นของ Reference คือ 100Mbps)

**รูปที่ 2:** การปรับแต่งค่า Cost บน Interface
![](Image/Pasted%20image%2020260512151450.png)

> [!note] ⚙️ การบังคับเส้นทาง
> เราสามารถบังคับให้ Router เลือกเส้นทางที่เราต้องการได้โดยการเปลี่ยนค่า Cost โดยตรงด้วยคำสั่ง `ip ospf cost [value]` หรือปรับเปลี่ยน `bandwidth` บน interface

**รูปที่ 3:** ตรวจสอบการเปลี่ยนแปลงเส้นทางใน Routing Table
![](Image/Pasted%20image%2020260512151459.png)

> [!note] ⚙️ ผลลัพธ์
> หลังจากปรับค่า Cost แล้ว Router จะเปลี่ยนเส้นทางไปใช้ Link ที่มีค่า Cost รวม (Accumulated Cost) ต่ำที่สุดไปยังปลายทาง

---

## 🧠 Technical Deep Dive: Why & How?

### 1. OSPF Metric (Cost)
- **Why:** ในระบบที่มี Link หลายระดับความเร็ว (เช่น 1Gbps, 10Gbps) ค่า Reference Bandwidth เริ่มต้น (100Mbps) จะมองเห็น Link เหล่านี้มีค่า Cost เท่ากับ 1 เหมือนกันหมด ทำให้การเลือกเส้นทางไม่มีประสิทธิภาพ
- **How:** เราควรปรับค่า Reference Bandwidth ให้สูงขึ้นด้วยคำสั่ง `auto-cost reference-bandwidth` เพื่อให้ OSPF แยกแยะความแตกต่างของ Link ความเร็วสูงได้

### 2. Manual Path Control
- การปรับค่า Cost เป็นวิธีที่ง่ายและรวดเร็วที่สุดในการทำ Traffic Engineering เพื่อระบายข้อมูลไปยังเส้นทางสำรอง หรือเลี่ยงเส้นทางที่หนาแน่น

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** ปรับค่า Cost เพียงฝั่งเดียว ทำให้เกิดการส่งข้อมูลแบบ Asymmetric Routing (ขาไปและขากลับวิ่งคนละทาง) ซึ่งอาจทำให้ยากต่อการตรวจสอบปัญหา
- **Solution:** ควรวางแผนและปรับค่า Cost ให้สอดคล้องกันทั้งระบบ
- **Verification Commands:**
    - `show ip route [destination]`: ดูค่า Metric (Cost) ของเส้นทางนั้นๆ
    - `show ip ospf interface [name]`: ตรวจสอบค่า Cost ที่ทำงานอยู่บน Interface จริง
    - `traceroute [destination]`: ทดสอบดูเส้นทางที่ข้อมูลวิ่งผ่านจริง
