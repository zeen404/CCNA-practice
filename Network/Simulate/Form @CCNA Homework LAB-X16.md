# CCNA Homework LAB-X16: Port Security

**รูปที่ 1:** แผนผัง Topology สำหรับการทดสอบ Port Security
![](Image/Pasted%20image%2020260512143745.png)

**รูปที่ 2:** การเปิดใช้งาน Port Security บน Interface
![](Image/Pasted%20image%2020260512143808.png)

> [!note] ⚙️ ขั้นตอนพื้นฐาน
> ก่อนจะตั้งค่า Port Security ต้องมั่นใจว่าพอร์ตนั้นอยู่ในโหมด access ด้วยคำสั่ง `switchport mode access` จากนั้นเปิดการทำงานด้วย `switchport port-security`

**รูปที่ 3:** การกำหนดค่า Maximum MAC Address
![](Image/Pasted%20image%2020260512143822.png)

**รูปที่ 4:** การตั้งค่า MAC Address แบบ Sticky
![](Image/Pasted%20image%2020260512143953.png)

> [!note] ⚙️ Sticky MAC Address
> คำสั่ง `switchport port-security mac-address sticky` จะช่วยให้ Switch เรียนรู้ MAC Address ของอุปกรณ์ที่มาเสียบโดยอัตโนมัติ และบันทึกลงใน Running-config ทันที

**รูปที่ 5:** การตรวจสอบสถานะ Port Security (Show Command)
![](Image/Pasted%20image%2020260512144004.png)

**รูปที่ 6:** รายละเอียดของพอร์ตที่เปิดใช้งาน Security
![](Image/Pasted%20image%2020260512144017.png)

**รูปที่ 7:** การทดสอบนำอุปกรณ์อื่นที่ไม่ได้อนุญาตมาเชื่อมต่อ (Violation)
![](Image/Pasted%20image%2020260512144027.png)

**รูปที่ 8:** พอร์ตเข้าสู่สถานะ Error-Disabled เมื่อเกิดการบุกรุก
![](Image/Pasted%20image%2020260512144038.png)

> [!note] ⚙️ Security Violation
> เมื่อมีการนำ MAC Address อื่นมาเสียบ พอร์ตจะถูกสั่ง `shutdown` ทันที (โหมด Shutdown เป็นค่าเริ่มต้น) และไฟที่พอร์ตบน Switch จะกลายเป็นสีแดง

**รูปที่ 9:** ตรวจสอบ Violation Counter
![](Image/Pasted%20image%2020260512144058.png)

**รูปที่ 10:** การกู้คืนพอร์ตกลับมาใช้งานใหม่
![](Image/Pasted%20image%2020260512144113.png)

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Layer 2 Security Concept
- **Why:** เพื่อป้องกันการนำอุปกรณ์แปลกปลอม (Unidentified Devices) มาเสียบใช้งานเครือข่าย หรือป้องกันการโจมตีประเภท MAC Address Flooding
- **How:** Switch จะตรวจสอบ Source MAC Address ใน Ethernet Frame ถ้าไม่ตรงกับที่อนุญาตไว้ จะทำการลงโทษ (Action) ตามโหมดที่ตั้งไว้

### 2. Violation Modes
- **Shutdown (Default):** ปิดพอร์ตทันที ต้องให้ Admin มา `shutdown` และ `no shutdown` เพื่อเปิดใหม่
- **Restrict:** ไม่ปิดพอร์ต แต่จะทิ้ง Frame ที่ผิดเงื่อนไข และส่งแจ้งเตือน (SNMP trap/Syslog) พร้อมเพิ่มตัวนับ Violation
- **Protect:** ทิ้ง Frame ที่ผิดเงื่อนไขเงียบๆ โดยไม่แจ้งเตือน

### 3. Sticky Learning
- ช่วยลดภาระของ Admin ที่ไม่ต้องมานั่งพิมพ์ MAC Address ทีละเครื่อง โดย Switch จะเรียนรู้จาก Traffic จริงที่วิ่งเข้ามาเป็นเครื่องแรก

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** พอร์ตไม่ยอมเปิดใช้งาน (Error-Disabled) แม้จะเอาเครื่องเดิมมาเสียบแล้ว
- **Solution:** ต้องทำการ Reset พอร์ตด้วยคำสั่ง `shutdown` และตามด้วย `no shutdown` ภายใต้อินเทอร์เฟซนั้นๆ
- **Verification Commands:**
    - `show port-security interface <type/number>`: ดูรายละเอียดเชิงลึกของแต่ละพอร์ต
    - `show port-security`: ดูภาพรวมพอร์ตที่เปิดใช้งาน Security ทั้งหมด
    - `show interface status`: ตรวจสอบว่าพอร์ตอยู่ในสถานะ `err-disabled` หรือไม่
