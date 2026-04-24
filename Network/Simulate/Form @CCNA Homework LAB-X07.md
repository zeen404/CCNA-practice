# CCNA Homework LAB-X07: VLAN Trunk Allowed List

**รูปที่ 1:** แผนผังเครือข่าย (Topology)
![[Pasted image 20260424132026.png]]

**รูปที่ 2:** การใช้คำสั่งจำกัด VLAN บน Switch แต่ละตัว
![[Pasted image 20260424132125.png]]

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Traffic Pruning & Optimization
- **Why:** โดยปกติ Trunk จะปล่อยให้ "ทุก VLAN" วิ่งผ่านได้ แต่ถ้าสาย Trunk เส้นหนึ่งเชื่อมไปยังตึกที่มีแค่แผนก IT (VLAN 30) เราก็ไม่ควรปล่อยให้ Broadcast ของแผนกบัญชี (VLAN 10) วิ่งไปรบกวนแบนด์วิธของตึกนั้น
- **How:** ใช้คำสั่ง `switchport trunk allowed vlan` เพื่อสร้าง "Whitelist" ของ VLAN ที่ได้รับอนุญาต

### 2. Security Enhancement
- **Mechanism:** เป็นการป้องกันการโจมตีแบบ **VLAN Hopping** เบื้องต้น โดยจำกัดทางเดินของข้อมูลให้เหลือเฉพาะจุดที่จำเป็นจริงๆ (Principle of Least Privilege)

### 📨 Packet Flow (Filtering)
- **Egress Filtering:** เมื่อเฟรมเดินทางมาถึงพอร์ตขาออก (Trunk Interface) -> Switch จะทำการตรวจสอบตาราง **Allowed List** ของพอร์ตนั้น
- **The Decision:** 
    - ถ้า VLAN ID ในเฟรม **ตรงกับ** รายชื่อใน Allowed List -> เฟรมจะถูกส่งออกไป
    - ถ้า VLAN ID **ไม่ตรง** -> Switch จะทำการ **Drop (ทิ้ง)** เฟรมนั้นทันทีที่ปากทางออก
- **Result:** ช่วยลดปริมาณ "ขยะ" ในเครือข่าย (Unnecessary Broadcasts) ได้อย่างมาก
