# การสร้างเครือข่ายไร้สาย (Building a Wireless Network)

หากคุณมีคอมพิวเตอร์หลายเครื่องที่เชื่อมต่อกันในบ้านอยู่แล้ว คุณสามารถสร้างเครือข่ายไร้สายได้ด้วย **จุดเชื่อมต่อไร้สาย (Wireless Access Point)** แต่ถ้าคุณมีคอมพิวเตอร์หลายเครื่องที่ยังไม่ได้เชื่อมต่อกัน หรือต้องการเปลี่ยนระบบเครือข่าย **อีเทอร์เน็ต (Ethernet)** เดิม คุณจะต้องมี **เราเตอร์ไร้สาย (Wireless Router)**
(If you already have several computers networked in your home, you can create a wireless network with a wireless access point. If you have several computers that are not networked, or if you want to replace your ethernet network, you will need a wireless router.)

---

## 📦 เราเตอร์ไร้สายคืออะไร? (What is a Wireless Router?)

เราเตอร์ไร้สายเป็นอุปกรณ์รวมศูนย์ที่ประกอบด้วย:
(This is a single unit that contains:)

-   **พอร์ตเชื่อมต่อโมเด็ม:** สำหรับเชื่อมต่อกับโมเด็มสายเคเบิล (Cable) หรือ DSL
    (A port to connect to your cable or DSL modem)
-   **เราเตอร์ (Router):** หรือที่เรียกว่าเครือข่ายท้องถิ่นไร้สาย (Wireless Local Area Network - WLAN)
    (A router, also known as a wireless local area network)
-   **ฮับอีเทอร์เน็ต (Ethernet Hub):** สำหรับการเชื่อมต่อแบบใช้สาย
    (An ethernet hub)
-   **ไฟร์วอลล์ (Firewall):** เพื่อความปลอดภัยของเครือข่าย
    (A firewall)
-   **จุดเชื่อมต่อไร้สาย (Wireless Access Point):** สำหรับส่งสัญญาณไร้สาย
    (A wireless access point)

เราเตอร์ไร้สายช่วยให้คุณใช้สัญญาณไร้สายหรือสายอีเทอร์เน็ตเพื่อเชื่อมต่อคอมพิวเตอร์ อุปกรณ์เคลื่อนที่ เครื่องพิมพ์ และอินเทอร์เน็ตเข้าด้วยกัน เราเตอร์ส่วนใหญ่ครอบคลุมพื้นที่ประมาณ 100 ฟุต (30.5 เมตร) ในทุกทิศทาง แต่อาจถูกบดบังด้วยผนังและประตู หากบ้านของคุณมีขนาดใหญ่มาก คุณสามารถซื้อ **ตัวขยายสัญญาณ (Range Extenders)** หรือ **เครื่องทวนสัญญาณ (Repeaters)** เพื่อเพิ่มระยะการทำงานของเราเตอร์ได้
(A wireless router allows you to use wireless signals or ethernet cables to connect your devices. Most routers provide coverage for about 100 feet in all directions. If your home is very large, you can buy range extenders or repeaters to increase the range.)

---

## ⚙️ การตั้งค่าเราเตอร์ (Router Settings)

เช่นเดียวกับอะแดปเตอร์ไร้สาย เราเตอร์หลายรุ่นสามารถใช้มาตรฐาน 802.11 ได้มากกว่าหนึ่งมาตรฐาน โดยปกติเราเตอร์ 802.11n จะมีราคาถูกกว่าแต่ช้ากว่ามาตรฐาน 802.11ac หรือ 802.11ax (WiFi 6)
(As with wireless adapters, many routers can use more than one 802.11 standard. 802.11n routers are slower than 802.11ac or 802.11ax.)

เมื่อคุณเสียบปลั๊กเราเตอร์ มันจะเริ่มทำงานด้วยการตั้งค่าเริ่มต้น (Default settings) คุณสามารถใช้เว็บอินเตอร์เฟสเพื่อเปลี่ยนการตั้งค่าต่อไปนี้:
(Once you plug in your router, it should start working at its default settings. You can change these via a web interface:)

-   **ชื่อเครือข่าย (SSID):** ย่อมาจาก Service Set Identifier โดยปกติจะเป็นชื่อผู้ผลิต
    (The name of the network, known as its service set identifier.)
