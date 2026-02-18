# สรุปโจทย์ Interview: Concurrency-Safe Order API

**เวลา:** 1 ชั่วโมง  
**ภาษา:** Golang  
**โฟกัส:** Transaction, Concurrency, Structure, Production Thinking

---

## โจทย์หลัก

พัฒนา REST API สำหรับระบบสั่งซื้อสินค้าแบบง่าย

---

## API Endpoints ที่ต้องทำ

### 1. Create Product
- **POST** `/products`
- **Request:** `{ "name": "Keyboard", "price": 1500, "stock": 10 }`
- **Response:** 201 Created พร้อม product data

### 2. List Products
- **GET** `/products`
- **Response:** 200 OK พร้อม array ของ products

### 3. Create Order
- **POST** `/orders`
- **Request:** `{ "product_id": 1, "quantity": 2 }`
- **Response:** 201 Created พร้อม order data (รวม total_price)

---

## Business Logic ที่ต้องทำ

1. **ห้ามสั่งซื้อถ้า stock ไม่พอ** → return error (409 Conflict)
2. **เมื่อสั่งซื้อ ต้องตัด stock ทันที** → stock ลดลงตาม quantity
3. **ป้องกัน race condition** → ยิง request พร้อมกันหลายครั้งต้องไม่ให้ stock ติดลบ

---