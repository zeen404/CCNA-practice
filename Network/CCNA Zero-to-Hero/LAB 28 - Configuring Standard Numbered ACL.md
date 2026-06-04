## ขั้นตอนการทดลอง (Lab Steps)

![](images/Pasted%20image%2020260518120017.png)



### 1. ตรวจสอบ Topology และการเชื่อมต่อพื้นฐาน
เริ่มต้นด้วยการตรวจสอบแผนผังเครือข่าย (Topology) และตั้งค่า IP Address ให้กับอุปกรณ์ต่างๆ ให้ถูกต้องตามที่กำหนด
![](images/Pasted%20image%2020260518101251.png)

### 2. ทดสอบการเชื่อมต่อก่อนการตั้งค่า (Pre-Test)
ก่อนการประกาศใช้ ACL ให้ทำการทดสอบ Ping เพื่อยืนยันว่าทุกเครื่องต้นทาง (Source) สามารถสื่อสารกับปลายทาง (Destination) ได้ตามปกติ
*   **ทดสอบเครื่องที่ 1:** ตรวจสอบการสื่อสารไปยัง Server (Success)
![](../Simulate/Image/Pasted%20image%2020260518100904.png)
*   **ทดสอบเครื่องที่ 2:** ตรวจสอบการสื่อสารไปยัง Server (Success)
![](../Simulate/Image/Pasted%20image%2020260518100934.png)

### 3. การสร้าง Standard Numbered ACL
เข้าสู่โหมด Configuration ของ Router เพื่อสร้าง Standard Access Control List (หมายเลข 1-99) โดยระบุเงื่อนไขที่ต้องการกรอง (ในที่นี้คือการ Deny เฉพาะ IP ของเครื่องแรก)
![](images/Pasted%20image%2020260518105753.png)

### 4. การตรวจสอบสถานะของ ACL
ใช้คำสั่ง `show access-lists` เพื่อตรวจสอบว่ากฎที่สร้างขึ้นนั้นถูกต้อง และมีหมายเลขลำดับ (Sequence Number) เรียงลำดับอย่างไร
![](images/Pasted%20image%2020260518105839.png)

### 5. การนำ ACL ไปผูกกับ Interface (Application)
เลือก Interface ที่อยู่ใกล้ปลายทางมากที่สุด (Destination) แล้วใช้คำสั่ง `ip access-group [หมายเลข] [ทิศทาง]` เพื่อเริ่มต้นการกรองทราฟฟิก
![](images/Pasted%20image%2020260518105852.png)

### 6. ทดสอบการทำงานหลังตั้งค่า (Post-Test)
ตรวจสอบผลลัพธ์ว่า ACL ทำงานตามเงื่อนไขที่กำหนดหรือไม่
*   **เครื่องที่ถูกสั่ง Deny:** จะต้องไม่สามารถ Ping ไปยังปลายทางได้ (Destination Host Unreachable)
![](images/Pasted%20image%2020260518105907.png)
*   **เครื่องอื่นๆ (Permit):** จะต้องยังคงสามารถสื่อสารได้ตามปกติ
![](images/Pasted%20image%2020260518110004.png)

---

### สรุปความรู้: LAB 28 - Configuring Standard Numbered ACL

#### 1. Standard Numbered ACL คืออะไร?
**Access Control List (ACL)** คือชุดของคำสั่ง (Rules/Policies) ที่สร้างขึ้นบน Router หรือ Layer 3 Switch เพื่อทำหน้าที่เป็นเหมือน "ยามรักษาความปลอดภัย" คอยคัดกรองแพ็กเก็ตข้อมูล (Traffic) ที่วิ่งเข้าหรือออกจากอุปกรณ์ ว่าจะ **อนุญาต (Permit)** หรือ **ปฏิเสธ (Deny)**

**Standard Numbered ACL** เป็นประเภทพื้นฐานและเรียบง่ายที่สุดของ ACL โดยมีลักษณะเฉพาะดังนี้:
*   **ใช้หมายเลขกำกับ:** ใช้หมายเลขตั้งแต่ **1 ถึง 99** (และช่วงขยาย 1300-1999) 
*   **ตรวจสอบแค่ IP ต้นทาง (Source IP Address) เท่านั้น:** กฎนี้จะสนใจแค่ว่า "ใครเป็นคนส่งข้อมูลมา" แต่จะไม่สนว่า "จะส่งไปหาใคร (Destination)" หรือ "ใช้บริการ/พอร์ตอะไร (Protocol/Port)" 

