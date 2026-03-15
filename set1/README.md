# ENGSE207 Software Architecture

## Final Lab — Set 1: Secure Microservices Task Board

---

# 1. Course Information

**รายวิชา:** ENGSE207 Software Architecture
**งาน:** Final Lab Set 1

**ผู้จัดทำ:** 67543210005-4 Sarisah Thawanwarasak

**Repository:** 
---
**Course:** ENGSE207 Software Architecture
**Assignment:** Final Lab — Set 1
**Topic:** Microservices Architecture with HTTPS, JWT Authentication, and Logging

**Student:**
67543210005-4
Sarisah Thawanwarasak

**Repository:**
`final-lab/set1`

---

# 2. System Overview

โปรเจกต์นี้เป็นระบบ **Task Board แบบ Microservices** ที่ออกแบบเพื่อฝึกการพัฒนาสถาปัตยกรรมซอฟต์แวร์ที่ปลอดภัยและสามารถแยกส่วนการทำงานได้อย่างชัดเจน

ระบบประกอบด้วยหลาย services ที่ทำงานร่วมกันผ่าน **API Gateway (Nginx)** และใช้ **HTTPS** เพื่อเข้ารหัสการสื่อสารทั้งหมด

ฟีเจอร์หลักของระบบ

* Login ด้วย **JWT Authentication**
* สร้าง / แก้ไข / ลบ Tasks
* Reverse Proxy ด้วย **Nginx**
* ใช้ **HTTPS (Self-Signed Certificate)**
* มี **Logging Service** สำหรับบันทึกเหตุการณ์
* รันทุก service ผ่าน **Docker Compose**

หมายเหตุ
ระบบนี้ **ไม่มี Register API** และใช้ **Seed Users** สำหรับทดสอบเท่านั้น

---

# 3. System Architecture

ระบบถูกออกแบบตามแนวคิด **Microservices Architecture**

```
Browser / Postman
<<<<<<< HEAD
│
│ HTTPS :443
▼
Nginx (API Gateway)
├─ /api/auth/* → auth-service
├─ /api/tasks/* → task-service
├─ /api/logs/* → log-service
└─ / → frontend

▼

PostgreSQL Database
```

```
        │
        │ HTTPS :443
        ▼
   Nginx (API Gateway)
        │
        ├── /api/auth/*  → auth-service
        ├── /api/tasks/* → task-service
        ├── /api/logs/*  → log-service
        └── /            → frontend
                │
                ▼
          PostgreSQL
```

Services ในระบบ

| Service      | Description                     |
| ------------ | ------------------------------- |
| nginx        | Reverse proxy และ HTTPS gateway |
| frontend     | หน้าเว็บ Task Board             |
| auth-service | Authentication และ JWT          |
| task-service | CRUD operations สำหรับ tasks    |
| log-service  | บันทึกและแสดง logs              |
| postgres     | ฐานข้อมูลของระบบ                |

---

# 4. Repository Structure

```
final-lab/set1/
├── README.md
├── docker-compose.yml
├── .env.example
├── .gitignore
│
├── nginx/
│   ├── nginx.conf
│   ├── Dockerfile
│   └── certs/
│       ├── cert.pem
│       └── key.pem
│
├── frontend/
│   ├── Dockerfile
│   ├── index.html
│   └── logs.html
│
├── auth-service/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│
├── task-service/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│
├── log-service/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│
├── db/
│   └── init.sql
│
├── scripts/
│   └── gen-certs.sh
│
└── screenshots/
```

---

# 5. Technologies Used

* Node.js
* Express.js
* PostgreSQL
* Docker
* Docker Compose
* Nginx
* JWT (jsonwebtoken)
* bcryptjs
* HTML / JavaScript

---

# 6. HTTPS Configuration

ระบบใช้ **Self-Signed Certificate** สำหรับการเข้ารหัสการสื่อสาร

สร้าง certificate ด้วย script

```
chmod +x scripts/gen-certs.sh
./scripts/gen-certs.sh
```

เมื่อเปิดผ่าน browser จะพบข้อความ

```
Your connection is not private
net::ERR_CERT_AUTHORITY_INVALID
```

เนื่องจาก certificate ไม่ได้ออกโดย Trusted CA
ซึ่งเป็น behavior ปกติสำหรับ development environment

---

# 7. Running the System

## 1. Create .env file

ตัวอย่าง

```
POSTGRES_DB=taskboard
POSTGRES_USER=admin
POSTGRES_PASSWORD=secret123

JWT_SECRET=engse207-secret
JWT_EXPIRES=1h
```

---

## 2. Generate bcrypt password hashes

ตัวอย่าง

```
node -e "const b=require('bcryptjs'); console.log(b.hashSync('alice123',10))"
```

