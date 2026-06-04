![](images/Pasted%20image%2020260518150751.png)

**รูปที่ 1: ตรวจสอบ Topology และ IP Address ของ Lab**  
Topology นี้แบ่งเครือข่ายออกเป็น 2 ฝั่ง คือ Sales network `172.16.1.0/24` และ Engineer network `172.31.1.0/24` โดย PC0 ใช้ IP `172.16.1.100/24`, PC1 ใช้ IP `172.31.1.100/24` และ Router ทำหน้าที่เป็น gateway ของแต่ละ VLAN/subinterface คือ `172.16.1.1` และ `172.31.1.1`

![](images/Pasted%20image%2020260518150705.png)

**รูปที่ 2: ตรวจสอบ Extended Numbered ACL หมายเลข `100`**  
ใช้คำสั่ง `show access-lists 100` เพื่อดู rule ใน ACL โดย ACL นี้บล็อก Telnet จาก Sales PC ไปยัง gateway `172.16.1.1`, อนุญาต Telnet จาก Engineer PC ไปยัง gateway `172.31.1.1` และอนุญาต ICMP เพื่อให้ยัง ping ได้

![](images/Pasted%20image%2020260518150823.png)

**รูปที่ 3: ตรวจสอบการผูก ACL เข้ากับ Subinterface**  
ACL หมายเลข `100` ถูกนำไปใช้แบบ inbound ด้วยคำสั่ง `ip access-group 100 in` บน subinterface `GigabitEthernet0/0/1.16` และ `GigabitEthernet0/0/1.31` เพื่อกรอง traffic ตั้งแต่ตอนเข้ามาที่ Router จากแต่ละ VLAN

![](images/Pasted%20image%2020260518150841.png)

**รูปที่ 4: ทดสอบจาก Sales PC0**  
Sales PC `172.16.1.100` ping ไปยัง gateway `172.16.1.1` ได้สำเร็จ เพราะ ACL มี `permit icmp any any` แต่เมื่อ telnet ไปที่ `172.16.1.1` จะไม่สำเร็จ เพราะถูก rule `deny tcp host 172.16.1.100 host 172.16.1.1 eq telnet` บล็อกไว้

![](images/Pasted%20image%2020260518150927.png)

**รูปที่ 5: ทดสอบจาก Engineer PC1**  
Engineer PC `172.31.1.100` ping ไปยัง gateway `172.31.1.1` ได้ และ telnet ไปยัง `172.31.1.1` ได้ เพราะ ACL มี rule `permit tcp host 172.31.1.100 host 172.31.1.1 eq telnet`

---

## สรุปความรู้: LAB 30 - Configuring Extended Numbered ACL

### 1. Lab 30 คืออะไร

Lab 30 เป็นการฝึกตั้งค่า **Extended Numbered ACL** บน Cisco Router เพื่อควบคุม traffic แบบละเอียดกว่า Standard ACL

Standard ACL ตรวจได้เฉพาะ:

```text
Source IP Address
```

แต่ Extended ACL ตรวจได้ละเอียดกว่า เช่น:

- Source IP
- Destination IP
- Protocol เช่น IP, TCP, UDP, ICMP
- Port เช่น Telnet `23`, HTTP `80`, HTTPS `443`, SSH `22`

ใน Lab นี้ใช้ Extended ACL หมายเลข `100` เพื่อควบคุมการ Telnet เข้า Router:

```text
Sales PC ห้าม Telnet เข้า Router
Engineer PC อนุญาตให้ Telnet เข้า Router
แต่ทั้งสองฝั่งยัง Ping ได้
```

---

### 2. IP Address ตามรูป Topology

| อุปกรณ์/เครือข่าย | IP Address |
|---|---|
| Sales PC0 | `172.16.1.100/24` |
| Engineer PC1 | `172.31.1.100/24` |
| Router subinterface VLAN 16 | `172.16.1.1/24` |
| Router subinterface VLAN 31 | `172.31.1.1/24` |
| Sales network | `172.16.1.0/24` |
| Engineer network | `172.31.1.0/24` |

---

### 3. Extended Numbered ACL คืออะไร

**Extended Numbered ACL** คือ ACL แบบใช้หมายเลขที่สามารถกรอง traffic ได้ละเอียด โดยใช้หมายเลขในช่วง:

```text
100-199
2000-2699
```

รูปแบบคำสั่งพื้นฐาน:

```text
access-list <number> <permit|deny> <protocol> <source> <destination> <operator> <port>
```

