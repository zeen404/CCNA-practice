# CCNA Homework LAB-X29: EIGRP Summarization

**รูปที่ 1:** แผนผังเครือข่ายและ Loopback Interfaces ที่ต้องการทำ Summarization
![](Image/Pasted%20image%2020260512152725.png)

> [!note] ⚙️ การเตรียมตัว
> ใน Lab นี้เรามี Network หลายวงที่เชื่อมต่ออยู่กับ Router ตัวเดียวกัน (เช่น 172.16.1.0/24, 172.16.2.0/24, 172.16.3.0/24) แทนที่เราจะส่ง Routing Update ของทุกวงออกไป เราจะทำการรวม (Summarize) ให้เป็นวงเดียวขนาดใหญ่เพื่อประสิทธิภาพ

**รูปที่ 2:** การกำหนดการสรุปเส้นทางด้วยคำสั่ง ip summary-address eigrp
![](Image/Pasted%20image%2020260512152737.png)

> [!note] ⚙️ การตั้งค่า
> เราเข้าไปที่อินเทอร์เฟซขาออกที่เชื่อมไปยัง Router ตัวอื่น แล้วใช้คำสั่ง `ip summary-address eigrp [AS-Number] [Summary-Address] [Mask]` เพื่อส่งเฉพาะเส้นทางที่สรุปแล้วออกไปแทนเส้นทางย่อยทั้งหมด

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Route Summarization Concept
- **Why:** การลดขนาด Routing Table ของ Router ตัวอื่นในระบบ ช่วยลดการใช้ Memory และ CPU ในการประมวลผล รวมถึงช่วยให้เครือข่ายมีความเสถียรมากขึ้น หาก Network วงย่อยเกิดปัญหา (Flapping) Router ตัวอื่นจะไม่ได้รับผลกระทบเพราะยังคงเห็น Summary Route เดิมอยู่
- **How:** EIGRP ใช้วิธี Manual Summarization โดยระบุที่ระดับ Interface ทำให้เราสามารถควบคุมได้ว่าต้องการส่งเส้นทางสรุปออกไปที่ช่องทางไหนบ้าง

### 2. Summary Route to Null0
- เมื่อเราทำ Summarization, Router จะสร้างเส้นทางพิเศษไปยัง `Null0` ใน Routing Table ของตัวเองโดยอัตโนมัติ เพื่อป้องกันการเกิด Routing Loop (Discard Route) หากได้รับแพ็กเก็ตที่ตรงกับ Summary Route แต่ไม่ตรงกับเส้นทางย่อยที่มีอยู่จริง

---

## 🛠️ Troubleshooting & Verification

- **Common Problem:** การสรุปเส้นทางกว้างเกินไป (Over-summarization) จนไปทับซ้อนกับเครือข่ายที่มีอยู่จริงในส่วนอื่นของระบบ ทำให้เกิดปัญหาการส่งข้อมูลผิดพลาด
- **Solution:** ตรวจสอบการคำนวณ Subnet Mask ให้แม่นยำ (Summary Mask) ให้ครอบคลุมเฉพาะกลุ่ม Network ที่ต้องการเท่านั้น
- **Verification Commands:**
    - `show ip route`: ตรวจสอบว่าในตารางเส้นทางมี Summary Route และเส้นทางไปยัง Null0 หรือไม่
    - `show ip eigrp topology`: ดูสถานะของ Topology Table ว่ามีการส่งเส้นทางสรุปออกไปอย่างไร
    - `show ip protocols`: ตรวจสอบสถานะการทำงานของ EIGRP และการทำ Summarization