#### 2. หลักการทำงาน (How it works)
*   **Top-Down Processing (ประมวลผลจากบนลงล่าง):** เมื่อแพ็กเก็ตวิ่งเข้ามา Router จะตรวจสอบเงื่อนไขใน ACL ทีละบรรทัดจากบนลงล่าง หากแพ็กเก็ตตรงกับเงื่อนไขบรรทัดใด Router จะทำตามคำสั่ง (Permit หรือ Deny) ทันที แล้ว**หยุดอ่านบรรทัดถัดไป**
*   **Implicit Deny Any (กฎปฏิเสธทุกสิ่งในตอนท้าย):** ในบรรทัดสุดท้ายของทุก ACL จะมีกฎล่องหนซ่อนอยู่เสมอ คือ "ถ้าไม่ตรงกับกฎข้อใดเลยข้างบน ให้ปฏิเสธ (ทิ้ง) แพ็กเก็ตนี้ซะ" ดังนั้นหากสร้าง ACL ขึ้นมา ต้องมีคำสั่ง Permit อย่างน้อย 1 บรรทัดเสมอ ไม่เช่นนั้นทุกอย่างจะถูกบล็อกหมด
*   **ตำแหน่งการวางที่ดีที่สุด (Placement):** ตาม Best Practice ของ Cisco การติดตั้ง Standard ACL ควรจะนำไปผูกกับ Interface ที่อยู่ **"ใกล้กับจุดหมายปลายทาง (Destination) มากที่สุด"** (เพราะถ้าเอาไปวางใกล้ต้นทาง ต้นทางนั้นจะถูกบล็อกไม่ให้ไปหาเครือข่ายอื่นๆ ทั้งหมดเลย เนื่องจากมันระบุปลายทางไม่ได้)
*   **Direction (ทิศทางการกรอง):** ต้องเลือกว่าจะดักจับข้อมูล **ขาเข้า (In)** หรือ **ขาออก (Out)** ของ Interface นั้นๆ

#### 3. ประโยชน์ (Benefits)
1. **ตั้งค่าได้ง่ายและรวดเร็ว:** เนื่องจากรูปแบบคำสั่งไม่ซับซ้อน (ไม่ต้องระบุ Destination IP หรือ Port) เหมาะสำหรับการบล็อกหรืออนุญาตแบบเหมาเข่ง (Host หรือ Subnet)
2. **ประหยัดทรัพยากร (Low Overhead):** Router ใช้ CPU และ Memory น้อยในการประมวลผล เพราะตรวจสอบแค่เพียงฟิลด์ Source IP ใน IP Header เท่านั้น
3. **จัดการทราฟฟิกและเพิ่มความปลอดภัยเบื้องต้น:** สามารถใช้ป้องกันเครือข่ายภายในไม่ให้ถูกเข้าถึงจาก IP หรือ เครือข่ายที่ไม่ได้รับอนุญาต หรือใช้คัดกรองว่า IP ใดสามารถ Remote เข้ามาจัดการ Router ผ่าน Telnet/SSH (VTY lines) ได้บ้าง

#### 4. ผลเสียและข้อจำกัด (Drawbacks / Limitations)
1. **ขาดความยืดหยุ่นและความละเอียด (Lack of Granularity):** นี่คือข้อเสียที่ใหญ่ที่สุด เพราะมันไม่สามารถระบุ IP ปลายทางได้ หากวาง ACL ผิดตำแหน่ง อาจส่งผลให้ Host ต้นทางไม่สามารถติดต่อกับเครือข่ายอื่นที่ควรจะติดต่อได้ (Collateral damage)
2. **บล็อกเป็นราย Service/Protocol ไม่ได้:** ไม่สามารถเลือกบล็อกเฉพาะบางแอปพลิเคชันได้ เช่น ไม่สามารถสั่งให้ "บล็อกแค่การเข้าเว็บ (HTTP) แต่ยังให้ Ping หากันได้" หากเราใช้ Standard ACL คือการ "บล็อกทุกอย่างแบบเบ็ดเสร็จ" จาก IP ต้นทางนั้น
3. **จัดการกฎยาก (ในรูปแบบ Numbered):** โดยปกติถ้าระบุเป็นตัวเลขแล้วพิมพ์เรียงกันลงไป หากต้องการแก้ไข แทรกกฎ หรือลบแค่บางบรรทัด มักจะทำได้ยาก (ถ้าเป็น IOS เก่าๆ อาจต้องลบทั้ง ACL แล้วพิมพ์ใหม่ทั้งหมด) *แม้ว่าปัจจุบันจะใช้วิธี Sequence Number เข้ามาช่วยได้ก็ตาม*

