<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OddJobber - Find Local Service Providers</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-auth-compat.js"></script>
    <style>
        :root {
            --primary: #4361ee;
            --primary-light: #4895ef;
            --secondary: #3a0ca3;
            --accent: #f72585;
            --success: #4cc9f0;
            --warning: #ffd166;
            --danger: #ef476f;
            --light: #f8f9fa;
            --dark: #212529;
            --gray: #6c757d;
            --light-gray: #e9ecef;
            --card-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
            --transition: all 0.3s ease;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            color: var(--dark);
            line-height: 1.6;
            padding: 0;
        }

        .container {
            max-width: 100%;
            margin: 0 auto;
            padding: 15px;
        }

        header {
            background: white;
            border-radius: 12px;
            box-shadow: var(--card-shadow);
            padding: 15px;
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 10px;
            z-index: 100;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 20px;
            font-weight: 700;
            color: var(--primary);
        }

        .logo i {
            color: var(--accent);
            font-size: 24px;
        }

        nav {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .nav-btn {
            background: transparent;
            border: none;
            padding: 8px 12px;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 14px;
        }

        .nav-btn:hover {
            background: var(--light);
            transform: translateY(-2px);
        }

        .nav-btn.primary {
            background: var(--primary);
            color: white;
        }

        .nav-btn.primary:hover {
            background: var(--secondary);
        }

        .card {
            background: white;
            border-radius: 12px;
            box-shadow: var(--card-shadow);
            padding: 20px;
            margin-bottom: 20px;
            transition: var(--transition);
            animation: fadeIn 0.5s ease;
        }

        .card:hover {
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
            transform: translateY(-3px);
        }

        h1, h2, h3, h4 {
            margin-bottom: 12px;
            color: var(--dark);
        }

        h1 {
            font-size: 28px;
        }

        h2 {
            font-size: 22px;
        }

        h3 {
            font-size: 20px;
        }

        p {
            margin-bottom: 15px;
            color: var(--gray);
            font-size: 15px;
        }

        .grid {
            display: grid;
            gap: 20px;
        }

        .grid-2 {
            grid-template-columns: 1fr;
        }

        .form-group {
            margin-bottom: 16px;
        }

        label {
            display: block;
            margin-bottom: 6px;
            font-weight: 600;
            color: var(--dark);
            font-size: 14px;
        }

        input, select, textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid var(--light-gray);
            border-radius: 8px;
            font-size: 16px;
            transition: var(--transition);
            -webkit-appearance: none;
        }

        input:focus, select:focus, textarea:focus {
            border-color: var(--primary);
            outline: none;
            box-shadow: 0 0 0 3px rgba(67, 97, 238, 0.2);
        }

        .btn {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 6px;
            padding: 12px 18px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
        }

        .btn-primary {
            background: var(--primary);
            color: white;
        }

        .btn-primary:hover {
            background: var(--secondary);
            transform: translateY(-2px);
            box-shadow: 0 6px 15px rgba(67, 97, 238, 0.3);
        }

        .btn-secondary {
            background: var(--light);
            color: var(--dark);
        }

        .btn-secondary:hover {
            background: var(--light-gray);
            transform: translateY(-2px);
        }

        .btn-success {
            background: var(--success);
            color: white;
        }

        .btn-success:hover {
            background: #3aa5d3;
            transform: translateY(-2px);
        }

        .btn-danger {
            background: var(--danger);
            color: white;
        }

        .btn-danger:hover {
            background: #d93654;
            transform: translateY(-2px);
        }

        .btn-block {
            display: block;
            width: 100%;
        }

        .job-card {
            border: 1px solid var(--light-gray);
            border-radius: 12px;
            padding: 16px;
            margin-bottom: 16px;
            transition: var(--transition);
            animation: slideUp 0.4s ease;
        }

        .job-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
        }

        .job-header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            margin-bottom: 12px;
            flex-wrap: wrap;
            gap: 8px;
        }

        .job-title {
            font-size: 18px;
            font-weight: 700;
            color: var(--dark);
        }

        .job-price {
            font-size: 18px;
            font-weight: 700;
            color: var(--success);
        }

        .job-category {
            display: inline-block;
            padding: 5px 10px;
            background: var(--light);
            color: var(--primary);
            border-radius: 20px;
            font-size: 13px;
            font-weight: 600;
            margin-bottom: 12px;
        }

        .job-description {
            color: var(--gray);
            margin-bottom: 12px;
            line-height: 1.6;
            font-size: 14px;
        }

        .job-details {
            display: flex;
            gap: 12px;
            margin-bottom: 12px;
            flex-wrap: wrap;
        }

        .job-detail {
            display: flex;
            align-items: center;
            gap: 5px;
            color: var(--gray);
            font-size: 13px;
        }

        .job-detail i {
            color: var(--primary);
            font-size: 14px;
        }

        .job-footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 8px;
        }

        .job-poster {
            color: var(--primary);
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 5px;
            font-size: 14px;
        }

        .job-actions {
            display: flex;
            gap: 8px;
            width: 100%;
            justify-content: space-between;
        }

        .job-actions button {
            flex: 1;
        }

        .feature-section {
            text-align: center;
            padding: 30px 0;
        }

        .features {
            display: flex;
            justify-content: center;
            gap: 20px;
            flex-wrap: wrap;
            margin-top: 30px;
        }

        .feature {
            background: white;
            border-radius: 12px;
            padding: 20px;
            width: 100%;
            max-width: 280px;
            box-shadow: var(--card-shadow);
            transition: var(--transition);
        }

        .feature:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 25px rgba(0, 0, 0, 0.1);
        }

        .feature i {
            font-size: 36px;
            color: var(--primary);
            margin-bottom: 12px;
        }

        .feature h3 {
            margin-bottom: 8px;
            font-size: 18px;
        }

        .feature p {
            font-size: 14px;
        }

        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 12px 16px;
            background: white;
            border-left: 4px solid var(--success);
            border-radius: 8px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            display: flex;
            align-items: center;
            gap: 8px;
            transform: translateX(100%);
            opacity: 0;
            transition: var(--transition);
            z-index: 1000;
            max-width: 90%;
        }

        .notification.show {
            transform: translateX(0);
            opacity: 1;
        }

        .notification i {
            color: var(--success);
            font-size: 18px;
        }

        .notification.error {
            border-left-color: var(--danger);
        }

        .notification.error i {
            color: var(--danger);
        }

        .hidden {
            display: none;
        }

        .profile-info {
            margin-bottom: 25px;
        }

        .profile-info-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid var(--light-gray);
        }

        .profile-info-item:last-child {
            border-bottom: none;
        }

        .info-label {
            font-weight: 600;
            color: var(--dark);
            font-size: 14px;
        }

        .info-value {
            color: var(--gray);
            font-size: 14px;
        }

        .category-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
            gap: 10px;
            margin-top: 15px;
        }

        .category-item {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 10px;
            background: var(--light);
            border-radius: 8px;
            cursor: pointer;
            transition: var(--transition);
            font-size: 14px;
        }

        .category-item:hover {
            background: var(--primary-light);
            color: white;
        }

        .category-item.selected {
            background: var(--primary);
            color: white;
        }

        .category-item i {
            font-size: 16px;
        }

        .application-card {
            border: 1px solid var(--light-gray);
            border-radius: 12px;
            padding: 16px;
            margin-bottom: 16px;
            background: white;
            animation: slideUp 0.4s ease;
        }

        .application-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
            flex-wrap: wrap;
            gap: 8px;
        }

        .application-title {
            font-size: 16px;
            font-weight: 700;
            color: var(--dark);
        }

        .application-status {
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
        }

        .status-pending {
            background: var(--warning);
            color: var(--dark);
        }

        .status-accepted {
            background: var(--success);
            color: white;
        }

        .status-completed {
            background: var(--primary);
            color: white;
        }

        .loader {
            border: 3px solid var(--light-gray);
            border-top: 3px solid var(--primary);
            border-radius: 50%;
            width: 30px;
            height: 30px;
            animation: spin 1s linear infinite;
            margin: 20px auto;
        }

        .mobile-tabs {
            display: none;
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: white;
            box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.1);
            z-index: 90;
            padding: 10px;
        }

        .tab-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 8px;
            border-radius: 8px;
            transition: var(--transition);
            color: var(--gray);
            text-decoration: none;
            font-size: 12px;
        }

        .tab-item.active {
            color: var(--primary);
            background: rgba(67, 97, 238, 0.1);
        }

        .tab-item i {
            font-size: 20px;
            margin-bottom: 4px;
        }

        /* Animations */
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Responsive styles */
        @media (max-width: 768px) {
            .container {
                padding: 10px;
            }
            
            header {
                padding: 12px;
                margin-bottom: 15px;
            }
            
            .logo {
                font-size: 18px;
            }
            
            .logo i {
                font-size: 22px;
            }
            
            nav {
                display: none;
            }
            
            .mobile-tabs {
                display: flex;
                justify-content: space-around;
            }
            
            .card {
                padding: 16px;
                margin-bottom: 15px;
            }
            
            h1 {
                font-size: 24px;
            }
            
            h2 {
                font-size: 20px;
            }
            
            .job-card {
                padding: 14px;
            }
            
            .job-title {
                font-size: 16px;
            }
            
            .job-price {
                font-size: 16px;
            }
            
            .feature {
                padding: 16px;
            }
            
            .feature i {
                font-size: 32px;
            }
            
            .category-grid {
                grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
            }
            
            .btn {
                padding: 10px 16px;
                font-size: 15px;
            }
        }

        @media (min-width: 480px) {
            .grid-2 {
                grid-template-columns: 1fr 1fr;
            }
        }

        @media (max-width: 480px) {
            .job-actions {
                flex-direction: column;
            }
            
            .job-footer {
                flex-direction: column;
                align-items: flex-start;
            }
            
            .job-actions {
                width: 100%;
            }
            
            .job-actions button {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="logo">
                <i class="fas fa-hammer"></i>
                <span>OddJobber</span>
            </div>
            <nav id="nav">
                <!-- Navigation will be dynamically populated -->
            </nav>
        </header>
        
        <main id="app">
            <!-- Main content will be dynamically populated -->
        </main>
    </div>
    
    <div class="mobile-tabs">
        <a href="#home" class="tab-item">
            <i class="fas fa-home"></i>
            <span>Home</span>
        </a>
        <a href="#jobs" class="tab-item">
            <i class="fas fa-briefcase"></i>
            <span>Jobs</span>
        </a>
        <a href="#post" class="tab-item">
            <i class="fas fa-plus"></i>
            <span>Post</span>
        </a>
        <a href="#profile" class="tab-item">
            <i class="fas fa-user"></i>
            <span>Profile</span>
        </a>
    </div>
    
    <div class="notification" id="notification">
        <i class="fas fa-check-circle"></i>
        <div class="notification-content">Operation completed successfully!</div>
    </div>

    <script>
        // Firebase configuration
        const firebaseConfig = {
            apiKey: "AIzaSyD1RSy5kqskKQKm2OyZCXyl7HdrQbh_goI",
            authDomain: "oddjobber-53831.firebaseapp.com",
            projectId: "oddjobber-53831",
            storageBucket: "oddjobber-53831.firebasestorage.app",
            messagingSenderId: "432449054979",
            appId: "1:432449054979:web:55b16b968d556a0978e136",
            measurementId: "G-K2HZ49N5QX"
        };
        
        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        const auth = firebase.auth();
        
        // Sample job categories
        const CATEGORIES = [
            'Carpenter', 'Mechanic', 'Helper', 'Agriculture', 'Electrician', 
            'Plumber', 'Painter', 'Mason', 'Tailor', 'Tutor', 'Delivery', 
            'Driver', 'Cleaner', 'Cook', 'Gardener', 'Welder', 'Roofer', 
            'HVAC Technician', 'Pest Control', 'Photographer', 'Event Staff', 
            'Babysitter', 'Animal Care', 'Laundry', 'Upholstery', 'CCTV Technician', 
            'Security Guard', 'IT Support', 'Beauty & Salon', 'Plumber Assistant'
        ];
        
        // DOM Elements
        const navEl = document.getElementById('nav');
        const appEl = document.getElementById('app');
        const notificationEl = document.getElementById('notification');
        const tabItems = document.querySelectorAll('.tab-item');
        
        // Current user data
        let currentUser = null;
        let allJobs = [];
        let userApplications = [];
        
        // Initialize the app
        function initApp() {
            // Set up auth state listener
            auth.onAuthStateChanged((user) => {
                if (user) {
                    // User is signed in
                    currentUser = {
                        id: user.uid,
                        email: user.email,
                        name: user.displayName || user.email.split('@')[0],
                        phone: user.phoneNumber || '',
                        location: '',
                        pincode: '',
                        categories: []
                    };
                    
                    // Get user profile from Firestore
                    getUserProfile(user.uid);
                } else {
                    // User is signed out
                    currentUser = null;
                    localStorage.removeItem('oddjobber_user');
                    renderApp();
                }
            });
            
            // Set up routing
            setupRouting();
            
            // Set up mobile tab clicks
            setupMobileTabs();
            
            // Render the appropriate view
            renderApp();
        }
        
        // Set up mobile tabs
        function setupMobileTabs() {
            tabItems.forEach(tab => {
                tab.addEventListener('click', function(e) {
                    e.preventDefault();
                    const page = this.getAttribute('href').substring(1);
                    navigate(page);
                    
                    // Update active tab
                    tabItems.forEach(t => t.classList.remove('active'));
                    this.classList.add('active');
                });
            });
        }
        
        // Get user profile from Firestore
        function getUserProfile(userId) {
            db.collection('users').doc(userId).get()
                .then((doc) => {
                    if (doc.exists) {
                        const userData = doc.data();
                        currentUser = { ...currentUser, ...userData };
                        localStorage.setItem('oddjobber_user', JSON.stringify(currentUser));
                    }
                    renderApp();
                })
                .catch((error) => {
                    console.error("Error getting user profile:", error);
                    showNotification('Error loading profile', true);
                    renderApp();
                });
        }
        
        // Set up routing
        function setupRouting() {
            window.addEventListener('hashchange', renderApp);
        }
        
        // Render the app based on the current route
        function renderApp() {
            renderNav();
            
            const hash = window.location.hash.substring(1) || 'home';
            
            // Update active tab
            tabItems.forEach(tab => {
                const tabPage = tab.getAttribute('href').substring(1);
                if (tabPage === hash) {
                    tab.classList.add('active');
                } else {
                    tab.classList.remove('active');
                }
            });
            
            switch(hash) {
                case 'home':
                    renderHome();
                    break;
                case 'login':
                    renderLogin();
                    break;
                case 'register':
                    renderRegister();
                    break;
                case 'jobs':
                    renderJobs();
                    break;
                case 'post':
                    renderPostJob();
                    break;
                case 'profile':
                    renderProfile();
                    break;
                default:
                    renderHome();
            }
        }
        
        // Render navigation
        function renderNav() {
            if (currentUser) {
                navEl.innerHTML = `
                    <button class="nav-btn" onclick="navigate('jobs')">
                        <i class="fas fa-briefcase"></i> Find Jobs
                    </button>
                    <button class="nav-btn" onclick="navigate('post')">
                        <i class="fas fa-plus-circle"></i> Post a Job
                    </button>
                    <button class="nav-btn" onclick="navigate('profile')">
                        <i class="fas fa-user"></i> Profile
                    </button>
                    <button class="nav-btn" onclick="logout()">
                        <i class="fas fa-sign-out-alt"></i> Logout
                    </button>
                `;
            } else {
                navEl.innerHTML = `
                    <button class="nav-btn" onclick="navigate('home')">
                        <i class="fas fa-home"></i> Home
                    </button>
                    <button class="nav-btn" onclick="navigate('login')">
                        <i class="fas fa-sign-in-alt"></i> Login
                    </button>
                    <button class="nav-btn primary" onclick="navigate('register')">
                        <i class="fas fa-user-plus"></i> Register
                    </button>
                `;
            }
        }
        
        // Render home page
        function renderHome() {
            appEl.innerHTML = `
                <div class="card">
                    <h1>Find Local Service Providers</h1>
                    <p>OddJobber connects you with skilled professionals for all your home needs. From plumbing to painting, find the right person for the job.</p>
                    
                    ${!currentUser ? `
                        <div style="display: flex; gap: 12px; margin-top: 20px; flex-wrap: wrap;">
                            <button class="btn btn-primary" onclick="navigate('register')">
                                <i class="fas fa-user-plus"></i> Get Started
                            </button>
                            <button class="btn btn-secondary" onclick="navigate('login')">
                                <i class="fas fa-sign-in-alt"></i> Login
                            </button>
                        </div>
                    ` : ''}
                </div>
                
                <div class="feature-section">
                    <h2>How It Works</h2>
                    <div class="features">
                        <div class="feature">
                            <i class="fas fa-search"></i>
                            <h3>Find Jobs</h3>
                            <p>Browse available jobs in your area and field of expertise.</p>
                        </div>
                        <div class="feature">
                            <i class="fas fa-briefcase"></i>
                            <h3>Apply Easily</h3>
                            <p>Apply to jobs with just a few clicks and get hired quickly.</p>
                        </div>
                        <div class="feature">
                            <i class="fas fa-money-bill-wave"></i>
                            <h3>Get Paid</h3>
                            <p>Complete jobs and get paid directly through the platform.</p>
                        </div>
                    </div>
                </div>
                
                <div class="card">
                    <h2>Popular Categories</h2>
                    <div class="category-grid">
                        ${CATEGORIES.slice(0, 6).map(category => `
                            <div class="category-item">
                                <i class="fas fa-wrench"></i>
                                <span>${category}</span>
                            </div>
                        `).join('')}
                    </div>
                </div>
            `;
        }
        
        // Render login page
        function renderLogin() {
            if (currentUser) {
                navigate('jobs');
                return;
            }
            
            appEl.innerHTML = `
                <div class="card">
                    <h2>Login to Your Account</h2>
                    <form id="loginForm">
                        <div class="form-group">
                            <label for="loginEmail">Email</label>
                            <input type="email" id="loginEmail" placeholder="Enter your email" required>
                        </div>
                        <div class="form-group">
                            <label for="loginPassword">Password</label>
                            <input type="password" id="loginPassword" placeholder="Enter your password" required>
                        </div>
                        <button type="submit" class="btn btn-primary btn-block">
                            <i class="fas fa-sign-in-alt"></i> Login
                        </button>
                    </form>
                    <p style="text-align: center; margin-top: 20px;">
                        Don't have an account? <a href="#" onclick="navigate('register')">Register here</a>
                    </p>
                </div>
            `;
            
            document.getElementById('loginForm').addEventListener('submit', function(e) {
                e.preventDefault();
                const email = document.getElementById('loginEmail').value;
                const password = document.getElementById('loginPassword').value;
                
                // Firebase authentication
                auth.signInWithEmailAndPassword(email, password)
                    .then((userCredential) => {
                        showNotification('Login successful!');
                        navigate('jobs');
                    })
                    .catch((error) => {
                        console.error("Login error:", error);
                        showNotification(error.message, true);
                    });
            });
        }
        
        // Render register page
        function renderRegister() {
            if (currentUser) {
                navigate('profile');
                return;
            }
            
            appEl.innerHTML = `
                <div class="card">
                    <h2>Create an Account</h2>
                    <form id="registerForm">
                        <div class="form-group">
                            <label for="registerName">Full Name</label>
                            <input type="text" id="registerName" placeholder="Enter your full name" required>
                        </div>
                        <div class="form-group">
                            <label for="registerEmail">Email</label>
                            <input type="email" id="registerEmail" placeholder="Enter your email" required>
                        </div>
                        <div class="form-group">
                            <label for="registerPassword">Password</label>
                            <input type="password" id="registerPassword" placeholder="Create a password (min. 6 characters)" required minlength="6">
                        </div>
                        <div class="form-group">
                            <label for="registerPhone">Phone Number</label>
                            <input type="tel" id="registerPhone" placeholder="Enter your phone number" required>
                        </div>
                        <div class="form-group">
                            <label for="registerLocation">Location</label>
                            <input type="text" id="registerLocation" placeholder="e.g. Mumbai, Maharashtra" required>
                        </div>
                        <div class="form-group">
                            <label for="registerPincode">Pincode</label>
                            <input type="text" id="registerPincode" placeholder="e.g. 400001" required>
                        </div>
                        <button type="submit" class="btn btn-primary btn-block">
                            <i class="fas fa-user-plus"></i> Register
                        </button>
                    </form>
                    <p style="text-align: center; margin-top: 20px;">
                        Already have an account? <a href="#" onclick="navigate('login')">Login here</a>
                    </p>
                </div>
            `;
            
            document.getElementById('registerForm').addEventListener('submit', function(e) {
                e.preventDefault();
                const name = document.getElementById('registerName').value;
                const email = document.getElementById('registerEmail').value;
                const password = document.getElementById('registerPassword').value;
                const phone = document.getElementById('registerPhone').value;
                const location = document.getElementById('registerLocation').value;
                const pincode = document.getElementById('registerPincode').value;
                
                // Firebase authentication
                auth.createUserWithEmailAndPassword(email, password)
                    .then((userCredential) => {
                        const user = userCredential.user;
                        
                        // Update user profile
                        return user.updateProfile({
                            displayName: name
                        }).then(() => {
                            // Save additional user data to Firestore
                            return db.collection('users').doc(user.uid).set({
                                name: name,
                                email: email,
                                phone: phone,
                                location: location,
                                pincode: pincode,
                                categories: []
                            });
                        });
                    })
                    .then(() => {
                        showNotification('Registration successful!');
                        navigate('profile');
                    })
                    .catch((error) => {
                        console.error("Registration error:", error);
                        showNotification(error.message, true);
                    });
            });
        }
        
        // Render jobs page
        function renderJobs() {
            if (!currentUser) {
                navigate('login');
                return;
            }
            
            appEl.innerHTML = `
                <div class="card">
                    <h2>Available Jobs</h2>
                    <p>Browse and apply to available jobs in your area.</p>
                    
                    <div class="form-group">
                        <input type="text" id="jobSearch" placeholder="Search for jobs...">
                    </div>
                </div>
                
                <div id="jobsContainer">
                    <div class="loader"></div>
                </div>
            `;
            
            // Load jobs from Firestore
            loadJobs();
            
            // Add search functionality
            document.getElementById('jobSearch').addEventListener('input', function(e) {
                filterJobs(e.target.value);
            });
        }
        
        // Load jobs from Firestore
        function loadJobs() {
            const jobsContainer = document.getElementById('jobsContainer');
            
            db.collection('jobs')
                .where('status', '==', 'open')
                .get()
                .then((querySnapshot) => {
                    allJobs = [];
                    querySnapshot.forEach((doc) => {
                        allJobs.push({ id: doc.id, ...doc.data() });
                    });
                    
                    displayJobs(allJobs);
                    
                    // Also load user applications
                    loadUserApplications();
                })
                .catch((error) => {
                    console.error("Error loading jobs:", error);
                    jobsContainer.innerHTML = '<p>Error loading jobs. Please try again.</p>';
                    showNotification('Error loading jobs', true);
                });
        }
        
        // Load user applications
        function loadUserApplications() {
            if (!currentUser) return;
            
            db.collection('applications')
                .where('applicantId', '==', currentUser.id)
                .get()
                .then((querySnapshot) => {
                    userApplications = [];
                    querySnapshot.forEach((doc) => {
                        userApplications.push({ id: doc.id, ...doc.data() });
                    });
                    
                    // Update job display with application status
                    displayJobs(allJobs);
                })
                .catch((error) => {
                    console.error("Error loading applications:", error);
                });
        }
        
        // Display jobs in the UI
        function displayJobs(jobs) {
            const jobsContainer = document.getElementById('jobsContainer');
            
            if (jobs.length === 0) {
                jobsContainer.innerHTML = '<p>No jobs available at the moment. Check back later!</p>';
                return;
            }
            
            jobsContainer.innerHTML = jobs.map(job => {
                const hasApplied = userApplications.some(app => app.jobId === job.id);
                
                return `
                    <div class="job-card">
                        <div class="job-header">
                            <div class="job-title">${job.title}</div>
                            <div class="job-price">₹${job.price}</div>
                        </div>
                        <div class="job-category">${job.category}</div>
                        <div class="job-description">${job.description}</div>
                        <div class="job-details">
                            <div class="job-detail">
                                <i class="fas fa-map-marker-alt"></i>
                                <span>${job.location}</span>
                            </div>
                            <div class="job-detail">
                                <i class="fas fa-map-pin"></i>
                                <span>${job.pincode}</span>
                            </div>
                        </div>
                        <div class="job-footer">
                            <div class="job-poster">
                                <i class="fas fa-user"></i>
                                <span>${job.posterName || job.posterEmail}</span>
                            </div>
                            <div class="job-actions">
                                <button class="btn btn-secondary" onclick="callNumber('${job.posterPhone}')">
                                    <i class="fas fa-phone"></i> Call
                                </button>
                                ${hasApplied ? 
                                    `<button class="btn btn-success" disabled>
                                        <i class="fas fa-check"></i> Applied
                                    </button>` :
                                    `<button class="btn btn-primary" onclick="applyForJob('${job.id}')">
                                        <i class="fas fa-check"></i> Apply
                                    </button>`
                                }
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }
        
        // Filter jobs based on search query
        function filterJobs(query) {
            if (!query) {
                displayJobs(allJobs);
                return;
            }
            
            const filteredJobs = allJobs.filter(job => 
                job.title.toLowerCase().includes(query.toLowerCase()) ||
                job.description.toLowerCase().includes(query.toLowerCase()) ||
                job.category.toLowerCase().includes(query.toLowerCase()) ||
                job.location.toLowerCase().includes(query.toLowerCase())
            );
            
            displayJobs(filteredJobs);
        }
        
        // Apply for a job
        function applyForJob(jobId) {
            if (!currentUser) {
                navigate('login');
                return;
            }
            
            const job = allJobs.find(j => j.id === jobId);
            if (!job) return;
            
            // Create application record
            db.collection('applications').add({
                jobId: jobId,
                jobTitle: job.title,
                applicantId: currentUser.id,
                applicantName: currentUser.name,
                applicantEmail: currentUser.email,
                applicantPhone: currentUser.phone,
                status: 'pending',
                appliedAt: firebase.firestore.FieldValue.serverTimestamp()
            })
            .then(() => {
                showNotification('Application submitted successfully!');
                loadUserApplications(); // Refresh applications
            })
            .catch((error) => {
                console.error("Error applying for job:", error);
                showNotification('Error applying for job', true);
            });
        }
        
        // Call phone number
        function callNumber(phone) {
            window.open(`tel:${phone}`);
        }
        
        // Render post job page
        function renderPostJob() {
            if (!currentUser) {
                navigate('login');
                return;
            }
            
            appEl.innerHTML = `
                <div class="card">
                    <h2>Post a New Job</h2>
                    <p>Fill out the form below to post a new job opportunity.</p>
                    
                    <form id="postJobForm">
                        <div class="form-group">
                            <label for="jobCategory">Job Category</label>
                            <select id="jobCategory" required>
                                <option value="">Select a category</option>
                                ${CATEGORIES.map(cat => `<option value="${cat}">${cat}</option>`).join('')}
                            </select>
                        </div>
                        <div class="form-group">
                            <label for="jobTitle">Job Title</label>
                            <input type="text" id="jobTitle" placeholder="e.g. House Painting" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="jobDescription">Description</label>
                            <textarea id="jobDescription" rows="4" placeholder="Describe the job in detail" required></textarea>
                        </div>
                        
                        <div class="grid grid-2">
                            <div class="form-group">
                                <label for="jobPrice">Price (₹)</label>
                                <input type="number" id="jobPrice" placeholder="e.g. 2500" required>
                            </div>
                            <div class="form-group">
                                <label for="jobLocation">Location</label>
                                <input type="text" id="jobLocation" placeholder="e.g. Mumbai, Maharashtra" required>
                            </div>
                        </div>
                        
                        <div class="grid grid-2">
                            <div class="form-group">
                                <label for="jobPincode">Pincode</label>
                                <input type="text" id="jobPincode" placeholder="e.g. 400001" required>
                            </div>
                            <div class="form-group">
                                <label for="posterPhone">Your Phone Number</label>
                                <input type="tel" id="posterPhone" value="${currentUser.phone}" placeholder="e.g. +91 9876543210" required>
                            </div>
                        </div>
                        
                        <button type="submit" class="btn btn-primary btn-block">
                            <i class="fas fa-paper-plane"></i> Post Job
                        </button>
                    </form>
                </div>
            `;
            
            document.getElementById('postJobForm').addEventListener('submit', function(e) {
                e.preventDefault();
                const category = document.getElementById('jobCategory').value;
                const title = document.getElementById('jobTitle').value;
                const description = document.getElementById('jobDescription').value;
                const price = document.getElementById('jobPrice').value;
                const location = document.getElementById('jobLocation').value;
                const pincode = document.getElementById('jobPincode').value;
                const phone = document.getElementById('posterPhone').value;
                
                // Save job to Firestore
                db.collection('jobs').add({
                    category: category,
                    title: title,
                    description: description,
                    price: price,
                    location: location,
                    pincode: pincode,
                    posterId: currentUser.id,
                    posterName: currentUser.name,
                    posterEmail: currentUser.email,
                    posterPhone: phone,
                    status: 'open',
                    createdAt: firebase.firestore.FieldValue.serverTimestamp()
                })
                .then(() => {
                    showNotification('Job posted successfully!');
                    navigate('jobs');
                })
                .catch((error) => {
                    console.error("Error posting job:", error);
                    showNotification('Error posting job', true);
                });
            });
        }
        
        // Render profile page
        function renderProfile() {
            if (!currentUser) {
                navigate('login');
                return;
            }
            
            appEl.innerHTML = `
                <div class="card">
                    <h2>Profile Information</h2>
                    
                    <div class="profile-info">
                        <div class="profile-info-item">
                            <span class="info-label">Name:</span>
                            <span class="info-value">${currentUser.name}</span>
                        </div>
                        <div class="profile-info-item">
                            <span class="info-label">Email:</span>
                            <span class="info-value">${currentUser.email}</span>
                        </div>
                        <div class="profile-info-item">
                            <span class="info-label">Phone:</span>
                            <span class="info-value">${currentUser.phone}</span>
                        </div>
                        <div class="profile-info-item">
                            <span class="info-label">Location:</span>
                            <span class="info-value">${currentUser.location}</span>
                        </div>
                        <div class="profile-info-item">
                            <span class="info-label">Pincode:</span>
                            <span class="info-value">${currentUser.pincode}</span>
                        </div>
                    </div>
                    
                    <h3>Your Skills/Categories</h3>
                    <p>Select the categories that match your skills:</p>
                    
                    <div class="category-grid" id="categoriesContainer">
                        ${CATEGORIES.map(category => `
                            <div class="category-item ${currentUser.categories && currentUser.categories.includes(category) ? 'selected' : ''}">
                                <i class="fas fa-wrench"></i>
                                <span>${category}</span>
                            </div>
                        `).join('')}
                    </div>
                    
                    <div style="margin-top: 20px;">
                        <button class="btn btn-primary" onclick="saveProfile()">
                            <i class="fas fa-save"></i> Save Profile
                        </button>
                    </div>
                </div>
                
                <div class="card">
                    <h3>Your Job Applications</h3>
                    <div id="applicationsContainer">
                        <div class="loader"></div>
                    </div>
                </div>
            `;
            
            // Load user applications
            loadProfileApplications();
            
            // Add event listeners to category items
            setTimeout(() => {
                const categoryItems = document.querySelectorAll('.category-item');
                categoryItems.forEach(item => {
                    item.addEventListener('click', function() {
                        const category = this.querySelector('span').textContent;
                        toggleCategory(category);
                    });
                });
            }, 100);
        }
        
        // Toggle category selection
        function toggleCategory(category) {
            if (!currentUser.categories) {
                currentUser.categories = [];
            }
            
            const index = currentUser.categories.indexOf(category);
            
            if (index > -1) {
                currentUser.categories.splice(index, 1);
            } else {
                currentUser.categories.push(category);
            }
            
            // Update UI
            const categoryElement = Array.from(document.querySelectorAll('.category-item')).find(
                el => el.querySelector('span').textContent === category
            );
            
            if (categoryElement) {
                categoryElement.classList.toggle('selected');
            }
        }
        
        // Save profile to Firestore
        function saveProfile() {
            db.collection('users').doc(currentUser.id).set({
                name: currentUser.name,
                email: currentUser.email,
                phone: currentUser.phone,
                location: currentUser.location,
                pincode: currentUser.pincode,
                categories: currentUser.categories || []
            }, { merge: true })
            .then(() => {
                showNotification('Profile saved successfully!');
            })
            .catch((error) => {
                console.error("Error saving profile:", error);
                showNotification('Error saving profile', true);
            });
        }
        
        // Load applications for profile page
        function loadProfileApplications() {
            const applicationsContainer = document.getElementById('applicationsContainer');
            
            if (!currentUser) {
                applicationsContainer.innerHTML = '<p>Please log in to view your applications.</p>';
                return;
            }
            
            db.collection('applications')
                .where('applicantId', '==', currentUser.id)
                .orderBy('appliedAt', 'desc')
                .get()
                .then((querySnapshot) => {
                    if (querySnapshot.empty) {
                        applicationsContainer.innerHTML = '<p>You haven\'t applied to any jobs yet.</p>';
                        return;
                    }
                    
                    applicationsContainer.innerHTML = '';
                    querySnapshot.forEach((doc) => {
                        const application = { id: doc.id, ...doc.data() };
                        
                        const applicationElement = document.createElement('div');
                        applicationElement.className = 'application-card';
                        applicationElement.innerHTML = `
                            <div class="application-header">
                                <div class="application-title">${application.jobTitle}</div>
                                <div class="application-status status-${application.status}">${application.status}</div>
                            </div>
                            <div class="job-details">
                                <div class="job-detail">
                                    <i class="fas fa-calendar"></i>
                                    <span>Applied on: ${application.appliedAt ? application.appliedAt.toDate().toLocaleDateString() : 'N/A'}</span>
                                </div>
                            </div>
                            ${application.status === 'accepted' ? `
                                <div style="margin-top: 15px;">
                                    <button class="btn btn-success" onclick="completeJob('${application.id}', '${application.jobId}')">
                                        <i class="fas fa-check-circle"></i> Mark as Completed
                                    </button>
                                </div>
                            ` : ''}
                        `;
                        
                        applicationsContainer.appendChild(applicationElement);
                    });
                })
                .catch((error) => {
                    console.error("Error loading applications:", error);
                    applicationsContainer.innerHTML = '<p>Error loading applications. Please try again.</p>';
                });
        }
        
        // Complete a job (mark as done)
        function completeJob(applicationId, jobId) {
            // Update application status to completed
            db.collection('applications').doc(applicationId).update({
                status: 'completed',
                completedAt: firebase.firestore.FieldValue.serverTimestamp()
            })
            .then(() => {
                // Update job status to closed
                return db.collection('jobs').doc(jobId).update({
                    status: 'closed'
                });
            })
            .then(() => {
                showNotification('Job marked as completed!');
                loadProfileApplications(); // Refresh the applications list
            })
            .catch((error) => {
                console.error("Error completing job:", error);
                showNotification('Error completing job', true);
            });
        }
        
        // Show notification
        function showNotification(message, isError = false) {
            const notification = document.getElementById('notification');
            notification.querySelector('.notification-content').textContent = message;
            
            if (isError) {
                notification.classList.add('error');
            } else {
                notification.classList.remove('error');
            }
            
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }
        
        // Navigate to a page
        function navigate(page) {
            window.location.hash = page;
        }
        
        // Logout
        function logout() {
            auth.signOut()
                .then(() => {
                    showNotification('Logged out successfully');
                    navigate('home');
                })
                .catch((error) => {
                    console.error("Logout error:", error);
                    showNotification('Error logging out', true);
                });
        }
        
        // Initialize the app
        initApp();
    </script>
</body>
</html>
