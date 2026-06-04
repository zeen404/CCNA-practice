# CCNA Homework LAB-X23: OSPF Summarization

**รูปที่ 1:** ตาราง Routing ก่อนทำการ Summarization
![](Image/Pasted%20image%2020260512150832.png)

> [!note] ⚙️ ปัญหาก่อนทำ
> ในเครือข่ายที่มี Subnet ย่อยๆ จำนวนมาก ตาราง Routing Table จะมีขนาดใหญ่มาก (Long table) ทำให้ Router ต้องใช้หน่วยความจำสูงในการจัดเก็บและค้นหาเส้นทาง

**รูปที่ 2:** การตั้งค่า Summarization บน ABR และผลลัพธ์หลังทำ
![](Image/Pasted%20image%2020260512150849.png)

> [!note] ⚙️ การยุบรวมเส้นทาง
> ใช้คำสั่ง `area [Area_ID] range [Summary_IP] [Mask]` ภายใต้ Router OSPF เพื่อรวม Network ย่อยๆ ให้กลายเป็นบรรทัดเดียว (Summary route) ส่งไปยัง Area อื่น

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Route Summarization Concept
- **Why:** เพื่อลดขนาดของ Routing Table และลดจำนวน LSA Update ที่ต้องส่งข้าม Area ช่วยให้เครือข่ายมีเสถียรภาพมากขึ้น (หาก Network ย่อยเส้นใดเส้นหนึ่ง Up/Down จะไม่ส่งผลกระทบต่อ Area อื่น)
- **How:** รวบรวมกลุ่มของ IP Address ที่มี Prefix ร่วมกัน (Contiguous blocks) แล้วประกาศออกไปเป็น Summary address เพียงอันเดียว

### 2. Summarization in OSPF
- OSPF ต่างจาก EIGRP ตรงที่ไม่สามารถทำ Summarization ที่ interface ใดก็ได้ แต่ต้องทำที่ **ABR** (ระหว่าง Area) หรือ **ASBR** (ระหว่าง Protocol) เท่านั้น

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** คำนวณ Summary Address ผิดพลาด ทำให้ครอบคลุม Network ไม่ครบ หรือครอบคลุม Network ของคนอื่นเข้ามาด้วย
- **Solution:** ใช้การแปลง IP เป็นฐานสอง (Binary) เพื่อหาค่า Prefix ที่ยาวที่สุดที่เหมือนกัน (Longest match prefix)
- **Verification Commands:**
    - `show ip route ospf`: ตรวจสอบว่าใน Area ปลายทาง เห็นเส้นทางที่ยุบรวมแล้วเป็นบรรทัดเดียวหรือไม่
    - `show ip ospf database summary`: ดูรายละเอียด LSA Type 3 ที่ถูกส่งออกมาจาก ABR