#### สรุปตัวอย่างรูปแบบคำสั่งใน Lab
**1. การสร้างกฎ (Global Configuration Mode):**
```text
Router(config)# access-list 10 deny 172.31.1.100 0.0.0.0  (บล็อก R&D PC)
Router(config)# access-list 10 permit 172.16.1.0 0.0.0.255 (อนุญาตทั้งวง Finance 172.16.1.0/24)
```
*(หมายเหตุ: ต้องใช้ Wildcard Mask แทน Subnet Mask)*

**2. การนำกฎไปผูกกับ Interface:**
```text
Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip access-group 10 out
```
*(แปลว่า: นำกฎเบอร์ 10 ไปบังคับใช้กับข้อมูลที่กำลังจะส่ง "ออก" ทางพอร์ต GigabitEthernet0/1)*

---

## สรุปเพิ่มเติมแบบละเอียด: LAB 28 - Configuring Standard Numbered ACL

### 1. Lab 28 คืออะไร

Lab 28 เป็นการฝึกตั้งค่า **Standard Numbered ACL** บน Router เพื่อควบคุมว่าเครื่องต้นทางเครื่องใดสามารถส่งข้อมูลผ่าน Router ได้ และเครื่องใดต้องถูกบล็อก

คำว่า ACL ย่อมาจาก **Access Control List** หมายถึงรายการกฎที่ Router ใช้ตัดสินใจว่า traffic ที่ผ่านเข้ามาควรถูกอนุญาตหรือปฏิเสธ

ใน Lab นี้ ACL ที่ใช้เป็นแบบ **Standard Numbered ACL** ซึ่งมีลักษณะสำคัญคือ:

- ใช้หมายเลข ACL เช่น `1-99` หรือช่วงขยาย `1300-1999`
- ตรวจสอบเฉพาะ **Source IP Address**
- ไม่สามารถตรวจสอบ Destination IP, Protocol หรือ Port ได้
- เหมาะกับการควบคุมแบบพื้นฐาน เช่น บล็อก host หรือ subnet บางกลุ่ม

ตัวอย่างแนวคิดของ Lab:

```text
ต้องการบล็อก R&D PC ไม่ให้ไปถึง Finance Server
แต่ Finance PC ยังต้องใช้งาน Finance Server ได้ตามปกติ
```

ตาราง IP ตามรูป topology:

| อุปกรณ์/เครือข่าย | IP Address |
|---|---|
| Finance PC0 | `172.16.1.100/24` |
| R&D PC1 | `172.31.1.100/24` |
| Finance Server | `10.1.100.200/24` |
| Router ฝั่ง LAN | `.1` |
| Router ฝั่ง Server | `10.1.100.1/24` |

---

### 2. Standard Numbered ACL ทำงานอย่างไร

Router จะนำ packet ที่ผ่าน interface มาเทียบกับกฎใน ACL ทีละบรรทัดจากบนลงล่าง เรียกว่า **Top-Down Processing**

ตัวอย่าง:

```text
access-list 10 deny host 172.31.1.100
access-list 10 permit any
```

หลักการทำงานคือ:

1. ถ้า packet มาจาก `172.31.1.100` จะถูก `deny`
2. ถ้า packet มาจาก IP อื่น จะไม่ match บรรทัดแรก
3. Router จะอ่านบรรทัดถัดไป
4. ถ้าเจอ `permit any` traffic นั้นจะผ่านได้
5. เมื่อ match กับกฎใดแล้ว Router จะหยุดอ่าน ACL ทันที

