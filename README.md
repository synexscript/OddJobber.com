<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>m squre </title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Firebase SDK -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-app.js";
        import { getDatabase, ref, onValue, push, set, remove, get, update } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-database.js";

        const firebaseConfig = {
            apiKey: "AIzaSyA2ETid4xmCppgWKUbdR1iPhgYGUzGnkMQ",
            authDomain: "circles-go-digital.firebaseapp.com",
            databaseURL: "https://circles-go-digital-default-rtdb.asia-southeast1.firebasedatabase.app",
            projectId: "circles-go-digital",
            storageBucket: "circles-go-digital.firebasestorage.app",
            messagingSenderId: "132271508600",
            appId: "1:132271508600:web:dfb30daaa0eb79534bf18d",
            measurementId: "G-RQK3QVXQPR"
        };

        const app = initializeApp(firebaseConfig);
        const database = getDatabase(app);

        window.firebase = {
            database,
            ref,
            onValue,
            push,
            set,
            remove,
            get,
            update
        };

        console.log('Firebase initialized successfully');
    </script>

    <style>
        :root {
            --primary: #dc2626;
            --primary-dark: #b91c1c;
            --secondary: #f59e0b;
            --success: #10b981;
            --danger: #ef4444;
            --warning: #f59e0b;
            --bg: #ffffff;
            --card: #ffffff;
            --text: #1e293b;
            --text-light: #64748b;
            --border: #e2e8f0;
            --shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
            --shadow-lg: 0 8px 25px rgba(0, 0, 0, 0.15);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        }

        body {
            background: var(--bg);
            color: var(--text);
            line-height: 1.6;
            padding-bottom: 80px; /* Space for bottom nav */
        }

        /* Header */
        .app-header {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: white;
            padding: 16px;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: var(--shadow);
        }

        .header-top {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
        }

        .restaurant-info h1 {
            font-size: 20px;
            font-weight: 700;
            margin-bottom: 4px;
        }

        .restaurant-info p {
            font-size: 14px;
            opacity: 0.9;
        }

        .table-info {
            background: rgba(255,255,255,0.2);
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 14px;
            font-weight: 600;
        }

        .order-status-badge {
            background: rgba(255,255,255,0.9);
            color: var(--primary);
            padding: 8px 12px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 6px;
            margin-top: 8px;
        }

        /* Bottom Navigation */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: white;
            display: flex;
            padding: 12px 16px;
            border-top: 1px solid var(--border);
            z-index: 1000;
            box-shadow: 0 -2px 10px rgba(0,0,0,0.1);
        }

        .nav-item {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4px;
            padding: 8px;
            border: none;
            background: none;
            color: var(--text-light);
            font-size: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .nav-item.active {
            color: var(--primary);
        }

        .nav-item i {
            font-size: 20px;
        }

        /* Categories */
        .categories-scroll {
            display: flex;
            gap: 8px;
            padding: 16px;
            overflow-x: auto;
            scrollbar-width: none;
            background: white;
            border-bottom: 1px solid var(--border);
        }

        .categories-scroll::-webkit-scrollbar {
            display: none;
        }

        .category-btn {
            padding: 10px 16px;
            background: white;
            border: 1px solid var(--border);
            border-radius: 25px;
            cursor: pointer;
            font-weight: 600;
            color: var(--text-light);
            white-space: nowrap;
            font-size: 14px;
            transition: all 0.3s ease;
        }

        .category-btn.active {
            background: var(--primary);
            color: white;
            border-color: var(--primary);
        }

        /* Menu Grid */
        .menu-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 12px;
            padding: 16px;
        }

        .menu-item {
            background: white;
            border-radius: 16px;
            padding: 16px;
            box-shadow: var(--shadow);
            transition: all 0.3s ease;
            border: 1px solid var(--border);
        }

        .menu-item:hover {
            transform: translateY(-2px);
            box-shadow: var(--shadow-lg);
        }

        .menu-item.disabled {
            opacity: 0.6;
            cursor: not-allowed;
        }

        .menu-item-header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            margin-bottom: 8px;
        }

        .menu-item-name {
            font-weight: 700;
            font-size: 16px;
            color: var(--text);
            flex: 1;
            margin-right: 12px;
        }

        .menu-item-price {
            font-weight: 700;
            color: var(--primary);
            font-size: 18px;
        }

        .menu-item-category {
            color: var(--text-light);
            font-size: 12px;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 4px;
        }

        .menu-item-desc {
            color: var(--text-light);
            font-size: 14px;
            margin-bottom: 12px;
            line-height: 1.4;
        }

        .menu-item-actions {
            display: flex;
            gap: 8px;
            align-items: center;
            justify-content: space-between;
        }

        .quantity-controls {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .quantity-btn {
            width: 32px;
            height: 32px;
            border: 2px solid var(--primary);
            background: white;
            color: var(--primary);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-weight: bold;
            font-size: 16px;
            transition: all 0.3s ease;
        }

        .quantity-btn:active {
            background: var(--primary);
            color: white;
            transform: scale(0.95);
        }

        .quantity-display {
            font-weight: 600;
            min-width: 30px;
            text-align: center;
            font-size: 16px;
        }

        .add-to-cart-btn {
            flex: 1;
            padding: 10px 16px;
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: 600;
            font-size: 14px;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 6px;
        }

        .add-to-cart-btn:active {
            background: var(--primary-dark);
            transform: scale(0.98);
        }

        .added-to-cart {
            background: var(--success);
        }

        .added-to-cart:active {
            background: #0da271;
        }

        /* Cart Sidebar */
        .cart-sidebar {
            position: fixed;
            top: 0;
            right: -100%;
            width: 100%;
            height: 100vh;
            background: white;
            transition: right 0.3s ease;
            z-index: 2000;
            display: flex;
            flex-direction: column;
        }

        .cart-sidebar.open {
            right: 0;
        }

        .cart-header {
            padding: 20px 16px;
            border-bottom: 1px solid var(--border);
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: white;
        }

        .cart-items {
            flex: 1;
            overflow-y: auto;
            padding: 16px;
        }

        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid var(--border);
        }

        .cart-item-info {
            flex: 1;
        }

        .cart-item-name {
            font-weight: 600;
            margin-bottom: 4px;
        }

        .cart-item-price {
            color: var(--text-light);
            font-size: 14px;
        }

        .cart-item-actions {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .cart-total {
            padding: 20px 16px;
            border-top: 2px solid var(--border);
            background: #f8fafc;
        }

        .total-amount {
            font-size: 20px;
            font-weight: 700;
            color: var(--primary);
            text-align: center;
            margin-bottom: 16px;
        }

        .checkout-btn {
            width: 100%;
            padding: 16px;
            background: var(--success);
            color: white;
            border: none;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            transition: all 0.3s ease;
        }

        .checkout-btn:active {
            background: #0da271;
            transform: scale(0.98);
        }

        .checkout-btn:disabled {
            background: var(--text-light);
            cursor: not-allowed;
            transform: none;
        }

        .cart-fab {
            position: fixed;
            bottom: 80px;
            right: 16px;
            width: 60px;
            height: 60px;
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 50%;
            cursor: pointer;
            box-shadow: var(--shadow-lg);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            transition: all 0.3s ease;
            z-index: 999;
        }

        .cart-fab:active {
            transform: scale(0.95);
        }

        .cart-count {
            position: absolute;
            top: -5px;
            right: -5px;
            background: var(--danger);
            color: white;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            font-size: 12px;
            font-weight: 600;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 1999;
            display: none;
        }

        /* Order Success */
        .order-success {
            text-align: center;
            padding: 40px 20px;
            background: white;
            border-radius: 16px;
            box-shadow: var(--shadow);
            margin: 20px 16px;
        }

        .success-icon {
            font-size: 64px;
            color: var(--success);
            margin-bottom: 20px;
        }

        /* Order Tracking */
        .order-card {
            background: white;
            border-radius: 12px;
            padding: 16px;
            margin: 12px 16px;
            box-shadow: var(--shadow);
        }

        .order-status {
            display: inline-block;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            margin-left: 8px;
        }

        .status-pending {
            background: #fef3c7;
            color: #d97706;
        }

        .status-accepted {
            background: #dbeafe;
            color: #2563eb;
        }

        .status-cooking {
            background: #fef3c7;
            color: #d97706;
        }

        .status-ready {
            background: #d1fae5;
            color: #065f46;
        }

        .status-completed {
            background: #d1fae5;
            color: #065f46;
        }

        .status-cancelled {
            background: #fee2e2;
            color: #dc2626;
        }

        /* Table Selection */
        .table-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 3000;
            align-items: center;
            justify-content: center;
            padding: 16px;
        }

        .table-modal-content {
            background: white;
            border-radius: 20px;
            padding: 24px;
            width: 100%;
            max-width: 400px;
            box-shadow: var(--shadow-lg);
        }

        .table-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 8px;
            margin: 20px 0;
        }

        .table-btn {
            padding: 16px;
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            font-weight: 600;
            font-size: 16px;
            transition: all 0.3s ease;
        }

        .table-btn:active {
            transform: scale(0.95);
        }

        .table-btn.selected {
            background: var(--success);
            transform: scale(0.95);
        }

        /* Empty State */
        .empty-state {
            text-align: center;
            padding: 60px 20px;
            color: var(--text-light);
        }

        .empty-icon {
            font-size: 48px;
            margin-bottom: 16px;
            opacity: 0.5;
        }

        /* Swipeable Tabs */
        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        /* Loading States */
        .skeleton {
            background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
            background-size: 200% 100%;
            animation: loading 1.5s infinite;
            border-radius: 8px;
        }

        @keyframes loading {
            0% { background-position: 200% 0; }
            100% { background-position: -200% 0; }
        }

        /* Responsive */
        @media (min-width: 768px) {
            .menu-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .cart-sidebar {
                width: 400px;
                right: -400px;
            }
        }

        @media (min-width: 1024px) {
            .menu-grid {
                grid-template-columns: repeat(3, 1fr);
            }
        }

        /* Touch Improvements */
        @media (hover: none) {
            .menu-item:hover {
                transform: none;
            }
            
            .quantity-btn:hover {
                background: white;
                color: var(--primary);
            }
            
            .add-to-cart-btn:hover {
                background: var(--primary);
            }
        }
    </style>
