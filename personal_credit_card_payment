สถานการณ์ **สั่งซื้อบริการ เช่นสั่งซื้อจาก Odoo.com โดยกรรมการ A ชำระด้วยบัตรเครดิตส่วนตัว (บัญชีในผังเป็น `112003 บัตรเครดิตกรรมการ A`) แล้วบริษัทเบิกคืนภายหลัง** 
(A) แนวทางทางบัญชีแบบ Journal Entries ที่ชัดเจน 2 วิธี (วิธีที่แนะนำกับวิธีสำรอง) 
(B) คำแนะนำการตั้งค่าและขั้นตอนใช้งานใน **Odoo** ทีละขั้นตอน 
(C) ข้อควรระวัง เอกสารประกอบ และรายงานที่ควรตรวจสอบ

---

# A. สรุปสองวิธีบันทึกบัญชี (ตัวอย่างตัวเลขสมมติ)

สมมติว่าบริการจาก Odoo.com ค่าใช้จ่ายรวม 10,000.00 บาท (ไม่มี VAT)
(ปรับค่าย่อย/สกุลเงินได้ตามจริง)

## วิธีที่ 1 — **วิธีแนะนำ (Preferred)**

> บันทึกว่าเจ้าหนี้ถูกชำระโดยกรรมการ → บริษัทคงหนี้ต่อกรรมการ (บัญชีเจ้าหนี้กรรมการ) → เบิกคืนจากธนาคารเมื่อคืนเงิน

**บัญชีที่เกี่ยวข้อง (ตัวอย่างรหัส)**

* ค่าใช้จ่าย (Service Expense) = `5xxxx`
* เจ้าหนี้ผู้ขาย (Odoo vendor) = `2xxxx` (ใช้บัญชีเจ้าหนี้ตามผังเดิม)
* เจ้าหนี้กรรมการ / เงินทดรองกรรมการ = แนะนำสร้าง `212010 Payable - Director (A)` (ถ้ายังไม่มี ให้เพิ่ม)
* ธนาคารหลัก = `112002` (Bank THB)

**รายการเมื่อบันทึกบิลผู้ขาย (Vendor Bill)**

```
(เมื่อได้รับ Vendor Bill จาก Odoo.com)
Dr 5xxxx   ค่าใช้จ่ายบริการ                        10,000
  Cr 2xxxx  เจ้าหนี้ - Odoo vendor                  10,000
```

**รายการเมื่อกรรมการจ่ายบิลแทน (บันทึกว่าเจ้าหนี้ถูกชำระโดยกรรมการ)**

```
(เมื่อบันทึกว่าผู้ขายได้รับชำระแล้ว โดยกรรมการจ่าย)
Dr 2xxxx  เจ้าหนี้ - Odoo vendor                   10,000
  Cr 212010 เจ้าหนี้กรรมการ (Payable - Director A) 10,000
```

> ความหมาย: ผู้ขายถูกปิดหนี้ แต่บริษัทเป็นผู้ติดหนี้ต่อกรรมการ (ต้องคืนเงินให้กรรมการ)

**รายการเมื่อบริษัทเบิกจ่ายคืนกรรมการ (จ่ายจากบัญชีบริษัท)**

```
(เมื่อจ่ายเงินคืนกรรมการด้วยการโอนจากบัญชีบริษัท)
Dr 212010 เจ้าหนี้กรรมการ (Payable - Director A)  10,000
  Cr 112002 บัญชีพักของธนาคาร (Bank - Main THB)   10,000
```

---

## วิธีที่ 2 — **ใช้บัญชีบัตรเครดิตกรรมการ (`112003`) เป็น Clearing Account**

> เหมาะกรณีคุณอยากติดตามค่าใช้จ่ายแยกในบัญชีบัตรของกรรมการ (tracking) — ให้ `112003` เป็นบัญชีชั่วคราวที่แสดงยอดที่กรรมการจ่ายไว้

