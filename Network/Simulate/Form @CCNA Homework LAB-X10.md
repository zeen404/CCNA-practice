# CCNA Homework LAB-X10: Inter-VLAN Routing (Router-on-a-Stick)

**รูปที่ 1:** แผนผังเครือข่าย (Topology)
![[Pasted image 20260424135425.png]]

**รูปที่ 2:** คำสั่งเปิดพอร์ต Physical
![[Pasted image 20260424135743.png]]

**รูปที่ 3:** การสร้าง Sub-Interface สำหรับแต่ละ VLAN
![[Pasted image 20260424135805.png]]

**รูปที่ 4:** ตรวจสอบพอร์ตและตารางเส้นทาง (Routing Table)
![[Pasted image 20260424135815.png]]

**รูปที่ 5:** ทดสอบ Ping ข้ามวงเครือข่าย
![[Pasted image 20260424135824.png]]

---

## 🧠 Technical Deep Dive: Why & How?

### 1. Breaking the VLAN Barrier
- **Why:** VLAN ถูกสร้างมาเพื่อแยก Layer 2 ออกจากกันเด็ดขาด (เครื่อง VLAN 10 จะคุยกับ VLAN 20 ไม่ได้เลย) หากต้องการให้คุยกันได้ เราต้องมีอุปกรณ์ **Layer 3 (Router)** มาทำหน้าที่ตัดสินใจส่งข้อมูลข้ามวง
- **How (Router-on-a-Stick):** แทนที่จะต้องใช้สายแลนหลายเส้น (1 เส้นต่อ 1 VLAN) เราใช้สายเส้นเดียวที่ทำเป็น **Trunk** แล้วไปแบ่ง "อินเทอร์เฟซเสมือน" (Sub-interface) บน Router แทน

### 2. Encapsulation dot1Q
- **Mechanism:** Router ต้องได้รับการสอนให้เข้าใจ "ภาษา Tag" (802.1Q) ของ Switch ผ่านคำสั่ง `encapsulation dot1Q [VLAN-ID]` เพื่อให้มันรู้ว่า Packet ที่วิ่งมาในพอร์ตย่อยนี้เป็นของแผนกไหน

### 📨 Packet Flow (The "Hairpin" Journey)
1. **PC1 (VLAN 10)** ส่ง Packet ไปหา **PC2 (VLAN 20)**:
   - Packet ถูกส่งไปที่ **Default Gateway (Router Sub-interface g0/0.10)**
   - Switch แปะ Tag VLAN 10 แล้วส่งขึ้นสาย Trunk ไปหา Router
2. **Router Processing:**
   - Router รับเฟรม -> เห็น Tag 10 -> ส่งเข้า Sub-interface 10
   - แกะ Tag ออก -> ดู IP ปลายทาง -> พบว่าต้องส่งไปวง VLAN 20
   - ตรวจสอบ Routing Table -> พบว่าทางออกคือ Sub-interface g0/0.20
3. **The Return Flow:**
   - Router **ห่อ Tag ใหม่เป็น VLAN 20** -> ส่งกลับลงมาที่ Switch ทางสายเดิม
   - Switch รับมา -> เห็น Tag 20 -> ส่งออกพอร์ตที่เป็นของ PC2
**ผลลัพธ์:** ข้อมูลวิ่งขึ้นและลงในสายเส้นเดียวเหมือนเส้นผม (Hairpin) จึงเป็นที่มาของชื่อ Router-on-a-Stick
