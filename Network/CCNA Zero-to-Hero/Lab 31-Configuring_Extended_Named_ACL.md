![](images/Pasted%20image%2020260518153644.png)

**รูปที่ 1: เริ่มสร้าง Extended Named ACL ชื่อ `RA_ACL_IN`**  
เข้า global configuration mode แล้วใช้คำสั่ง `ip access-list extended RA_ACL_IN` เพื่อสร้าง ACL แบบ Extended Named ACL จากนั้น Router จะเข้าสู่โหมด `config-ext-nacl` สำหรับใส่ rule แบบละเอียด เช่น protocol, source, destination และ port

![](images/Pasted%20image%2020260518153708.png)

**รูปที่ 2: สร้าง rule เพื่อบล็อก Telnet จาก Sales PC**  
กำหนด rule `deny tcp host 172.16.1.100 host 172.16.1.1 eq 23` เพื่อบล็อก Sales PC `172.16.1.100` ไม่ให้ Telnet เข้า Router gateway `172.16.1.1` โดย `eq 23` คือ TCP port ของ Telnet

![](images/Pasted%20image%2020260518153739.png)

**รูปที่ 3: อนุญาต ICMP เพื่อให้ Ping ได้**  
ใส่ rule `permit icmp any any` เพื่อให้ traffic ICMP ผ่านได้ ทำให้ PC ยังสามารถ ping ตรวจสอบการเชื่อมต่อได้ แม้ Telnet บางรายการจะถูกบล็อก

![](images/Pasted%20image%2020260518153805.png)

**รูปที่ 4: อนุญาต Telnet จาก Engineer PC**  
กำหนด rule `permit tcp host 172.31.1.100 host 172.31.1.1 eq 23` เพื่ออนุญาต Engineer PC `172.31.1.100` ให้ Telnet เข้า Router gateway `172.31.1.1` ได้

![](images/Pasted%20image%2020260518153827.png)

**รูปที่ 5: ผูก ACL `RA_ACL_IN` เข้ากับ Subinterface แบบ Inbound**  
นำ ACL ไปใช้กับ `GigabitEthernet0/0/1.16` และ `GigabitEthernet0/0/1.31` ด้วยคำสั่ง `ip access-group RA_ACL_IN in` เพื่อให้ Router กรอง traffic ตั้งแต่ตอน packet เข้ามาจากแต่ละ VLAN

![](images/Pasted%20image%2020260518153844.png)

**รูปที่ 6: ตรวจสอบว่า ACL ถูกผูกกับ Subinterface แล้ว**  
ใน running-config จะเห็นว่า subinterface VLAN 16 และ VLAN 31 มีคำสั่ง `ip access-group RA_ACL_IN in` แปลว่า ACL ถูกเปิดใช้งานกับ traffic ขาเข้าแล้ว

![](images/Pasted%20image%2020260518153909.png)

**รูปที่ 7: ตรวจสอบรายละเอียด Extended Named ACL**  
แสดง config ของ `ip access-list extended RA_ACL_IN` โดยมี rule บล็อก Sales Telnet, permit ICMP และ permit Engineer Telnet ตาม policy ที่ต้องการ

---

## สรุปความรู้: LAB 31 - Configuring Extended Named ACL

### 1. Lab 31 คืออะไร

Lab 31 เป็นการฝึกตั้งค่า **Extended Named ACL** บน Cisco Router เพื่อควบคุม traffic แบบละเอียดโดยใช้ชื่อ ACL แทนหมายเลข

Lab 30 ใช้ Extended Numbered ACL:

```text
access-list 100 deny tcp host 172.16.1.100 host 172.16.1.1 eq telnet
```

แต่ Lab 31 ใช้ Extended Named ACL:

```text
ip access-list extended RA_ACL_IN
 deny tcp host 172.16.1.100 host 172.16.1.1 eq telnet
```

ข้อดีคือชื่อ `RA_ACL_IN` ช่วยสื่อว่าเป็น ACL สำหรับกรอง traffic ขาเข้า ทำให้อ่าน config ง่ายกว่า ACL ที่ใช้แค่หมายเลข

---

### 2. IP Address และ Policy ของ Lab

Topology ของ Lab นี้ใช้เครือข่ายเดียวกับ Lab 30:

