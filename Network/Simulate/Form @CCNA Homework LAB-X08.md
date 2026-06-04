# CCNA Homework LAB-X08: Native VLAN Configuration

**รูปที่ 1:** แผนผังเครือข่าย (Topology)
![](Image/Pasted%20image%2020260424132807.png)

**รูปที่ 2 - 4:** การทดสอบ Ping ในสภาวะเริ่มต้น (ค่า Default)
![](Image/Pasted%20image%2020260424132826.png)

**รูปที่ 5:** เปลี่ยนค่า Native VLAN เป็น 10
![](Image/Pasted%20image%2020260424132859.png)

**รูปที่ 6:** ทดสอบ Ping หลังจากเปลี่ยน Native VLAN เป็น 10
![](Image/Pasted%20image%2020260424132921.png)

**รูปที่ 7 - 8:** เปลี่ยนค่า Native VLAN เป็น 11
![](Image/Pasted%20image%2020260424132934.png)

**รูปที่ 9:** ทดสอบ Ping ในกลุ่มวง Native VLAN 11
![](Image/Pasted%20image%2020260424133000.png)

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Handling Untagged Traffic
- **Why:** มาตรฐาน 802.1Q กำหนดว่าต้องมี 1 VLAN ที่ข้อมูลสามารถวิ่งผ่าน Trunk ได้โดย **ไม่ต้องแปะป้าย (Untagged)** เพื่อรองรับอุปกรณ์ที่ไม่เข้าใจเรื่อง VLAN (เช่น Hub หรือ IP Phone บางรุ่น)
- **How:** ค่าเริ่มต้นคือ **VLAN 1** แต่เพื่อความปลอดภัย Admin มักจะเปลี่ยนเป็นเลขอื่นที่ไม่ได้ใช้งาน

### 2. Native VLAN Mismatch (Common Issue)
- **Mechanism:** ถ้า Switch สองฝั่งตั้ง Native VLAN ไม่ตรงกัน จะเกิดปัญหา **VLAN Leaking** (ข้อมูลจาก VLAN A วิ่งไปโผล่ใน VLAN B) ซึ่งเป็นช่องโหว่ความปลอดภัยร้ายแรง

### 📨 Packet Flow
- **Ingress (Trunk Port):** เมื่อเฟรมวิ่งเข้ามาที่พอร์ต Trunk โดย "ไม่มีป้าย Tag" (จาก Hub) -> Switch จะทำการติดป้าย (Internally Tag) ให้เป็นหมายเลขของ **Native VLAN** ที่ตั้งไว้ทันที
- **Egress (Trunk Port):** เมื่อ Switch จะส่งข้อมูลที่เป็นของ Native VLAN ออกทางพอร์ต Trunk -> มันจะ **ถอด Tag ออก** เพื่อส่งเป็นเฟรมดั้งเดิม (Untagged) ออกไป
- **Result:** เครื่องที่ต่ออยู่กับ Hub (ซึ่งไม่รู้จัก VLAN) จึงสามารถสื่อสารกับเครื่องที่อยู่ใน VLAN 10 บน Switch ได้ ถ้าเราตั้ง Native VLAN ให้ตรงกัน

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** เกิด Error ข้อความ `%CDP-4-NATIVE_VLAN_MISMATCH` เด้งขึ้นมาตลอดเวลา
- **Solution:** ต้องตรวจสอบพอร์ต Trunk ทั้งสองฝั่ง แล้วตั้งค่า `switchport trunk native vlan [เลขเดียวกัน]`
- **Verification:**
    - ใช้คำสั่ง `show interfaces trunk` เพื่อดูว่า Native VLAN ของทั้งสองฝั่งตรงกันหรือไม่
    - ทดสอบส่งข้อมูลจากอุปกรณ์ที่ทำ Tag ไม่ได้ (เช่น Hub) ว่าวิ่งไปโผล่ใน VLAN ที่เรากำหนดจริงไหม
