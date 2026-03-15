# ENGSE207 Software Architecture

## Final Lab — Set 1: Microservices + HTTPS + Lightweight Logging

---

# 1. รายวิชาและผู้จัดทำ

**รายวิชา:** ENGSE207 Software Architecture
**งาน:** Final Lab Set 1

**ผู้จัดทำ:** 67543210005-4 Sarisah Thawanwarasak

**Repository:** `final-lab/set1`

---

# 2. ภาพรวมของระบบ

โปรเจกต์นี้เป็นระบบ **Task Board แบบ Microservices** ที่ออกแบบเพื่อฝึกการพัฒนาสถาปัตยกรรมซอฟต์แวร์ โดยมีองค์ประกอบหลักดังนี้

* แยกระบบออกเป็นหลาย services
* ใช้ **Nginx เป็น API Gateway**
* เปิดใช้งาน **HTTPS ด้วย Self-Signed Certificate**
* ใช้ **JWT สำหรับ Authentication**
* ใช้ **Lightweight Logging Service**
* ใช้ **Docker Compose** สำหรับรันทุก service พร้อมกัน

ระบบนี้ **ไม่มีระบบสมัครสมาชิก (Register)** และใช้เฉพาะ **Seed Users**

---

# 3. Architecture Overview
```
Browser / Postman
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

Services ในระบบ

* nginx – API Gateway และ HTTPS
* frontend – หน้าเว็บ Task Board
* auth-service – Login และ JWT verification
* task-service – CRUD Tasks
* log-service – จัดเก็บและแสดง logs
* postgres – ฐานข้อมูล

---

# 4. โครงสร้างโปรเจกต์
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
│       ├── index.js
│       ├── routes/auth.js
│       ├── middleware/jwtUtils.js
│       └── db/db.js
│
├── task-service/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│       ├── index.js
│       ├── routes/tasks.js
│       ├── middleware/
│       │   ├── authMiddleware.js
│       │   └── jwtUtils.js
│       └── db/db.js
│
├── log-service/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│       └── index.js
│
├── db/
│   └── init.sql
│
├── scripts/
│   └── gen-certs.sh
│
└── screenshots/
    ├── 01_docker_running.png
    ├── 02_https_browser.png
    ├── 03_login_success.png
    ├── 04_login_fail.png
    ├── 05_create_task.png
    ├── 06_get_tasks.png
    ├── 07_update_task.png
    ├── 08_delete_task.png
    ├── 09_no_jwt_401.png
    ├── 10_logs_api.png
    ├── 11_rate_limit.png
    └── 12_frontend_screenshot.png
```

---

# 5. เทคโนโลยีที่ใช้

* Node.js
* Express.js
* PostgreSQL
* Docker
* Docker Compose
* Nginx
* JWT
* bcryptjs
* HTML / JavaScript

---

# 6. วิธีรันระบบ

## สร้าง certificate
```
chmod +x scripts/gen-certs.sh
./scripts/gen-certs.sh
```
---

## สร้าง .env

ตัวอย่าง
```
POSTGRES_DB=taskboard
POSTGRES_USER=admin
POSTGRES_PASSWORD=secret123

JWT_SECRET=engse207-secret
JWT_EXPIRES=1h
```

---

## สร้าง bcrypt hash
```
node -e "const b=require('bcryptjs'); console.log(b.hashSync('alice123',10))"
```
นำ hash ไปใส่ใน
```
db/init.sql
```
---

## รันระบบ
```
docker compose down -v
docker compose up --build
```
---

# 7. การเข้าใช้งาน

Frontend

https://localhost

Log Dashboard

https://localhost/logs.html

เนื่องจากใช้ self-signed certificate browser อาจแสดง warning ให้กดยอมรับก่อนเข้าใช้งาน

---

# 8. Seed Users

| Username | Email | Password | Role |   
| alice | [alice@lab.local](mailto:alice@lab.local) | alice123 | member |   
| bob | [bob@lab.local](mailto:bob@lab.local) | bob456 | member |   
| admin | [admin@lab.local](mailto:admin@lab.local) | adminpass | admin |   

---

# 9. API Summary

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

# 11. Screenshots

screenshots ที่แนบ

* docker running
* https browser
* login success
* create task
* update task
* delete task
* logs dashboard

---

# 12. ปัญหาที่พบ

* bcrypt hash ไม่ตรงทำให้ login ไม่ได้
* nginx route ไม่ตรง path ของ service
* docker volume ทำให้ seed users ไม่อัปเดต

---

# 13. ข้อจำกัดของระบบ

* ใช้ self-signed certificate
* ใช้ shared database
* ไม่มีระบบ register
* logging เป็น lightweight

---

# 14. การต่อยอด

สามารถพัฒนาต่อในอนาคต เช่น

* เพิ่ม register API
* แยก database per service
* deploy บน cloud
