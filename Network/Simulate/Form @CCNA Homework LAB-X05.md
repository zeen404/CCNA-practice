# CCNA Homework LAB-X05: MAC Address Table

**รูปที่ 1:** โครงสร้างเครือข่าย (Topology)
![[Pasted image 20260424112136.png]]

**รูปที่ 2:** การทดสอบ Ping จาก PC0 ไปยัง PC1
![[Pasted image 20260424112305.png]]

**รูปที่ 3:** การตรวจสอบตาราง MAC Address บน SW1 (Switch0)
![[Pasted image 20260424112226.png]]

**รูปที่ 4:** การทดสอบ Ping ข้าม Switch (PC1 ไปยัง PC3)
![[Pasted image 20260424112452.png]]

**รูปที่ 5:** ตาราง MAC Address ของ SW1 หลังจากการ Ping ข้าม Switch
![[Pasted image 20260424112556.png]]

**รูปที่ 6:** ตาราง MAC Address ของ SW2 (Switch1) หลังจากการ Ping ข้าม Switch
![[Pasted image 20260424112710.png]]

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Learning & Forwarding Logic
- **Why:** Switch ถูกสร้างมาเพื่อแก้ปัญหาการชนกันของข้อมูล (Collision) และเพิ่มประสิทธิภาพ โดยมันต้องรู้ว่า "ใคร อยู่ที่ไหน" เพื่อส่งข้อมูลให้ถูกคนโดยไม่กวนคนอื่น
- **How (The Learning Phase):** 
    - เมื่อเฟรมวิ่งเข้ามา Switch จะมองไปที่ **Source MAC Address**
    - นำ MAC นั้นมาจับคู่กับเลขพอร์ตที่เฟรมนั้นวิ่งเข้ามา บันทึกลงใน **CAM Table** (Content Addressable Memory)
- **Forwarding Phase:** เมื่อจะส่งออก Switch จะมองที่ **Destination MAC Address** และค้นในตารางเพื่อเลือกพอร์ตส่งออกเพียงพอร์ตเดียว

### 2. Inter-Switch Learning
- **Mechanism:** เมื่อมีการต่อ Switch สองตัวเข้าหากัน พอร์ตที่เชื่อมต่อ (Trunk/Uplink) จะต้องจดจำ MAC Address "หลายตัว" (Multiple MACs) เพราะมันเปรียบเสมือนประตูที่รวมเครื่องคอมพิวเตอร์ทั้งหมดจากอีกฝั่งมาไว้ที่นี่

### 📨 Packet Flow (The Journey of a Frame)
1. **The ARP Request:** ก่อน Ping จะเกิด PC0 จะส่ง **Broadcast Frame** (Dest MAC: `FF:FF:FF:FF:FF:FF`)
2. **Flooding:** Switch รับเฟรม Broadcast และส่งออกทุกพอร์ต (Flooding) เพื่อตามหาเจ้าของ IP นั้น
3. **Table Building:** ทันทีที่ PC1 ตอบกลับ Switch จะเห็น Source MAC ของ PC1 และบันทึกลงตารางทันที
4. **Unicast Delivery:** การ Ping ครั้งถัดไป Switch จะไม่ Flood อีกแล้ว แต่จะส่งเป็น **Unicast** ตรงไปยังพอร์ตปลายทางโดยตรง (Forwarding)
