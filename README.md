# JobHeavy-Lowsalary

หน้า redirect สำหรับเลือก LAN ก่อน แล้วใช้ URL ของ ngrok ที่ Cloudflare Worker ส่งกลับ

- แสดงคำอธิบายก่อนเริ่มเชื่อมต่อ และตรวจ LAN เมื่อผู้ใช้กดปุ่มเท่านั้น ปุ่มเชื่อมต่อสำรองจะข้ามการตรวจ LAN
- ตรวจ LAN ภายใน 15 วินาทีเพื่อให้เวลากดอนุญาต และตรวจ Cloudflare ภายใน 5 วินาที รวมเวลาอ่าน response
- Worker ต้องส่ง URL แบบข้อความธรรมดา ใช้ HTTPS ไม่มี username/password, port พิเศษ, query หรือ hash
- รองรับ subdomain ของ `ngrok-free.app`, `ngrok-free.dev`, `ngrok.app`, `ngrok.dev` และ `ngrok.io`
- หากใช้โดเมนส่วนตัว ให้เพิ่ม hostname ใน `ALLOWED_TUNNEL_HOSTS` ของทั้งสองไฟล์ รายการ suffix อนุญาตทุก subdomain ของผู้ให้บริการ หากมี hostname คงที่ ควรระบุ hostname นั้นและล้าง `ALLOWED_TUNNEL_SUFFIXES`
- `index.html` และ `404.html` ต้องมีโค้ดเหมือนกัน เพื่อรองรับการเปิดลิงก์ไปยังหน้าย่อยโดยตรง

LAN ตรวจการเข้าถึง `/` ด้วย `no-cors` จึงไม่ต้องมี `/health` หรือ CORS header โดย response แบบ opaque ยืนยันได้เพียงว่าได้รับการตอบกลับ ไม่สามารถตรวจ HTTP status หรือยืนยันว่าระบบทำงานสมบูรณ์ได้

Worker ต้องอนุญาต CORS จาก origin ที่ใช้โฮสต์หน้านี้ เบราว์เซอร์ต้องรองรับ `targetAddressSpace` และอนุญาต Local Network Access สำหรับการตรวจ HTTP LAN จากหน้า HTTPS ควรทดสอบในเบราว์เซอร์ที่ใช้งานจริง

การแก้ไฟล์ในโฟลเดอร์นี้ยังไม่อัปเดต GitHub Pages ต้องนำ `index.html` และ `404.html` ไปเผยแพร่ใน repository ที่ใช้โฮสต์เว็บด้วย
