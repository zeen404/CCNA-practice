# CCNA Homework LAB-X27: IPv6 OSPF Routing (OSPFv3)

**รูปที่ 1:** แผนผังเครือข่าย IPv6
![](Image/Pasted%20image%2020260512151546.png)

**รูปที่ 2:** การเปิดใช้งาน IPv6 Unicast Routing
![](Image/Pasted%20image%2020260512151603.png)

> [!note] ⚙️ ขั้นตอนแรกที่สำคัญ
> ก่อนเริ่มทำ Routing บน IPv6 ต้องใช้คำสั่ง `ipv6 unicast-routing` เสมอ มิฉะนั้น Router จะไม่สามารถรับส่งข้อมูล Routing ของ IPv6 ได้

**รูปที่ 3:** การประกาศ OSPFv3 ภายใต้ Interface
![](Image/Pasted%20image%2020260512151622.png)

> [!note] ⚙️ ความแตกต่างจาก IPv4
> OSPFv3 จะไม่ใช้คำสั่ง `network` ในโหมด `router ospf` แต่จะใช้การเข้าไปเปิดที่ตัว interface โดยตรงด้วยคำสั่ง `ipv6 ospf [Process_ID] area [Area_ID]`

**รูปที่ 4:** ตรวจสอบเพื่อนบ้าน OSPFv3
![](Image/Pasted%20image%2020260512151641.png)

**รูปที่ 5:** ตรวจสอบตาราง Routing IPv6
![](Image/Pasted%20image%2020260512151652.png)

> [!note] ⚙️ สัญลักษณ์ OE
> เส้นทางที่เรียนรู้ผ่าน OSPFv3 จะแสดงด้วยตัวอักษร `OE` (หรือ `OI` สำหรับ Inter-area) ในตาราง Routing ของ IPv6

---

## 🧠 Technical Deep Dive: Why & How?

### 1. OSPFv3 (OSPF for IPv6)
- **Why:** เพื่อรองรับโครงสร้าง Address ของ IPv6 ที่ยาวขึ้น (128-bit) และมีการแยกการเจรจาโปรโตคอลออกจากตัว IP Address
- **Key Change:** OSPFv3 ใช้ Link-local Address (FE80::) ในการสร้างความสัมพันธ์ระหว่างเพื่อนบ้าน (Neighbor Adjacency) แทนการใช้ Global Unicast Address

### 2. Router ID in OSPFv3
- แม้จะเป็น IPv6 แต่ OSPFv3 ยังต้องการ **Router ID** ในรูปแบบ 32-bit (เหมือน IPv4 Address) หากใน Router ไม่มี IPv4 ตั้งค่าไว้เลย เราต้องระบุ Router ID ด้วยตัวเองเสมอ

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** ลืมตั้งค่า Router ID ทำให้ OSPFv3 ไม่เริ่มทำงาน หรือลืมเปิด `ipv6 unicast-routing`
- **Solution:** ตรวจสอบ Router ID ด้วยคำสั่ง `show ipv6 ospf` และแน่ใจว่าได้เปิดใช้งาน OSPF บน interface ที่ถูกต้อง
- **Verification Commands:**
    - `show ipv6 ospf neighbor`: ตรวจสอบสถานะ Adjacency
    - `show ipv6 route ospf`: ดูเส้นทาง IPv6 ที่ได้รับ
    - `show ipv6 ospf interface`: ดูรายละเอียด OSPF บนแต่ละ interface