นำ hash ที่ได้ไปแทนค่าใน

```
db/init.sql
```

---

## 3. Run the system

```
docker compose down -v
docker compose up --build
```

เมื่อ container ทำงานแล้ว สามารถเข้าใช้งานได้ที่

```
https://localhost
```

---

# 8. Seed Users

<<<<<<< HEAD
| Username | Email | Password | Role |   
|----------|-------|----------|------|    
| alice | [alice@lab.local](mailto:alice@lab.local) | alice123 | member |   
| bob | [bob@lab.local](mailto:bob@lab.local) | bob456 | member |   
| admin | [admin@lab.local](mailto:admin@lab.local) | adminpass | admin |   
=======
| Username | Email                                     | Password  | Role   |
| -------- | ----------------------------------------- | --------- | ------ |
| alice    | [alice@lab.local](mailto:alice@lab.local) | alice123  | member |
| bob      | [bob@lab.local](mailto:bob@lab.local)     | bob456    | member |
| admin    | [admin@lab.local](mailto:admin@lab.local) | adminpass | admin  |

Password จะถูก hash ด้วย **bcrypt** ก่อนเก็บในฐานข้อมูล
>>>>>>> 71cba75 (Update Set1)

---

# 9. Security Design

## HTTPS

Nginx ทำหน้าที่เป็น HTTPS gateway

Browser → HTTPS → Nginx → Services

ทุก request จาก client จะถูกเข้ารหัสก่อนเข้าสู่ระบบ

---

## JWT Authentication

หลัง login สำเร็จ

Auth Service จะสร้าง JWT token

```
Authorization: Bearer <token>
```

Task Service และ Log Service จะตรวจสอบ token ก่อนอนุญาตให้ใช้งาน

---

## Role-based Access Control

| Role   | Permissions     |
| ------ | --------------- |
| member | ใช้งาน Task API |
| admin  | ดู logs ของระบบ |

---

# 10. Logging System

ทุก service สามารถส่ง log ไปยัง **log-service**

ตัวอย่างเหตุการณ์ที่ถูกบันทึก

* login success / failure
* create task
* update task
* delete task
* unauthorized request

Admin สามารถดู logs ผ่าน

```
/api/logs
```

หรือผ่านหน้า

```
https://localhost/logs.html
```

---

# 11. API Summary

Auth Service

```
POST /api/auth/login
GET /api/auth/verify
GET /api/auth/me
```

Task Service

```
GET /api/tasks
POST /api/tasks
PUT /api/tasks/:id
DELETE /api/tasks/:id
```

Log Service

```
POST /api/logs/internal
GET /api/logs
GET /api/logs/stats
```
---

# 10. การทดสอบระบบ

ขั้นตอนการทดสอบ

1 รัน docker compose   
2 เปิด https://localhost   
3 login ด้วย seed users   
4 สร้าง task   
5 แก้ไข task   
6 ลบ task   
7 ทดสอบ request ที่ไม่มี JWT → ต้องได้ 401   
8 ตรวจสอบ logs    

---

# 12. Test Cases

ระบบถูกทดสอบตามข้อกำหนดใน assignment

| Test | Description                           |
| ---- | ------------------------------------- |
| T1   | Docker compose run successfully       |
| T2   | HTTPS access with certificate warning |
| T3   | Login success                         |
| T4   | Login fail                            |
| T5   | Create task                           |
| T6   | Get tasks                             |
| T7   | Update task                           |
| T8   | Delete task                           |
| T9   | Request without JWT → 401             |
| T10  | Admin access logs                     |
| T11  | Rate limiting on login                |

หลักฐาน screenshots อยู่ในโฟลเดอร์

```
/screenshots
```

---

# 13. Screenshots

ตัวอย่าง screenshots ที่แนบ

* Docker containers running
* HTTPS browser warning
* Login success
* Login failure
* Create task
* Get tasks
* Update task
* Delete task
* Unauthorized request
* Log dashboard
* Rate limit response

---

# 14. Problems Encountered

ปัญหาที่พบระหว่างพัฒนา

* bcrypt hash ไม่ตรง ทำให้ login ไม่สำเร็จ
* nginx route path ไม่ตรงกับ service endpoint
* docker volume ทำให้ seed users ไม่อัปเดต
* HTTPS certificate warning ใน browser

---

# 15. Known Limitations

ข้อจำกัดของระบบ

* ใช้ self-signed certificate
* database ยังเป็น shared database
* ไม่มีระบบ register
* logging เป็น lightweight

---

# 16. Future Improvements

แนวทางพัฒนาต่อ

* เพิ่ม register API

* แยก database per service   
* deploy บน cloud   
* แยก database ต่อ service   
* เพิ่ม refresh token   
* เพิ่ม monitoring dashboard   
* deploy บน cloud environment   