| อุปกรณ์/เครือข่าย | IP Address |
|---|---|
| Sales PC0 | `172.16.1.100/24` |
| Engineer PC1 | `172.31.1.100/24` |
| Router subinterface VLAN 16 | `172.16.1.1/24` |
| Router subinterface VLAN 31 | `172.31.1.1/24` |
| Sales network | `172.16.1.0/24` |
| Engineer network | `172.31.1.0/24` |

เป้าหมายของ ACL:

```text
บล็อก Sales PC 172.16.1.100 ไม่ให้ Telnet เข้า 172.16.1.1
อนุญาต Engineer PC 172.31.1.100 ให้ Telnet เข้า 172.31.1.1
อนุญาต ICMP เพื่อให้ Ping ได้
```

---

### 3. Extended Named ACL คืออะไร

**Extended Named ACL** คือ ACL แบบ Extended ที่ใช้ชื่อแทนหมายเลข และสามารถกรอง traffic ได้ละเอียด เช่น:

- Source IP
- Destination IP
- Protocol เช่น TCP, UDP, ICMP
- Port เช่น Telnet `23`, SSH `22`, HTTP `80`, HTTPS `443`

รูปแบบคำสั่ง:

```text
ip access-list extended <ACL_NAME>
 permit|deny <protocol> <source> <destination> <operator> <port>
```

ตัวอย่างใน Lab:

```text
ip access-list extended RA_ACL_IN
 deny tcp host 172.16.1.100 host 172.16.1.1 eq telnet
 permit icmp any any
 permit tcp host 172.31.1.100 host 172.31.1.1 eq telnet
```

---

### 4. คำสั่งที่ใช้ใน Lab

สร้าง Extended Named ACL:

```text
Router(config)# ip access-list extended RA_ACL_IN
Router(config-ext-nacl)# deny tcp host 172.16.1.100 host 172.16.1.1 eq 23
Router(config-ext-nacl)# permit icmp any any
Router(config-ext-nacl)# permit tcp host 172.31.1.100 host 172.31.1.1 eq 23
Router(config-ext-nacl)# exit
```

ความหมายของ rule:

| Rule | ความหมาย |
|---|---|
| `deny tcp host 172.16.1.100 host 172.16.1.1 eq 23` | บล็อก Sales PC ไม่ให้ Telnet เข้า gateway ของ Sales |
| `permit icmp any any` | อนุญาต ping/ICMP ทั้งหมด |
| `permit tcp host 172.31.1.100 host 172.31.1.1 eq 23` | อนุญาต Engineer PC ให้ Telnet เข้า gateway ของ Engineer |

หมายเหตุ: `eq 23` และ `eq telnet` มีความหมายเดียวกัน คือ TCP port 23

---

### 5. ผูก ACL กับ Subinterface

ใน Lab นี้ใช้ router-on-a-stick จึงมี subinterface แยก VLAN:

```text
GigabitEthernet0/0/1.16
GigabitEthernet0/0/1.31
```

นำ ACL ไปใช้แบบ inbound:

```text
Router(config)# interface GigabitEthernet0/0/1.16
Router(config-subif)# ip access-group RA_ACL_IN in

Router(config)# interface GigabitEthernet0/0/1.31
Router(config-subif)# ip access-group RA_ACL_IN in
```

เหตุผลที่ใช้ `in`:

- traffic จาก PC เข้ามาที่ Router ทาง subinterface
- Router ตรวจ ACL ก่อนรับ traffic ไปประมวลผลต่อ
- Extended ACL ควรวางใกล้ source เพื่อหยุด traffic ที่ไม่ต้องการตั้งแต่ต้นทาง

---

### 6. การทำงานของ ACL ใน Lab นี้

เมื่อ Sales PC `172.16.1.100` telnet ไป `172.16.1.1`:

```text
Source: 172.16.1.100
Destination: 172.16.1.1
Protocol: TCP
Port: 23
```

จะ match rule:

```text
deny tcp host 172.16.1.100 host 172.16.1.1 eq 23
```

ผลคือ Telnet ไม่ผ่าน

เมื่อ Sales PC หรือ Engineer PC ใช้ ping:

```text
Protocol: ICMP
```

จะ match rule:

```text
permit icmp any any
```

ผลคือ ping ผ่าน

