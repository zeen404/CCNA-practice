![](images/Pasted%20image%2020260518133924.png)

**รูปที่ 1: ตรวจสอบ Topology และ IP Address ของ Lab**  
Topology นี้มี Finance PC `172.16.1.100/24`, R&D PC `172.31.1.100/24`, Router เป็น gateway `.1` และ Finance Server อยู่ใน network `10.1.100.0/24` โดย Server ใช้ IP `10.1.100.200/24`

![](images/Pasted%20image%2020260518134015.png)

**รูปที่ 2: สร้าง Standard Named ACL ชื่อ `FIN_ACL_OUT`**  
ใช้คำสั่ง `ip access-list standard FIN_ACL_OUT` เพื่อเข้าโหมดตั้งค่า Standard Named ACL โดย ACL ตัวนี้จะใช้กรอง traffic ขาออกไปยังฝั่ง Finance Server

![](images/Pasted%20image%2020260518134044.png)

**รูปที่ 3: ใส่ rule สำหรับ Deny และ Permit ใน ACL**  
กำหนดให้ `deny host 172.31.1.100` เพื่อบล็อก R&D PC และ `permit 172.16.1.0 0.0.0.255` เพื่ออนุญาต Finance network ให้ผ่านไปยัง Finance Server ได้

![](images/Pasted%20image%2020260518134118.png)

**รูปที่ 4: นำ ACL ไปผูกกับ Interface ขาออก**  
เข้า interface `GigabitEthernet0/0/0` แล้วใช้ `ip access-group FIN_ACL_OUT out` เพื่อให้ Router ใช้ ACL นี้กรอง packet ก่อนส่งออกไปยัง network `10.1.100.0/24`

![](images/Pasted%20image%2020260518134632.png)

**รูปที่ 5: ตรวจสอบ ACL และจำนวน Match**  
ใช้ `show access-lists FIN_ACL_OUT` เพื่อตรวจสอบ rule ภายใน ACL และดูจำนวน match ว่า traffic จาก R&D ถูก deny และ traffic จาก Finance ถูก permit ตามที่ตั้งค่าไว้

![](images/Pasted%20image%2020260518134656.png)

**รูปที่ 6: ตรวจสอบว่า ACL ถูกผูกกับ Interface แล้ว**  
จาก running-config ของ `GigabitEthernet0/0/0` จะเห็น `ip access-group FIN_ACL_OUT out` แปลว่า ACL ถูกใช้งานกับ traffic ขาออกของ interface นี้แล้ว

![](images/Pasted%20image%2020260518134706.png)

**รูปที่ 7: ตรวจสอบ Named ACL ใน Running Config**  
แสดง config ของ `ip access-list standard FIN_ACL_OUT` ซึ่งมี rule หลักคือ deny R&D PC, permit Finance network และมี `deny any` ต่อท้ายเพื่อปฏิเสธ traffic อื่นอย่างชัดเจน แม้ไม่ใส่ก็ยังมี implicit deny ซ่อนอยู่

---

## สรุปความรู้: LAB 29 - Configuring Standard Named ACL

### 1. Lab 29 คืออะไร

Lab 29 เป็นการฝึกตั้งค่า **Standard Named ACL** บน Cisco Router เพื่อควบคุม traffic โดยอ้างอิงจาก **Source IP Address** เหมือน Standard Numbered ACL แต่เปลี่ยนจากการใช้หมายเลข ACL เช่น `10` มาเป็นการใช้ชื่อ ACL แทน

ตัวอย่างความแตกต่าง:

```text
Standard Numbered ACL:
access-list 10 deny host 172.31.1.100

Standard Named ACL:
ip access-list standard FIN_ACL_OUT
 deny host 172.31.1.100
```

ข้อดีของ Named ACL คืออ่านง่ายกว่า เพราะชื่อ ACL สามารถสื่อความหมายของ policy ได้ เช่น `FIN_ACL_OUT`

---

### 2. IP Address ตามรูป Topology

| อุปกรณ์/เครือข่าย | IP Address |
|---|---|
| Finance PC0 | `172.16.1.100/24` |
| R&D PC1 | `172.31.1.100/24` |
| Finance Server | `10.1.100.200/24` |
| Network ฝั่ง Finance | `172.16.1.0/24` |
| Network ฝั่ง R&D | `172.31.1.0/24` |
| Network ฝั่ง Server | `10.1.100.0/24` |
| Router ฝั่ง Server | `10.1.100.1/24` |

