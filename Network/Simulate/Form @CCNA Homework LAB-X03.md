# CCNA Homework LAB-X03: Backup Configuration

**รูปที่ 1:** การสำรองไฟล์การตั้งค่าไปยัง TFTP Server
![[Pasted image 20260424104830.png]]

**รูปที่ 2:** การล้างค่าคอนฟิกและการ Restart เครื่อง
![[Pasted image 20260424104846.png]]

**รูปที่ 3:** กระบวนการกู้คืนไฟล์การตั้งค่าจาก TFTP Server
![[Pasted image 20260424104935.png]]

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Backup Strategy (External Storage)
- **Why:** NVRAM บนอุปกรณ์มีพื้นที่จำกัดและอุปกรณ์อาจพังได้ (Hardware Failure) การเก็บไฟล์คอนฟิกไว้ข้างนอก (Off-box storage) จึงจำเป็นสำหรับการกู้ระบบแบบรวดเร็ว
- **How:** ใช้โปรโตคอล **TFTP (Trivial File Transfer Protocol)** ซึ่งเน้นความเรียบง่ายและกินทรัพยากรต่ำมาก เหมาะสำหรับอุปกรณ์ขนาดเล็กที่ไม่มี OS ใหญ่โต

### 2. Factory Reset Mechanism
- **Mechanism:** คำสั่ง `erase startup-config` จะทำการล้าง Header ของ NVRAM ทำให้ระบบมองไม่เห็นไฟล์คอนฟิกเดิม เมื่อสั่ง `reload` ตัว Router จะเริ่มกระบวนการ "Setup Dialogue" ใหม่เพราะถือว่ายังไม่มีการตั้งค่า

### 📨 Packet Flow
- **TFTP Interaction:**
    1. **UDP Port 69:** Router ส่ง Packet คำขอเขียนไฟล์ (Write Request) ไปที่ TFTP Server
    2. **Fragmentation:** ไฟล์คอนฟิกจะถูกหั่นเป็นบล็อก บล็อกละ **512 Bytes**
    3. **Reliability:** แม้ใช้ UDP แต่ TFTP มีการควบคุมความถูกต้องเอง โดยถ้าส่งไปแล้ว Server ไม่ตอบกลับ ACK (Acknowledgment) ภายในเวลาที่กำหนด Router จะส่งบล็อกเดิมซ้ำอีกครั้ง
    4. **Completion:** เมื่อได้รับไฟล์ขนาดน้อยกว่า 512 bytes จะถือว่าสิ้นสุดการส่ง
- **ARP Request:** ก่อนการส่ง TFTP ครั้งแรก Router จะส่ง **ARP Broadcast** เพื่อถามหา MAC Address ของ TFTP Server เสมอ