**รายการเมื่อกรรมการจ่ายจริง (บันทึกทันทีโดยไม่ต้องสร้าง vendor payment ก่อน)**

```
Dr 5xxxx   ค่าใช้จ่ายบริการ                       10,000
  Cr 112003 บัตรเครดิตกรรมการ A (liability)       10,000
```

**รายการเมื่อบริษัทเบิกจ่ายคืนกรรมการ (โอนเงินคืนจากธนาคารบริษัท)**

```
Dr 112003 บัตรเครดิตกรรมการ A                     10,000
  Cr 112002 บัญชีพักของธนาคาร                       10,000
```

> ความหมาย: `112003` แสดงว่ากรรมการชำระแทนบริษัทเป็นหนี้ที่บริษัทต้องคืน เมื่อบริษัทจ่ายคืน จะเคลียร์บัญชีนี้

**เปรียบเทียบสองวิธี**

* วิธี 1 บันทึก vendor bill → ปิด vendor → สร้างหนี้ต่อกรรมการ เหมาะกับการรักษาประวัติ vendor และการจับคู่กับ bill/credit note ได้ชัด
* วิธี 2 ง่ายกว่า เหมาะถ้าการจ่ายเป็นรายการเล็กและต้องการติดตามแยกตามบัตรกรรมการโดยตรง แต่ถ้ามี VAT / supplier invoice แบบเป็นหลักฐาน ควรใช้ วิธี 1 เพื่อให้ vendor bill และการตรวจสอบตรงตามเอกสาร

ผมแนะนำใช้ **วิธีที่ 1 (Preferred)** เป็นหลัก สำหรับกรณีจ่ายบริการ Odoo.com เพราะเป็น vendor ที่มีเอกสารชัดเจน และช่วยให้การตรวจสอบบัญชี/ภาษีสมบูรณ์

---

# B. Step-by-Step ใน Odoo (แยกเป็น 2 ขั้นตอนหลักตามที่ขอ)

## หมวด 1 — ขั้นตอน: **การจ่ายด้วยบัตรเครดิตกรรมการ (กรรมการ A ชำระจริง)**

### การตั้งค่าเบื้องต้น (ทำครั้งเดียว)

1. **สร้าง Vendor สำหรับ Odoo.com** (Contacts → Suppliers)

   * ตรวจสอบ VAT/Tax ID หากมี
2. **สร้าง Internal Vendor / Partner สำหรับ Director A** (Contacts → Vendors)

   * ตั้งประเภทเป็น *Contact Type = Vendor* หรือ *Other* เพื่อใช้ reconciliation ในกรณีจำเป็น
3. **เพิ่มบัญชี Payable - Director** (หากยังไม่มี)

   * Accounting → Chart of Accounts → เพิ่ม `212010 Payable - Director (A)` (ประเภท = Current Liability / Payable)
4. **สร้าง Journal สำหรับ Director Card (Optional)**

   * Accounting → Configuration → Journals → สร้าง Journal ชื่อ `Card - Director A`
   * Type: Bank (หรือ Misc) — Default Account: `112003` (ถ้าต้องการใช้แบบ Clearing Account)

### ขั้นตอนปฏิบัติเมื่อกรรมการจ่ายบัตร

(สมมติมี Vendor Bill จาก Odoo.com หรือ Invoice จากระบบ)

#### 1) บันทึก Vendor Bill (สร้างบิลกับ Odoo.com)

* Accounting → Vendor Bills → Create

  * Supplier: Odoo.com
  * Date, Due date, Description, Amount = 10,000
  * Assign expense account (5xxxx) ให้กับบรรทัดรายการ
  * Save / Validate (สร้าง Bill จริงในระบบ)

--> ระบบจะสร้างบัญชีเจ้าหนี้ `2xxxx` (Odoo vendor)

#### 2) บันทึกการชำระโดยกรรมการ (ใน Odoo ลงเป็นการชำระแทน)

