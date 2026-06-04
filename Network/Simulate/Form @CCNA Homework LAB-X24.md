# CCNA Homework LAB-X24: OSPF Default-information Originate

**รูปที่ 1:** แผนผังเครือข่ายเชื่อมต่อกับ ISP
![](Image/Pasted%20image%2020260512151017.png)

> [!note] ⚙️ การกระจายเส้นทางเริ่มต้น
> ในระบบเครือข่ายที่มี Router ตัวหนึ่งเชื่อมต่อกับอินเทอร์เน็ต (ISP) เราไม่จำเป็นต้องประกาศเส้นทาง Default Route แยกกันที่ Router ทุกตัว แต่เราจะใช้ OSPF ช่วยกระจายข้อมูลนี้ให้โดยอัตโนมัติ

**รูปที่ 2:** การใช้คำสั่ง Default-information Originate
![](Image/Pasted%20image%2020260512151030.png)

> [!note] ⚙️ คำสั่งกระจายเส้นทาง
> ใช้คำสั่ง `default-information originate` ภายใต้โหมด `router ospf` เพื่อสั่งให้ Router ตัวนี้ประกาศตนเองว่าเป็นทางออก (Gateway) ให้กับ Router ตัวอื่นๆ ในระบบ OSPF

**รูปที่ 3:** ตรวจสอบตาราง Routing ของ Router ตัวอื่นๆ
![](Image/Pasted%20image%2020260512151039.png)

> [!note] ⚙️ ผลลัพธ์ที่ได้
> Router ตัวอื่นในระบบจะเห็นเส้นทาง `O*E2 0.0.0.0/0` ปรากฏขึ้นในตาราง Routing โดยอัตโนมัติ (E2 คือ External Type 2)

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Default Route Propagation
- **Why:** เพื่อความสะดวกในการจัดการ หากมีการเปลี่ยน Next-hop ที่เชื่อมต่อกับ ISP เราแก้ไขเพียงที่ Edge Router ตัวเดียว ข้อมูลจะถูก Update ไปยัง Router ทุกตัวใน OSPF Domain ทันที
- **How:** Router ที่ตั้งค่าคำสั่งนี้จะสร้าง LSA Type 5 (External LSA) เพื่อประกาศ Default Route ออกไป

### 2. Always Flag (Optional)
- หากเราใช้คำสั่ง `default-information originate always` Router จะประกาศ Default Route ออกไปเสมอ แม้ว่าตัวมันเองจะไม่มี Default Route จริงๆ ในตาราง Routing ก็ตาม

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** ลืมสร้าง Static Default Route (`ip route 0.0.0.0 ...`) ไว้ที่ Edge Router ก่อนใช้คำสั่งนี้ (ยกเว้นกรณีใช้ keyword `always`)
- **Solution:** ตรวจสอบว่ามีเส้นทาง 0.0.0.0/0 อยู่ในตาราง Routing ของ Edge Router หรือยัง
- **Verification Commands:**
    - `show ip route ospf`: มองหาสัญลักษณ์ `O*E2`
    - `show ip ospf database external`: ดูรายละเอียด LSA Type 5 สำหรับ Default Route
