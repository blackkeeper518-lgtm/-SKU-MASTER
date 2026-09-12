# วิธีดึงและรัน Drakside System รุ่นล่าสุดในเครื่อง

รุ่นล่าสุดที่รวมฟีเจอร์เดิมและระบบ Canonical/Alien คือ Commit `debc7ff` ชื่อ `restore full feature baseline with canonical alien pipeline`.

## วิธีที่ 1: ดึงจาก GitHub หลัง Push สำเร็จ

เปิด Terminal แล้วรัน:

```bash
git clone https://github.com/kumairoo102-cmyk/drakside-system.git drakside-system
cd drakside-system
git checkout debc7ff
pnpm install
pnpm exec tsc --noEmit
pnpm run build
pnpm run dev
```

จากนั้นเปิด URL ที่ Vite แสดงใน Terminal โดยปกติคือ `http://localhost:5173`.

## วิธีที่ 2: ใช้โฟลเดอร์ที่ Commit ไว้แล้วก่อน Push

ถ้ายังไม่ได้ Push GitHub ให้คัดลอกโฟลเดอร์นี้ไปยังเครื่องของคุณ:

```text
/home/ubuntu/work/chatfix/drakside-system-publish
```

โฟลเดอร์นี้เป็นชุดเว็บเต็มที่มี Commit `debc7ff` อยู่แล้ว ไม่ต้องแตก ZIP เพราะไม่มี ZIP ให้แตก:

```bash
cd drakside-system-publish
pnpm install
pnpm exec tsc --noEmit
pnpm run dev
```

ถ้าต้องการทดสอบ Production build ให้รัน:

```bash
pnpm run build
pnpm start
```

## ตั้งค่า Supabase

เว็บรุ่นนี้ใช้ Supabase แบบ Browser Direct และอ่านค่าจากหน้า `/connect`.

หลังเปิดเว็บ ให้เข้า:

```text
http://localhost:5173/connect
```

กรอกค่าคนละช่อง:

```text
Supabase URL:
https://<ชื่อโปรเจกต์>.supabase.co

Supabase Anon/Publishable Key:
<คีย์ public จริง>
```

ห้ามใช้ `service_role_key` ในเว็บ และห้ามใส่ค่า placeholder เช่น `https://supabase.co` หรือ `your-anon-key`.

กดบันทึก แล้วกลับหน้าเว็บ ระบบจะเก็บ URL และ Anon Key ไว้ใน `localStorage` ของ Browser เครื่องนั้น.

## หน้าหลักของรุ่นล่าสุด

```text
/chats              ห้องแชท
/daily-chat-summary สรุปออเดอร์จากแชทรายวัน
/orders             Order Control
/order-history      ประวัติออเดอร์
/order-buckets      แยกออเดอร์ตามจำนวน
/order-performance  ประสิทธิภาพออเดอร์
/stock-room         คลังสินค้า
/mapping-dashboard  ภาพรวม Mapping
/aliases            ห้องเรียนรู้ Alias
/alien-room         Alien Inbox ตรวจคำดิบและผลแมป
/secret-gallery     ห้องลับคลังรูป
/connect            ตั้งค่าฐาน Supabase
```

## ทดสอบสาย Alien และสร้างออเดอร์จากแชท

1. เข้า `/connect` และบันทึก Supabase URL กับ Anon/Publishable Key
2. เข้า `/daily-chat-summary`
3. เลือกวันที่และห้องแชทที่มี COD/ข้อมูลสินค้า
4. กด `สร้าง Canonical`
5. ระบบอ่านข้อความจาก `chat_customer_messages` และ `chat_page_messages`
6. ระบบอ่าน Alias จาก `product_map_master`
7. ระบบจับกับ `product_master`
8. ระบบ Upsert หัวออเดอร์เข้า `canonical_orders`
9. ระบบ Upsert รายการเข้า `canonical_order_items`
10. เข้า `/alien-room` แล้วกดรีเฟรชเพื่อตรวจคำดิบ, SKU, Display และสถานะ `MATCHED`/`REVIEW`

## หากต้องแก้เฉพาะหน้าเดียว

แก้ไฟล์ในโฟลเดอร์:

```text
client/src/pages/
```

ตัวอย่าง:

```text
client/src/pages/ChatHub.tsx
client/src/pages/DailyChatSummary.tsx
client/src/pages/AlienRoom.tsx
client/src/pages/StockRoom.tsx
```

ถ้าต้องแก้การอ่าน Supabase หรือ Mapping ให้ดู:

```text
client/src/lib/canonical.ts
client/src/lib/chat-to-canonical.ts
```

หลังแก้ทุกครั้งให้รัน:

```bash
pnpm exec tsc --noEmit
pnpm run build
git add .
git commit -m "update requested page"
git push origin main
```

## สรุปคำสั่งสั้นที่สุด

```bash
git clone https://github.com/kumairoo102-cmyk/drakside-system.git drakside-system
cd drakside-system
git checkout debc7ff
pnpm install
pnpm run dev
```

จากนั้นเปิด `http://localhost:5173/connect` เพื่อต่อ Supabase ก่อนใช้งานทุกห้อง.
