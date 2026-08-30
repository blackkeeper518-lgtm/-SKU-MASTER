<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Order Management (Admin Mobile)</title>
    <!-- นำเข้า Font สวยๆ อ่านง่าย -->
    <link rel="preconnect" href="https://googleapis.com">
    <link rel="preconnect" href="https://gstatic.com" crossorigin>
    <link href="https://googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        /* --- Premium Dark Mode Theme --- */
        :root {
            --bg-main: #121212;       /* ดำสนิท ประหยัดแบต OLED */
            --bg-card: #1E1E1E;       /* เทาเข้ม ยกระดับมิติของการ์ด */
            --text-primary: #FFFFFF;  /* ตัวหนังสือหลัก */
            --text-secondary: #A0A0A0;/* ตัวหนังสือรอง */
            --accent-gold: #D4AF37;   /* สีทองพรีเมียม */
            --status-wait: #FF9F43;   /* สีส้ม รอตรวจสลิป */
            --status-success: #28C76F;/* สีเขียว สำเร็จ */
            --btn-gray: #333333;      /* สีปุ่มทั่วไป */
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Sarabun', sans-serif;
            -webkit-tap-highlight-color: transparent; /* ลบแสงสีฟ้าตอนกดบนมือถือ */
        }

        body {
            background-color: var(--bg-main);
            color: var(--text-primary);
            padding-bottom: 80px; /* เว้นพื้นที่ให้แถบเมนูด้านล่าง */
        }

        /* --- Header Section --- */
        .header {
            padding: 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #2A2A2A;
            position: sticky;
            top: 0;
            background-color: var(--bg-main);
            z-index: 100;
        }

        .header h1 {
            font-size: 1.1rem;
            font-weight: 600;
            letter-spacing: 0.5px;
        }

        .notification-badge {
            background-color: #EA5455;
            color: white;
            padding: 2px 8px;
            border-radius: 12px;
            font-size: 0.8rem;
            font-weight: bold;
        }

        /* --- Search Bar --- */
        .search-container {
            padding: 12px 16px;
        }

        .search-input {
            width: 100%;
            padding: 12px 16px;
            background-color: var(--bg-card);
            border: 1px solid #333;
            border-radius: 8px;
            color: var(--text-primary);
            font-size: 0.95rem;
            outline: none;
            transition: 0.3s;
        }

        .search-input:focus {
            border-color: var(--accent-gold);
        }

        /* --- Status Tabs --- */
        .tabs-container {
            display: flex;
            overflow-x: auto;
            padding: 4px 16px 12px 16px;
            gap: 8px;
        }

        /* ซ่อน Scrollbar ของแท็บ */
        .tabs-container::-webkit-scrollbar {
            display: none;
        }

        .tab {
            padding: 8px 16px;
            background-color: var(--bg-card);
            border-radius: 20px;
            font-size: 0.85rem;
            white-space: nowrap;
            color: var(--text-secondary);
            border: 1px solid transparent;
            cursor: pointer;
        }

        .tab.active {
            background-color: #2A261D;
            color: var(--accent-gold);
            border-color: var(--accent-gold);
            font-weight: 500;
        }

        /* --- Order List & Cards --- */
        .order-list {
            padding: 0 16px;
        }

        .order-card {
            background-color: var(--bg-card);
            border-radius: 12px;
            padding: 16px;
            margin-bottom: 12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
            border: 1px solid #252525;
        }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        .order-id {
            font-weight: bold;
            font-size: 1rem;
            color: var(--text-primary);
        }

        .order-time {
            font-size: 0.8rem;
            color: var(--text-secondary);
        }

        .customer-info {
            font-size: 0.95rem;
            margin-bottom: 6px;
        }

        .channel-tag {
            background-color: #2D3748;
            color: #90CDF4;
            padding: 2px 6px;
            border-radius: 4px;
            font-size: 0.75rem;
            margin-left: 4px;
        }

        .order-price {
            font-size: 1.05rem;
            font-weight: 600;
            color: var(--accent-gold);
            margin-bottom: 12px;
        }

        /* Status Badges */
        .status-badge {
            display: inline-flex;
            align-items: center;
            padding: 6px 12px;
            border-radius: 6px;
            font-size: 0.85rem;
            font-weight: 500;
            margin-bottom: 12px;
            width: 100%;
            justify-content: space-between;
            cursor: pointer;
        }

        .status-badge.waiting {
            background-color: rgba(255, 159, 67, 0.15);
            color: var(--status-wait);
        }

        .status-badge.success {
            background-color: rgba(40, 199, 111, 0.15);
            color: var(--status-success);
        }

        .click-action-text {
            font-size: 0.75rem;
            text-decoration: underline;
            opacity: 0.8;
        }

        /* --- Action Buttons (Thumb-Zone Optimization) --- */
        .card-actions {
            display: flex;
            gap: 8px;
            border-top: 1px solid #2A2A2A;
            padding-top: 12px;
            margin-top: 4px;
        }

        .btn {
            flex: 1;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 0.9rem;
            font-weight: 500;
            cursor: pointer;
            text-align: center;
            transition: 0.2s;
        }

        .btn-secondary {
            background-color: var(--btn-gray);
            color: var(--text-primary);
        }

        .btn-primary {
            background-color: var(--status-success);
            color: white;
            font-weight: 600;
        }

        /* --- Bottom Navigation Bar --- */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background-color: #1A1A1A;
            display: flex;
            justify-content: space-around;
            padding: 10px 0;
            border-top: 1px solid #2A2A2A;
            z-index: 100;
        }

        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            color: var(--text-secondary);
            font-size: 0.75rem;
            text-decoration: none;
            gap: 4px;
        }

        .nav-item.active {
            color: var(--accent-gold);
        }

        .nav-icon {
            font-size: 1.2rem;
        }
    </style>
