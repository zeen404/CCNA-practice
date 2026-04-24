# CCNA Homework LAB-X06: VLAN & Trunking Configuration

**รูปที่ 1:** แผนผังเครือข่าย (Topology)
![[Pasted image 20260424114456.png]]

**รูปที่ 2 - 6:** การสร้าง VLAN 10, 20, 30 บน SW1 ถึง SW5
![[Pasted image 20260424114521.png]]

**รูปที่ 7:** ตรวจสอบความถูกต้องของ VLAN บน SW1
![[Pasted image 20260424114615.png]]

**รูปที่ 8 - 9:** กำหนดพอร์ตเชื่อมต่อระหว่าง Switch ให้เป็นโหมด Trunk
![[Pasted image 20260424114624.png]]

**รูปที่ 10 - 11:** ตรวจสอบสถานะ Trunk
![[Pasted image 20260424114658.png]]

**รูปที่ 12 - 13:** การกำหนดพอร์ตให้กับ PC และ Server ตาม VLAN ที่ต้องการ
![[Pasted image 20260424114719.png]]

**รูปที่ 14:** ตรวจสอบรายละเอียดของพอร์ต
![[Pasted image 20260424114753.png]]

**รูปที่ 15:** การทดสอบ Ping
![[Pasted image 20260424114803.png]]

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Logical Segmentation (VLAN)
- **Why:** เพื่อลดขนาดของ **Broadcast Domain** ถ้าไม่มี VLAN ทุกเครื่องในตึกจะได้รับ Broadcast ของกันและกัน ทำให้เครือข่ายช้าและไม่ปลอดภัย (เช่น แผนกบัญชีเห็นข้อมูลแผนก IT)
- **How:** Switch จะทำเครื่องหมาย (Tag) ให้กับทุกเฟรมที่วิ่งเข้ามา เพื่อบอกว่าเฟรมนี้เป็นของกลุ่มไหน

### 2. The 802.1Q Trunking Protocol
- **Mechanism:** พอร์ต Trunk คือพอร์ต "สาธารณะ" ที่ยอมให้หลาย VLAN วิ่งผ่านได้ โดยใช้มาตรฐาน **IEEE 802.1Q** ซึ่งจะทำการแทรกข้อมูล Tag ขนาด **4 Bytes** ลงไปใน Ethernet Frame เดิม

### 📨 Packet Flow (Tagging Process)
1. **Ingress (Access Port):** เมื่อ PC ส่งข้อมูล (Untagged) เข้าพอร์ต Fa0/1 ของ Switch -> Switch จะจำแนกเฟรมนั้นให้อยู่ใน VLAN 10 ทันทีตามที่เราคอนฟิกไว้
2. **Trunking (Between Switches):** เมื่อเฟรมต้องข้ามไปยัง Switch อีกตัว -> Switch จะแปะ **VLAN Tag 10** ลงไปในเฟรม ข้อมูลนี้จะเดินทางไปตามสาย Trunk
3. **Egress (Access Port):** เมื่อถึง Switch ปลายทาง -> มันจะดู Tag แล้วพบว่าเป็นของ VLAN 10 -> มันจะ **ถอด Tag ออก** แล้วส่งเฟรมดั้งเดิมไปให้ PC ปลายทาง