จุดสำคัญคือ ACL ทุกตัวมีคำสั่งซ่อนอยู่ท้ายสุดเสมอ:

```text
deny any
```

คำสั่งนี้เรียกว่า **Implicit Deny** หมายความว่า ถ้า traffic ไม่ตรงกับกฎใดเลย จะถูกปฏิเสธทั้งหมด

ดังนั้นถ้าเขียน ACL แบบนี้:

```text
access-list 10 deny host 172.31.1.100
```

ผลที่เกิดขึ้นคือ:

- `172.31.1.100` ถูกบล็อก
- IP อื่นทั้งหมดก็ถูกบล็อกด้วย เพราะท้าย ACL มี `deny any` ซ่อนอยู่

จึงควรใส่ permit ต่อท้ายเสมอถ้าต้องการให้ traffic อื่นผ่านได้:

```text
access-list 10 deny host 172.31.1.100
access-list 10 permit any
```

---

### 3. Wildcard Mask คืออะไร

ในการเขียน ACL บน Cisco Router จะใช้ **Wildcard Mask** ไม่ใช่ Subnet Mask

หลักการจำง่าย:

```text
0 = ต้องตรงกัน
255 = ค่าอะไรก็ได้
```

ตัวอย่าง match host เดียว:

```text
172.31.1.100 0.0.0.0
```

หมายถึงต้องเป็น IP `172.31.1.100` เท่านั้น

ตัวอย่าง match ทั้ง network:

```text
172.16.1.0 0.0.0.255
```

หมายถึง IP ตั้งแต่:

```text
172.16.1.0 - 172.16.1.255
```

เทียบเท่ากับ network `172.16.1.0/24`

คำสั่งแบบสั้น:

```text
access-list 10 deny host 172.31.1.100
```

มีความหมายเท่ากับ:

```text
access-list 10 deny 172.31.1.100 0.0.0.0
```

และคำสั่ง:

```text
access-list 10 permit any
```

มีความหมายเท่ากับ:

```text
access-list 10 permit 0.0.0.0 255.255.255.255
```

---

### 4. ขั้นตอนหลักใน Lab

#### 4.1 ตรวจสอบ topology และ IP Address

ก่อนตั้งค่า ACL ต้องตรวจสอบว่า:

- Router เปิด interface แล้ว
- PC ได้ IP Address ถูกต้อง
- Default Gateway ของ PC ถูกต้อง
- Server มี IP Address ถูกต้อง
- Routing ระหว่าง network ทำงานได้

ถ้าพื้นฐานเหล่านี้ผิด การทดสอบ ACL จะสับสน เพราะ ping ไม่ผ่านอาจไม่ได้เกิดจาก ACL แต่อาจเกิดจาก IP หรือ routing ผิด

#### 4.2 ทดสอบก่อนตั้งค่า ACL

ก่อนใช้ ACL ต้อง ping ทดสอบก่อนว่าเครื่องต้นทางสามารถไปถึงปลายทางได้จริง

ถ้าก่อนตั้งค่า ACL ยัง ping ไม่ผ่าน ไม่ควรเริ่มสร้าง ACL ทันที ควรแก้ปัญหา basic connectivity ก่อน

#### 4.3 สร้าง ACL

ตัวอย่าง:

```text
Router(config)# access-list 10 deny host 172.31.1.100
Router(config)# access-list 10 permit 172.16.1.0 0.0.0.255
```

ความหมาย:

- ACL หมายเลข `10`
- บล็อก source IP `172.31.1.100`
- อนุญาต traffic จาก Finance network `172.16.1.0/24`

#### 4.4 ตรวจสอบ ACL

ใช้คำสั่ง:

```text
Router# show access-lists
```

หรือ:

```text
Router# show running-config
```

สิ่งที่ควรตรวจสอบ:

- หมายเลข ACL ถูกต้องหรือไม่
- มี deny และ permit ตามที่ต้องการหรือไม่
- ลำดับ rule ถูกต้องหรือไม่
- มี match counter เพิ่มขึ้นหรือไม่หลังทดสอบ traffic

#### 4.5 ผูก ACL กับ interface

สร้าง ACL อย่างเดียว ACL ยังไม่ทำงาน ต้องนำไปผูกกับ interface ก่อน

