# WiFi คืออะไรและทำงานอย่างไร? (What is WiFi and How does it work!)

การเชื่อมต่ออินเทอร์เน็ตแบบไร้สาย หรือที่เรียกว่า **WiFi** หรือเครือข่ายมาตรฐาน **802.11**
(Wireless internet connection also known as WiFi or 802.11 networking)

---

## 📡 WiFi คืออะไร? (What is WiFi?)

เครือข่ายไร้สายใช้ **คลื่นวิทยุ (Radio waves)** ในความเป็นจริง การสื่อสารผ่านเครือข่าย WiFi นั้นคล้ายกับการสื่อสารด้วยวิทยุสื่อสารแบบสองทาง (Two-way radio communication)
(Wireless network uses radio waves. In fact, communication across a WiFi network is a lot like two-way radio communication.)

### ขั้นตอนการทำงาน (How it works):

1.  **การส่งข้อมูล (Transmission):** อะแดปเตอร์ไร้สายของคอมพิวเตอร์จะแปลงข้อมูลเป็นสัญญาณวิทยุและส่งออกไปโดยใช้เสาอากาศ
    (A computer's wireless adapter translates data into a radio signal and transmits it using an antenna.)![[Dipole_xmting_antenna_animation_4_408x318x150ms.gif]]
2.  **การรับข้อมูล (Reception):** เราเตอร์ไร้สายจะรับสัญญาณและถอดรหัส จากนั้นเราเตอร์จะส่งข้อมูลไปยังอินเทอร์เน็ตผ่านการเชื่อมต่อสาย LAN (Ethernet)
    (A wireless router receives the signal and decodes it. The router sends the information to the internet using a physical, wired ethernet connection.)

กระบวนการนี้ยังทำงานในทางกลับกันด้วย โดยเราเตอร์จะรับข้อมูลจากอินเทอร์เน็ต แปลงเป็นสัญญาณวิทยุ และส่งไปยังอะแดปเตอร์ไร้สายของคอมพิวเตอร์
(The process also works in reverse, with the router receiving information from the internet, translating it into a radio signal and sending it to the computer's wireless adapter.)

---

## 📻 วิทยุ WiFi ต่างจากวิทยุทั่วไปอย่างไร? (WiFi Radios vs. Other Radios)

วิทยุที่ใช้สำหรับการสื่อสาร WiFi นั้นคล้ายกับวิทยุสื่อสาร (Walkie-talkies), โทรศัพท์มือถือ และอุปกรณ์อื่นๆ มาก สามารถรับส่งคลื่นวิทยุและแปลงเลขฐานสอง ([1s and 0s](https://computer.howstuffworks.com/bytes.htm)) เป็นคลื่นวิทยุได้ แต่ WiFi มีความแตกต่างที่สำคัญบางประการ:
(The radios used for WiFi communication are very similar to those used for walkie-talkies and cell phones. They can convert 1s and 0s into radio waves and back, but have key differences:)

-   **ความถี่ (Frequencies):** WiFi ส่งสัญญาณที่ความถี่ **2.4 GHz** หรือ **5 GHz** ซึ่งสูงกว่าความถี่ที่ใช้ในโทรศัพท์มือถือหรือโทรทัศน์ ความถี่ที่สูงกว่าช่วยให้สัญญาณสามารถบรรจุข้อมูลได้มากขึ้น
    (They transmit at 2.4 GHz or 5 GHz. Higher frequency allows the signal to carry more data.)
-   **ความแตกต่างระหว่าง 2.4 GHz vs 5 GHz:**
    -   **2.4 GHz:** เริ่มล้าสมัยเพราะความเร็วต่ำกว่า แต่ยังนิยมใช้เพราะส่งสัญญาณได้ **ไกลกว่า** (หลายร้อยฟุต)
        (2.4 GHz is slower but has a longer range, several hundred feet.)
    -   **5 GHz:** มีระยะการส่งประมาณ 200 ฟุต (61 เมตร) และมักถูกรบกวนได้ง่ายจากผนังหรือสิ่งกีดขวาง แต่ให้ **ความเร็วที่สูงกว่า** มากสำหรับการเชื่อมต่อในระยะใกล้
        (5 GHz has a shorter range and is more prone to interference but is much faster for close connections.)

---

## 📊 มาตรฐานเครือข่าย 802.11 (802.11 Networking Standards)

WiFi ใช้มาตรฐาน 802.11 ซึ่งมีการพัฒนามาอย่างต่อเนื่องตลอดหลายทศวรรษ:
(WiFi uses 802.11 standards, which have evolved over the decades:)

### 1. 802.11b (1999)
-   **ความเร็ว:** สูงสุด 11 Mbps (Megabits per second)
-   **ความถี่:** 2.4 GHz
-   **เทคโนโลยี:** ใช้การมอดูเลตแบบ **CCK (Complementary Code Keying)** เพื่อเพิ่มความเร็ว
    (Uses CCK modulation to improve speeds.)

![[Pasted image 20260505104155.jpg]]
![[Pasted image 20260505104213.jpg]]

### 2. 802.11a (หลังจาก 802.11b)
-   **ความเร็ว:** สูงสุด 54 Mbps
-   **ความถี่:** 5 GHz
-   **เทคโนโลยี:** ใช้ **OFDM (Orthogonal Frequency-Division Multiplexing)** ซึ่งเป็นเทคนิคการเข้ารหัสที่มีประสิทธิภาพมากขึ้น โดยการแยกสัญญาณวิทยุออกเป็นสัญญาณย่อยหลายๆ สัญญาณเพื่อลดการรบกวน
    (Uses OFDM to split radio signals into sub-signals, greatly reducing interference.)

![[OFDM_Signal_-_Freq-Time_Representation_12-11-23_email_hero_2.webp]]
> **Orthogonal Frequency-Division Multiplexing (OFDM)**

### 3. 802.11g
-   **ความเร็ว:** สูงสุด 54 Mbps
-   **ความถี่:** 2.4 GHz
-   **จุดเด่น:** เร็วกว่า 11b เพราะใช้การเข้ารหัส OFDM แบบเดียวกับ 11a
    (Fast like 11a but operates on the 2.4 GHz band.)

### 4. 802.11n (2009)
-   **ความเร็ว:** สูงถึง 140 Mbps (ในทางปฏิบัติ) หรือ 150 Mbps ต่อหนึ่งสตรีม
-   **จุดเด่น:** รองรับการทำงานย้อนหลัง (Backward compatible) กับ a, b และ g ปรับปรุงความเร็วและระยะการส่งอย่างมาก โดยสามารถส่งข้อมูลได้สูงสุด 4 สตรีมพร้อมกัน
    (Significantly improved speed and range. Supports up to four streams of data.)

### 5. 802.11ac (2014)
-   **ความถี่:** 5 GHz เท่านั้น (ทำงานร่วมกับ n บน 2.4 GHz)
-   **ความเร็ว:** สูงสุด 450 Mbps ต่อหนึ่งสตรีม (สามารถเพิ่มได้ถึงระดับ Gigabit)
-   **ฉายา:** บางครั้งเรียกว่า **5G** (ตามย่านความถี่), **Gigabit WiFi** หรือ **Very High Throughput (VHT)**
    (Operates exclusively at 5 GHz, much faster and less prone to interference.)

### 6. 802.11ax (WiFi 6 - 2019)
-   **ความเร็ว:** สูงสุด 9.2 Gbps
-   **จุดเด่น:** ติดตั้งเสาอากาศได้มากขึ้น รองรับการเชื่อมต่อหลายอุปกรณ์พร้อมกันโดยไม่สะดุด บางอุปกรณ์รองรับย่านความถี่ **6 GHz** ซึ่งเร็วกว่า 5 GHz ประมาณ 20%
    (Allows higher data flow and multiple connections without interference.)

### 7. 802.11be (WiFi 7 - 2024)
-   **จุดเด่น:** ให้ระยะการส่งที่กว้างขึ้น การเชื่อมต่อที่มากขึ้น และอัตราการรับส่งข้อมูลที่รวดเร็วกว่ารุ่นก่อนหน้าทั้งหมด
    (Offering even better range, more connections, and faster data rates.)

---

## 🛠️ รายละเอียดเพิ่มเติม (Additional Details)

-   **การกระโดดความถี่ (Frequency Hopping):** วิทยุ WiFi สามารถส่งสัญญาณบนย่านความถี่ใดก็ได้ หรือ "กระโดด" ระหว่างย่านความถี่ต่างๆ อย่างรวดเร็ว เพื่อลดการรบกวนและช่วยให้หลายอุปกรณ์ใช้การเชื่อมต่อเดียวกันได้พร้อมกัน
    (WiFi radios can "frequency hop" to reduce interference and let multiple devices connect simultaneously.)
-   **ความหนาแน่นของเครือข่าย (Network Congestion):** หลายอุปกรณ์สามารถใช้ [router](https://computer.howstuffworks.com/router.htm) ตัวเดียวกันได้ แต่หากมีผู้ใช้งานแอปพลิเคชันที่ใช้แบนด์วิดท์สูงมากเกินไปพร้อมกัน อาจเกิดการรบกวนหรือหลุดจากการเชื่อมต่อได้ ซึ่งมาตรฐานใหม่อย่าง 802.11ax จะเข้ามาช่วยแก้ปัญหานี้
    (Multiple devices can use one router, but high-bandwidth usage by many users can cause interference, which newer standards like WiFi 6 help mitigate.)