ตัวอย่าง:

```text
access-list 100 deny tcp host 172.16.1.100 host 172.16.1.1 eq telnet
```

ความหมาย:

- `100` คือหมายเลข Extended ACL
- `deny` คือปฏิเสธ traffic
- `tcp` คือ protocol ที่ต้องการกรอง
- `host 172.16.1.100` คือ source IP
- `host 172.16.1.1` คือ destination IP
- `eq telnet` คือ match port Telnet หรือ TCP port `23`

---

### 4. คำสั่ง ACL ที่ใช้ใน Lab

สร้าง Extended Numbered ACL หมายเลข `100`:

```text
Router(config)# access-list 100 deny tcp host 172.16.1.100 host 172.16.1.1 eq telnet
Router(config)# access-list 100 permit tcp host 172.31.1.100 host 172.31.1.1 eq telnet
Router(config)# access-list 100 permit icmp any any
```

ความหมายของแต่ละบรรทัด:

| Rule | ความหมาย |
|---|---|
| `deny tcp host 172.16.1.100 host 172.16.1.1 eq telnet` | บล็อก Sales PC ไม่ให้ Telnet เข้า Router gateway ของ Sales |
| `permit tcp host 172.31.1.100 host 172.31.1.1 eq telnet` | อนุญาต Engineer PC ให้ Telnet เข้า Router gateway ของ Engineer |
| `permit icmp any any` | อนุญาต ICMP เพื่อให้ ping ได้ |

จุดสำคัญ: ถ้าไม่มี `permit icmp any any` การ ping อาจถูกบล็อก เพราะท้าย ACL มี **implicit deny any** ซ่อนอยู่เสมอ

---

### 5. การนำ ACL ไปผูกกับ Interface

ใน Lab นี้ Router ใช้ subinterface แยก VLAN:

```text
GigabitEthernet0/0/1.16
GigabitEthernet0/0/1.31
```

นำ ACL ไปผูกแบบ inbound:

```text
Router(config)# interface GigabitEthernet0/0/1.16
Router(config-subif)# ip access-group 100 in

Router(config)# interface GigabitEthernet0/0/1.31
Router(config-subif)# ip access-group 100 in
```

เหตุผลที่ใช้ `in`:

- traffic จาก PC จะเข้ามาที่ Router ทาง subinterface ก่อน
- Router ตรวจ ACL ทันทีตอน packet เข้ามา
- เหมาะกับ Extended ACL เพราะควรวางใกล้ source เพื่อหยุด traffic ที่ไม่ต้องการตั้งแต่ต้นทาง

---

### 6. หลักการทำงานของ ACL ใน Lab นี้

เมื่อ Sales PC `172.16.1.100` telnet ไป `172.16.1.1`:

```text
Source: 172.16.1.100
Destination: 172.16.1.1
Protocol: TCP
Port: Telnet 23
```

Router จะ match rule แรก:

```text
deny tcp host 172.16.1.100 host 172.16.1.1 eq telnet
```

ผลคือ Telnet ไม่ผ่าน

เมื่อ Sales PC ping ไป `172.16.1.1`:

```text
Protocol: ICMP
```

จะไม่ match rule deny telnet เพราะไม่ใช่ TCP port 23 แต่จะ match:

```text
permit icmp any any
```

ผลคือ ping ผ่าน

เมื่อ Engineer PC `172.31.1.100` telnet ไป `172.31.1.1`:

```text
Source: 172.31.1.100
Destination: 172.31.1.1
Protocol: TCP
Port: Telnet 23
```

จะ match:

```text
permit tcp host 172.31.1.100 host 172.31.1.1 eq telnet
```

ผลคือ Telnet ผ่าน

---

### 7. คำสั่งตรวจสอบ

ตรวจสอบ ACL:

```text
Router# show access-lists 100
```

หรือ:

```text
Router# show ip access-lists 100
```

ตรวจสอบว่า ACL ถูกผูกกับ subinterface หรือยัง:

```text
Router# show running-config interface GigabitEthernet0/0/1.16
Router# show running-config interface GigabitEthernet0/0/1.31
```

ตรวจสอบ IP interface:

```text
Router# show ip interface brief
```

---

### 8. ผลการทดสอบที่ควรได้