ตัวอย่าง:

```text
Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip access-group 10 out
```

ความหมาย:

- ใช้ ACL หมายเลข `10`
- กรอง traffic ที่กำลังออกจาก interface `GigabitEthernet0/1`

#### 4.6 ทดสอบหลังตั้งค่า

หลังผูก ACL แล้วให้ทดสอบ:

- เครื่องที่ถูก deny ต้อง ping ไม่ผ่าน
- เครื่องที่ถูก permit ต้อง ping ผ่าน
- ใช้ `show access-lists` ตรวจสอบว่ามีจำนวน match เพิ่มขึ้นหรือไม่

---

### 5. ตำแหน่งการวาง Standard ACL

หลักการสำคัญของ Standard ACL คือ:

```text
ควรวาง Standard ACL ให้ใกล้ Destination มากที่สุด
```

เหตุผลคือ Standard ACL ตรวจสอบได้เฉพาะ Source IP ถ้าวางใกล้ต้นทางเกินไป อาจทำให้ host นั้นถูกบล็อกไม่ให้ไปทุก network ทั้งที่จริงต้องการบล็อกเฉพาะการไปยังปลายทางบางแห่งเท่านั้น

ตัวอย่าง:

```text
PC1 ต้องถูกบล็อกไม่ให้เข้า Server เท่านั้น
แต่ยังควรไป network อื่นได้
```

ถ้านำ ACL ไปวางใกล้ PC1 อาจทำให้ PC1 ถูกบล็อกจากทุกปลายทาง แต่ถ้าวางใกล้ Server จะควบคุมได้ตรงกว่า

---

### 6. Direction: in และ out

ตอนผูก ACL กับ interface ต้องกำหนดทิศทาง:

```text
ip access-group 10 in
ip access-group 10 out
```

ความหมาย:

- `in` คือกรอง packet ตอนกำลังเข้ามาที่ interface
- `out` คือกรอง packet ตอนกำลังออกจาก interface

การเลือกทิศทางผิดจะทำให้ ACL ไม่ทำงานตามที่คาด หรืออาจบล็อก traffic ผิดฝั่ง

ตัวอย่าง:

```text
Router(config)# interface g0/1
Router(config-if)# ip access-group 10 out
```

หมายถึง Router จะตรวจ traffic ที่กำลังจะออกทาง `g0/1`

---

### 7. ประโยชน์ของ Standard Numbered ACL

1. **ควบคุมการเข้าถึง network เบื้องต้น**

ใช้บล็อกหรืออนุญาต host/subnet บางกลุ่มได้ เช่น ไม่ให้เครื่องบางเครื่องเข้า Server

2. **ตั้งค่าได้ง่าย**

คำสั่งไม่ซับซ้อน เหมาะกับการเรียน CCNA และงานพื้นฐาน

3. **ใช้ทรัพยากรน้อย**

Router ตรวจเฉพาะ Source IP จึงประมวลผลน้อยกว่า ACL ที่ตรวจละเอียดกว่า

4. **ใช้จำกัดสิทธิ์เข้าจัดการอุปกรณ์ได้**

สามารถใช้กับ VTY line เพื่ออนุญาตเฉพาะเครื่องของผู้ดูแลระบบให้ SSH หรือ Telnet เข้า Router ได้

5. **เพิ่มความปลอดภัยพื้นฐาน**

ช่วยลดโอกาสที่ host ที่ไม่ได้รับอนุญาตจะเข้าถึง network สำคัญ

---

### 8. ข้อดี

- เข้าใจง่าย
- เหมาะสำหรับผู้เริ่มต้น
- ใช้คำสั่งน้อย
- ใช้บล็อก host หรือ subnet ได้รวดเร็ว
- เหมาะกับ policy ที่ไม่ซับซ้อน
- ตรวจสอบได้ด้วยคำสั่ง `show access-lists`
- ใช้ได้กับ Router Cisco ทั่วไป

---

### 9. ข้อเสียและข้อจำกัด

1. **ตรวจสอบได้เฉพาะ Source IP**

ไม่สามารถกำหนดได้ว่า traffic จะไปปลายทางไหน

2. **ไม่สามารถเลือก Protocol หรือ Port ได้**

