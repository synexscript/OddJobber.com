
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Synex - Professional Script Writing Services</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #000;
            color: #fff;
            line-height: 1.6;
            overflow-x: hidden;
            -webkit-text-size-adjust: 100%;
        }
        
        .container {
            max-width: 100%;
            margin: 0 auto;
            padding: 0 15px;
        }
        
        /* Netflix-style intro animation */
        .intro-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #000;
            z-index: 1000;
            display: flex;
            justify-content: center;
            align-items: center;
            animation: fadeOut 1.5s ease-in-out 2s forwards;
        }
        
        .intro-logo {
            width: 200px;
            height: 100px;
            position: relative;
            opacity: 0;
            animation: zoomIn 1.5s ease-in-out 0.5s forwards;
        }
        
        .intro-logo::before {
            content: '';
            position: absolute;
            top: -5px;
            left: -5px;
            right: -5px;
            bottom: -5px;
            background: linear-gradient(45deg, 
                #ff0000, #ff7300, #fffb00, 
                #48ff00, #00ffd5, #002bff, 
                #7a00ff, #ff00c8, #ff0000);
            background-size: 400%;
            border-radius: 10px;
            z-index: -1;
            filter: blur(10px);
            animation: rgbGlow 20s linear infinite;
        }
        
        @keyframes zoomIn {
            0% { transform: scale(0.5); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        
        @keyframes fadeOut {
            0% { opacity: 1; }
            100% { opacity: 0; visibility: hidden; }
        }
        
        @keyframes rgbGlow {
            0% { background-position: 0 0; }
            50% { background-position: 400% 0; }
            100% { background-position: 0 0; }
        }
        
        header {
            background-color: rgba(0, 0, 0, 0.9);
            color: white;
            padding: 15px 0;
            box-shadow: 0 2px 10px rgba(0,0,0,0.5);
            position: fixed;
            width: 100%;
            z-index: 100;
            transition: all 0.3s;
        }
        
        header.scrolled {
            background-color: rgba(20, 20, 20, 0.95);
            padding: 10px 0;
        }
        
        nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .logo {
            font-size: 22px;
            font-weight: bold;
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 10px rgba(0, 198, 255, 0.3);
        }
        
        /* Mobile menu toggle */
        .menu-toggle {
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            width: 24px;
            height: 18px;
            cursor: pointer;
            z-index: 101;
        }
        
        .menu-toggle span {
            display: block;
            width: 100%;
            height: 2px;
            background: #fff;
            transition: all 0.3s;
        }
        
        .menu-toggle.active span:nth-child(1) {
            transform: translateY(8px) rotate(45deg);
        }
        
        .menu-toggle.active span:nth-child(2) {
            opacity: 0;
        }
        
        .menu-toggle.active span:nth-child(3) {
            transform: translateY(-8px) rotate(-45deg);
        }
        
        /* Mobile navigation */
        .nav-links {
            position: fixed;
            top: 0;
            right: -100%;
            width: 70%;
            height: 100vh;
            background: rgba(10, 10, 10, 0.95);
            backdrop-filter: blur(10px);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            transition: right 0.3s ease;
            z-index: 100;
            padding: 20px;
            box-shadow: -5px 0 15px rgba(0,0,0,0.3);
        }
        
        .nav-links.active {
            right: 0;
        }
        
        .nav-links a {
            color: white;
            text-decoration: none;
            margin: 15px 0;
            transition: all 0.3s;
            font-weight: 500;
            position: relative;
            font-size: 1.2rem;
            padding: 10px 0;
            width: 100%;
            text-align: center;
        }
        
        .nav-links a:hover {
            color: #00c6ff;
        }
        
        .nav-links a::after {
            content: '';
            position: absolute;
            width: 0;
            height: 2px;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            transition: width 0.3s;
        }
        
        .nav-links a:hover::after {
            width: 80%;
        }
        
        /* Overlay when menu is open */
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            z-index: 99;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s;
        }
        
        .overlay.active {
            opacity: 1;
            visibility: visible;
        }
        
        .hero {
            background: linear-gradient(rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.7)), 
                        url('https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80');
            background-size: cover;
            background-position: center;
            height: 100vh;
            min-height: 600px;
            display: flex;
            align-items: center;
            text-align: center;
            position: relative;
            overflow: hidden;
            padding-top: 70px;
        }
        
        .hero::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at center, transparent 0%, rgba(0,0,0,0.8) 100%);
        }
        
        .hero-content {
            position: relative;
            z-index: 2;
            width: 100%;
        }
        
        .hero h1 {
            font-size: 2.2rem;
            margin-bottom: 15px;
            background: linear-gradient(90deg, #00c6ff, #0072ff, #00c6ff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 20px rgba(0, 198, 255, 0.3);
            animation: rgbTextGlow 8s linear infinite;
            line-height: 1.2;
        }
        
        @keyframes rgbTextGlow {
            0% { filter: hue-rotate(0deg); }
            100% { filter: hue-rotate(360deg); }
        }
        
        .hero p {
            font-size: 1.1rem;
            max-width: 100%;
            margin: 0 auto 25px;
            color: rgba(255,255,255,0.9);
            padding: 0 15px;
        }
        
        .cta-button {
            display: inline-block;
            background: linear-gradient(45deg, #00c6ff, #0072ff);
            color: white;
            padding: 12px 30px;
            border-radius: 30px;
            text-decoration: none;
            font-weight: bold;
            font-size: 1rem;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(0, 198, 255, 0.4);
            position: relative;
            overflow: hidden;
            border: none;
            cursor: pointer;
            min-width: 200px;
        }
        
        .cta-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 198, 255, 0.6);
        }
        
        .cta-button::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transition: 0.5s;
        }
        
        .cta-button:hover::before {
            left: 100%;
        }
        
        .pricing {
            padding: 60px 0;
            background-color: #0a0a0a;
            position: relative;
        }
        
        .pricing::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 3px;
            background: linear-gradient(90deg, #00c6ff, #0072ff, #00c6ff, #0072ff, #00c6ff);
        }
        
        .section-title {
            text-align: center;
            margin-bottom: 40px;
        }
        
        .section-title h2 {
            font-size: 1.8rem;
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 10px;
        }
        
        .section-title p {
            font-size: 1rem;
            color: rgba(255,255,255,0.7);
            padding: 0 15px;
        }
        
        .pricing-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 20px;
            margin-top: 30px;
        }
        
        .pricing-card {
            background: rgba(30, 30, 30, 0.8);
            border-radius: 12px;
            padding: 20px 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
            text-align: center;
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        .pricing-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 3px;
            background: linear-gradient(90deg, #00c6ff, #0072ff);
        }
        
        .free-plan::before {
            background: linear-gradient(90deg, #2ecc71, #27ae60);
        }
        
        .popular-plan::before {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
        }
        
        .pricing-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0, 198, 255, 0.2);
        }
        
        .plan-tag {
            position: absolute;
            top: 10px;
            right: 10px;
            padding: 4px 12px;
            border-radius: 15px;
            font-size: 0.7rem;
            font-weight: bold;
            color: white;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        .free-tag {
            background: linear-gradient(90deg, #2ecc71, #27ae60);
        }
        
        .popular-tag {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        .pricing-card h3 {
            font-size: 1.3rem;
            margin-bottom: 12px;
            color: white;
        }
        
        .price {
            font-size: 1.8rem;
            font-weight: bold;
            margin-bottom: 15px;
            position: relative;
            display: inline-block;
        }
        
        .price::after {
            content: '';
            position: absolute;
            bottom: -8px;
            left: 0;
            width: 100%;
            height: 2px;
            background: rgba(255,255,255,0.1);
        }
        
        .price span {
            font-size: 0.9rem;
            color: rgba(255,255,255,0.6);
        }
        
        .pricing-features {
            list-style: none;
            margin-bottom: 20px;
            text-align: left;
            font-size: 0.85rem;
        }
        
        .pricing-features li {
            margin-bottom: 10px;
            padding-bottom: 10px;
            border-bottom: 1px solid rgba(255,255,255,0.05);
            position: relative;
            padding-left: 22px;
            color: rgba(255,255,255,0.8);
        }
        
        .pricing-features li:before {
            content: "✓";
            color: #00c6ff;
            position: absolute;
            left: 0;
            font-weight: bold;
        }
        
        .free-plan .pricing-features li:before {
            color: #2ecc71;
        }
        
        .select-plan {
            display: inline-block;
            padding: 10px 20px;
            border-radius: 25px;
            text-decoration: none;
            font-weight: bold;
            font-size: 0.85rem;
            transition: all 0.3s;
            border: none;
            cursor: pointer;
            width: 100%;
            text-transform: uppercase;
            letter-spacing: 1px;
            position: relative;
            overflow: hidden;
        }
        
        .select-plan::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transition: 0.5s;
        }
        
        .select-plan:hover::before {
            left: 100%;
        }
        
        .free-plan-btn {
            background: linear-gradient(90deg, #2ecc71, #27ae60);
            color: white;
        }
        
        .synex-starter-btn {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            color: white;
        }
        
        .synex-basic-btn {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            color: white;
        }
        
        .synex-standard-btn {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            color: white;
        }
        
        .synex-pro-btn {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            color: white;
        }
        
        .synex-premium-btn {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            color: white;
        }
        
        .synex-business-btn {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            color: white;
        }
        
        .synex-enterprise-btn {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            color: white;
        }
        
        .synex-elite-btn {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            color: white;
        }
        
        .synex-platinum-btn {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            color: white;
        }
        
        .synex-vip-btn {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            color: white;
        }
        
        footer {
            background: linear-gradient(135deg, #1a1a1a 0%, #000 100%);
            color: white;
            padding: 40px 0 20px;
            text-align: center;
            position: relative;
        }
        
        footer::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 3px;
            background: linear-gradient(90deg, #00c6ff, #0072ff, #00c6ff, #0072ff, #00c6ff);
        }
        
        .footer-links {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .footer-links a {
            color: rgba(255,255,255,0.7);
            text-decoration: none;
            margin: 8px 0;
            transition: all 0.3s;
            position: relative;
            font-size: 0.95rem;
        }
        
        .footer-links a:hover {
            color: #00c6ff;
        }
        
        .footer-links a::after {
            display: none;
        }
        
        .copyright {
            color: rgba(255,255,255,0.5);
            font-size: 0.8rem;
            margin-top: 20px;
        }
        
        /* RGB floating elements - simplified for mobile */
        .rgb-circle {
            position: absolute;
            border-radius: 50%;
            filter: blur(40px);
            opacity: 0.2;
            z-index: -1;
            animation: float 15s infinite linear;
        }
        
        .rgb-circle:nth-child(1) {
            width: 200px;
            height: 200px;
            background: #00c6ff;
            top: 20%;
            left: 10%;
            animation-delay: 0s;
        }
        
        .rgb-circle:nth-child(2) {
            width: 250px;
            height: 250px;
            background: #0072ff;
            bottom: 10%;
            right: 10%;
            animation-delay: 3s;
        }
        
        .rgb-circle:nth-child(3) {
            width: 150px;
            height: 150px;
            background: #00c6ff;
            top: 60%;
            left: 30%;
            animation-delay: 6s;
        }
        
        @keyframes float {
            0% { transform: translate(0, 0) rotate(0deg); }
            25% { transform: translate(30px, 30px) rotate(90deg); }
            50% { transform: translate(60px, 0) rotate(180deg); }
            75% { transform: translate(30px, -30px) rotate(270deg); }
            100% { transform: translate(0, 0) rotate(360deg); }
        }
        
        /* Mobile bottom navigation */
        .mobile-bottom-nav {
            display: flex;
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: rgba(20, 20, 20, 0.95);
            backdrop-filter: blur(10px);
            z-index: 90;
            box-shadow: 0 -2px 10px rgba(0,0,0,0.3);
            padding: 10px 0;
        }
        
        .mobile-nav-item {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            color: rgba(255,255,255,0.7);
            text-decoration: none;
            font-size: 0.8rem;
            transition: all 0.3s;
        }
        
        .mobile-nav-item i {
            font-size: 1.3rem;
            margin-bottom: 5px;
        }
        
        .mobile-nav-item.active, 
        .mobile-nav-item:hover {
            color: #00c6ff;
        }
        
        /* Responsive adjustments */
        @media (min-width: 480px) {
            .hero h1 {
                font-size: 2.5rem;
            }
            
            .hero p {
                font-size: 1.2rem;
            }
            
            .pricing-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        
        @media (min-width: 768px) {
            .menu-toggle {
                display: none;
            }
            
            .nav-links {
                position: static;
                width: auto;
                height: auto;
                background: transparent;
                flex-direction: row;
                padding: 0;
                box-shadow: none;
            }
            
            .nav-links a {
                margin: 0 0 0 20px;
                font-size: 1rem;
                padding: 5px 0;
                width: auto;
            }
            
            .nav-links a::after {
                left: 0;
                transform: none;
            }
            
            .nav-links a:hover::after {
                width: 100%;
            }
            
            .overlay {
                display: none;
            }
            
            .mobile-bottom-nav {
                display: none;
            }
            
            .hero h1 {
                font-size: 3rem;
            }
            
            .section-title h2 {
                font-size: 2.2rem;
            }
        }
    </style>
</head>
<body>
    <!-- Netflix-style intro animation -->
    <div class="intro-overlay">
        <div class="intro-logo">
            <svg viewBox="0 0 500 200">
                <path fill="#00c6ff" d="M50,100 L150,100 L150,50 L250,150 L250,100 L350,100 L350,150 L450,50 L450,150 L400,150 L400,100 L300,100 L300,150 L200,50 L200,100 L100,100 L100,150 L50,150 Z"/>
            </svg>
        </div>
    </div>

    <!-- RGB floating elements -->
    <div class="rgb-circle"></div>
    <div class="rgb-circle"></div>
    <div class="rgb-circle"></div>

    <header id="main-header">
        <div class="container">
            <nav>
                <div class="logo">Synex</div>
                <div class="menu-toggle">
                    <span></span>
                    <span></span>
                    <span></span>
                </div>
                <div class="nav-links">
                    <a href="#pricing">Pricing</a>
                    <a href="#contact">Contact</a>
                </div>
            </nav>
        </div>
    </header>
    
    <!-- Overlay for mobile menu -->
    <div class="overlay"></div>
    
    <section class="hero">
        <div class="hero-content">
            <div class="container">
                <h1>Professional Script Writing Services</h1>
                <p>Our expert writers craft compelling scripts for films, TV shows, commercials, and more. No AI - just human creativity and expertise.</p>
                <button class="cta-button" onclick="selectPlan('Synex Plans')">View All Plans</button>
            </div>
        </div>
    </section>
    
    <section class="pricing" id="pricing">
        <div class="container">
            <div class="section-title">
                <h2>Synex Script Writing Plans</h2>
                <p>Choose the perfect plan for your creative needs</p>
            </div>
            
            <div class="pricing-grid">
                <!-- Free Trial Plan -->
                <div class="pricing-card free-plan">
                    <div class="plan-tag free-tag">Free Trial</div>
                    <h3>Synex Trial</h3>
                    <div class="price">$0<span>/month</span></div>
                    <ul class="pricing-features">
                        <li>1 short script (3 pages max)</li>
                        <li>Basic formatting</li>
                        <li>1 revision</li>
                        <li>7-day delivery</li>
                        <li>Email support</li>
                        <li>7-day trial period</li>
                    </ul>
                    <button class="select-plan free-plan-btn" onclick="selectPlan('Synex Free Trial')">Try For Free</button>
                </div>
                
                <!-- Starter Plan -->
                <div class="pricing-card">
                    <h3>Synex Starter</h3>
                    <div class="price">$49<span>/month</span></div>
                    <ul class="pricing-features">
                        <li>2 short scripts (5 pages each)</li>
                        <li>Standard formatting</li>
                        <li>2 revisions per script</li>
                        <li>5-day delivery</li>
                        <li>Email support</li>
                        <li>Basic character outlines</li>
                    </ul>
                    <button class="select-plan synex-starter-btn" onclick="selectPlan('Synex Starter $49/month')">Get Started</button>
                </div>
                
                <!-- Basic Plan -->
                <div class="pricing-card">
                    <h3>Synex Basic</h3>
                    <div class="price">$79<span>/month</span></div>
                    <ul class="pricing-features">
                        <li>3 scripts (8 pages each)</li>
                        <li>Professional formatting</li>
                        <li>3 revisions per script</li>
                        <li>4-day delivery</li>
                        <li>Email support</li>
                        <li>Character development</li>
                    </ul>
                    <button class="select-plan synex-basic-btn" onclick="selectPlan('Synex Basic $79/month')">Select Plan</button>
                </div>
                
                <!-- Standard Plan -->
                <div class="pricing-card popular-plan">
                    <div class="plan-tag popular-tag">Popular</div>
                    <h3>Synex Standard</h3>
                    <div class="price">$99<span>/month</span></div>
                    <ul class="pricing-features">
                        <li>5 scripts (10 pages each)</li>
                        <li>Advanced formatting</li>
                        <li>5 revisions per script</li>
                        <li>3-day delivery</li>
                        <li>Priority email support</li>
                        <li>Full character development</li>
                        <li>Basic plot consultation</li>
                    </ul>
                    <button class="select-plan synex-standard-btn" onclick="selectPlan('Synex Standard $99/month')">Select Plan</button>
                </div>
                
                <!-- Pro Plan -->
                <div class="pricing-card">
                    <h3>Synex Pro</h3>
                    <div class="price">$129<span>/month</span></div>
                    <ul class="pricing-features">
                        <li>7 scripts (12 pages each)</li>
                        <li>Studio-quality formatting</li>
                        <li>Unlimited revisions</li>
                        <li>48-hour delivery</li>
                        <li>Priority email support</li>
                        <li>Advanced character development</li>
                        <li>Plot consultation</li>
                    </ul>
                    <button class="select-plan synex-pro-btn" onclick="selectPlan('Synex Pro $129/month')">Select Plan</button>
                </div>
                
                <!-- Premium Plan -->
                <div class="pricing-card">
                    <h3>Synex Premium</h3>
                    <div class="price">$159<span>/month</span></div>
                    <ul class="pricing-features">
                        <li>10 scripts (15 pages each)</li>
                        <li>Studio-quality formatting</li>
                        <li>Unlimited revisions</li>
                        <li>36-hour delivery</li>
                        <li>Priority email/chat support</li>
                        <li>Full development services</li>
                        <li>Weekly consultations</li>
                    </ul>
                    <button class="select-plan synex-premium-btn" onclick="selectPlan('Synex Premium $159/month')">Select Plan</button>
                </div>
            </div>
            
            <!-- "Load More" button for additional plans -->
            <div style="text-align: center; margin-top: 20px;">
                <button class="cta-button" id="load-more-plans" style="background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2);">
                    View More Plans
                </button>
            </div>
            
            <!-- Additional plans (hidden by default) -->
            <div id="additional-plans" style="display: none;">
                <div class="pricing-grid">
                    <!-- Business Plan -->
                    <div class="pricing-card">
                        <h3>Synex Business</h3>
                        <div class="price">$199<span>/month</span></div>
                        <ul class="pricing-features">
                            <li>15 scripts (20 pages each)</li>
                            <li>Studio-quality formatting</li>
                            <li>Unlimited revisions</li>
                            <li>24-hour delivery</li>
                            <li>Phone & email support</li>
                            <li>Full development services</li>
                            <li>Twice weekly consultations</li>
                            <li>Dedicated account manager</li>
                        </ul>
                        <button class="select-plan synex-business-btn" onclick="selectPlan('Synex Business $199/month')">Select Plan</button>
                    </div>
                    
                    <!-- Enterprise Plan -->
                    <div class="pricing-card">
                        <h3>Synex Enterprise</h3>
                        <div class="price">$249<span>/month</span></div>
                        <ul class="pricing-features">
                            <li>20 scripts (25 pages each)</li>
                            <li>Studio-quality formatting</li>
                            <li>Unlimited revisions</li>
                            <li>18-hour delivery</li>
                            <li>24/7 priority support</li>
                            <li>Full development services</li>
                            <li>Dedicated writer</li>
                            <li>Three weekly consultations</li>
                        </ul>
                        <button class="select-plan synex-enterprise-btn" onclick="selectPlan('Synex Enterprise $249/month')">Select Plan</button>
                    </div>
                    
                    <!-- Elite Plan -->
                    <div class="pricing-card">
                        <h3>Synex Elite</h3>
                        <div class="price">$299<span>/month</span></div>
                        <ul class="pricing-features">
                            <li>25 scripts (30 pages each)</li>
                            <li>Studio-quality formatting</li>
                            <li>Unlimited revisions</li>
                            <li>12-hour delivery</li>
                            <li>24/7 VIP support</li>
                            <li>Full development services</li>
                            <li>Senior dedicated writer</li>
                            <li>Daily consultations</li>
                        </ul>
                        <button class="select-plan synex-elite-btn" onclick="selectPlan('Synex Elite $299/month')">Select Plan</button>
                    </div>
                    
                    <!-- Platinum Plan -->
                    <div class="pricing-card">
                        <h3>Synex Platinum</h3>
                        <div class="price">$349<span>/month</span></div>
                        <ul class="pricing-features">
                            <li>30 scripts (35 pages each)</li>
                            <li>Studio-quality formatting</li>
                            <li>Unlimited revisions</li>
                            <li>8-hour delivery</li>
                            <li>24/7 executive support</li>
                            <li>Full development services</li>
                            <li>Senior writer team</li>
                            <li>Unlimited consultations</li>
                        </ul>
                        <button class="select-plan synex-platinum-btn" onclick="selectPlan('Synex Platinum $349/month')">Select Plan</button>
                    </div>
                    
                    <!-- VIP Plan -->
                    <div class="pricing-card">
                        <h3>Synex VIP</h3>
                        <div class="price">$499<span>/month</span></div>
                        <ul class="pricing-features">
                            <li>Unlimited scripts</li>
                            <li>Studio-quality formatting</li>
                            <li>Unlimited revisions</li>
                            <li>6-hour delivery</li>
                            <li>24/7 dedicated support</li>
                            <li>Full development services</li>
                            <li>Executive writer team</li>
                            <li>Unlimited consultations</li>
                            <li>Priority scheduling</li>
                            <li>Custom contract terms</li>
                        </ul>
                        <button class="select-plan synex-vip-btn" onclick="selectPlan('Synex VIP $499/month')">Select Plan</button>
                    </div>
                </div>
            </div>
        </div>
    </section>
    
    <footer id="contact">
        <div class="container">
            <div class="footer-links">
                <a href="#">Privacy Policy</a>
                <a href="#">Terms of Service</a>
                <a href="mailto:contact@synex.com">Contact Us</a>
                <a href="#">FAQ</a>
                <a href="#">Refund Policy</a>
            </div>
            <p class="copyright">© 2023 Synex Script Writing. All rights reserved.</p>
        </div>
    </footer>
    
    <!-- Mobile bottom navigation -->
    <nav class="mobile-bottom-nav">
        <a href="#" class="mobile-nav-item">
            <i class="fas fa-home"></i>
            <span>Home</span>
        </a>
        <a href="#pricing" class="mobile-nav-item">
            <i class="fas fa-tags"></i>
            <span>Pricing</span>
        </a>
        <a href="#contact" class="mobile-nav-item">
            <i class="fas fa-envelope"></i>
            <span>Contact</span>
        </a>
    </nav>
    
    <!-- Font Awesome for icons -->
    <script src="https://kit.fontawesome.com/a076d05399.js" crossorigin="anonymous"></script>
    
    <script>
        // Netflix-style intro
        document.addEventListener('DOMContentLoaded', function() {
            setTimeout(function() {
                document.querySelector('.intro-overlay').style.display = 'none';
            }, 3500);
        });

        // Header scroll effect
        window.addEventListener('scroll', function() {
            const header = document.getElementById('main-header');
            if (window.scrollY > 50) {
                header.classList.add('scrolled');
            } else {
                header.classList.remove('scrolled');
            }
        });

        // Mobile menu toggle
        const menuToggle = document.querySelector('.menu-toggle');
        const navLinks = document.querySelector('.nav-links');
        const overlay = document.querySelector('.overlay');
        
        menuToggle.addEventListener('click', function() {
            this.classList.toggle('active');
            navLinks.classList.toggle('active');
            overlay.classList.toggle('active');
            
            // Toggle body scroll when menu is open
            if (navLinks.classList.contains('active')) {
                document.body.style.overflow = 'hidden';
            } else {
                document.body.style.overflow = '';
            }
        });
        
        // Close menu when clicking on overlay
        overlay.addEventListener('click', function() {
            menuToggle.classList.remove('active');
            navLinks.classList.remove('active');
            this.classList.remove('active');
            document.body.style.overflow = '';
        });
        
        // Close menu when clicking on a link
        document.querySelectorAll('.nav-links a').forEach(link => {
            link.addEventListener('click', function() {
                menuToggle.classList.remove('active');
                navLinks.classList.remove('active');
                overlay.classList.remove('active');
                document.body.style.overflow = '';
            });
        });
        
        // Load more plans button
        document.getElementById('load-more-plans').addEventListener('click', function() {
            const additionalPlans = document.getElementById('additional-plans');
            if (additionalPlans.style.display === 'none') {
                additionalPlans.style.display = 'block';
                this.textContent = 'Show Less Plans';
            } else {
                additionalPlans.style.display = 'none';
                this.textContent = 'View More Plans';
            }
            
            // Scroll to the button after showing more plans
            this.scrollIntoView({ behavior: 'smooth' });
        });

        function selectPlan(planName) {
            // WhatsApp number in international format (91 = India country code)
            const whatsappNumber = "918618365136";
            
            // Custom message template
            const message = `Hello Synex Team,\n\nI'm interested in your ${planName}.\n\nPlease send me more details about:\n- Exact pricing\n- Delivery timelines\n- Revision process\n- Payment options\n\nThank you!\n\n[Your Name]`;
            
            // Create WhatsApp link
            const whatsappUrl = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`;
            
            // Try to open in new tab
            try {
                const newWindow = window.open(whatsappUrl, '_blank', 'noopener,noreferrer');
                
                // Fallback if popup is blocked
                if (!newWindow || newWindow.closed || typeof newWindow.closed === 'undefined') {
                    window.location.href = whatsappUrl;
                }
            } catch (e) {
                // Ultimate fallback to email
                const emailSubject = `Inquiry about ${planName}`;
                const emailBody = `Hello Synex Team,\n\nI'm interested in your ${planName} plan.\nPlease contact me with details about:\n- Exact pricing\n- Delivery timelines\n- Revision process\n- Payment options\n\nBest regards,\n[Your Name]`;
                window.location.href = `mailto:contact@synex.com?subject=${encodeURIComponent(emailSubject)}&body=${encodeURIComponent(emailBody)}`;
            }
        }
    </script>
</body>
</html>
