# Live Coding Interview: Concurrency-Safe Order API

**เวลา:** 1 ชั่วโมง  
**Topic:** Concurrency-Safe Order API (Golang)  
**โฟกัส:** Backend เป็นหลัก - วัดเรื่อง transaction, concurrency, structure, และการคิดแบบ production

---

## 🎯 โจทย์

ให้พัฒนา REST API สำหรับ "ระบบสั่งซื้อสินค้าแบบง่าย" โดยมีเงื่อนไขดังนี้:

---

## 📦 Requirements

### 1️⃣ API Endpoints ที่ต้องทำ

#### ✅ 1. Create Product
```
POST /products
```

**Request Body:**
```json
{
  "name": "Keyboard",
  "price": 1500,
  "stock": 10
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "name": "Keyboard",
  "price": 1500,
  "stock": 10
}
```

#### ✅ 2. List Products
```
GET /products
```

**Response (200 OK):**
```json
[
  {
    "id": 1,
    "name": "Keyboard",
    "price": 1500,
    "stock": 10
  },
  {
    "id": 2,
    "name": "Mouse",
    "price": 500,
    "stock": 20
  }
]
```

#### ✅ 3. Create Order
```
POST /orders
```

**Request Body:**
```json
{
  "product_id": 1,
  "quantity": 2
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "product_id": 1,
  "quantity": 2,
  "total_price": 3000,
  "created_at": "2024-01-15T10:30:00Z"
}
```

### 📌 Business Logic สำคัญ

- ❗ **ห้ามสั่งซื้อถ้า stock ไม่พอ** - ต้อง return error ที่เหมาะสม
- ❗ **เมื่อลูกค้าสั่งซื้อ ต้องตัด stock ทันที** - stock ต้องลดลงตาม quantity ที่สั่ง
- ❗ **ต้องป้องกันปัญหา race condition** - เช่น ยิง request พร้อมกัน 5 ครั้ง ต้องไม่ให้ stock ติดลบ

---

## 🗄 Database Schema

ให้ออกแบบเอง หรือใช้โครงสร้างนี้เป็น hint:

### Product Table
```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    stock INTEGER NOT NULL DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Order Table
```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    product_id INTEGER NOT NULL REFERENCES products(id),
    quantity INTEGER NOT NULL,
    total_price DECIMAL(10, 2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**หมายเหตุ:** สามารถปรับ schema ได้ตามความเหมาะสม แต่ต้องรองรับ business logic ที่กำหนด


---

## 📚 Additional Resources

- [Go Database/SQL Tutorial](https://go.dev/doc/database/)
- [PostgreSQL Transactions](https://www.postgresql.org/docs/current/tutorial-transactions.html)
- [SELECT FOR UPDATE](https://www.postgresql.org/docs/current/sql-select.html#SQL-FOR-UPDATE-SHARE)

---

**Good luck! 🚀**
