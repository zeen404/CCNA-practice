![](Image/Pasted%20image%2020260424102037.png)
### Layer 7 ถึง Layer 5: Data (ข้อมูลแอปพลิเคชัน)
(ดูภาพรวม: [OSI Model](Network%20Layers.md#Layer%207:%20Application%20Layer%20(ชั้นประยุกต์)))

สามชั้นบนสุดมักจะทำงานร่วมกันในซอฟต์แวร์แอปพลิเคชันและระบบปฏิบัติการ:

- **Layer 7 - Application Layer:** เป็นจุดเชื่อมต่อกับผู้ใช้ เช่น เมื่อคุณเปิดเว็บเบราว์เซอร์แล้วพิมพ์ HTTP/HTTPS หรือใช้ DNS เพื่อหา IP address
    
- **Layer 6 - Presentation Layer:** รับข้อมูลมาแปลงฟอร์แมต เข้ารหัส (Encryption เช่น TLS/SSL) หรือบีบอัด เพื่อให้อีกฝั่งเข้าใจตรงกัน
    
- **Layer 5 - Session Layer:** สร้าง จัดการ และยกเลิกการเชื่อมต่อ (Session) ระหว่างเครื่องต้นทางและปลายทาง **ก้อนข้อมูลในสถานะนี้เรียกว่า:** `Data` หรือ `Message`
    

### Layer 4: Transport Layer -> Segment / Datagram
(ดูภาพรวม: [OSI Model](Network%20Layers.md#Layer%204:%20Transport%20Layer%20(ชั้นขนส่ง)))

ข้อมูลจาก Layer 5 จะถูกส่งลงมา ชั้นนี้จะทำหน้าที่หั่นข้อมูลใหญ่ๆ ออกเป็นชิ้นเล็กๆ เรียกว่ากระบวนการ Segmentation

- ระบบจะเลือกโปรโตคอลส่งข้อมูล เช่น **TCP** (เน้นชัวร์ มีการตอบรับ) หรือ **UDP** (เน้นไว ไม่รอตอบรับ)
    
- มีการแปะ **Transport Header** เข้าไป ซึ่งประกอบด้วย **Source Port** และ **Destination Port** เพื่อให้รู้ว่าปลายทางจะต้องส่งข้อมูลนี้ไปให้แอปพลิเคชันไหน **ก้อนข้อมูลในสถานะนี้เรียกว่า:** `Segment` (สำหรับ TCP) หรือ `Datagram` (สำหรับ UDP)
    

### Layer 3: Network Layer -> Packet
(ดูภาพรวม: [OSI Model](Network%20Layers.md#Layer%203:%20Network%20Layer%20(ชั้นเครือข่าย)))

Layer นี้ดูแลเรื่องการหาเส้นทาง (Routing) ข้ามข่ายเครือข่าย

- นำ Segment จาก Layer 4 มาห่อหุ้มด้วย **Network Header** (มักจะเป็น IP Header)
    
- ใน Header จะระบุ **Source IP Address** (ไอพีต้นทาง) และ **Destination IP Address** (ไอพีปลายทาง) **ก้อนข้อมูลในสถานะนี้เรียกว่า:** `Packet`
    

### Layer 2: Data Link Layer -> Ethernet Frame
(ดูภาพรวม: [OSI Model](Network%20Layers.md#Layer%202:%20Data%20Link%20Layer%20(ชั้นเชื่อมโยงข้อมูล)))

นี่คือชั้นที่ **Ethernet Frame** ถูกสร้างขึ้น Layer นี้จะดูแลการส่งข้อมูลแบบ Node-to-Node ภายในเครือข่ายเดียวกัน (LAN) ผ่านฮาร์ดแวร์เช่น Switch หรือ Network Interface Card (NIC)

มันจะนำ `Packet` จาก Layer 3 มาห่อหุ้มด้วย **Mac Header** ไว้ด้านหน้า และแปะ **Trailer** ไว้ด้านหลัง

โครงสร้างของ **Ethernet Frame (มาตรฐาน Ethernet II)** แบบละเอียดมีดังนี้:

1. **Preamble (7 Bytes):** เป็นลำดับของบิตสลับกัน (10101010...) ใช้เพื่อซิงโครไนซ์จังหวะสัญญาณนาฬิกา (Clock synchronization) ระหว่างการ์ดแลนต้นทางและปลายทาง
    
2. **SFD - Start of Frame Delimiter (1 Byte):** มีค่าบิต `10101011` เป็นตัวส่งสัญญาณบอกการ์ดแลนปลายทางว่า "ข้อมูล Preamble จบแล้วนะ บิตต่อไปคือเนื้อหาของ Frame ของจริง"
    
3. **Destination MAC Address (6 Bytes):** ที่อยู่ MAC Address ของเครื่องปลายทาง (หรือ MAC ของ Router/Gateway หากต้องส่งข้ามวงเน็ตเวิร์ก)
    
4. **Source MAC Address (6 Bytes):** ที่อยู่ MAC Address ของเครื่องต้นทาง
    
5. **EtherType / Length (2 Bytes):** เป็นฟิลด์ที่ระบุว่าก้อนข้อมูล (Payload) ที่ห่ออยู่ข้างในนั้นคือโปรโตคอลอะไรของ Layer 3 เช่น:
    
    - `0x0800` = IPv4 Packet
        
    - `0x86DD` = IPv6 Packet
        
    - `0x0806` = ARP (Address Resolution Protocol)
        
6. **Payload หรือ Data (46 ถึง 1500 Bytes):** นี่คือส่วนที่เป็น `Packet` จาก Layer 3 (IP Header + TCP Header + Data) ที่ถูกห่อหุ้มมา
    
    - _ข้อควรระวัง:_ มาตรฐาน Ethernet กำหนดว่า Payload ต้องมีขนาดขั้นต่ำ 46 Bytes หากข้อมูลมีขนาดเล็กกว่านี้ ระบบจะทำการแทรกข้อมูลว่างๆ (Padding) เข้าไปให้ครบ 46 Bytes
        
7. **FCS - Frame Check Sequence (4 Bytes):** เป็น Trailer ที่ต่อท้ายสุด ใช้สมการคณิตศาสตร์ CRC (Cyclic Redundancy Check) คำนวณค่าแฮชของทั้ง Frame (ตั้งแต่ Destination MAC จนถึง Payload) หากระหว่างทางมีสัญญาณรบกวนทำให้บิตข้อมูลเพี้ยนไปแม้แต่บิตเดียว ปลายทางคำนวณ FCS ออกมาไม่ตรงกัน Frame นี้จะถูก "ดรอปทิ้ง" (Drop) ทันที
    

**ก้อนข้อมูลทั้งหมดนี้รวมกันเรียกว่า:** `Frame`

### Layer 1: Physical Layer -> Bits
(ดูภาพรวม: [OSI Model](Network%20Layers.md#Layer%201:%20Physical%20Layer%20(ชั้นกายภาพ)))

เมื่อ Layer 2 ประกอบร่าง Ethernet Frame เสร็จแล้ว มันจะส่งต่อลงมาที่ Layer 1 (ส่วนประกอบฮาร์ดแวร์บนการ์ดแลน)

- หน้าที่ของชั้นนี้คือการแปลงก้อน `Frame` ซึ่งเป็นข้อมูลดิจิทัล (0 และ 1) ให้กลายเป็นสัญญาณทางกายภาพเพื่อพุ่งออกไปตามสาย
    
- ถ้าใช้สายทองแดง (Twisted pair / RJ45) จะแปลงเป็นสัญญาณไฟฟ้า (Electrical voltage)
    
- ถ้าใช้สายไฟเบอร์ออปติก จะแปลงเป็นพัลส์แสง (Light pulses)
    
- ถ้าใช้ Wi-Fi จะแปลงเป็นคลื่นวิทยุ (Radio waves) **ก้อนข้อมูลในสถานะนี้เรียกว่า:** `Bits` (สายธารของ 0 และ 1 ทางกายภาพ)
    

เมื่อสัญญาณเดินทางไปถึงปลายทาง กระบวนการทั้งหมดจะถูกทำย้อนกลับ (Decapsulation) จาก Layer 1 แกะเปลือกขึ้นไปเรื่อยๆ จนถึง Layer 7 เพื่อให้โปรแกรมปลายทางอ่านข้อมูลได้ครับ
