# CCNA Homework LAB-X13: Rapid Per-VLAN STP (RSTP)

**รูปที่ 1:** แผนผังเครือข่ายสำหรับการทำ Rapid STP
![](Image/Pasted%20image%2020260511171853.png)

**รูปที่ 2:** การเปลี่ยนโหมด STP เป็น `rapid-pvst`
![](Image/Pasted%20image%2020260511171913.png)

**รูปที่ 3:** การตรวจสอบโหมด RSTP บน Switch
![](Image/Pasted%20image%2020260511171931.png)

**รูปที่ 4:** สถานะพอร์ตในโหมด Rapid STP (Edge Ports)
![](Image/Pasted%20image%2020260511172003.png)

**รูปที่ 5:** การทดสอบการล้มเหลวของ Link (Convergence Test)
![](Image/Pasted%20image%2020260511172030.png)

**รูปที่ 6:** ความเร็วในการกลับมาใช้งานได้ของระบบ
![](Image/Pasted%20image%2020260511172054.png)

---

## 🧠 Technical Deep Dive: Why & How?

### 1. The Need for Speed (Convergence)
- **Why:** ในธุรกิจสมัยใหม่ การรอเน็ตกลับมาใช้งานได้นานถึง 50 วินาทีเมื่อสายแลนขาด (STP ปกติ) เป็นเรื่องที่ยอมรับไม่ได้
- **How (RSTP - 802.1w):** พัฒนาการสื่อสารระหว่าง Switch ให้มีการตอบโต้ (Proposal/Agreement) ทำให้เมื่อเกิดเหตุสายขาด ระบบจะหาเส้นทางใหม่ได้ภายในเวลาไม่เกิน **1-2 วินาที**

### 2. Port Roles in RSTP
- **Mechanism:** RSTP เพิ่มสถานะพอร์ตแบบใหม่ เช่น **Alternate Port** (เส้นทางสำรองที่พร้อมเสียบแทนทันที)

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** ลืมเปลี่ยนโหมดทั้งระบบ ทำให้ Switch บางตัวทำงานช้า บางตัวทำงานเร็ว เกิดความสับสนในการส่งข้อมูล
- **Solution:** ใช้คำสั่ง `spanning-tree mode rapid-pvst` บน Switch ทุกตัวในระบบ
- **Verification:**
    - ใช้คำสั่ง `show spanning-tree` และสังเกตคำว่า **"Spanning tree enabled protocol rstp"**
    - ทดสอบโดยการลบสายเส้นหลักทิ้ง แล้วจับเวลาว่า PC กลับมา Ping เจอ Gateway เร็วแค่ไหน
