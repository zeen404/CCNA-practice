# CCNA Homework LAB-X11: Inter-VLAN with L3 Switch (SVI)

**รูปที่ 1:** แผนผังเครือข่ายและการตั้งค่าเบื้องต้น
![](Image/Pasted%20image%2020260511163948.png)

**รูปที่ 2:** การกำหนด IP Address ให้กับ Interface VLAN (SVI)
![](Image/Pasted%20image%2020260511164030.png)

**รูปที่ 3:** การเปิดใช้งาน Routing (`ip routing`)
![](Image/Pasted%20image%2020260511164045.png)

**รูปที่ 4:** การตรวจสอบตารางเส้นทางและทดสอบการเชื่อมต่อ
![](Image/Pasted%20image%2020260511164059.png)

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Hardware-Based Routing
- **Why:** การใช้ Router-on-a-Stick (LAB-10) มีคอขวดที่สายแลนเส้นเดียว ถ้าข้อมูลวิ่งเยอะจะช้า การใช้ **L3 Switch (Multi-Layer Switch)** จะส่งข้อมูลข้าม VLAN ได้เร็วเท่าความเร็วสาย (Wire-speed) เพราะใช้ชิปพิเศษ (ASIC) ในการประมวลผล
- **How:** เราสร้างพอร์ตเสมือนที่เรียกว่า **SVI (Switched Virtual Interface)** ขึ้นมาทำหน้าที่เป็น Default Gateway แทน Router

### 2. Switching to Routing Mode
- **Mechanism:** โดยปกติ L3 Switch จะทำงานเหมือน L2 ทั่วไป เราต้องสั่ง `ip routing` เพื่อปลุกวิญญาณ Router ในตัวมันขึ้นมา

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** ลืมพิมพ์คำสั่ง `ip routing` ทำให้ Switch ไม่ยอมส่ง Packet ข้ามวง แม้จะตั้ง IP ถูกต้องแล้วก็ตาม
- **Solution:** พิมพ์ `ip routing` ในโหมด Global Configuration
- **Verification:**
    - ใช้คำสั่ง `show ip route` เพื่อดูตารางเส้นทาง
    - ทดสอบ `ping` ระหว่าง PC ต่าง VLAN ผ่าน Switch ตัวเดียว