* บนหน้า Vendor Bill → ปุ่ม **Register Payment**

  * Payment Journal: เลือก `Card - Director A` (Journal ที่ตั้งไว้ โดย Default Account ชี้ไปที่ 112003) หรือเลือก Payment Method ที่สื่อความว่าจ่ายโดยกรรมการ
  * Amount: เต็มจำนวน (หรือเท่าที่ชำระ)
  * กด Validate

**ผลลัพธ์บัญชี (ตามวิธี 1 แบบที่แนะนำ)**

* Odoo จะบันทึกว่าเจ้าหนี้ถูกจ่าย แต่เครดิตจะไปลงบัญชี `112003` (ถ้า Journal ตั้งค่า default account เป็น 112003) หรือถ้าคุณต้องการให้ลงเป็น `212010` ให้ตั้ง Journal ให้เครดิต `212010` แทน — วิธีที่เหมาะสมคือให้เครดิตเป็น `212010` (เจ้าหนี้กรรมการ) เพื่อสะท้อนว่าเป็นหนี้ต่อกรรมการ

> วิธีปฏิบัติที่ชัดเจน: ตั้ง Journal `Card - Director A` ให้ Default Credit Account = `212010` (Payable - Director) — เมื่อ Register Payment จะ:
>
> * Dr AP (เจ้าหนี้ผู้ขาย) — ปิดบิลผู้ขาย
> * Cr 212010 — บันทึกหนี้ต่อกรรมการ

#### 3) เก็บเอกสารหลักฐาน

* แนบ Receipt / Payment confirmation จากบัตรเครดิตกรรมการ (ภาพ/ PDF) เข้าไปที่ Vendor Bill (Attachment)
* บันทึก Approver ใน Notes / Internal Notes

---

## หมวด 2 — ขั้นตอน: **การเบิกจ่ายคืนบัตรเครดิต (บริษัทคืนเงินให้กรรมการ)**

### ขั้นตอนปฏิบัติเมื่อบริษัทจะคืนเงินให้กรรมการ

(บริษัทโอนคืนจากบัญชีธนาคารบริษัท `112002`)

#### 1) สร้าง Bank Payment (ทำการจ่ายคืน)

* Accounting → Bank → Create Payment (หรือ Accounting → Vendors → Make Payment on Vendor)?

  * Partner: Director A (use the internal vendor/partner created)
  * Payment Journal: `Bank - Main THB` (Journal ที่ผูกกับบัญชี `112002`)
  * Amount: 10,000
  * Memo: “Reimbursement for Odoo.com subscription paid by Director A on DD/MM/YYYY”
  * Validate/Confirm

**รายการบัญชีที่เกิดขึ้น (เมื่อจ่ายคืน):**

```
Dr 212010 เจ้าหนี้กรรมการ (Payable - Director A)   10,000
  Cr 112002 บัญชีพักของธนาคาร (Bank - Main THB)    10,000
```

> ถ้าคุณใช้วิธีที่ 2 (ใช้ 112003 clearing account) จะเป็น:
>
> * Dr 112003 (clear) / Cr 112002 (bank) — เคลียร์ยอดใน 112003

#### 2) แนบเอกสารการโอน (Payment slip / Bank statement)

* แนบภาพสลิป / PDF เข้าสู่ Payment record และผูกกับ Vendor Bill (ถ้าทำ link ได้) เพื่อ audit trail

#### 3) Reconcile (กระทบยอดเงิน)

* Accounting → Reconciliation → Reconcile Bank Statement with Payment → ตรวจสอบว่าจำนวนที่ออกจาก Bank statement ตรงกับ Payment ที่บันทึก

---

# C. การตั้งค่าใน Odoo ที่แนะนำ (สรุปเป็นรายการทำงาน)

1. **Contacts**

   * สร้าง Supplier “Odoo.com” (Vendor)
   * สร้าง Partner “Director A (Reimbursements)” — ใช้เป็น vendor/internal partner สำหรับ reimburse

