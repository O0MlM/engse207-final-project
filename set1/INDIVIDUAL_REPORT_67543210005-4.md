# INDIVIDUAL REPORT

## Student Information

Name: นางสาวสาริศา  ถวัลย์วราศักดิ์   
Student ID: 67543210005-4	  
Course: ENGSE207 Software Architecture   

---

## Responsibility

ในโปรเจคนี้ ข้าพเจ้ารับผิดชอบการพัฒนาระบบทั้งหมด ได้แก่

- Backend services (Auth, Task, Log)
- Frontend development
- Database configuration
- Docker container setup
- Nginx reverse proxy configuration
- Integration และ testing

---

## Implementation Work

สิ่งที่ได้ลงมือพัฒนา ได้แก่

- พัฒนา Auth Service สำหรับ authentication และ JWT token
- พัฒนา Task Service สำหรับการจัดการ tasks
- พัฒนา Log Service สำหรับบันทึก activity logs
- พัฒนา Frontend ด้วย HTML, CSS และ JavaScript
- เชื่อมต่อ Frontend กับ Backend ผ่าน REST API
- ตั้งค่า PostgreSQL database
- ตั้งค่า Docker Compose สำหรับ deploy services
- ตั้งค่า Nginx สำหรับ reverse proxy และ HTTPS

---

## Problems Encountered

ปัญหาที่พบระหว่างการพัฒนา ได้แก่

- การเชื่อมต่อ API ระหว่าง frontend และ backend
- การจัดการ JWT token และ authentication
- ปัญหา JavaScript error เช่น `apiFetch is not defined`
- การตั้งค่า nginx routing ให้ถูกต้อง
- การตั้งค่า Docker networking ระหว่าง services

---

## Solutions

แนวทางในการแก้ไขปัญหา ได้แก่

- ตรวจสอบ API requests ผ่าน browser developer tools
- ปรับโครงสร้าง JavaScript functions
- แก้ไข nginx configuration
- ใช้ Docker Compose เพื่อจัดการ services ทั้งหมด
- ทดสอบระบบแบบ end-to-end
- bcrypt hash ไม่ตรง ทำให้ login ไม่สำเร็จ
- nginx route path ไม่ตรงกับ service endpoint
- docker volume ทำให้ seed users ไม่อัปเดต
- HTTPS certificate warning ใน browser

---

## Lessons Learned

จากโปรเจคนี้ ได้เรียนรู้เกี่ยวกับ

- Microservices architecture
- REST API communication
- JWT authentication
- Docker และ containerization
- Nginx reverse proxy
- การ deploy ระบบหลาย services

---

## Future Improvements (Set 2)

หากมีการพัฒนาต่อใน Set 2 ต้องการปรับปรุงดังนี้

- ปรับปรุง UI/UX ของ frontend
- เพิ่ม validation และ error handling
- เพิ่ม monitoring และ logging
- เพิ่มระบบ role-based access control
