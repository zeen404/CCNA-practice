# CCNA Homework LAB-X09: Dynamic Trunking Protocol (DTP)

**รูปที่ 1:** แผนผังเครือข่ายและการกำหนดโหมด DTP
![[Pasted image 20260424134353.png]]

**รูปที่ 2:** คำสั่งการตั้งค่าในแต่ละ Task
![[Pasted image 20260424134336.png]]

**รูปที่ 3:** ตรวจสอบสถานะด้วยคำสั่ง show interfaces switchport
![[Pasted image 20260424134428.png]]

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Automation of Trunk Formation
- **Why:** เพื่อความสะดวกในการต่อขยายเครือข่าย เมื่อเสียบสายระหว่าง Switch ระบบจะพยายามตกลงกันเองว่าจะเชื่อมต่อแบบไหน (Access หรือ Trunk)
- **How (DTP Modes):** 
    - **Dynamic Auto:** "ถ้าเธอขอ ฉันจะเป็นให้" (Passive)
    - **Dynamic Desirable:** "ฉันอยากเป็น Trunk นะ เธอเป็นด้วยไหม?" (Active)

### 2. Security Best Practice (Nonegotiate)
- **Mechanism:** ในแง่ความปลอดภัย เรามักจะ **ปิด DTP** (`nonegotiate`) และบังคับเป็น Trunk ถาวร เพื่อป้องกันไม่ให้ Hacker ปลอมตัวเป็น Switch แล้วมาร้องขอการเป็น Trunk เพื่อแอบดูข้อมูลทุก VLAN (VLAN Hopping Attack)

### 📨 Packet Flow (DTP Negotiation)
- **The Protocol:** DTP ส่ง Packet ออกทุกๆ 30 วินาที ไปยัง MAC Address `01:00:0C:CC:CC:CC`
- **Negotiation Logic:** 
    - ถ้าฝั่งหนึ่งส่ง **Desirable** และอีกฝั่งเป็น **Auto** -> ทั้งคู่จะตกลงกันและเปลี่ยนสถานะพอร์ตเป็น **Trunk**
    - ถ้าเป็น **Auto** ทั้งคู่ -> ต่างคนต่างรอให้คนอื่นร้องขอ ผลคือจะเป็นแค่ **Access** (ไม่เกิด Trunk)
- **Nonegotiate:** เมื่อใช้คำสั่งนี้ Switch จะเลิกส่ง DTP Packet อย่างเด็ดขาด