</head>
<body>
    <!-- Table Selection Modal -->
    <div id="tableModal" class="table-modal">
        <div class="table-modal-content">
            <h2 style="text-align: center; margin-bottom: 16px;">
                <i class="fas fa-table"></i> Select Your Table
            </h2>
            <p style="text-align: center; color: var(--text-light); margin-bottom: 20px;">
                Please select your table number to continue
            </p>
            <div class="table-grid" id="tableGrid">
                <!-- Tables will be generated here -->
            </div>
            <button class="checkout-btn" onclick="useCurrentTable()" style="margin-top: 20px;">
                <i class="fas fa-check"></i> Confirm Table
            </button>
        </div>
    </div>

    <!-- App Header -->
    <div class="app-header">
        <div class="header-top">
            <div class="restaurant-info">
                <h1>CircleApps Restaurant</h1>
                <p>Order your favorite food</p>
            </div>
            <div class="table-info" id="tableInfo">
                <i class="fas fa-table"></i>
                Table: Not Selected
            </div>
        </div>
        <div id="orderStatusBadge" class="order-status-badge" style="display: none;">
            <i class="fas fa-clock"></i>
            <span id="currentOrderStatusText">No active orders</span>
        </div>
    </div>

    <!-- Main Content -->
    <div id="tab-menu" class="tab-content active">
        <!-- Categories -->
        <div class="categories-scroll" id="categoriesNav">
            <button class="category-btn active" onclick="filterCategory('all')">
                <i class="fas fa-star"></i> All
            </button>
        </div>

        <!-- Menu Grid -->
        <div id="menuGrid" class="menu-grid">
            <div class="empty-state">
                <div class="empty-icon">
                    <i class="fas fa-utensils"></i>
                </div>
                <h3>Loading Menu...</h3>
                <p>Please wait while we load the menu</p>
            </div>
        </div>
    </div>

    <!-- Orders Tab -->
    <div id="tab-orders" class="tab-content">
        <div id="orderTracking">
            <div class="empty-state">
                <div class="empty-icon">
                    <i class="fas fa-receipt"></i>
                </div>
                <h3>No Active Orders</h3>
                <p>Your order status will appear here</p>
            </div>
        </div>
    </div>

    <!-- Order Success -->
    <div id="orderSuccess" class="order-success" style="display: none;">
        <div class="success-icon">
            <i class="fas fa-check-circle"></i>
        </div>
        <h2>Order Placed Successfully!</h2>
        <p>Your order has been received and is being prepared.</p>
        <button class="checkout-btn" onclick="continueOrdering()" style="margin-top: 20px; background: var(--primary);">
            <i class="fas fa-utensils"></i> Continue Ordering
        </button>
    </div>

    <!-- Cart Sidebar -->
    <div class="cart-sidebar" id="cartSidebar">
        <div class="cart-header">
            <h3><i class="fas fa-shopping-cart"></i> Your Order</h3>
            <button class="quantity-btn" onclick="toggleCart()" style="border: none; background: var(--danger); color: white;">
                <i class="fas fa-times"></i>
            </button>
        </div>
        <div class="cart-items" id="cartItems">
            <div class="empty-state">
                <div class="empty-icon">
                    <i class="fas fa-shopping-cart"></i>
                </div>
                <h3>Your cart is empty</h3>
                <p>Add some delicious items to get started</p>
            </div>
        </div>
        <div class="cart-total">
            <div class="total-amount" id="cartTotal">Total: ₹0</div>
            <button class="checkout-btn" id="checkoutBtn" onclick="placeOrder()" disabled>
                <i class="fas fa-paper-plane"></i> Place Order
            </button>
        </div>
    </div>

    <div class="overlay" id="overlay" onclick="toggleCart()"></div>

    <!-- Cart FAB -->
    <button class="cart-fab" onclick="toggleCart()">
        <i class="fas fa-shopping-cart"></i>
        <span class="cart-count" id="cartCount">0</span>
    </button>

    <!-- Bottom Navigation -->
    <div class="bottom-nav">
        <button class="nav-item active" onclick="switchTab('menu')">
            <i class="fas fa-utensils"></i>
            <span>Menu</span>
        </button>
        <button class="nav-item" onclick="switchTab('orders')">
            <i class="fas fa-receipt"></i>
            <span>Orders</span>
        </button>
        <button class="nav-item" onclick="toggleCart()">
            <i class="fas fa-shopping-cart"></i>
            <span>Cart</span>
        </button>
    </div>

    <script>
        // Global variables
        let menuItems = {};
        let cart = [];
        let currentTable = '';
        let currentCategory = 'all';
        let orders = [];
        let restaurantId = 'restaurant_001';
        let hasTableBeenSet = false;
        let currentTab = 'menu';

        // Initialize
        document.addEventListener('DOMContentLoaded', function() {
            console.log('Mobile Customer App initialized');
            getTableFromURL();
            setupFirebaseListeners();
            setupEventListeners();
            generateTableSelection();
            
            // Add touch improvements
            setupTouchEvents();
        });

        // Setup touch events for better mobile experience
        function setupTouchEvents() {
            // Prevent zoom on double tap
            let lastTouchEnd = 0;
            document.addEventListener('touchend', function (event) {
                const now = (new Date()).getTime();
                if (now - lastTouchEnd <= 300) {
                    event.preventDefault();
                }
                lastTouchEnd = now;
            }, false);

            // Add touch feedback
            document.addEventListener('touchstart', function() {}, {passive: true});
        }

        // Get table number from URL
        function getTableFromURL() {
            const urlParams = new URLSearchParams(window.location.search);
            const urlTable = urlParams.get('table');
            
            if (urlTable) {
                currentTable = 'Table ' + urlTable;
                document.getElementById('tableInfo').innerHTML = `
                    <i class="fas fa-table"></i>
                    ${currentTable}
                `;
                hasTableBeenSet = true;
            } else {
                // Show table selection modal if no table in URL
                setTimeout(() => {
                    document.getElementById('tableModal').style.display = 'flex';
                }, 500);
            }
        }

        // Generate table selection buttons
        function generateTableSelection() {
            const tableGrid = document.getElementById('tableGrid');
            let tablesHTML = '';
            
            for (let i = 1; i <= 12; i++) {
                tablesHTML += `
                    <button class="table-btn" onclick="selectTable(${i})">
                        Table ${i}
                    </button>
                `;
            }
            
            tableGrid.innerHTML = tablesHTML;
        }

        // Select table
        function selectTable(tableNumber) {
            currentTable = 'Table ' + tableNumber;
            document.querySelectorAll('.table-btn').forEach(btn => {
                btn.classList.remove('selected');
            });
            event.target.classList.add('selected');
        }

        // Use current table
        function useCurrentTable() {
            if (currentTable) {
                document.getElementById('tableInfo').innerHTML = `
                    <i class="fas fa-table"></i>
                    ${currentTable}
                `;
                document.getElementById('tableModal').style.display = 'none';
                hasTableBeenSet = true;
                
                // Show success feedback
                showNotification('Table selected successfully!', 'success');
            } else {
                showNotification('Please select a table first', 'error');
            }
        }

        // Switch between tabs
        function switchTab(tabName) {
            currentTab = tabName;
            
            // Update nav active state
            document.querySelectorAll('.nav-item').forEach(item => {
                item.classList.remove('active');
            });
            event.target.classList.add('active');
            
            // Update tab content
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            document.getElementById('tab-' + tabName).classList.add('active');
        }

        // Setup Firebase listeners
        function setupFirebaseListeners() {
            const { database, ref, onValue } = window.firebase;
            
            // Listen to menuItems from Firebase
            onValue(ref(database, 'menuItems'), (snapshot) => {
                const data = snapshot.val();
                menuItems = data || {};
                console.log('Menu items loaded from Firebase:', Object.keys(menuItems).length);
                
                if (Object.keys(menuItems).length > 0) {
                    displayMenuItems();
                    updateCategoriesFromMenuItems();
                } else {
                    loadSampleMenuItems();
                }
            });

            // Listen to orders from Firebase
            onValue(ref(database, 'orders'), (snapshot) => {
                const data = snapshot.val();
                orders = data ? Object.values(data) : [];
                console.log('Orders loaded from Firebase:', orders.length);
                
                updateCustomerOrderStatus();
                updateOrderStatusBadge();
            });
        }

        // Load sample data if Firebase is empty
        function loadSampleMenuItems() {
            const sampleItems = {
                'item1': { id: 'item1', name: 'Margherita Pizza', price: 299, category: 'Main Course', description: 'Classic pizza with tomato sauce and mozzarella', available: true },
                'item2': { id: 'item2', name: 'Garlic Bread', price: 149, category: 'Starters', description: 'Freshly baked bread with garlic butter', available: true },
                'item3': { id: 'item3', name: 'Chocolate Lava Cake', price: 179, category: 'Desserts', description: 'Warm chocolate cake with molten center', available: true },
                'item4': { id: 'item4', name: 'Fresh Lime Soda', price: 89, category: 'Drinks', description: 'Refreshing lime soda with mint', available: true },
                'item5': { id: 'item5', name: 'Pasta Alfredo', price: 249, category: 'Main Course', description: 'Creamy pasta with parmesan cheese', available: true },
                'item6': { id: 'item6', name: 'Caesar Salad', price: 199, category: 'Starters', description: 'Fresh greens with caesar dressing', available: false }
            };
            
            // Save sample data to Firebase
            Object.keys(sampleItems).forEach(itemId => {
                saveMenuItemToFirebase(sampleItems[itemId]);
            });
            
            displayMenuItems();
            updateCategoriesFromMenuItems();
        }

        // Save menu item to Firebase
        function saveMenuItemToFirebase(item) {
            const { database, ref, set } = window.firebase;
            set(ref(database, 'menuItems/' + item.id), item);
        }

        // Save order to Firebase
        function saveOrderToFirebase(order) {
            const { database, ref, push, set } = window.firebase;
            const newOrderRef = push(ref(database, 'orders'));
            order.firebaseId = newOrderRef.key;
            order.id = 'ORD' + Date.now();
            order.restaurantId = restaurantId;
            order.table = currentTable;
            order.status = 'pending';
            order.createdAt = new Date().toISOString();
            set(newOrderRef, order);
            return order.firebaseId;
        }

        // Setup event listeners
        function setupEventListeners() {
            // Add swipe support for tabs
            setupSwipeEvents();
        }

        // Setup swipe events for tab switching
        function setupSwipeEvents() {
            let startX = 0;
            let endX = 0;
            
            document.addEventListener('touchstart', e => {
                startX = e.changedTouches[0].screenX;
            });
            
            document.addEventListener('touchend', e => {
                endX = e.changedTouches[0].screenX;
                handleSwipe();
            });
            
            function handleSwipe() {
                const diff = startX - endX;
                const minSwipe = 50; // Minimum swipe distance
                
                if (Math.abs(diff) > minSwipe) {
                    if (diff > 0 && currentTab === 'orders') {
                        // Swipe left on orders -> go to menu
                        switchTab('menu');
                        document.querySelector('.nav-item').click();
                    } else if (diff < 0 && currentTab === 'menu') {
                        // Swipe right on menu -> go to orders
                        switchTab('orders');
                        document.querySelectorAll('.nav-item')[1].click();
                    }
                }
            }
        }

        // Extract categories from menu items
        function updateCategoriesFromMenuItems() {
            const nav = document.getElementById('categoriesNav');
            const categories = new Set(['all']);
            
            // Get categories from menu items
            Object.values(menuItems).forEach(item => {
                if (item && item.category) {
                    categories.add(item.category);
                }
            });
            
            const categoriesArray = Array.from(categories);
            
            nav.innerHTML = categoriesArray.map(category => `
                <button class="category-btn ${category === currentCategory ? 'active' : ''}" 
                        onclick="filterCategory('${category}')">
                    <i class="fas ${category === 'all' ? 'fa-star' : 'fa-tag'}"></i> 
                    ${category === 'all' ? 'All' : category}
                </button>
            `).join('');
        }

        // Display menu items
        function displayMenuItems() {
            const container = document.getElementById('menuGrid');
            const itemsArray = Object.values(menuItems);
            
            const filteredItems = currentCategory === 'all' 
                ? itemsArray 
                : itemsArray.filter(item => item && item.category === currentCategory);

            if (filteredItems.length === 0) {
                container.innerHTML = `
                    <div class="empty-state">
                        <div class="empty-icon">
                            <i class="fas fa-utensils"></i>
                        </div>
                        <h3>No ${currentCategory === 'all' ? '' : currentCategory + ' '}Items</h3>
                        <p>No items available in this category</p>
                    </div>
                `;
                return;
            }

            container.innerHTML = Object.keys(menuItems).map(itemId => {
                const item = menuItems[itemId];
                if (!item || (currentCategory !== 'all' && item.category !== currentCategory)) return '';
                
                const cartItem = cart.find(ci => ci.id === itemId);
                const quantity = cartItem ? cartItem.quantity : 0;
                const isAvailable = item.available !== false;
                const isInCart = quantity > 0;
                
                return `
                    <div class="menu-item ${!isAvailable ? 'disabled' : ''}">
                        <div class="menu-item-header">
                            <div class="menu-item-name">${item.name || 'Unnamed Item'}</div>
                            <div class="menu-item-price">₹${item.price || 0}</div>
                        </div>
                        <div class="menu-item-category">
                            <i class="fas fa-tag"></i>
                            ${item.category || 'General'}
                        </div>
                        <div class="menu-item-desc">${item.description || 'Delicious food item'}</div>
                        ${isAvailable ? `
                            <div class="menu-item-actions">
                                <div class="quantity-controls">
                                    <button class="quantity-btn" onclick="removeFromCart('${itemId}')">-</button>
                                    <span class="quantity-display">${quantity}</span>
                                    <button class="quantity-btn" onclick="addToCart('${itemId}')">+</button>
                                </div>
                                <button class="add-to-cart-btn ${isInCart ? 'added-to-cart' : ''}" onclick="addToCart('${itemId}')">
                                    <i class="fas ${isInCart ? 'fa-check' : 'fa-plus'}"></i> ${isInCart ? 'Added' : 'Add'}
                                </button>
                            </div>
                        ` : `
                            <div style="color: var(--danger); text-align: center; padding: 8px;">
                                <i class="fas fa-times-circle"></i> Out of Stock
                            </div>
                        `}
                    </div>
                `;
            }).join('');
        }

        // Filter by category
        function filterCategory(category) {
            currentCategory = category;
            document.querySelectorAll('.category-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            displayMenuItems();
        }

        // Cart functions
        function addToCart(itemId) {
            if (!hasTableBeenSet) {
                document.getElementById('tableModal').style.display = 'flex';
                showNotification('Please select a table first', 'error');
                return;
            }
            
            const item = menuItems[itemId];
            if (!item) return;
            
            const existingItem = cart.find(ci => ci.id === itemId);
            if (existingItem) {
                existingItem.quantity += 1;
            } else {
                cart.push({
                    id: itemId,
                    name: item.name,
                    price: item.price,
                    quantity: 1
                });
            }
            updateCart();
            displayMenuItems(); // Refresh to update button text
            
            // Haptic feedback for mobile
            if (navigator.vibrate) {
                navigator.vibrate(50);
            }
        }

        function removeFromCart(itemId) {
            const itemIndex = cart.findIndex(item => item.id === itemId);
            if (itemIndex !== -1) {
                if (cart[itemIndex].quantity > 1) {
                    cart[itemIndex].quantity -= 1;
                } else {
                    cart.splice(itemIndex, 1);
                }
            }
            updateCart();
            displayMenuItems(); // Refresh to update button text
            
            // Haptic feedback for mobile
            if (navigator.vibrate) {
                navigator.vibrate(50);
            }
        }

        function updateCart() {
            const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
            document.getElementById('cartCount').textContent = totalItems;

            const cartTotal = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            document.getElementById('cartTotal').textContent = `Total: ₹${cartTotal.toFixed(2)}`;
            document.getElementById('checkoutBtn').disabled = cart.length === 0;

            const cartItemsContainer = document.getElementById('cartItems');
            if (cart.length === 0) {
                cartItemsContainer.innerHTML = `
                    <div class="empty-state">
                        <div class="empty-icon">
                            <i class="fas fa-shopping-cart"></i>
                        </div>
                        <h3>Your cart is empty</h3>
                        <p>Add some delicious items to get started</p>
                    </div>
                `;
            } else {
                cartItemsContainer.innerHTML = cart.map(item => `
                    <div class="cart-item">
                        <div class="cart-item-info">
                            <div class="cart-item-name">${item.name}</div>
                            <div class="cart-item-price">₹${item.price} x ${item.quantity}</div>
                        </div>
                        <div class="cart-item-actions">
                            <button class="quantity-btn" onclick="removeFromCart('${item.id}')">-</button>
                            <span class="quantity-display">${item.quantity}</span>
                            <button class="quantity-btn" onclick="addToCart('${item.id}')">+</button>
                        </div>
                    </div>
                `).join('');
            }
        }

        function toggleCart() {
            const cartSidebar = document.getElementById('cartSidebar');
            const overlay = document.getElementById('overlay');
            cartSidebar.classList.toggle('open');
            overlay.style.display = overlay.style.display === 'block' ? 'none' : 'block';
            
            // Prevent body scroll when cart is open
            document.body.style.overflow = cartSidebar.classList.contains('open') ? 'hidden' : '';
        }

        // Place order
        function placeOrder() {
            if (cart.length === 0) return;
            if (!hasTableBeenSet) {
                showNotification('Please select a table first', 'error');
                document.getElementById('tableModal').style.display = 'flex';
                return;
            }
            
            const order = {
                id: 'ORD' + Date.now(),
                table: currentTable,
                restaurantId: restaurantId,
                items: [...cart],
                total: cart.reduce((sum, item) => sum + (item.price * item.quantity), 0),
                status: 'pending',
                createdAt: new Date().toISOString()
            };

            // Save to Firebase
            saveOrderToFirebase(order);
            
            // Show success message
            cart = [];
            updateCart();
            toggleCart();
            document.getElementById('orderSuccess').style.display = 'block';
            document.getElementById('tab-menu').style.display = 'none';
            
            // Switch to orders tab
            switchTab('orders');
            document.querySelectorAll('.nav-item')[1].click();
            
            showNotification('Order placed successfully!', 'success');
        }

        function continueOrdering() {
            document.getElementById('orderSuccess').style.display = 'none';
            document.getElementById('tab-menu').style.display = 'block';
            switchTab('menu');
            document.querySelector('.nav-item').click();
        }

        // Update customer order status
        function updateCustomerOrderStatus() {
            const container = document.getElementById('orderTracking');
            const myOrders = orders.filter(order => 
                order.table === currentTable && order.status !== 'completed' && order.status !== 'cancelled'
            );
            
            if (myOrders.length === 0) {
                container.innerHTML = `
                    <div class="empty-state">
                        <div class="empty-icon">
                            <i class="fas fa-receipt"></i>
                        </div>
                        <h3>No Active Orders</h3>
                        <p>Your order status will appear here</p>
                    </div>
                `;
                return;
            }

            container.innerHTML = myOrders.map(order => {
                const statusText = {
                    'pending': 'Order Received',
                    'accepted': 'Order Accepted',
                    'cooking': 'Cooking in Progress',
                    'ready': 'Ready for Serving',
                    'completed': 'Order Completed'
                }[order.status] || order.status;
                
                const statusClass = `status-${order.status}`;
                const elapsedTime = getElapsedTime(order.createdAt);
                
                return `
                    <div class="order-card">
                        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
                            <h4 style="font-size: 16px;">Order #${order.id}</h4>
                            <span class="order-status ${statusClass}">${statusText}</span>
                        </div>
                        <div style="color: var(--text-light); margin-bottom: 12px; font-size: 14px;">
                            ${order.items.map(item => `${item.quantity}x ${item.name}`).join(', ')}
                        </div>
                        <div style="display: flex; justify-content: space-between; align-items: center;">
                            <div style="font-weight: 600; color: var(--primary);">₹${order.total}</div>
                            <div style="font-size: 12px; color: var(--text-light);">
                                ${elapsedTime}
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        // Calculate elapsed time
        function getElapsedTime(createdAt) {
            const created = new Date(createdAt);
            const now = new Date();
            const diffMs = now - created;
            const diffMins = Math.floor(diffMs / 60000);
            const diffHours = Math.floor(diffMins / 60);
            
            if (diffHours > 0) {
                return `${diffHours}h ${diffMins % 60}m ago`;
            } else if (diffMins > 0) {
                return `${diffMins}m ago`;
            } else {
                return 'Just now';
            }
        }

        // Update order status badge in header
        function updateOrderStatusBadge() {
            const badge = document.getElementById('orderStatusBadge');
            const statusText = document.getElementById('currentOrderStatusText');
            
            const myActiveOrders = orders.filter(order => 
                order.table === currentTable && order.status !== 'completed' && order.status !== 'cancelled'
            );
            
            if (myActiveOrders.length === 0) {
                badge.style.display = 'none';
                return;
            }
            
            badge.style.display = 'flex';
            
            // Get the most recent order
            const latestOrder = myActiveOrders.reduce((latest, order) => {
                return new Date(order.createdAt) > new Date(latest.createdAt) ? order : latest;
            }, myActiveOrders[0]);
            
            const statusMap = {
                'pending': { text: 'Order Received', icon: 'fa-clock', color: '#d97706' },
                'accepted': { text: 'Order Accepted', icon: 'fa-check', color: '#2563eb' },
                'cooking': { text: 'Cooking', icon: 'fa-fire', color: '#d97706' },
                'ready': { text: 'Ready to Serve', icon: 'fa-check-circle', color: '#065f46' }
            };
            
            const statusInfo = statusMap[latestOrder.status] || { text: latestOrder.status, icon: 'fa-clock', color: '#64748b' };
            
            statusText.textContent = statusInfo.text;
            badge.innerHTML = `<i class="fas ${statusInfo.icon}"></i> <span id="currentOrderStatusText">${statusInfo.text}</span>`;
            badge.style.background = `rgba(255,255,255,0.9)`;
            badge.style.color = statusInfo.color;
        }

        // Show notification
        function showNotification(message, type) {
            // Create notification element
            const notification = document.createElement('div');
            notification.style.cssText = `
                position: fixed;
                top: 20px;
                left: 50%;
                transform: translateX(-50%);
                background: ${type === 'success' ? '#10b981' : type === 'error' ? '#ef4444' : '#6366f1'};
                color: white;
                padding: 12px 20px;
                border-radius: 10px;
                box-shadow: var(--shadow-lg);
                z-index: 3000;
                animation: slideInDown 0.3s ease;
                max-width: 90%;
                text-align: center;
                font-weight: 600;
            `;
            notification.textContent = message;
            
            document.body.appendChild(notification);
            
            // Remove after 3 seconds
            setTimeout(() => {
                notification.style.animation = 'slideOutUp 0.3s ease';
                setTimeout(() => {
                    if (notification.parentNode) {
                        document.body.removeChild(notification);
                    }
                }, 300);
            }, 3000);
        }

        // Add CSS for notifications
        const style = document.createElement('style');
        style.textContent = `
            @keyframes slideInDown {
                from { transform: translateX(-50%) translateY(-100%); opacity: 0; }
                to { transform: translateX(-50%) translateY(0); opacity: 1; }
            }
            @keyframes slideOutUp {
                from { transform: translateX(-50%) translateY(0); opacity: 1; }
                to { transform: translateX(-50%) translateY(-100%); opacity: 0; }
            }
        `;
        document.head.appendChild(style);
    </script>
</body>
</html>