เช่น ไม่สามารถบล็อกเฉพาะ HTTP แต่ปล่อย Ping ได้ ถ้าต้องการควบคุมแบบนั้นต้องใช้ Extended ACL

3. **ถ้าวางผิดตำแหน่งอาจกระทบ traffic มากเกินไป**

เพราะ Standard ACL ไม่รู้ destination ถ้าวางใกล้ source อาจทำให้ source นั้นไปไหนไม่ได้เลย

4. **Numbered ACL อ่านยากกว่า Named ACL**

หมายเลขอย่าง `10` หรือ `20` ไม่สื่อความหมายเท่าชื่อ เช่น `BLOCK_PC1`

5. **ต้องระวัง Implicit Deny**

ถ้าลืมใส่ permit อาจทำให้ traffic ทั้งหมดถูกบล็อก

---

### 10. คำสั่งที่ควรรู้

สร้าง ACL เพื่อบล็อก host เดียว:

```text
Router(config)# access-list 10 deny host 172.31.1.100
Router(config)# access-list 10 permit 172.16.1.0 0.0.0.255
```

สร้าง ACL ด้วย wildcard mask:

```text
Router(config)# access-list 10 deny 172.31.1.100 0.0.0.0
Router(config)# access-list 10 permit 172.16.1.0 0.0.0.255
```

ผูก ACL กับ interface:

```text
Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip access-group 10 out
```

ตรวจสอบ ACL:

```text
Router# show access-lists
```

ตรวจสอบ ACL บน interface:

```text
Router# show ip interface GigabitEthernet0/1
```

ลบ ACL ออกจาก interface:

```text
Router(config)# interface GigabitEthernet0/1
Router(config-if)# no ip access-group 10 out
```

ลบ ACL ทั้งชุด:

```text
Router(config)# no access-list 10
```

---

### 11. ตัวอย่างสถานการณ์จริง

ต้องการบล็อกเครื่อง R&D `172.31.1.100` ไม่ให้เข้าถึง Finance Server `10.1.100.200` แต่ให้ Finance PC `172.16.1.100` ใช้งานได้ตามปกติ

```text
Router(config)# access-list 10 deny host 172.31.1.100
Router(config)# access-list 10 permit 172.16.1.0 0.0.0.255
Router(config)# interface g0/1
Router(config-if)# ip access-group 10 out
```

ผลลัพธ์:

- R&D PC ที่มี IP `172.31.1.100` เข้า Finance Server ไม่ได้
- Finance PC ที่มี IP `172.16.1.100` ยังเข้า Finance Server `10.1.100.200` ได้
- Router ใช้ ACL หมายเลข `10` ตรวจ traffic ก่อนปล่อยออก interface

---

### 12. ข้อควรจำสำหรับสอบ CCNA

- Standard ACL ตรวจเฉพาะ Source IP
- Standard ACL numbered ใช้ช่วง `1-99` และ `1300-1999`
- ACL อ่านจากบนลงล่าง
- เมื่อ match แล้วจะหยุดอ่านทันที
- ACL ทุกตัวมี `implicit deny any` ซ่อนอยู่ท้ายสุด
- ต้องมี permit อย่างน้อยหนึ่งบรรทัดถ้าต้องการให้ traffic ผ่าน
- Standard ACL ควรวางใกล้ destination
- ต้องผูก ACL กับ interface ก่อนจึงจะเริ่มทำงาน
- ต้องเลือก direction `in` หรือ `out` ให้ถูกต้อง
- ใช้ wildcard mask ไม่ใช่ subnet mask

---

### 13. สรุปสั้น

LAB 28 สอนการใช้ **Standard Numbered ACL** เพื่อควบคุม traffic โดยพิจารณาจาก **Source IP Address** เท่านั้น เหมาะกับการบล็อกหรืออนุญาต host/subnet แบบพื้นฐาน จุดที่ต้องระวังคือการเรียงลำดับ rule, การมี `implicit deny`, การใช้ wildcard mask, การเลือกตำแหน่งวาง ACL และการเลือกทิศทาง `in/out` เพราะถ้ากำหนดผิดอาจทำให้ traffic ถูกบล็อกเกินกว่าที่ต้องการ