-   **ช่องสัญญาณ (Channel):** ส่วนใหญ่จะใช้ช่องสัญญาณ 6 เป็นค่าเริ่มต้น หากเพื่อนบ้านใช้ช่องเดียวกันอาจเกิดการรบกวน การเปลี่ยนไปใช้ช่องอื่นจะช่วยแก้ปัญหานี้ได้
    (The channel that the router uses. Switching to a different channel can eliminate interference from neighbors.)
-   **ตัวเลือกความปลอดภัย (Security Options):** ควรตั้งชื่อผู้ใช้งานและรหัสผ่านใหม่แทนที่ค่าเริ่มต้นจากโรงงาน
    (Your router's security options. It is a good idea to set your own username and password.)

---

## 🛡️ ความปลอดภัยของเครือข่าย (Network Security)

ความปลอดภัยเป็นสิ่งสำคัญมาก หากคุณเปิดเครือข่ายเป็นแบบสาธารณะ (Open hot spot) ใครก็ตามที่มีอุปกรณ์ไร้สายก็จะสามารถใช้สัญญาณของคุณได้
(Security is an important part of a home wireless network. If you create an open hot spot, anyone can use your signal.)

### วิวัฒนาการของความปลอดภัย (Security Evolution):

1.  **WEP (Wired Equivalency Privacy):** เคยเป็นมาตรฐานเดิมแต่ปัจจุบันไม่ปลอดภัยแล้ว เพราะแฮกเกอร์สามารถเจาะระบบได้ง่าย
    (WEP was once the standard but is now easy to compromise.)
2.  **WPA (WiFi Protected Access):** พัฒนาขึ้นมาแทน WEP โดยใช้การเข้ารหัส TKIP แต่ปัจจุบันก็ไม่ถือว่าปลอดภัยเพียงพอแล้ว
    (Succeeded WEP but is also no longer considered secure.)
3.  **WPA2:** เป็นมาตรฐานที่แนะนำในปัจจุบัน (Successor to WEP/WPA) โดยใช้การเข้ารหัส **AES (Advanced Encryption Standard)** ซึ่งมีความปลอดภัยสูงสุด
    (WPA2 is the recommended security standard. AES is considered the most secure.)
    -   *หมายเหตุ:* ฟีเจอร์ **WPS (WiFi Protected Setup)** อาจเป็นช่องโหว่ ควรปิดหากเป็นไปได้
        (WPS can create a vulnerability; consider turning it off.)
4.  **WPA3 (2018):** มาตรฐานใหม่ล่าสุดที่แก้ช่องโหว่ของ WPA2 โดยมีการเข้ารหัสที่ซับซ้อนขึ้นและเปลี่ยนไปตามเวลา
    (WPA3 solves vulnerabilities in WPA2 with more complex, time-changing encryption.)

### การกรองที่อยู่แมค (MAC Address Filtering):

เป็นการระบุว่าอุปกรณ์ใดบ้างที่ได้รับอนุญาตให้เข้าใช้เครือข่ายโดยอ้างอิงจาก **ที่อยู่แมค (MAC Address)** ซึ่งเป็นที่อยู่ทางกายภาพที่ไม่ซ้ำกันของแต่ละอุปกรณ์ อย่างไรก็ตาม ระบบนี้ก็ไม่ได้ป้องกันได้ 100% เพราะแฮกเกอร์สามารถปลอมแปลง (Spoof) ที่อยู่แมคได้
(MAC address filtering uses a computer's physical hardware address. It is not foolproof as hackers can "spoof" MAC addresses.)

---

## 💡 เคล็ดลับเพิ่มเติม (Additional Security Tips)

-   **เปลี่ยนชื่อ SSID:** อย่าใช้ชื่อเริ่มต้นเพื่อให้แฮกเกอร์ไม่ทราบรุ่นของเราเตอร์ที่ใช้อยู่
    (Change the SSID to something other than default.)
-   **ใช้รหัสผ่านที่คาดเดายาก:** (Selecting a strong password never hurts.)
-   **ปิดการจัดการระยะไกล (Remote Administration):** เพื่อให้เฉพาะคอมพิวเตอร์ที่เสียบสายตรงกับเราเตอร์เท่านั้นที่เปลี่ยนการตั้งค่าได้
    (Disable remote administration so only direct-plugged computers can change settings.)

เครือข่ายไร้สายติดตั้งได้ง่ายและราคาไม่แพง และเราเตอร์ส่วนใหญ่มีหน้าตั้งค่าที่เข้าใจง่าย
(Wireless networks are easy and inexpensive to set up, and most routers' web interfaces are virtually self-explanatory.)
