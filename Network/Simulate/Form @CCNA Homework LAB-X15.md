# CCNA Homework LAB-X15: L2 EtherChannel

**รูปที่ 1:** แผนผังการต่อเชื่อม (Topology) ของ Lab
![](Image/Pasted%20image%2020260512142844.png)

> [!note] ⚙️ การเตรียมตัว
> ใน Lab นี้เรามี Switch 2 ตัวเชื่อมต่อกันด้วยสาย Lan หลายเส้น ซึ่งตามปกติแล้ว Spanning Tree (STP) จะทำการ Block สายเส้นที่เกินมาเพื่อป้องกัน Loop แต่เราจะใช้ EtherChannel เพื่อรวมสายเหล่านี้เข้าด้วยกัน

**รูปที่ 2:** ตรวจสอบสถานะเริ่มต้นของ Spanning Tree
![](Image/Pasted%20image%2020260512142910.png)

> [!note] ⚙️ ก่อนทำ EtherChannel
> จะเห็นว่าอินเทอร์เฟซบางตัวอยู่ในสถานะ "Altn BLK" (Blocking) เพราะ STP ตรวจพบ Loop ทำให้สายเส้นนั้นไม่ได้ถูกใช้งานเพื่อส่งข้อมูล

**รูปที่ 3:** การเลือกอินเทอร์เฟซที่ต้องการทำ EtherChannel (Interface Range)
![](Image/Pasted%20image%2020260512142918.png)

**รูปที่ 4:** การกำหนด Channel Group และ Mode (LACP/PAgP)
![](Image/Pasted%20image%2020260512142928.png)

> [!note] ⚙️ คำสั่ง Channel-group
> ใช้คำสั่ง `interface range f0/1 - 2` เพื่อเลือกหลายพอร์ตพร้อมกัน และใช้ `channel-group 1 mode active` เพื่อสร้าง EtherChannel โดยใช้โปรโตคอล LACP

**รูปที่ 5:** การตั้งค่าบน Switch ตัวที่สอง
![](Image/Pasted%20image%2020260512142939.png)

**รูปที่ 6:** การตรวจสอบสถานะ EtherChannel (Show Summary)
![](Image/Pasted%20image%2020260512142950.png)

> [!note] ⚙️ การตรวจสอบ
> ใช้คำสั่ง `show etherchannel summary` เพื่อดูว่าพอร์ตต่างๆ ถูกรวมกลุ่มกันสำเร็จหรือไม่ (สถานะควรเป็น P - In Port-channel)

**รูปที่ 7:** ตรวจสอบ Spanning Tree หลังจากทำ EtherChannel
![](Image/Pasted%20image%2020260512142956.png)

> [!note] ⚙️ ผลลัพธ์ของ STP
> หลังทำ EtherChannel แล้ว STP จะมองเห็นสายหลายเส้นที่รวมกันเป็นอินเทอร์เฟซเดียวคือ `Po1` (Port-channel 1) ทำให้ไม่มีการ Block พอร์ตอีกต่อไป และสามารถใช้งาน Bandwidth ได้เต็มที่

**รูปที่ 8:** สรุปผลการทดลองและการทำงานที่สมบูรณ์
![](Image/Pasted%20image%2020260512143010.png)

---

## 🧠 Technical Deep Dive: Why & How?

### 1. EtherChannel Concept (Link Aggregation)
- **Why:** ในระบบเครือข่าย เราต้องการ Bandwidth ที่สูงขึ้นและความซ้ำซ้อน (Redundancy) แต่ STP จะ Block สายเส้นที่เกินมาเสมอ EtherChannel จึงถูกออกแบบมาเพื่อ "หลอก" STP ว่าสายหลายเส้นนั้นคือ "สายเส้นเดียวที่มีความเร็วสูงขึ้น"
- **How:** Switch จะทำ Logical Bundling ของ Physical Ports เข้าด้วยกัน ข้อมูลจะถูกกระจายไปตามสายเส้นต่างๆ โดยใช้ Load-balancing Hash Algorithm

### 2. Protocols: LACP vs PAgP
- **LACP (Link Aggregation Control Protocol):** เป็นมาตรฐานกลาง (IEEE 802.3ad) ที่ใช้ได้กับ Switch ทุกยี่ห้อ (ใช้ Mode: Active/Passive)
- **PAgP (Port Aggregation Protocol):** เป็นโปรโตคอลลิขสิทธิ์ของ Cisco เท่านั้น (ใช้ Mode: Desirable/Auto)
- **On Mode:** เป็นการบังคับให้ทำ EtherChannel โดยไม่ใช้โปรโตคอลเจรจา (ไม่แนะนำเพราะอาจเกิด Loop ได้ถ้าฝั่งตรงข้ามตั้งค่าไม่ตรงกัน)

### 3. Loop Prevention & Bandwidth
- **Bandwidth:** หากเรานำสาย 100Mbps 2 เส้นมาทำ EtherChannel เราจะได้ท่อส่งข้อมูลขนาด 200Mbps (Logical)
- **Redundancy:** หากสายเส้นใดเส้นหนึ่งขาด ข้อมูลจะสลับไปวิ่งเส้นที่เหลือทันทีโดยที่ STP ไม่ต้องคำนวณใหม่ (Convergence Time รวดเร็วมาก)

---

### 🛠️ Troubleshooting & Verification
- **Common Problem:** พอร์ตที่นำมารวมกันมีคุณสมบัติไม่เหมือนกัน เช่น ความเร็วไม่เท่ากัน (Speed mismatch) หรือตั้งค่า VLAN ไม่ตรงกัน (Duplex/VLAN mismatch) ทำให้ EtherChannel ไม่ทำงาน
- **Solution:** ต้องแน่ใจว่าพอร์ตที่จะรวมกันมี `speed`, `duplex`, `switchport mode (access/trunk)` และ `allowed vlan` ที่เหมือนกันทุกประการ
- **Verification Commands:**
    - `show etherchannel summary`: ตรวจสอบสถานะภาพรวม (Flag ต้องเป็น SU และ P)
    - `show interface port-channel 1`: ดูสถานะทาง Logical ของพอร์ตที่สร้างขึ้น
    - `show spanning-tree`: ตรวจสอบว่า STP เห็นพอร์ตเป็น Po1 หรือไม่