เป้าหมายของ Lab:

```text
บล็อก R&D PC 172.31.1.100 ไม่ให้เข้า Finance Server 10.1.100.200
แต่อนุญาต Finance PC 172.16.1.100 ให้เข้า Finance Server ได้
```

---

### 3. Standard Named ACL ทำงานอย่างไร

Standard Named ACL ยังเป็น Standard ACL เหมือนเดิม ดังนั้นตรวจสอบได้เฉพาะ:

```text
Source IP Address
```

ไม่สามารถตรวจสอบสิ่งเหล่านี้ได้:

- Destination IP
- Protocol เช่น TCP, UDP, ICMP
- Port เช่น 80, 443, 22

Router จะอ่าน rule ใน ACL จากบนลงล่าง เมื่อเจอ rule ที่ match แล้วจะทำตามคำสั่ง `permit` หรือ `deny` ทันที และหยุดอ่าน rule ถัดไป

ท้าย ACL ทุกตัวมี rule ซ่อนอยู่เสมอ:

```text
deny any
```

จึงต้องเขียน `permit` ให้ชัดเจน ไม่เช่นนั้น traffic ที่ไม่ match rule ด้านบนจะถูกบล็อกทั้งหมด

---

### 4. ตัวอย่างคำสั่งใน Lab

สร้าง Named ACL ชื่อ `FIN_ACL_OUT`:

```text
Router(config)# ip access-list standard FIN_ACL_OUT
Router(config-std-nacl)# deny host 172.31.1.100
Router(config-std-nacl)# permit 172.16.1.0 0.0.0.255
Router(config-std-nacl)# deny any
Router(config-std-nacl)# exit
```

ความหมาย:

- `FIN_ACL_OUT` คือชื่อ Standard Named ACL
- `deny host 172.31.1.100` คือบล็อก R&D PC
- `permit 172.16.1.0 0.0.0.255` คืออนุญาต Finance network
- `deny any` คือปฏิเสธ traffic อื่นที่ไม่ match rule ด้านบน และทำให้เห็น deny ชัดเจนใน config

นำ ACL ไปผูกกับ interface ฝั่งที่ออกไปหา Finance Server:

```text
Router(config)# interface GigabitEthernet0/0/0
Router(config-if)# ip access-group FIN_ACL_OUT out
```

หมายความว่า Router จะตรวจ packet ก่อนส่งออกจาก interface ไปยัง network `10.1.100.0/24`

---

### 5. ทำไม Standard ACL ควรวางใกล้ Destination

Standard ACL ดูเฉพาะ source IP เท่านั้น ถ้านำไปวางใกล้ต้นทางมากเกินไป อาจทำให้ host นั้นถูกบล็อกไม่ให้ไปทุกที่

ใน topology นี้ ถ้าเป้าหมายคือป้องกัน Finance Server ควรวาง ACL ใกล้ฝั่ง Server มากที่สุด เช่น interface ที่ออกไปยัง network:

```text
10.1.100.0/24
```

เหตุผล:

- R&D PC จะถูกบล็อกเฉพาะตอนพยายามไป Finance Server
- Finance PC ยังเข้า Finance Server ได้
- ลดโอกาสบล็อก traffic อื่นผิดพลาด

---

### 6. คำสั่งตรวจสอบ

ดู ACL ที่สร้างไว้:

```text
Router# show access-lists FIN_ACL_OUT
```

หรือ:

```text
Router# show ip access-lists
```

ดูว่า ACL ถูกผูกกับ interface หรือยัง:

```text
Router# show ip interface GigabitEthernet0/0/0
```

ดู running config เฉพาะส่วน ACL:

```text
Router# show running-config | section ip access-list
```

ดู running config เฉพาะ interface:

```text
Router# show running-config interface GigabitEthernet0/0/0
```

---

### 7. การทดสอบผลลัพธ์

หลังตั้งค่า ACL แล้วให้ทดสอบดังนี้:

| Source | Destination | ผลที่ควรได้ |
|---|---|---|
| R&D PC `172.31.1.100` | Finance Server `10.1.100.200` | Ping ไม่ผ่าน |
| Finance PC `172.16.1.100` | Finance Server `10.1.100.200` | Ping ผ่าน |

ถ้า R&D ยัง ping ผ่าน แสดงว่าอาจมีปัญหา เช่น:

- ACL ยังไม่ได้ผูกกับ interface
- ผูกผิด interface
- เลือก direction `in/out` ผิด
- IP ใน ACL ไม่ตรงกับ source จริง
- routing หรือ gateway ใน topology ไม่ตรงกับที่คิด