2. **Accounts**

   * สร้าง/ยืนยัน `212010 Payable - Director (A)` (Current Liability) — ถ้ายังไม่มี
   * ยืนยัน `112003` ถูกตั้งเป็น Clearing / Credit Card account (ประเภท = Current Liability ถ้าต้องการ)
   * ยืนยัน `112002` เป็น Bank THB (type = Bank)

3. **Journals**

   * สร้าง `Card - Director A` (Journal type Bank/Misc) → Default Credit Account = `212010` (หรือ `112003` ถ้าต้องการใช้ clearing)
   * ตั้ง `Bank - Main THB` (Payment Journal) ให้ผูกกับ `112002`

4. **Workflows / Approval**

   * ตั้งนโยบายต้องมีการอนุมัติค่าใช้จ่ายก่อน reimburse (Approvals)
   * กำหนดเอกสารที่ต้องแนบ: invoice/screenshot payment confirmation, ใบเสร็จ, รายงานจาก Odoo.com

5. **Multi-Currency / Taxes**

   * หากจ่ายเป็นสกุลอื่น (USD) ให้ตั้ง Currency และอัตราแลกเปลี่ยนใน Odoo
   * จัดการ FX gain/loss เมื่อเกิดขึ้น (บัญชี 999001/999002)

6. **Security**

   * จำกัดสิทธิ์ให้เฉพาะฝ่ายบัญชีสามารถ Validate Payment และ Reconcile

---

# D. ตัวอย่าง Complete Journal Entries (สรุป)

**ตัวอย่าง (วิธีแนะนำ) — จำนวน 10,000 บาท**

1. รับใบแจ้งหนี้จาก Odoo.com

```
Dr 5xxxx (Service Expense)           10,000
  Cr 2xxxx (AP — Odoo vendor)         10,000
```

2. กรรมการ A ชำระด้วยบัตรส่วนตัว → บันทึกว่าเจ้าหนี้ได้รับชำระแล้ว แต่บริษัทติดหนี้กรรมการ

```
Dr 2xxxx (AP — Odoo vendor)          10,000
  Cr 212010 (Payable - Director A)    10,000
```

3. บริษัทคืนเงินให้กรรมการ (Bank → Director)

```
Dr 212010 (Payable - Director A)     10,000
  Cr 112002 (Bank - Main THB)         10,000
```

---

# E. ข้อควรระวัง / Best practices

* **หลักฐานต้องครบ** — เก็บใบเสร็จ/Invoice จาก Odoo.com + Payment confirmation จากบัตรกรรมการ + Bank slip ตอนบริษัทโอนคืน
* **นโยบาย reimburse ชัดเจน** — ระบุวงเงิน อนุมัติผู้มีอำนาจ และเอกสารประกอบ
* **แยกบัญชีสำหรับติดตาม** — หากกรรมการหลายคน ให้สร้าง Payable per Director หรือใช้ analytic account / tags เพื่อแยกตามบุคคล
* **ภาษี** — ถ้าการซื้อมี VAT/Tax หรือต้องนำไปเป็นต้นทุนที่มีผลต่อ VAT input ให้แน่ใจว่ามี invoice ที่รับได้ตามกฎหมาย
* **Audit trail** — ใน Odoo แนบเอกสารให้ครบและใช้ Chatter/Notes เพื่อบันทึกเหตุผลและการอนุมัติ

---

# F. รายงานที่ควรตรวจสอบเป็นประจำ

* Aged Payables — ดูยอดเจ้าหนี้รวมและแต่ละ Vendor
* Vendor Ledger — ดูประวัติการจ่าย Odoo.com
* GL / Journal Items — ตรวจสอบรายการที่เชื่อมกับ `212010`, `112003`, `112002`
* Bank Reconciliation Report — ยืนยันการคืนเงินให้กรรมการและการกระทบยอด statement
* Expense Reimbursements Report — ดูรายการ reimburse ที่รอดำเนินการ / ที่จ่ายแล้ว

---
