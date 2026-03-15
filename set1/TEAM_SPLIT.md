## Team Members
- 67543210005-4	นางสาวสาริศา  ถวัลย์วราศักดิ์

---

## Work Allocation

### รับผิดชอบการพัฒนาระบบทั้งหมด ได้แก่

- พัฒนา Auth Service สำหรับระบบ login และ JWT authentication
- พัฒนา Task Service สำหรับการจัดการ tasks (create, read, update, delete)
- พัฒนา Log Service สำหรับบันทึก activity logs
- พัฒนา Frontend (HTML, CSS, JavaScript)
- เชื่อมต่อ Frontend กับ Backend APIs
- ตั้งค่า PostgreSQL database
- ตั้งค่า Docker และ Docker Compose
- ตั้งค่า Nginx reverse proxy และ HTTPS certificate
- ทดสอบระบบแบบ end-to-end
- จัดทำ README และ screenshots สำหรับการส่งงาน

---

## Shared Responsibilities

เนื่องจากโปรเจคนี้ดำเนินการโดยสมาชิกเพียงคนเดียว จึงไม่มีการแบ่งงานระหว่างสมาชิก โดยผู้จัดทำรับผิดชอบทุกส่วนของระบบตั้งแต่การออกแบบ การพัฒนา ไปจนถึงการทดสอบ

---

## Reason for Work Split

เนื่องจากโปรเจคนี้มีสมาชิกเพียง 1 คน จึงไม่ได้มีการแบ่งงานตามบุคคล แต่ใช้การแบ่งงานตาม **Service Architecture** ของระบบ ได้แก่

- Auth Service
- Task Service
- Log Service
- Frontend
- Infrastructure (Docker, Nginx)

การแบ่งงานในลักษณะนี้ช่วยให้สามารถพัฒนาและทดสอบแต่ละ service ได้เป็นขั้นตอน

---

## Integration Notes

การทำงานของระบบมีขั้นตอนดังนี้

1. ผู้ใช้เข้าสู่ระบบผ่านหน้า Frontend
2. Frontend ส่ง request ไปยัง Auth Service
3. Auth Service ตรวจสอบข้อมูลผู้ใช้และสร้าง JWT token
4. Frontend ใช้ JWT token ในการเรียก API ของ Task Service
5. Task Service จัดการข้อมูล tasks ใน PostgreSQL
6. Log Service บันทึก activity logs ของระบบ
7. Nginx ทำหน้าที่เป็น reverse proxy และจัดการ HTTPS

ทุก services ทำงานร่วมกันผ่าน Docker Compose
