# CCNA Homework LAB-X01: Basic Configuration

**รูปที่ 1:** การตั้งชื่อ Router
![[Pasted image 20260424102201.png]]

> [!note] ⚙️ การตั้งชื่อ Router
> เป็นการเข้าสู่โหมดการตั้งค่า (Configuration Mode) และทำการเปลี่ยนชื่อของ Router จากค่าเริ่มต้นเป็น **"R1"** ด้วยคำสั่ง `hostname R1`

**รูปที่ 2:** การตั้งรหัสผ่าน Enable
![[Pasted image 20260424102346.png]]

**รูปที่ 3:** การตั้งรหัสผ่าน Console
![[Pasted image 20260424102412.png]]

**รูปที่ 4:** การตั้งรหัสผ่าน VTY (Telnet/SSH)
![[Pasted image 20260424102426.png]]

**รูปที่ 5:** การตั้งค่าข้อความประกาศ (Banner MOTD)
![[Pasted image 20260424102440.png]]

**รูปที่ 6:** การกำหนด IP Address ให้อินเทอร์เฟซ
![[Pasted image 20260424102450.png]]

**รูปที่ 7:** การตั้งค่า Default Gateway ให้กับ Switch
![[Pasted image 20260424102528.png]]

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Management Identity (Hostname & Banner)
- **Why:** ในระบบเครือข่ายขนาดใหญ่ที่มีอุปกรณ์นับร้อย การตั้ง `hostname` ช่วยระบุ "ตำแหน่ง" และ "หน้าที่" ของอุปกรณ์ผ่าน CLI ป้องกันความผิดพลาดในการคอนฟิกผิดเครื่อง ส่วน `Banner MOTD` มีไว้เพื่อผลทางกฎหมาย (Legal Notice) เพื่อแจ้งเตือนว่าผู้ที่ไม่มีส่วนเกี่ยวข้องห้ามเข้าระบบ
- **How:** คำสั่งเหล่านี้จะไปแก้ไขไฟล์ `running-config` ใน RAM ทันที

### 2. Multi-Layer Security
- **Enable Password:** ปกป้องการเข้าถึงโหมดสิทธิพิเศษ (Privileged EXEC) ซึ่งสามารถดูคอนฟิกและลบไฟล์ได้
- **Console Password:** ป้องกันการเสียบสายต่อตรง (Physical Access) เข้าหน้าเครื่อง
- **VTY Password:** ป้องกันการรีโมทผ่านเครือข่าย (Telnet/SSH) โดยแบ่งเป็นช่องสัญญาณ (Virtual Lines 0-15)

### 3. Interface & Gateway Logic
- **Router IP:** การตั้ง IP ที่ `FastEthernet 0/0` คือการสร้างประตูทางออก (Default Gateway) ให้กับเครือข่ายวงนั้น
- **Switch Default Gateway:** Switch L2 ไม่มีการ Routing ในตัว มันต้องการ Default Gateway เพื่อให้ตัวมันเองสามารถ "ส่งข้อมูลตอบกลับ" ไปยังผู้จัดการ (Admin) ที่อยู่คนละวง Network ได้

### 📨 Packet Flow
- **Local Console:** ข้อมูลวิ่งเป็นสัญญาณ Serial (Bits) ไม่ผ่าน Protocol Stack ของ Network
- **VTY (Telnet):** ข้อมูลถูกห่อหุ้มด้วย **TCP Port 23** เมื่อ Admin พิมพ์ตัวอักษร 1 ตัว จะเกิดการส่ง 1 Packet ไปที่ Router และ Router จะส่ง Packet นั้นกลับมาแสดงผลที่หน้าจอ Admin (Echo) เพื่อยืนยันว่าได้รับข้อมูลแล้ว