| Source | Destination | Protocol/Application | ผลที่ควรได้ |
|---|---|---|---|
| Sales PC `172.16.1.100` | Router `172.16.1.1` | ICMP/Ping | ผ่าน |
| Sales PC `172.16.1.100` | Router `172.16.1.1` | Telnet | ไม่ผ่าน |
| Engineer PC `172.31.1.100` | Router `172.31.1.1` | ICMP/Ping | ผ่าน |
| Engineer PC `172.31.1.100` | Router `172.31.1.1` | Telnet | ผ่าน |

---

### 9. ข้อดีของ Extended ACL

- กรอง traffic ได้ละเอียดกว่า Standard ACL
- ระบุ source และ destination ได้
- ระบุ protocol ได้ เช่น TCP, UDP, ICMP
- ระบุ port ได้ เช่น Telnet, SSH, HTTP, HTTPS
- เหมาะกับ policy ที่ต้องการควบคุมเฉพาะ service
- ลดผลกระทบกับ traffic อื่น เพราะบล็อกเฉพาะสิ่งที่ต้องการ

ตัวอย่างเช่น ใน Lab นี้บล็อกเฉพาะ Telnet ของ Sales PC แต่ยังปล่อยให้ Ping ได้

---

### 10. ข้อเสียและข้อควรระวัง

- คำสั่งซับซ้อนกว่า Standard ACL
- ต้องระวังลำดับ rule เพราะ ACL อ่านจากบนลงล่าง
- ถ้าลืม permit traffic ที่จำเป็น traffic จะถูก implicit deny ทิ้ง
- ถ้าใส่ wildcard mask หรือ host ผิด อาจเปิดหรือบล็อกผิดเครื่อง
- Telnet ส่งข้อมูลแบบ clear text ในงานจริงควรใช้ SSH แทน

---

### 11. Extended ACL ควรวางตรงไหน

หลักทั่วไป:

```text
Extended ACL ควรวางใกล้ Source มากที่สุด
```

เหตุผลคือ Extended ACL ระบุได้ละเอียดว่า traffic ใดต้องถูกบล็อก จึงควรหยุด traffic ที่ไม่ต้องการตั้งแต่ต้นทาง เพื่อลดการวิ่งของ packet ที่ไม่จำเป็นใน network

ใน Lab นี้จึงผูก ACL แบบ `in` บน subinterface ที่รับ traffic จาก Sales และ Engineer:

```text
interface GigabitEthernet0/0/1.16
 ip access-group 100 in

interface GigabitEthernet0/0/1.31
 ip access-group 100 in
```

---

### 12. เปรียบเทียบ Standard ACL กับ Extended ACL

| หัวข้อ | Standard ACL | Extended ACL |
|---|---|---|
| ตรวจ Source IP | ได้ | ได้ |
| ตรวจ Destination IP | ไม่ได้ | ได้ |
| ตรวจ Protocol | ไม่ได้ | ได้ |
| ตรวจ Port | ไม่ได้ | ได้ |
| ตำแหน่งวางที่แนะนำ | ใกล้ Destination | ใกล้ Source |
| ตัวอย่างหมายเลข | `1-99`, `1300-1999` | `100-199`, `2000-2699` |
| เหมาะกับ | policy ง่าย ๆ | policy รายละเอียดสูง |

---

### 13. ข้อควรจำสำหรับสอบ CCNA

- Extended Numbered ACL ใช้หมายเลข `100-199` และ `2000-2699`
- Extended ACL ตรวจ source, destination, protocol และ port ได้
- ACL อ่าน rule จากบนลงล่าง
- เมื่อ match rule แล้วจะหยุดอ่านทันที
- ทุก ACL มี implicit deny อยู่ท้ายสุด
- ถ้าต้องการให้ ping ผ่าน ต้อง permit ICMP
- Extended ACL ควรวางใกล้ source
- `eq telnet` หมายถึง TCP port `23`
- `host x.x.x.x` เท่ากับ `x.x.x.x 0.0.0.0`

---

### 14. สรุปสั้น

Lab 30 สอนการใช้ **Extended Numbered ACL** เพื่อควบคุม traffic แบบละเอียด โดยใน topology นี้ใช้ ACL หมายเลข `100` เพื่อบล็อก Sales PC `172.16.1.100` ไม่ให้ Telnet เข้า Router `172.16.1.1`, อนุญาต Engineer PC `172.31.1.100` ให้ Telnet เข้า Router `172.31.1.1` และอนุญาต ICMP เพื่อให้ ping ได้ จุดสำคัญคือ Extended ACL สามารถระบุ source, destination, protocol และ port ได้ จึงควรวางใกล้ source และต้องระวัง implicit deny เสมอ