</head>
<body>

    <!-- 1. ส่วนหัวเว็บ -->
    <div class="header">
        <h1>📦 จัดการออเดอร์ (แอดมิน)</h1>
        <div class="notification-badge">5 งานค้าง</div>
    </div>

    <!-- 2. ช่องค้นหา -->
    <div class="search-container">
        <input type="text" class="search-input" placeholder="🔍 เลขออเดอร์ / ชื่อ / เบอร์โทร...">
    </div>

    <!-- 3. แท็บกรองสถานะคำสั่งซื้อ -->
    <div class="tabs-container">
        <div class="tab active">📥 รอตรวจสลิป (5)</div>
        <div class="tab">📦 ต้องจัดส่ง (12)</div>
        <div class="tab">🚚 กำลังส่ง (20)</div>
        <div class="tab">✅ สำเร็จแล้ว (13)</div>
    </div>

    <!-- 4. รายการออเดอร์ในรูปแบบการ์ด -->
    <div class="order-list">

        <!-- ออเดอร์ที่ 1: รอตรวจสลิป -->
        <div class="order-card">
            <div class="card-header">
                <span class="order-id">#OD-2026-002</span>
                <span class="order-time">🕒 10 นาทีที่แล้ว</span>
            </div>
            <div class="customer-info">
                <strong>คุณ สมหญิง รักดี</strong> <span class="channel-tag">Facebook</span>
                <div style="color: var(--text-secondary); font-size: 0.85rem; margin-top: 2px;">089-765-xxxx</div>
            </div>
            <div class="order-price">ยอดสุทธิ: ฿450</div>
            
            <!-- แท็บสถานะที่กดเพื่อดูรูปสลิปได้เลย -->
            <div class="status-badge waiting">
                <span>⚠️ สถานะเงิน: รอตรวจสลิป (โอนเงิน)</span>
                <span class="click-action-text">🔘 จิ้มดูรูป</span>
            </div>

            <div class="card-actions">
                <button class="btn btn-secondary">❌ ยกเลิก</button>
                <button class="btn btn-primary">✅ อนุมัติออเดอร์</button>
            </div>
        </div>

        <!-- ออเดอร์ที่ 2: ชำระเงินแล้ว -->
        <div class="order-card">
            <div class="card-header">
                <span class="order-id">#OD-2026-001</span>
                <span class="order-time">🕒 1 ชั่วโมงที่แล้ว</span>
            </div>
            <div class="customer-info">
                <strong>คุณ สมชาย สายเปย์</strong> <span class="channel-tag">Line Shopping</span>
                <div style="color: var(--text-secondary); font-size: 0.85rem; margin-top: 2px;">081-234-xxxx</div>
            </div>
            <div class="order-price">ยอดสุทธิ: ฿1,250</div>
            
            <div class="status-badge success">
                <span>✅ สถานะเงิน: ชำระเงินเรียบร้อย</span>
                <span style="font-size: 0.8rem; opacity: 0.8;">🚚 ขนส่ง: Flash</span>
            </div>

            <div class="card-actions">
                <button class="btn btn-secondary">🖨️ ปะหน้า</button>
