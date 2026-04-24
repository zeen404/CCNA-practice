# CCNA Homework LAB-X04: CDP Discovery

**รูปที่ 1:** โครงร่างเครือข่ายสำหรับการทำแล็บ
![[Pasted image 20260424105654.png]]

**รูปที่ 2:** รายละเอียดการเชื่อมต่อเข้าสู่ CDP Cloud
![[Pasted image 20260424105745.png]]

**รูปที่ 3:** การใช้คำสั่งตรวจสอบอุปกรณ์ข้างเคียง (CDP Neighbors)
![[Pasted image 20260424110705.png]]

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Discovery Protocol (The Visualizer)
- **Why:** ในกรณีที่เราเข้าถึงอุปกรณ์ได้เพียงตัวเดียว แต่ต้องการรู้ว่าเครือข่าย "หลังบ้าน" ต่อกันอย่างไร CDP คือเครื่องมือที่ดีที่สุดในการวาด Topology โดยไม่ต้องเข้าหน้างานจริง
- **How:** อุปกรณ์ Cisco จะส่งข้อมูลของตัวเอง (Hostname, Model, Port, Capability) ออกไปทุกพอร์ตที่มีการใช้งานเป็นระยะ

### 2. Layer 2 Independence
- **Mechanism:** CDP ทำงานที่ **Layer 2 (Data Link Layer)** โดยตรง ไม่ต้องพึ่งพา IP Address นั่นหมายความว่าแม้ Router สองตัวจะตั้ง IP คนละวง (ซึ่งจะ Ping ไม่เจอ) แต่ CDP จะยังมองเห็นกันและบอกความผิดพลาดนี้ได้ (Native VLAN Mismatch, Duplex Mismatch)

### 📨 Packet Flow
- **Multicast Advertisement:** อุปกรณ์จะส่ง CDP Packet ไปยัง Multicast MAC Address **`01:00:0C:CC:CC:CC`** ทุกๆ 60 วินาที
- **L2 Encapsulation:** ข้อมูลจะถูกบรรจุลงในโครงสร้างเฟรมพิเศษ (SNAP/LLC) เพื่อให้ Switch ทราบว่าเฟรมนี้มีไว้เพื่อ "อ่านเอง" และห้ามส่งต่อ (Flood) ออกไปยังอุปกรณ์อื่นนอกเหนือจากตัวมันเอง
- **Timers:** หากไม่ได้รับข้อมูลอัปเดตจากเพื่อนบ้านภายใน 180 วินาที (Holdtime) รายชื่อเพื่อนบ้านจะหายไปจากตาราง