ถ้า Finance PC ping ไม่ผ่าน อาจเกิดจาก:

- ลืมใส่ permit สำหรับ `172.16.1.0/24`
- wildcard mask ผิด
- implicit deny บล็อก traffic
- ปัญหา IP address, default gateway หรือ routing

---

### 8. ข้อดีของ Standard Named ACL

- อ่านง่ายกว่า numbered ACL เพราะใช้ชื่อสื่อความหมายได้
- แก้ไขและตรวจสอบง่ายกว่าใน config
- เหมาะกับ network ที่มี policy หลายชุด
- ลดความสับสนจากการจำหมายเลข ACL
- ใช้ sequence number เพื่อแทรก ลบ หรือจัดการ rule ได้สะดวกกว่า

ตัวอย่างชื่อ ACL ที่ดี:

```text
FIN_ACL_OUT
PERMIT_FINANCE_ONLY
MGMT_ACCESS_ONLY
```

---

### 9. ข้อเสียและข้อจำกัด

Standard Named ACL ยังมีข้อจำกัดของ Standard ACL:

- ตรวจสอบได้เฉพาะ Source IP
- ระบุ Destination IP ไม่ได้
- ระบุ port ไม่ได้
- ระบุ protocol ไม่ได้
- ถ้าวางผิดตำแหน่ง อาจบล็อก traffic เกินกว่าที่ต้องการ

ถ้าต้องการ policy แบบละเอียด เช่น:

```text
บล็อก R&D เข้าเว็บ server port 80
แต่ยังให้ ping ได้
```

ควรใช้ **Extended ACL** แทน เพราะ Extended ACL สามารถตรวจ source, destination, protocol และ port ได้

---

### 10. การลบหรือแก้ไข ACL

ลบ ACL ออกจาก interface ก่อน:

```text
Router(config)# interface GigabitEthernet0/0/0
Router(config-if)# no ip access-group FIN_ACL_OUT out
```

ลบ Named ACL:

```text
Router(config)# no ip access-list standard FIN_ACL_OUT
```

เพิ่ม rule ใหม่ใน ACL เดิม:

```text
Router(config)# ip access-list standard FIN_ACL_OUT
Router(config-std-nacl)# deny host 172.31.1.100
Router(config-std-nacl)# permit 172.16.1.0 0.0.0.255
Router(config-std-nacl)# deny any
```

---

### 11. เปรียบเทียบ Lab 28 กับ Lab 29

| หัวข้อ | Lab 28 | Lab 29 |
|---|---|---|
| ประเภท ACL | Standard Numbered ACL | Standard Named ACL |
| การอ้างอิง ACL | ใช้หมายเลข เช่น `10` | ใช้ชื่อ เช่น `FIN_ACL_OUT` |
| ตรวจสอบอะไร | Source IP | Source IP |
| ความอ่านง่าย | น้อยกว่า | มากกว่า |
| เหมาะกับ | lab พื้นฐาน/คำสั่งสั้น | config ที่ต้องการชื่อ policy ชัดเจน |

---

### 12. ข้อควรจำสำหรับสอบ CCNA

- Standard Named ACL ตรวจเฉพาะ Source IP
- ใช้คำสั่ง `ip access-list standard NAME`
- ต้องเข้า sub-mode ของ ACL ก่อนใส่ `permit` หรือ `deny`
- ทุก ACL มี implicit deny อยู่ท้ายสุด
- ต้องผูก ACL กับ interface ด้วย `ip access-group NAME in/out`
- Standard ACL ควรวางใกล้ destination
- ใช้ wildcard mask ไม่ใช่ subnet mask
- Named ACL อ่านง่ายและดูแล config ง่ายกว่า numbered ACL

---

### 13. สรุปสั้น

Lab 29 คือการตั้งค่า **Standard Named ACL** เพื่อควบคุม traffic โดยใช้ชื่อ ACL แทนหมายเลข ใน topology นี้ใช้ ACL ชื่อ `FIN_ACL_OUT` เพื่อบล็อก R&D PC `172.31.1.100` ไม่ให้เข้า Finance Server `10.1.100.200` และอนุญาต Finance network `172.16.1.0/24` ให้ใช้งานได้ จุดสำคัญคือต้องวาง ACL ใกล้ปลายทาง เลือก direction ให้ถูก และระวัง `implicit deny`