เมื่อ Engineer PC `172.31.1.100` telnet ไป `172.31.1.1`:

```text
Source: 172.31.1.100
Destination: 172.31.1.1
Protocol: TCP
Port: 23
```

จะ match rule:

```text
permit tcp host 172.31.1.100 host 172.31.1.1 eq 23
```

ผลคือ Telnet ผ่าน

---

### 7. คำสั่งตรวจสอบ

ตรวจสอบ ACL:

```text
Router# show access-lists RA_ACL_IN
```

หรือ:

```text
Router# show ip access-lists RA_ACL_IN
```

ตรวจสอบ running-config ของ ACL:

```text
Router# show running-config | section ip access-list
```

ตรวจสอบว่า ACL ถูกผูกกับ subinterface:

```text
Router# show running-config interface GigabitEthernet0/0/1.16
Router# show running-config interface GigabitEthernet0/0/1.31
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

### 9. ข้อดีของ Extended Named ACL

- อ่านง่ายกว่า Extended Numbered ACL เพราะใช้ชื่อสื่อ policy
- แก้ไข rule ง่ายกว่า โดยเฉพาะเมื่อใช้ sequence number
- ระบุ source, destination, protocol และ port ได้ละเอียด
- เหมาะกับ policy ที่ต้องควบคุม application เฉพาะ เช่น Telnet, SSH, HTTP
- ลดความสับสนจากการจำหมายเลข ACL

---

### 10. ข้อเสียและข้อควรระวัง

- คำสั่งยาวและละเอียด ต้องระวังพิมพ์ source/destination สลับกัน
- ต้องระวังลำดับ rule เพราะ ACL อ่านจากบนลงล่าง
- ถ้าลืม permit traffic ที่ต้องการ จะถูก implicit deny ทิ้ง
- ถ้าใช้ Telnet ในงานจริงไม่ปลอดภัย เพราะส่งข้อมูลแบบ clear text ควรใช้ SSH แทน
- ชื่อ ACL ควรตั้งให้สื่อความหมาย เช่น `RA_ACL_IN` ไม่ควรตั้งชื่อกำกวม

---

### 11. เปรียบเทียบ Lab 30 กับ Lab 31

| หัวข้อ | Lab 30 | Lab 31 |
|---|---|---|
| ประเภท ACL | Extended Numbered ACL | Extended Named ACL |
| การอ้างอิง ACL | ใช้หมายเลข `100` | ใช้ชื่อ `RA_ACL_IN` |
| ตรวจ Source IP | ได้ | ได้ |
| ตรวจ Destination IP | ได้ | ได้ |
| ตรวจ Protocol/Port | ได้ | ได้ |
| อ่าน config | อ่านยากกว่า | อ่านง่ายกว่า |
| การผูก interface | `ip access-group 100 in` | `ip access-group RA_ACL_IN in` |

---

### 12. ข้อควรจำสำหรับสอบ CCNA

- Extended Named ACL ใช้คำสั่ง `ip access-list extended <name>`
- Extended ACL ตรวจ source, destination, protocol และ port ได้
- ACL อ่านจากบนลงล่าง และหยุดเมื่อ match rule แรก
- ทุก ACL มี implicit deny อยู่ท้ายสุด
- ถ้าต้องการให้ ping ผ่าน ต้อง `permit icmp`
- `eq 23` คือ Telnet port 23
- `host x.x.x.x` เท่ากับ `x.x.x.x 0.0.0.0`
- Extended ACL ควรวางใกล้ source
- ต้องผูก ACL กับ interface ด้วย `ip access-group <name> in/out`

---

### 13. สรุปสั้น

Lab 31 สอนการใช้ **Extended Named ACL** โดยใช้ชื่อ `RA_ACL_IN` เพื่อควบคุม traffic แบบละเอียด ใน Lab นี้บล็อก Sales PC `172.16.1.100` ไม่ให้ Telnet เข้า Router `172.16.1.1`, อนุญาต ICMP ให้ ping ได้ และอนุญาต Engineer PC `172.31.1.100` ให้ Telnet เข้า Router `172.31.1.1` ได้ จุดสำคัญคือ Extended Named ACL อ่านง่ายกว่า numbered ACL และยังควบคุม source, destination, protocol และ port ได้ครบ
