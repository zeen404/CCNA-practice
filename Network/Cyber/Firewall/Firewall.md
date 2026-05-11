# ไฟร์วอลล์คืออะไร? (What is a Firewall?)
![[Pasted image 20260506134741.png]]

> [!abstract] สรุปจากวิดีโอ (Summary from: [What is a Firewall?](https://www.youtube.com/watch?v=dRLzldiMRhs))
> ไฟร์วอลล์คือระบบความปลอดภัยของเครือข่ายที่ทำหน้าที่ตรวจสอบและควบคุมทราฟฟิก (Traffic) ที่เข้าและออกจากเครือข่ายตามกฎความปลอดภัยที่กำหนดไว้ เปรียบเสมือน "ยามเฝ้าประตู" ระหว่างเครือข่ายภายในที่เชื่อถือได้กับอินเทอร์เน็ตภายนอกที่ไม่ปลอดภัย
> (A firewall is a network security system that monitors and controls incoming and outgoing network traffic based on predetermined security rules. It acts as a "gatekeeper" between a trusted internal network and untrusted external networks like the internet.)

---

## 🛡️ การทำงานของไฟร์วอลล์ (How a Firewall Works)

ไฟร์วอลล์ทำหน้าที่เป็นปราการด่านแรก (First line of defense) โดยการวิเคราะห์ **แพ็กเก็ตข้อมูล (Data Packets)** ที่พยายามจะผ่านเข้ามาในเครือข่าย
(A firewall acts as the first line of defense by analyzing data packets that attempt to enter the network.)

### สิ่งที่ไฟร์วอลล์ตรวจสอบ (What it inspects):
-   **ที่อยู่ IP ต้นทางและปลายทาง (Source & Destination IP):** ข้อมูลมาจากไหนและจะไปที่ไหน
    (Where the data is coming from and where it is going.)
-   **หมายเลขพอร์ต (Port Numbers):** บริการหรือช่องทางที่ข้อมูลใช้ (เช่น Port 80 สำหรับเว็บ)
    (The specific service or "door" being used, such as Port 80 for web traffic.)
-   **กฎการเข้าถึง (Access Control Lists - ACL):** ไฟร์วอลล์จะตัดสินใจว่าจะ **อนุญาต (Allow)** หรือ **บล็อก (Block)** ข้อมูลตามกฎที่ตั้งไว้
    (Firewalls use rules to decide whether to Allow or Block a packet based on set criteria.)

---

## 🏗️ ประเภทของไฟร์วอลล์แบ่งตามการติดตั้ง (Form Factors)

ในวิดีโอระบุประเภทหลักๆ ไว้ 2 รูปแบบคือ:
(The video identifies two primary forms:)

### 1. ไฟร์วอลล์แบบโฮสต์ (Host-based Firewall)
-   **ลักษณะ:** เป็นซอฟต์แวร์ที่ติดตั้งลงในคอมพิวเตอร์แต่ละเครื่อง (เช่น Windows Firewall)
    (Software installed on individual computers.)
-   **ข้อดี:** ปกป้องเครื่องนั้นๆ โดยเฉพาะ ไม่ว่าเครื่องจะไปเชื่อมต่อกับเครือข่ายใด
    (Protects that specific device regardless of which network it joins.)

### 2. ไฟร์วอลล์แบบเครือข่าย (Network-based Firewall)
-   **ลักษณะ:** เป็นอุปกรณ์ฮาร์ดแวร์ที่วางไว้ระหว่างเครือข่ายภายในกับอินเทอร์เน็ต (มักรวมอยู่ใน Router)
    (Hardware appliance placed between the local network and the internet.)
-   **ข้อดี:** ปกป้องอุปกรณ์ทุกเครื่องที่อยู่ในเครือข่ายนั้นพร้อมกัน
    (Protects all devices within the entire network at once.)

---

## 🔍 ประเภทของเทคโนโลยีไฟร์วอลล์ (Types of Firewall Technology)

เพื่อให้เข้าใจลึกซึ้งขึ้น นี่คือเทคโนโลยีต่างๆ ที่ไฟร์วอลล์ใช้:
(To better understand, here are the various technologies firewalls use:)

| ประเภท (Type) | คำอธิบาย (Description) |
| :--- | :--- |
| **Packet Filtering** | ตรวจสอบข้อมูลพื้นฐาน เช่น IP และ Port เป็นระดับที่เร็วที่สุดแต่ความปลอดภัยต่ำสุด (Inspects basic info like IP/Port; fastest but least secure.) |
| **Stateful Inspection** | ตรวจสอบสถานะการเชื่อมต่อว่าเป็นการสื่อสารที่ต่อเนื่องและได้รับอนุญาตจริงหรือไม่ (Tracks the state of active connections to ensure valid communication.) |
| **Proxy / Application Layer** | ทำหน้าที่เป็นตัวกลางตรวจสอบข้อมูลในระดับแอปพลิเคชัน (เช่น HTTP) เพื่อความปลอดภัยสูงสุด (Acts as a middleman, inspecting data at the application level for high security.) |
| **Next-Generation (NGFW)** | รวมความสามารถของไฟร์วอลล์แบบเดิมเข้ากับการตรวจจับไวรัส (Antivirus) และการป้องกันการบุกรุก (IPS) (Combines traditional firewall features with antivirus and Intrusion Prevention Systems.) |

---

## 🎭 เทคนิคการหลบเลี่ยงไฟร์วอลล์ (Firewall Evasion Techniques)

ผู้โจมตีมักใช้วิธีการต่างๆ เพื่อข้ามผ่านการตรวจสอบของไฟร์วอลล์เพื่อเข้าถึงเครือข่ายเป้าหมาย
(Attackers use various methods to bypass firewall inspections and gain access to target networks.)

### 1. การแบ่งส่วนแพ็กเก็ต (IP Fragmentation)
-   **English:** Breaking a large data packet into smaller pieces. Some firewalls only inspect the first fragment, allowing malicious code in subsequent fragments to pass through.
-   **Thai:** การแบ่งแพ็กเก็ตข้อมูลขนาดใหญ่ออกเป็นส่วนย่อยๆ ไฟร์วอลล์บางตัวอาจตรวจสอบเฉพาะส่วนแรกเท่านั้น ทำให้โค้ดอันตรายที่อยู่ในส่วนถัดๆ ไปสามารถหลุดลอดเข้าไปได้

### 2. การทำอุโมงค์ข้อมูล (Tunneling / Encapsulation)
-   **English:** Hiding restricted traffic (e.g., SSH) inside a permitted protocol like HTTP (Port 80) or DNS (Port 53).
-   **Thai:** การนำข้อมูลที่ถูกสั่งห้าม (เช่น SSH) ไปซ่อนไว้ในโปรโตคอลที่ไฟร์วอลล์อนุญาต เช่น HTTP (พอร์ต 80) หรือ DNS (พอร์ต 53)

### 3. การปลอมแปลงหมายเลข IP (IP Spoofing)
-   **English:** Modifying the source IP address to make the packet appear as if it is coming from a trusted internal source.
-   **Thai:** การแก้ไขหมายเลข IP ต้นทาง เพื่อหลอกไฟร์วอลล์ว่าข้อมูลมาจากแหล่งข้อมูลภายในที่เชื่อถือได้

### 4. การพรางข้อมูล (Obfuscation / Encoding)
-   **English:** Encoding malicious commands (e.g., Base64, URL Encoding) to hide the attack's signature from simple keyword-based detection.
-   **Thai:** การแปลงรหัสคำสั่งอันตราย (เช่น Base64 หรือ URL Encoding) เพื่อซ่อนรูปแบบการโจมตีจากการตรวจจับด้วยคำสำคัญพื้นฐาน

### 5. การใช้การเข้ารหัส (Encryption)
-   **English:** Using SSL/TLS (HTTPS) to hide traffic. Without Deep Packet Inspection (DPI), a firewall cannot see what is inside the encrypted tunnel.
-   **Thai:** การใช้การเข้ารหัส SSL/TLS (HTTPS) เพื่อบังทราฟฟิก หากไม่มีระบบ DPI ไฟร์วอลล์จะไม่สามารถมองเห็นข้อมูลที่อยู่ภายในอุโมงค์ที่เข้ารหัสได้

---

## 💡 ทำไมไฟร์วอลล์จึงสำคัญ? (Why is it Essential?)

-   **ป้องกันการเข้าถึงที่ไม่ได้รับอนุญาต (Prevents Unauthorized Access):** หยุดแฮกเกอร์ไม่ให้แอบเข้ามาในระบบ
    (Stops hackers from gaining entry to private systems.)
-   **บล็อกข้อมูลที่เป็นอันตราย (Blocks Malicious Traffic):** คัดกรองมัลแวร์และไวรัสก่อนถึงเครื่องผู้ใช้
    (Filters out threats, malware, and viruses before they reach your computer.)
-   **ความเป็นส่วนตัว (Privacy):** ป้องกันไม่ให้ข้อมูลสำคัญถูกส่งออกไปภายนอกโดยไม่ได้รับอนุญาต
    (Prevents sensitive data from being sent out without permission.)

> [!warning] ข้อควรจำ
> ไฟร์วอลล์เป็นเพียงส่วนหนึ่งของความปลอดภัย แม้จะมีไฟร์วอลล์ที่ดี แต่หากผู้ใช้คลิกลิงก์อันตรายในอีเมลเอง ระบบก็อาจถูกโจมตีได้
> (A firewall is a critical component but not a complete solution. It cannot stop threats if a user manually bypasses security, such as clicking a malicious link.)
