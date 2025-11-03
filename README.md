<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WAROENG MASHER - Panel Kasir (POS)</title>
    
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Lora:wght@400;700&family=Montserrat:wght@400;500;700&display=swap" rel="stylesheet">

    <style>
        /* GAYA UNTUK PANEL KASIR (POS) */
        body { 
            font-family: 'Montserrat', sans-serif; 
            background-color: #1A1A1A; /* Latar belakang sangat gelap */
            color: #E0E0E0;
            display: flex; /* Menggunakan Flexbox untuk full height */
            flex-direction: column;
            min-height: 100vh;
        }
        h1, h2, h3, h4, h5, h6, .navbar-brand { 
            font-family: 'Lora', serif; 
            font-weight: 700; 
            color: #FFFFFF;
        }
        .navbar-brand { 
            color: #FFD700 !important; /* Brand dengan warna emas/kuning cerah */
            font-size: 1.8rem;
            letter-spacing: 1px;
        }
        .navbar {
            background-color: #282828 !important; /* Navbar gelap */
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        /* Konten utama untuk tata letak 2 kolom */
        #pos-content {
            display: flex;
            flex-grow: 1; /* Konten mengisi sisa ruang */
            padding-top: 15px;
        }
        #menu-panel {
            flex: 3; /* Panel Menu (kiri) 3/5 lebar */
            padding: 1rem;
            border-right: 1px solid #333333;
            overflow-y: auto; /* Scrollable jika menu banyak */
        }
        #checkout-panel {
            flex: 2; /* Panel Checkout (kanan) 2/5 lebar */
            background-color: #282828; /* Warna kontras untuk keranjang */
            padding: 1rem;
            display: flex;
            flex-direction: column;
        }
        .product-card { 
            border: none;
            background-color: #333333; /* Card lebih gelap dari body */
            transition: transform 0.2s, box-shadow 0.2s;
            border-radius: 8px;
            overflow: hidden; 
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.4);
            cursor: pointer; /* Menandakan bisa diklik */
            position: relative; /* Diperlukan untuk posisi tombol detail */
        }
        .product-card:hover { 
            transform: translateY(-3px); 
            box-shadow: 0 8px 16px rgba(255, 215, 0, 0.2); 
            border: 1px solid #FFD700;
        }
        .product-card .card-img-top { 
            height: 120px; /* Lebih kecil untuk POS */
            object-fit: cover;
        }
        .product-card .card-body { 
            padding: 0.75rem; 
            min-height: 100px; /* Tambahkan tinggi minimum untuk card body */
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        .product-card .card-title { 
            color: #FFD700;
            font-size: 1rem;
            margin-bottom: 0.2rem;
            /* Menghapus white-space: nowrap, overflow: hidden, dan text-overflow: ellipsis */
            /* Mengganti dengan animasi scroll untuk teks panjang */
            display: -webkit-box;
            -webkit-line-clamp: 2; /* Maksimal 2 baris */
            -webkit-box-orient: vertical;
            overflow: hidden;
            text-overflow: ellipsis;
            transition: all 0.3s ease;
            min-height: 2.4em; /* Minimal tinggi untuk 2 baris */
        }
        .product-card:hover .card-title {
            -webkit-line-clamp: unset; /* Tampilkan semua baris saat hover */
            max-height: none;
            overflow: visible;
            text-overflow: unset;
            animation: fadeIn 0.3s ease;
        }
        @keyframes fadeIn {
            from { opacity: 0.7; }
            to { opacity: 1; }
        }
        .product-card h6 { 
            color: #FFFFFF; 
            font-size: 1.1rem;
            margin: 0;
        }
        .section-title {
            position: sticky;
            top: 0;
            background-color: #1A1A1A;
            z-index: 10;
            padding: 10px 0;
            margin-top: -10px;
        }
        /* Tombol Detail Produk */
        .btn-detail {
            position: absolute;
            top: 5px;
            right: 5px;
            background-color: rgba(40, 40, 40, 0.8);
            color: #FFD700;
            border: none;
            border-radius: 50%;
            width: 30px;
            height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1rem;
            z-index: 2;
            transition: background-color 0.2s;
        }
        .btn-detail:hover {
            background-color: rgba(0, 0, 0, 0.9);
            color: #FFFFFF;
        }

        /* Styling Keranjang di Panel Kasir */
        #cart-items-container-pos {
            flex-grow: 1;
            overflow-y: auto;
            padding-right: 5px;
            margin-bottom: 15px;
        }
        .cart-item-pos {
            border-bottom: 1px solid #333333;
            padding: 8px 0;
        }
        .cart-item-pos h6 {
            color: #E0E0E0;
            font-size: 1rem;
        }
        .cart-item-pos .price-qty {
            font-size: 0.9rem;
            color: #B0B0B0;
        }
        .btn-qty {
            width: 30px;
            height: 30px;
            padding: 0;
            font-size: 1rem;
            line-height: 1;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .btn-success {
            background-color: #4CAF50;
            border-color: #4CAF50;
        }
        .btn-success:hover {
            background-color: #388E3C;
            border-color: #388E3C;
        }
        .btn-primary {
            background-color: #FFD700;
            border-color: #FFD700;
            color: #1A1A1A;
        }
        .btn-primary:hover {
            background-color: #E0B400;
            border-color: #E0B400;
        }
        .payment-summary {
            background-color: #1A1A1A;
            padding: 1rem;
            border-radius: 8px;
        }
        .form-control-dark {
            background-color: #333333;
            color: #FFFFFF;
            border: 1px solid #555555;
        }
        .form-control-dark:focus {
            background-color: #333333;
            color: #FFFFFF;
            border-color: #FFD700;
            box-shadow: 0 0 0 0.25rem rgba(255, 215, 0, 0.25);
        }
        /* Gaya untuk tombol diskon */
        .discount-buttons {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
            margin-bottom: 10px;
        }
        .discount-btn {
            flex: 1;
            min-width: 60px;
            padding: 5px;
            font-size: 0.8rem;
        }
        .discount-btn.active {
            background-color: #FFD700;
            color: #1A1A1A;
            border-color: #FFD700';
        }
        .discount-info {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            font-size: 0.9rem;
        }
        .original-total {
            text-decoration: line-through;
            color: #999;
        }
        .discount-amount {
            color: #4CAF50;
        }
        /* Gaya Modal */
        .modal-content {
            background-color: #282828;
            color: #E0E0E0;
        }
        .modal-header {
            border-bottom: 1px solid #444;
        }
        .modal-footer {
            border-top: 1px solid #444;
        }
        .btn-close {
            filter: invert(1);
        }
        /* Gaya untuk Receipt/Struk */
        #receipt-content {
            font-family: 'Courier New', Courier, monospace;
            background-color: #fff;
            color: #000;
            padding: 20px;
            width: 100%;
            max-width: 400px;
            margin: 0 auto;
            border-radius: 5px;
        }
        #receipt-content .receipt-header {
            text-align: center;
            border-bottom: 2px dashed #000;
            padding-bottom: 10px;
            margin-bottom: 15px;
        }
        #receipt-content .receipt-header h3 {
            margin: 0;
            font-size: 1.5rem;
            color: #000;
        }
        #receipt-content .receipt-header p {
            margin: 5px 0;
            font-size: 0.8rem;
        }
        #receipt-content .receipt-body {
            margin-bottom: 15px;
        }
        #receipt-content .receipt-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
            font-size: 0.9rem;
        }
        #receipt-content .receipt-footer {
            border-top: 2px dashed #000;
            padding-top: 10px;
            text-align: center;
        }
        #receipt-content .receipt-footer p {
            margin: 5px 0;
            font-size: 0.8rem;
        }
        /* Gaya untuk QR yang bisa discan */
        .qr-scannable-section {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            padding: 30px;
            border-radius: 15px;
            margin: 25px 0;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            position: relative;
            overflow: hidden;
        }
        .qr-scannable-section::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(45deg, transparent, rgba(255, 255, 255, 0.1), transparent);
            transform: rotate(45deg);
            animation: shimmer 3s infinite;
        }
        @keyframes shimmer {
            0% { transform: translateX(-100%) translateY(-100%) rotate(45deg); }
            100% { transform: translateX(100%) translateY(100%) rotate(45deg); }
        }
        .qr-scannable-title {
            text-align: center;
            color: rgb(0, 0, 0);
            font-size: 1.3rem;
            font-weight: bold;
            margin-bottom: 20px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
        }
        .qr-scannable-container {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 30px;
            flex-wrap: wrap;
        }
        .qr-code-main {
            background: rgb(0, 0, 0);
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
            position: relative;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .qr-code-main:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.3);
        }
        .qr-code-main::before {
            content: '';
            position: absolute;
            top: -3px;
            left: -3px;
            right: -3px;
            bottom: -3px;
            background: linear-gradient(45deg, #FFD700, #FFA500, #FF6347, #FFD700);
            border-radius: 18px;
            z-index: -1;
            animation: borderGlow 3s linear infinite;
        }
        @keyframes borderGlow {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        .qr-code-main img {
            display: block !important;
            width: 250px !important;
            height: 250px !important;
            border-radius: 10px;
        }
        .qr-scan-animation {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border-radius: 15px;
            overflow: hidden;
            pointer-events: none;
        }
        .qr-scan-line {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 3px;
            background: linear-gradient(90deg, transparent, #FFD700, transparent);
            animation: scanLine 2s linear infinite;
        }
        @keyframes scanLine {
            0% { top: 0; }
            50% { top: calc(100% - 3px); }
            100% { top: 0; }
        }
        .qr-corner {
            position: absolute;
            width: 20px;
            height: 20px;
            border: 3px solid #FFD700;
        }
        .qr-corner-tl {
            top: 10px;
            left: 10px;
            border-right: none;
            border-bottom: none;
        }
        .qr-corner-tr {
            top: 10px;
            right: 10px;
            border-left: none;
            border-bottom: none;
        }
        .qr-corner-bl {
            bottom: 10px;
            left: 10px;
            border-right: none;
            border-top: none;
        }
        .qr-corner-br {
            bottom: 10px;
            right: 10px;
            border-left: none;
            border-top: none;
        }
        .qr-info-panel {
            background: rgba(255, 255, 255, 0.95);
            padding: 20px;
            border-radius: 15px;
            max-width: 250px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
        }
        .qr-info-title {
            font-size: 1.1rem;
            font-weight: bold;
            color: #333;
            margin-bottom: 15px;
            text-align: center;
        }
        .qr-info-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            padding: 5px 0;
            border-bottom: 1px solid #eee;
            font-size: 0.9rem;
        }
        .qr-info-item:last-child {
            border-bottom: none;
            font-weight: bold;
            color: #FF6347;
            font-size: 1rem;
        }
        .qr-instructions {
            text-align: center;
            color: white;
            margin-top: 20px;
            font-size: 0.9rem;
        }
        .qr-instruction-item {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin-bottom: 8px;
        }
        .qr-instruction-icon {
            font-size: 1.2rem;
        }
        /* Tombol Share QR */
        .qr-share-buttons {
            display: flex;
            gap: 10px;
            margin-top: 20px;
            justify-content: center;
            flex-wrap: wrap;
        }
        .qr-share-btn {
            padding: 10px 18px;
            border-radius: 25px;
            border: none;
            font-size: 0.9rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        .qr-share-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }
        .qr-share-download {
            background: linear-gradient(135deg, #4CAF50, #45a049);
            color: white;
        }
        .qr-share-copy {
            background: linear-gradient(135deg, #2196F3, #1976D2);
            color: white;
        }
        .qr-share-native {
            background: linear-gradient(135deg, #FF9800, #F57C00);
            color: white;
        }
        .qr-share-expand {
            background: linear-gradient(135deg, #9C27B0, #7B1FA2);
            color: white;
        }
        /* Modal QR Expanded */
        .qr-expanded-container {
            text-align: center;
            padding: 30px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 15px;
        }
        .qr-expanded-img {
            max-width: 350px;
            width: 100%;
            height: auto;
            border: 5px solid white;
            border-radius: 15px;
            box-shadow: 0 8px 30px rgba(0, 0, 0, 0.3);
        }
        .qr-expanded-info {
            margin-top: 20px;
            padding: 20px;
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
        }
        .qr-expanded-title {
            font-size: 1.3rem;
            font-weight: bold;
            color: #333;
            margin-bottom: 15px;
        }
        .qr-expanded-details {
            font-size: 0.9rem;
            color: #666;
            text-align: left;
        }
        .qr-expanded-details .detail-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            padding: 8px 0;
            border-bottom: 1px solid #eee;
        }
        .qr-expanded-details .detail-item:last-child {
            border-bottom: none;
            font-weight: bold;
            color: #FF6347;
            font-size: 1.1rem;
        }
        /* Gaya untuk cetak */
        @media print {
            body * {
                visibility: hidden;
            }
            #receiptModal, #receiptModal * {
                visibility: visible;
            }
            #receiptModal {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
            }
            .modal-header, .modal-footer {
                display: none !important;
            }
            .modal-dialog {
                margin: 0;
                max-width: 100%;
            }
            .qr-scannable-section {
                background: white !important;
                box-shadow: none !important;
            }
            .qr-share-buttons {
                display: none !important;
            }
            .qr-scan-animation {
                display: none !important;
            }
        }
        /* Gaya untuk badge kategori */
        .category-badge {
            position: absolute;
            top: 5px;
            left: 5px;
            background-color: rgba(255, 215, 0, 0.8);
            color: #1A1A1A;
            font-size: 0.7rem;
            font-weight: bold;
            padding: 2px 6px;
            border-radius: 4px;
            z-index: 2;
        }
        .hot-badge {
            background-color: rgba(255, 87, 34, 0.8);
            color: #FFFFFF;
        }
        .cold-badge {
            background-color: rgba(33, 150, 243, 0.8);
            color: #FFFFFF;
        }
        /* Canvas untuk receipt image */
        #receipt-canvas {
            display: none;
        }
        /* Toast notification */
        .toast-container {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 9999;
        }
        .custom-toast {
            background-color: #282828;
            color: #fff;
            border-radius: 8px;
            padding: 15px 20px;
            margin-bottom: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
            display: flex;
            align-items: center;
            gap: 10px;
            min-width: 300px;
            animation: slideIn 0.3s ease;
        }
        @keyframes slideIn {
            from {
                transform: translateX(100%);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }
        .toast-success {
            border-left: 4px solid #4CAF50;
        }
        .toast-error {
            border-left: 4px solid #f44336;
        }
        .toast-info {
            border-left: 4px solid #2196F3;
        }
        /* Mobile responsive */
        @media (max-width: 768px) {
            .qr-scannable-container {
                flex-direction: column;
                gap: 20px;
            }
            .qr-code-main img {
                width: 200px !important;
                height: 200px !important;
            }
            .qr-info-panel {
                max-width: 100%;
            }
        }
    </style>
</head>
<body>

    <nav class="navbar navbar-expand-lg navbar-dark bg-dark shadow-sm">
        <div class="container-fluid px-4">
            <a class="navbar-brand" href="#">💰 KASIR - WAROENG MASHER</a>
            <span class="text-white ms-auto me-3 d-none d-md-inline">Halo, Kasir!</span>
            <button class="btn btn-sm btn-outline-light" onclick="window.location.reload();"><i class="bi bi-arrow-clockwise"></i> Reset Transaksi</button>
        </div>
    </nav>

    <div id="pos-content" class="container-fluid">
        
        <div id="menu-panel">
            <h4 class="section-title text-center text-white">Pilih Menu</h4>
            <input type="text" id="menu-search-input" class="form-control form-control-dark mb-4" placeholder="Cari Menu..." onkeyup="filterProducts()">

            <h5 class="text-white mt-4 mb-3">☕ Minuman Panas</h5>
            <div class="row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 g-3" id="hot-drink-list-container">
                </div>

            <h5 class="text-white mt-5 mb-3">🧊 Minuman Dingin</h5>
            <div class="row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 g-3" id="cold-drink-list-container">
                </div>

            <h5 class="text-white mt-5 mb-3">🍽️ Makanan</h5>
            <div class="row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 g-3" id="food-list-container">
                </div>
        </div>

        <div id="checkout-panel">
            <h4 class="text-center text-white mb-3">Rincian Pesanan</h4>

            <div id="cart-items-container-pos" class="bg-dark rounded p-2 mb-3">
                <p class="text-center text-muted small mt-5" id="cart-empty-message">Ketuk menu di kiri untuk memulai pesanan.</p>
            </div>

            <div class="payment-summary mt-auto">
                <div class="d-flex justify-content-between text-white mb-2">
                    <span>Subtotal:</span>
                    <span id="cart-subtotal-pos" class="text-white">Rp 0</span>
                </div>
                
                <div class="mb-3">
                    <label class="form-label small text-white">Diskon:</label>
                    <div class="discount-buttons">
                        <button class="btn btn-outline-warning discount-btn" onclick="applyDiscount(0)">Tidak Ada</button>
                        <button class="btn btn-outline-warning discount-btn" onclick="applyDiscount(10)">10%</button>
                        <button class="btn btn-outline-warning discount-btn" onclick="applyDiscount(20)">20%</button>
                        <button class="btn btn-outline-warning discount-btn" onclick="applyDiscount(30)">30%</button>
                        <button class="btn btn-outline-warning discount-btn" onclick="applyDiscount(40)">40%</button>
                        <button class="btn btn-outline-warning discount-btn" onclick="applyDiscount(50)">50%</button>
                    </div>
                    <div class="discount-info">
                        <span>Potongan Diskon:</span>
                        <span id="discount-amount" class="discount-amount">Rp 0</span>
                    </div>
                </div>
                
                <hr class="border-secondary">

                <div class="d-flex justify-content-between text-white fw-bold fs-5 mb-2">
                    <span>TOTAL BAYAR:</span>
                    <span id="cart-total-pos" class="text-warning">Rp 0</span>
                </div>
                
                <hr class="border-secondary">

                <div class="mb-3">
                    <label for="bayar-input" class="form-label small text-white">Jumlah Dibayarkan (Cash):</label>
                    <input type="number" id="bayar-input" class="form-control form-control-dark" placeholder="Masukkan jumlah uang" min="0" oninput="calculateChange()">
                </div>

                <div class="d-flex justify-content-between text-white fw-bold fs-5 mb-3">
                    <span>Kembalian:</span>
                    <span id="kembalian-text" class="text-success">Rp 0</span>
                </div>
                
                <button type="button" class="btn btn-primary w-100 py-3 fw-bold" id="complete-sale-btn" onclick="completeSale()" disabled>
                    <i class="bi bi-cash-stack"></i> Selesaikan Penjualan
                </button>
                
            </div>
        </div>
    </div>

    <!-- Modal Detail Produk -->
    <div class="modal fade" id="productDetailModal" tabindex="-1" aria-labelledby="productDetailModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="productDetailModalLabel">Detail Produk</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body text-center">
                    <img id="modal-product-image" src="" class="img-fluid rounded mb-3" alt="Product Image">
                    <h4 id="modal-product-name" class="text-warning"></h4>
                    <p id="modal-product-description" class="text-white-50"></p>
                    <h3 id="modal-product-price" class="text-white fw-bold"></h3>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
                    <button type="button" id="modal-add-to-cart-btn" class="btn btn-primary">
                        <i class="bi bi-cart-plus"></i> Tambah ke Keranjang
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Receipt/Struk -->
    <div class="modal fade" id="receiptModal" tabindex="-1" aria-labelledby="receiptModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="receiptModalLabel">Struk Pembayaran</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div id="receipt-content">
                        <div class="receipt-header">
                            <h3>WAROENG MASHER</h3>
                            <p>Jl. pancing mabar,pasar 4[</p>
                            <p>Telp: </p>
                            <p id="receipt-date"></p>
                            <p id="receipt-trans-id"></p>
                        </div>
                        <div class="receipt-body">
                            <div id="receipt-items"></div>
                            <hr>
                            <div class="receipt-item">
                                <span>Subtotal:</span>
                                <span id="receipt-subtotal"></span>
                            </div>
                            <div class="receipt-item">
                                <span>Diskon (<span id="receipt-discount-percent"></span>%):</span>
                                <span id="receipt-discount-amount"></span>
                            </div>
                            <div class="receipt-item fw-bold">
                                <span>Total:</span>
                                <span id="receipt-total"></span>
                            </div>
                            <div class="receipt-item">
                                <span>Tunai:</span>
                                <span id="receipt-cash"></span>
                            </div>
                            <div class="receipt-item">
                                <span>Kembalian:</span>
                                <span id="receipt-change"></span>
                            </div>
                        </div>
                        
                        <!-- QR Code Scannable Section -->
                        <div class="qr-scannable-section">
                            <div class="qr-scannable-title">
                                <i class="bi bi-qr-code-scan"></i> SCAN QR CODE UNTUK STRUK DIGITAL
                            </div>
                            <div class="qr-scannable-container">
                                <div class="qr-code-main">
                                    <div class="qr-scan-animation">
                                        <div class="qr-scan-line"></div>
                                        <div class="qr-corner qr-corner-tl"></div>
                                        <div class="qr-corner qr-corner-tr"></div>
                                        <div class="qr-corner qr-corner-bl"></div>
                                        <div class="qr-corner qr-corner-br"></div>
                                    </div>
                                    <div id="receipt-qr"></div>
                                </div>
                                <div class="qr-info-panel">
                                    <div class="qr-info-title">Detail Transaksi</div>
                                    <div class="qr-info-item">
                                        <span>ID:</span>
                                        <span id="qr-info-id">-</span>
                                    </div>
                                    <div class="qr-info-item">
                                        <span>Tanggal:</span>
                                        <span id="qr-info-date">-</span>
                                    </div>
                                    <div class="qr-info-item">
                                        <span>Item:</span>
                                        <span id="qr-info-items">0</span>
                                    </div>
                                    <div class="qr-info-item">
                                        <span>Total:</span>
                                        <span id="qr-info-total">Rp 0</span>
                                    </div>
                                </div>
                            </div>
                            
                            <!-- Tombol Share QR -->
                            <div class="qr-share-buttons">
                                <button class="qr-share-btn qr-share-download" onclick="downloadQR()">
                                    <i class="bi bi-download"></i> Download
                                </button>
                                <button class="qr-share-btn qr-share-copy" onclick="copyQRData()">
                                    <i class="bi bi-clipboard"></i> Salin
                                </button>
                                <button class="qr-share-btn qr-share-native" onclick="shareQR()">
                                    <i class="bi bi-share"></i> Bagikan
                                </button>
                                <button class="qr-share-btn qr-share-expand" onclick="showExpandedQR()">
                                    <i class="bi bi-zoom-in"></i> Perbesar
                                </button>
                            </div>
                            
                            <div class="qr-instructions">
                                <div class="qr-instruction-item">
                                    <i class="bi bi-phone qr-instruction-icon"></i>
                                    <span>Buka kamera smartphone Anda</span>
                                </div>
                                <div class="qr-instruction-item">
                                    <i class="bi bi-qr-code qr-instruction-icon"></i>
                                    <span>Arahkan ke QR code di atas</span>
                                </div>
                                <div class="qr-instruction-item">
                                    <i class="bi bi-check-circle qr-instruction-icon"></i>
                                    <span>Struk digital akan otomatis tersimpan</span>
                                </div>
                            </div>
                        </div>
                        
                        <div class="receipt-footer">
                            <p>Terima Kasih</p>
                            <p>Selamat Berbelanja Kembali</p>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
                    <button type="button" class="btn btn-primary" onclick="printReceipt()">
                        <i class="bi bi-printer"></i> Cetak
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal QR Expanded -->
    <div class="modal fade" id="qrExpandedModal" tabindex="-1" aria-labelledby="qrExpandedModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="qrExpandedModalLabel">QR Code Struk Digital</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="qr-expanded-container">
                        <img id="qr-expanded-image" class="qr-expanded-img" src="" alt="QR Code Struk">
                        <div class="qr-expanded-info">
                            <div class="qr-expanded-title">Detail Transaksi</div>
                            <div class="qr-expanded-details" id="qr-expanded-details">
                                <!-- Details will be inserted here -->
                            </div>
                        </div>
                        <div class="qr-share-buttons mt-3">
                            <button class="qr-share-btn qr-share-download" onclick="downloadQR()">
                                <i class="bi bi-download"></i> Download
                            </button>
                            <button class="qr-share-btn qr-share-copy" onclick="copyQRData()">
                                <i class="bi bi-clipboard"></i> Salin Data
                            </button>
                            <button class="qr-share-btn qr-share-native" onclick="shareQR()">
                                <i class="bi bi-share"></i> Bagikan
                            </button>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Hidden canvas for receipt image generation -->
    <canvas id="receipt-canvas"></canvas>

    <!-- Toast Container -->
    <div class="toast-container" id="toast-container"></div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore-compat.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/qrcodejs@1.0.0/qrcode.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script>
        // --- INISIALISASI FIREBASE ---
        const firebaseConfig = {
            apiKey: "AIzaSyBhRCNri0TjBdMx9fEN8jgAfDD-jV_FdNo",
            authDomain: "waroengmasher.firebaseapp.com",
            projectId: "waroengmasher",
            storageBucket: "waroengmasher.firebasestorage.app",
            messagingSenderId: "984134106741",
            appId: "1:984134106741:web:81d129033d09475173c23c",
            measurementId: "G-7RRW43T1LT"
        };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();

        // --- DATA PRODUK (Disimpan di sini untuk kecepatan akses Kasir) ---
        const products = [
            { id: 'INDONESIANO HOT', name: 'INDONESIANO HOT', price: 10000, image: 'sasya.jpg', description: 'Kopi hitam klasik Indonesia disajikan hangat dengan rasa otentik.', category: 'minuman', temperature: 'hot' },
            { id: 'LONGBLACK HOT', name: 'LONGBLACK HOT', price: 10000, image: 'sasya.jpg', description: 'Espresso ganda yang diekstraksi ke dalam air panas, kuat dan menyegarkan.', category: 'minuman', temperature: 'hot' },
            { id: 'SANGER HOT', name: 'SANGER HOT', price: 12000, image: 'a.jpg', description: 'Kopi kental dicampur dengan sedikit susu kental manis, rasa manis yang pas.', category: 'minuman', temperature: 'hot' },
            { id: 'FILTER COFFE HOT', name: 'FILTER KOPI HOT', price: 10000, image: '3.jpg', description: 'Kopi yang diseduh dengan metode filter, menonjolkan karakter biji kopi.', category: 'minuman', temperature: 'hot' },
            { id: 'INDONESIANO ICED', name: 'INDONESIANO ICED', price: 12000, image: 'americano.jpg', description: 'Kopi Indonesia disajikan dingin, segar dan penuh semangat.', category: 'minuman', temperature: 'cold' },
            { id: 'LONG BLACK ICED', name: 'LONG BLACK ICED', price: 12000, image: 'americano.jpg', description: 'Long Black dingin, pendamping sempurna di hari yang panas.', category: 'minuman', temperature: 'cold' },
            { id: 'SANGER ICED', name: 'SANGER ICED', price: 15000, image: 'b.jpg', description: 'Sanger dingin yang creamy, minuman kopi favorit untuk penyuka rasa manis.', category: 'minuman', temperature: 'cold' },
            { id: 'FILTER ICED', name: 'FILTER ICED', price: 12000, image: '4.jpg', description: 'Filter Kopi disajikan dingin, lebih lembut dan mudah dinikmati.', category: 'minuman', temperature: 'cold' },
            { id: 'ESPRESSO', name: 'ESPRESSO', price: 8000, image: '6.jpg', description: 'kopi yang memiliki rasa yang sangat stroong.', category: 'minuman', temperature: 'hot' },
            { id: 'KOPI SUSU GULA AREN ICED', name: 'KOPI SUSU GULA AREN ICED', price: 10000, image: '5.jpg', description: 'Kopi susu gula aren disajikan dingin, minuman kekinian yang populer.', category: 'minuman', temperature: 'cold' },
            { id: 'TEH HIJAU HOT&COLD', name: 'TEH HIJAU HOT&COLD', price: 5000, image: '8.jpg', description: 'Teh hijau kaya antioksidan, bisa disajikan panas atau dingin.', category: 'minuman', temperature: 'both' },
            { id: 'TEH MANIS HOT&COLD', name: 'TEH MANIS HOT&COLD', price: 5000, image: '7.jpg', description: 'Teh diseduh manis dan disajikan dingin, klasik dan melegakan dahaga.', category: 'minuman', temperature: 'both' },
            { id: 'WEDANG JAHE HOT', name: 'WEDANG JAHE HOT', price: 10000, image: '9.jpg', description: 'Minuman jahe hangat yang menenangkan, cocok untuk cuaca dingin.', category: 'minuman', temperature: 'hot' },
            { id: 'MILO COLD&HOT', name: 'MILO COLD&HOT', price: 10000, image: '10.jpg', description: 'Minuman cokelat malt favorit, bisa disajikan dingin atau hangat.', category: 'minuman', temperature: 'both' },
            { id: 'KENTANG GORENG', name: 'KENTANG GORENG', price: 10000, image: '11.jpg', description: 'Kentang goreng renyah disajikan dengan saus pilihan.', category: 'makanan', temperature: 'none' },
            { id: 'ROTI BAKAR', name: 'ROTI BAKAR', price: 7000, image: '12.jpg', description: 'Roti bakar dengan berbagai topping manis.', category: 'makanan', temperature: 'none' },
            { id: 'AYAM PENYET', name: 'AYAM PENYET', price: 10000, image: '13.jpg', description: 'Ayam goreng dengan sambal penyet pedas, disajikan dengan nasi hangat.', category: 'makanan', temperature: 'none' },
            { id: 'BAKARAN', name: 'ANEKA BAKARAN', price: 1000, image: '17.jpg', description: 'Pilihan sate-satean bakar (per tusuk) dengan bumbu spesial.', category: 'makanan', temperature: 'none' },
            { id: 'UBI GORENG', name: 'UBI GORENG', price: 10000, image: '15.jpg', description: 'Ubi manis digoreng renyah, camilan tradisional yang lezat.', category: 'makanan', temperature: 'none' },
            { id: 'AYAM PENYET SAMBEL IJO', name: 'AYAM PENYET SAMBEL IJO', price: 10000, image: '14.jpg', description: 'Ayam goreng dengan sambal hijau pedas yang khas.', category: 'makanan', temperature: 'none' }
        ];

        // --- LOGIKA KASIR (POS) ---
        
        let cart = []; // Keranjang sementara untuk kasir
        let discountPercentage = 0; // Persentase diskon default
        let currentTransactionData = null; // Data transaksi saat ini
        let currentQRData = null; // Data QR saat ini
        const hotDrinkContainer = document.getElementById('hot-drink-list-container');
        const coldDrinkContainer = document.getElementById('cold-drink-list-container');
        const foodContainer = document.getElementById('food-list-container');
        const cartItemsContainerPOS = document.getElementById('cart-items-container-pos');
        const cartSubtotalPOS = document.getElementById('cart-subtotal-pos');
        const cartTotalPOS = document.getElementById('cart-total-pos');
        const discountAmount = document.getElementById('discount-amount');
        const bayarInput = document.getElementById('bayar-input');
        const kembalianText = document.getElementById('kembalian-text');
        const completeSaleBtn = document.getElementById('complete-sale-btn');
        const cartEmptyMessage = document.getElementById('cart-empty-message');

        // Elemen Modal
        const productDetailModal = new bootstrap.Modal(document.getElementById('productDetailModal'));
        const receiptModal = new bootstrap.Modal(document.getElementById('receiptModal'));
        const qrExpandedModal = new bootstrap.Modal(document.getElementById('qrExpandedModal'));
        const modalProductName = document.getElementById('modal-product-name');
        const modalProductImage = document.getElementById('modal-product-image');
        const modalProductDescription = document.getElementById('modal-product-description');
        const modalProductPrice = document.getElementById('modal-product-price');
        const modalAddToCartBtn = document.getElementById('modal-add-to-cart-btn');

        function formatRupiah(number) {
            return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', minimumFractionDigits: 0 }).format(number);
        }

        function calculateSubtotal() {
            return cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
        }

        function calculateTotal() {
            const subtotal = calculateSubtotal();
            const discountValue = subtotal * (discountPercentage / 100);
            return subtotal - discountValue;
        }

        // --- RENDER MENU ---
        function renderProducts(filteredProducts = products) {
            hotDrinkContainer.innerHTML = '';
            coldDrinkContainer.innerHTML = '';
            foodContainer.innerHTML = '';
            
            const hotDrinks = filteredProducts.filter(p => p.category === 'minuman' && (p.temperature === 'hot' || p.temperature === 'both'));
            const coldDrinks = filteredProducts.filter(p => p.category === 'minuman' && (p.temperature === 'cold' || p.temperature === 'both'));
            const foods = filteredProducts.filter(p => p.category === 'makanan');
            
            hotDrinks.forEach(product => {
                hotDrinkContainer.innerHTML += createProductCard(product);
            });

            coldDrinks.forEach(product => {
                coldDrinkContainer.innerHTML += createProductCard(product);
            });

            foods.forEach(product => {
                foodContainer.innerHTML += createProductCard(product);
            });
            
            // Tambahkan event listener untuk kartu yang diklik
            document.querySelectorAll('.product-card').forEach(card => {
                card.addEventListener('click', (e) => {
                    // Jika yang diklik adalah tombol detail, jangan tambahkan ke keranjang
                    if (e.target.closest('.btn-detail')) {
                        return;
                    }
                    e.preventDefault();
                    addToCart(e.currentTarget.dataset.id);
                });
            });
        }

        function createProductCard(product) {
            // Tentukan badge berdasarkan suhu
            let badgeHtml = '';
            if (product.temperature === 'hot') {
                badgeHtml = '<span class="category-badge hot-badge">PANAS</span>';
            } else if (product.temperature === 'cold') {
                badgeHtml = '<span class="category-badge cold-badge">DINGIN</span>';
            } else if (product.temperature === 'both') {
                badgeHtml = '<span class="category-badge">PANAS/DINGIN</span>';
            }
            
            return `
                <div class="col">
                    <div class="card product-card h-100" data-id="${product.id}">
                        ${badgeHtml}
                        <button class="btn-detail" onclick="showProductDetail('${product.id}')">
                            <i class="bi bi-info-circle"></i>
                        </button>
                        <img src="${product.image}" class="card-img-top" alt="${product.name}">
                        <div class="card-body d-flex flex-column justify-content-between">
                            <h5 class="card-title text-center">${product.name}</h5>
                            <h6 class="text-center text-white">${formatRupiah(product.price)}</h6>
                        </div>
                    </div>
                </div>
            `;
        }

        // --- FUNGSI DETAIL PRODUK ---
        function showProductDetail(productId) {
            const product = products.find(p => p.id === productId);
            if (!product) return;

            // Isi modal dengan data produk
            modalProductName.textContent = product.name;
            modalProductImage.src = product.image;
            modalProductDescription.textContent = product.description;
            modalProductPrice.textContent = formatRupiah(product.price);

            // Set event listener untuk tombol tambah ke keranjang di modal
            modalAddToCartBtn.onclick = () => {
                addToCart(productId);
                productDetailModal.hide(); // Tutup modal setelah menambahkan
            };

            // Tampilkan modal
            productDetailModal.show();
        }

        // --- FUNGSI KERANJANG KASIR ---
        function addToCart(productId) {
            const existingItemIndex = cart.findIndex(item => item.id === productId);
            const product = products.find(p => p.id === productId);
            
            if (!product) return;

            if (existingItemIndex > -1) {
                cart[existingItemIndex].quantity += 1;
            } else {
                cart.push({ id: product.id, name: product.name, price: product.price, quantity: 1 });
            }
            renderCartPOS();
        }
        
        function updateQuantityPOS(productId, change) {
            const itemIndex = cart.findIndex(item => item.id === productId);
            if (itemIndex > -1) {
                cart[itemIndex].quantity += change;
                if (cart[itemIndex].quantity <= 0) {
                    cart.splice(itemIndex, 1); // Hapus jika kuantitas 0
                }
            }
            renderCartPOS();
        }

        function renderCartPOS() {
            cartItemsContainerPOS.innerHTML = '';
            
            if (cart.length === 0) {
                cartEmptyMessage.style.display = 'block';
                completeSaleBtn.disabled = true;
            } else {
                cartEmptyMessage.style.display = 'none';
                completeSaleBtn.disabled = calculateTotal() > 0 ? false : true;
                
                let html = ``;
                cart.forEach(item => {
                    const subtotal = item.price * item.quantity;
                    html += `
                        <div class="d-flex cart-item-pos align-items-center">
                            <div class="flex-grow-1">
                                <h6 class="mb-0">${item.name}</h6>
                                <p class="price-qty mb-0">${formatRupiah(item.price)}</p>
                            </div>
                            <div class="d-flex align-items-center me-2">
                                <button class="btn btn-sm btn-danger btn-qty me-1" onclick="updateQuantityPOS('${item.id}', -1)"><i class="bi bi-dash"></i></button>
                                <span class="text-white fw-bold mx-2">${item.quantity}</span>
                                <button class="btn btn-sm btn-success btn-qty" onclick="updateQuantityPOS('${item.id}', 1)"><i class="bi bi-plus"></i></button>
                            </div>
                            <span class="text-warning fw-bold">${formatRupiah(subtotal)}</span>
                        </div>
                    `;
                });
                cartItemsContainerPOS.innerHTML = html;
            }

            // Update Subtotal, Diskon, dan Total
            const subtotal = calculateSubtotal();
            const discountValue = subtotal * (discountPercentage / 100);
            const total = subtotal - discountValue;
            
            cartSubtotalPOS.textContent = formatRupiah(subtotal);
            discountAmount.textContent = formatRupiah(discountValue);
            cartTotalPOS.textContent = formatRupiah(total);
            
            calculateChange(); // Hitung kembalian setiap ada perubahan keranjang
        }

        // --- FUNGSI DISKON ---
        function applyDiscount(percentage) {
            discountPercentage = percentage;
            
            // Update UI untuk menunjukkan tombol diskon yang aktif
            document.querySelectorAll('.discount-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            
            renderCartPOS();
        }

        // --- FUNGSI PEMBAYARAN DAN KEMBALIAN ---
        function calculateChange() {
            const total = calculateTotal();
            const bayar = parseInt(bayarInput.value) || 0;
            const kembalian = bayar - total;

            if (kembalian >= 0) {
                kembalianText.textContent = formatRupiah(kembalian);
                kembalianText.classList.remove('text-danger');
                kembalianText.classList.add('text-success');
                completeSaleBtn.disabled = cart.length === 0;
            } else {
                kembalianText.textContent = formatRupiah(kembalian);
                kembalianText.classList.remove('text-success');
                kembalianText.classList.add('text-danger');
                completeSaleBtn.disabled = true;
            }

            if (cart.length === 0) {
                completeSaleBtn.disabled = true;
                bayarInput.value = '';
            }
        }

        function completeSale() {
            const subtotal = calculateSubtotal();
            const discountValue = subtotal * (discountPercentage / 100);
            const total = calculateTotal();
            const bayar = parseInt(bayarInput.value) || 0;
            const kembalian = bayar - total;
            
            if (cart.length === 0) {
                alert('Gagal! Keranjang masih kosong.');
                return;
            }

            if (kembalian < 0) {
                alert('Gagal! Jumlah bayar kurang.');
                return;
            }
            
            // Logika Penyelesaian Transaksi
            const transactionId = 'TRX-' + Date.now();
            const transactionData = {
                transactionId: transactionId,
                timestamp: firebase.firestore.FieldValue.serverTimestamp(),
                subtotal: subtotal,
                discount: discountPercentage,
                discountAmount: discountValue,
                total: total,
                bayar: bayar,
                kembalian: kembalian,
                items: cart.map(item => ({ id: item.id, name: item.name, price: item.price, qty: item.quantity })),
                kasir: 'User Kasir (Ganti dengan nama kasir)',
                status: 'Selesai'
            };

            // Simpan ke Firestore (Opsional, untuk laporan penjualan)
            db.collection('transactions').add(transactionData)
                .then(() => {
                    // Tampilkan struk pembayaran
                    showReceipt(transactionData);
                    
                    // Reset Kasir
                    cart = [];
                    discountPercentage = 0;
                    bayarInput.value = '';
                    
                    // Reset UI diskon
                    document.querySelectorAll('.discount-btn').forEach(btn => {
                        btn.classList.remove('active');
                    });
                    document.querySelector('.discount-btn').classList.add('active');
                    
                    renderCartPOS();
                    calculateChange();
                })
                .catch(error => {
                    console.error("Error writing document: ", error);
                    alert('Error! Gagal menyimpan transaksi ke database.');
                });
        }
        
        // --- FUNGSI STRUK/RECEIPT ---
        function showReceipt(transactionData) {
            // Format tanggal
            const date = new Date();
            const formattedDate = date.toLocaleDateString('id-ID') + ' ' + date.toLocaleTimeString('id-ID');
            
            // Simpan data transaksi saat ini
            currentTransactionData = transactionData;
            
            // Isi data struk
            document.getElementById('receipt-date').textContent = formattedDate;
            document.getElementById('receipt-trans-id').textContent = 'ID: ' + transactionData.transactionId;
            document.getElementById('receipt-subtotal').textContent = formatRupiah(transactionData.subtotal);
            document.getElementById('receipt-discount-percent').textContent = transactionData.discount;
            document.getElementById('receipt-discount-amount').textContent = formatRupiah(transactionData.discountAmount);
            document.getElementById('receipt-total').textContent = formatRupiah(transactionData.total);
            document.getElementById('receipt-cash').textContent = formatRupiah(transactionData.bayar);
            document.getElementById('receipt-change').textContent = formatRupiah(transactionData.kembalian);
            
            // Isi item-item
            let itemsHtml = '';
            transactionData.items.forEach(item => {
                itemsHtml += `
                    <div class="receipt-item">
                        <span>${item.name} (${item.qty}x)</span>
                        <span>${formatRupiah(item.price * item.qty)}</span>
                    </div>
                `;
            });
            document.getElementById('receipt-items').innerHTML = itemsHtml;
            
            // Update QR info panel
            document.getElementById('qr-info-id').textContent = transactionData.transactionId;
            document.getElementById('qr-info-date').textContent = formattedDate;
            document.getElementById('qr-info-items').textContent = transactionData.items.length;
            document.getElementById('qr-info-total').textContent = formatRupiah(transactionData.total);
            
            // Generate QR Code dengan gambar struk
            generateReceiptQR(transactionData, formattedDate);
            
            // Tampilkan modal struk
            receiptModal.show();
            
            // Tambahkan notifikasi
            setTimeout(() => {
                showToast('QR Code struk digital berhasil dibuat! Scan untuk menyimpan struk Anda.', 'success');
            }, 500);
        }
        
        // Generate QR Code dengan gambar struk
        async function generateReceiptQR(transactionData, formattedDate) {
            const qrContainer = document.getElementById('receipt-qr');
            qrContainer.innerHTML = ''; // Kosongkan dulu
            
            try {
                // Generate receipt image
                const receiptImage = await generateReceiptImage(transactionData, formattedDate);
                
                // Create QR data with image
                const qrData = {
                    type: 'receipt',
                    merchant: "WAROENG MASHER",
                    transactionId: transactionData.transactionId,
                    date: formattedDate,
                    receiptImage: receiptImage, // Base64 image
                    total: transactionData.total,
                    verifyUrl: `https://waroengmasher.firebaseapp.com/verify/${transactionData.transactionId}`
                };
                
                // Simpan QR data
                currentQRData = qrData;
                
                // Generate QR Code dengan ukuran besar untuk kemudahan scanning
                new QRCode(qrContainer, {
                    text: JSON.stringify(qrData),
                    width: 250,
                    height: 250,
                    colorDark: "#000000",
                    colorLight: "#ffffff",
                    correctLevel: QRCode.CorrectLevel.H,
                    margin: 1
                });
                
            } catch (error) {
                console.error('Error generating receipt QR:', error);
                
                // Fallback: QR without image
                const fallbackData = {
                    type: 'receipt',
                    merchant: "WAROENG MASHER",
                    transactionId: transactionData.transactionId,
                    date: formattedDate,
                    total: transactionData.total,
                    verifyUrl: `https://waroengmasher.firebaseapp.com/verify/${transactionData.transactionId}`
                };
                
                currentQRData = fallbackData;
                
                new QRCode(qrContainer, {
                    text: JSON.stringify(fallbackData),
                    width: 250,
                    height: 250,
                    colorDark: "#000000",
                    colorLight: "#ffffff",
                    correctLevel: QRCode.CorrectLevel.H,
                    margin: 1
                });
            }
        }
        
        // Generate receipt image using canvas
        async function generateReceiptImage(transactionData, formattedDate) {
            const canvas = document.getElementById('receipt-canvas');
            const ctx = canvas.getContext('2d');
            
            // Set canvas size
            canvas.width = 300;
            canvas.height = 400;
            
            // White background
            ctx.fillStyle = '#ffffff';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // Set font
            ctx.fillStyle = '#000000';
            ctx.textAlign = 'center';
            
            // Header
            ctx.font = 'bold 18px Courier New';
            ctx.fillText('WAROENG MASHER', canvas.width / 2, 30);
            
            ctx.font = '10px Courier New';
            ctx.fillText('Jl. pancing mabar,pasar 4[', canvas.width / 2, 50);
            ctx.fillText('Telp: ', canvas.width / 2, 65);
            ctx.fillText(formattedDate, canvas.width / 2, 80);
            ctx.fillText('ID: ' + transactionData.transactionId, canvas.width / 2, 95);
            
            // Draw line
            ctx.beginPath();
            ctx.moveTo(20, 105);
            ctx.lineTo(canvas.width - 20, 105);
            ctx.strokeStyle = '#000000';
            ctx.stroke();
            
            // Items
            ctx.textAlign = 'left';
            let yPosition = 125;
            
            transactionData.items.forEach(item => {
                ctx.font = '10px Courier New';
                ctx.fillText(`${item.name} (${item.qty}x)`, 20, yPosition);
                ctx.textAlign = 'right';
                ctx.fillText(formatRupiah(item.price * item.qty), canvas.width - 20, yPosition);
                ctx.textAlign = 'left';
                yPosition += 15;
            });
            
            // Draw line
            ctx.beginPath();
            ctx.moveTo(20, yPosition + 5);
            ctx.lineTo(canvas.width - 20, yPosition + 5);
            ctx.strokeStyle = '#000000';
            ctx.stroke();
            
            // Summary
            yPosition += 20;
            ctx.font = '10px Courier New';
            ctx.fillText('Subtotal:', 20, yPosition);
            ctx.textAlign = 'right';
            ctx.fillText(formatRupiah(transactionData.subtotal), canvas.width - 20, yPosition);
            
            yPosition += 15;
            ctx.textAlign = 'left';
            ctx.fillText(`Diskon (${transactionData.discount}%):`, 20, yPosition);
            ctx.textAlign = 'right';
            ctx.fillText(formatRupiah(transactionData.discountAmount), canvas.width - 20, yPosition);
            
            yPosition += 15;
            ctx.font = 'bold 12px Courier New';
            ctx.textAlign = 'left';
            ctx.fillText('TOTAL:', 20, yPosition);
            ctx.textAlign = 'right';
            ctx.fillText(formatRupiah(transactionData.total), canvas.width - 20, yPosition);
            
            yPosition += 20;
            ctx.font = '10px Courier New';
            ctx.textAlign = 'left';
            ctx.fillText('Tunai:', 20, yPosition);
            ctx.textAlign = 'right';
            ctx.fillText(formatRupiah(transactionData.bayar), canvas.width - 20, yPosition);
            
            yPosition += 15;
            ctx.textAlign = 'left';
            ctx.fillText('Kembalian:', 20, yPosition);
            ctx.textAlign = 'right';
            ctx.fillText(formatRupiah(transactionData.kembalian), canvas.width - 20, yPosition);
            
            // Footer
            ctx.beginPath();
            ctx.moveTo(20, yPosition + 15);
            ctx.lineTo(canvas.width - 20, yPosition + 15);
            ctx.strokeStyle = '#000000';
            ctx.stroke();
            
            yPosition += 30;
            ctx.textAlign = 'center';
            ctx.font = '10px Courier New';
            ctx.fillText('Terima Kasih', canvas.width / 2, yPosition);
            yPosition += 15;
            ctx.fillText('Selamat Berbelanja Kembali', canvas.width / 2, yPosition);
            
            // Convert canvas to base64
            return canvas.toDataURL('image/png');
        }
        
        // --- FUNGSI SHARE QR ---
        
        // Download QR Code
        function downloadQR() {
            if (!currentQRData) {
                showToast('QR Code belum tersedia!', 'error');
                return;
            }
            
            // Create QR code canvas
            const qrCanvas = document.createElement('canvas');
            const qrSize = 400;
            qrCanvas.width = qrSize;
            qrCanvas.height = qrSize;
            
            // Generate QR code on canvas
            new QRCode(qrCanvas, {
                text: JSON.stringify(currentQRData),
                width: qrSize,
                height: qrSize,
                colorDark: "#000000",
                colorLight: "#ffffff",
                correctLevel: QRCode.CorrectLevel.H
            });
            
            // Convert to blob and download
            qrCanvas.toBlob(function(blob) {
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = `QR-Struk-${currentTransactionData.transactionId}.png`;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
                
                showToast('QR Code berhasil diunduh!', 'success');
            });
        }
        
        // Copy QR Data
        async function copyQRData() {
            if (!currentQRData) {
                showToast('QR Code belum tersedia!', 'error');
                return;
            }
            
            try {
                // Copy transaction data as text
                const textData = `
STRUK PEMBAYARAN - WAROENG MASHER
ID Transaksi: ${currentTransactionData.transactionId}
Tanggal: ${new Date().toLocaleString('id-ID')}
Total: ${formatRupiah(currentTransactionData.total)}
Items: ${currentTransactionData.items.map(item => `${item.name} (${item.qty}x)`).join(', ')}
                `.trim();
                
                await navigator.clipboard.writeText(textData);
                showToast('Data struk berhasil disalin!', 'success');
            } catch (error) {
                console.error('Error copying data:', error);
                showToast('Gagal menyalin data!', 'error');
            }
        }
        
        // Share QR using Web Share API
        async function shareQR() {
            if (!currentQRData) {
                showToast('QR Code belum tersedia!', 'error');
                return;
            }
            
            // Create share data
            const shareData = {
                title: 'Struk Pembayaran - WAROENG MASHER',
                text: `ID: ${currentTransactionData.transactionId}\nTotal: ${formatRupiah(currentTransactionData.total)}\nTanggal: ${new Date().toLocaleString('id-ID')}`,
                url: currentQRData.verifyUrl || window.location.href
            };
            
            try {
                if (navigator.share) {
                    await navigator.share(shareData);
                    showToast('Struk berhasil dibagikan!', 'success');
                } else {
                    // Fallback: copy to clipboard
                    await copyQRData();
                }
            } catch (error) {
                console.error('Error sharing:', error);
                showToast('Gagal membagikan struk!', 'error');
            }
        }
        
        // Show Expanded QR
        function showExpandedQR() {
            if (!currentQRData || !currentTransactionData) {
                showToast('QR Code belum tersedia!', 'error');
                return;
            }
            
            // Generate larger QR code
            const expandedQRContainer = document.createElement('div');
            expandedQRContainer.style.width = '350px';
            expandedQRContainer.style.height = '350px';
            expandedQRContainer.style.margin = '0 auto';
            
            new QRCode(expandedQRContainer, {
                text: JSON.stringify(currentQRData),
                width: 350,
                height: 350,
                colorDark: "#000000",
                colorLight: "#ffffff",
                correctLevel: QRCode.CorrectLevel.H
            });
            
            // Update modal content
            document.getElementById('qr-expanded-image').src = expandedQRContainer.querySelector('img').src;
            
            // Update details
            const detailsHtml = `
                <div class="detail-item">
                    <span>ID Transaksi:</span>
                    <span>${currentTransactionData.transactionId}</span>
                </div>
                <div class="detail-item">
                    <span>Tanggal:</span>
                    <span>${new Date().toLocaleString('id-ID')}</span>
                </div>
                <div class="detail-item">
                    <span>Jumlah Item:</span>
                    <span>${currentTransactionData.items.length}</span>
                </div>
                <div class="detail-item">
                    <span>Subtotal:</span>
                    <span>${formatRupiah(currentTransactionData.subtotal)}</span>
                </div>
                ${currentTransactionData.discount > 0 ? `
                <div class="detail-item">
                    <span>Diskon (${currentTransactionData.discount}%):</span>
                    <span>${formatRupiah(currentTransactionData.discountAmount)}</span>
                </div>
                ` : ''}
                <div class="detail-item">
                    <span>Total Bayar:</span>
                    <span>${formatRupiah(currentTransactionData.total)}</span>
                </div>
                <div class="detail-item">
                    <span>Tunai:</span>
                    <span>${formatRupiah(currentTransactionData.bayar)}</span>
                </div>
                <div class="detail-item">
                    <span>Kembalian:</span>
                    <span>${formatRupiah(currentTransactionData.kembalian)}</span>
                </div>
            `;
            document.getElementById('qr-expanded-details').innerHTML = detailsHtml;
            
            // Show modal
            qrExpandedModal.show();
        }
        
        // Toast notification
        function showToast(message, type = 'info') {
            const toastContainer = document.getElementById('toast-container');
            const toast = document.createElement('div');
            toast.className = `custom-toast toast-${type}`;
            
            const icon = type === 'success' ? 'check-circle-fill' : 
                         type === 'error' ? 'x-circle-fill' : 
                         'info-circle-fill';
            
            toast.innerHTML = `
                <i class="bi bi-${icon}"></i>
                <span>${message}</span>
            `;
            
            toastContainer.appendChild(toast);
            
            // Auto remove after 3 seconds
            setTimeout(() => {
                toast.style.animation = 'slideIn 0.3s ease reverse';
                setTimeout(() => {
                    toast.remove();
                }, 300);
            }, 3000);
        }
        
        function printReceipt() {
            window.print();
        }
        
        // --- FUNGSI SEARCH ---
        function filterProducts() {
            const searchTerm = document.getElementById('menu-search-input').value.toLowerCase();
            const filtered = products.filter(product => 
                product.name.toLowerCase().includes(searchTerm) || 
                product.description.toLowerCase().includes(searchTerm)
            );
            renderProducts(filtered);
        }

        // --- INISIALISASI ---
        document.addEventListener('DOMContentLoaded', () => {
            renderProducts();
            renderCartPOS();
            // Panggil filterproducts saat mengetik di search bar
            document.getElementById('menu-search-input').addEventListener('input', filterProducts);
            
             // Mencegah form submission default pada input pembayaran
            bayarInput.closest('div').addEventListener('submit', (e) => e.preventDefault());
            
            // Set tombol diskon default
            document.querySelector('.discount-btn').classList.add('active');
        });
    </script>
</body>
</html>
