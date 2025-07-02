<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Synex - Professional Script Writing Services</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
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
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 0 20px;
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
            flex-direction: column;
            transition: opacity 1s ease-out;
        }
        
        .intro-logo {
            width: 300px;
            height: 150px;
            position: relative;
            animation: pulse 2s infinite;
        }
        
        .loading-bar {
            width: 200px;
            height: 4px;
            background: rgba(255,255,255,0.2);
            margin-top: 30px;
            border-radius: 2px;
            overflow: hidden;
            position: relative;
        }
        
        .loading-progress {
            position: absolute;
            left: 0;
            top: 0;
            height: 100%;
            width: 0;
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            animation: loading 3s forwards;
        }
        
        @keyframes loading {
            0% { width: 0; }
            100% { width: 100%; }
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        .intro-logo::before {
            content: '';
            position: absolute;
            top: -10px;
            left: -10px;
            right: -10px;
            bottom: -10px;
            background: linear-gradient(45deg, 
                #ff0000, #ff7300, #fffb00, 
                #48ff00, #00ffd5, #002bff, 
                #7a00ff, #ff00c8, #ff0000);
            background-size: 400%;
            border-radius: 20px;
            z-index: -1;
            filter: blur(20px);
            animation: rainbow 20s linear infinite;
        }
        
        @keyframes rainbow {
            0% { background-position: 0% 50%; }
            100% { background-position: 400% 50%; }
        }
        
        header {
            background-color: rgba(0, 0, 0, 0.9);
            color: white;
            padding: 20px 0;
            box-shadow: 0 2px 10px rgba(0,0,0,0.5);
            position: fixed;
            width: 100%;
            z-index: 100;
            transition: all 0.3s ease;
        }
        
        header.scrolled {
            padding: 15px 0;
            background-color: rgba(0, 0, 0, 0.95);
            box-shadow: 0 5px 20px rgba(0, 198, 255, 0.2);
        }
        
        nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .logo {
            font-size: 28px;
            font-weight: bold;
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 10px rgba(0, 198, 255, 0.3);
        }
        
        .nav-links a {
            color: white;
            text-decoration: none;
            margin-left: 25px;
            font-weight: 500;
            position: relative;
        }
        
        .nav-links a:hover {
            color: #00c6ff;
        }
        
        .nav-links a::after {
            content: '';
            position: absolute;
            width: 0;
            height: 2px;
            bottom: -5px;
            left: 0;
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            transition: width 0.3s;
        }
        
        .nav-links a:hover::after {
            width: 100%;
        }
        
        .hero {
            background: linear-gradient(rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.7)), 
                        url('https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80');
            background-size: cover;
            background-position: center;
            height: 100vh;
            display: flex;
            align-items: center;
            text-align: center;
            position: relative;
            overflow: hidden;
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
            font-size: 4.5rem;
            margin-bottom: 20px;
            background: linear-gradient(90deg, #00c6ff, #0072ff, #00c6ff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 20px rgba(0, 198, 255, 0.3);
        }
        
        .hero p {
            font-size: 1.5rem;
            max-width: 700px;
            margin: 0 auto 30px;
            color: rgba(255,255,255,0.9);
        }
        
        .cta-button {
            display: inline-block;
            background: linear-gradient(45deg, #00c6ff, #0072ff);
            color: white;
            padding: 15px 40px;
            border-radius: 30px;
            text-decoration: none;
            font-weight: bold;
            font-size: 1.1rem;
            box-shadow: 0 5px 15px rgba(0, 198, 255, 0.4);
            position: relative;
            overflow: hidden;
            transition: transform 0.3s, box-shadow 0.3s;
        }
        
        .cta-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 198, 255, 0.6);
        }
        
        /* New Script Showcase Section */
        .showcase {
            padding: 100px 0;
            background-color: #111;
            position: relative;
            overflow: hidden;
        }
        
        .showcase::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, #00c6ff, #0072ff, #00c6ff, #0072ff, #00c6ff);
        }
        
        .script-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 30px;
            margin-top: 50px;
        }
        
        .script-card {
            background: rgba(30, 30, 30, 0.8);
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            transition: transform 0.3s, box-shadow 0.3s;
            position: relative;
        }
        
        .script-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 15px 35px rgba(0, 198, 255, 0.2);
        }
        
        .script-image {
            height: 200px;
            background-size: cover;
            background-position: center;
            position: relative;
        }
        
        .script-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(to top, rgba(0,0,0,0.8) 0%, rgba(0,0,0,0.3) 100%);
            display: flex;
            align-items: flex-end;
            padding: 20px;
        }
        
        .script-info {
            padding: 20px;
        }
        
        .script-info h3 {
            font-size: 1.3rem;
            margin-bottom: 10px;
            color: #00c6ff;
        }
        
        .script-info p {
            color: rgba(255,255,255,0.7);
            font-size: 0.9rem;
            margin-bottom: 15px;
        }
        
        .script-tag {
            display: inline-block;
            padding: 5px 10px;
            background: rgba(0, 198, 255, 0.2);
            color: #00c6ff;
            border-radius: 5px;
            font-size: 0.8rem;
            margin-right: 5px;
            margin-bottom: 5px;
        }
        
        /* New Process Section */
        .process {
            padding: 100px 0;
            background-color: #0a0a0a;
            position: relative;
        }
        
        .process::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, #00c6ff, #0072ff, #00c6ff, #0072ff, #00c6ff);
        }
        
        .timeline {
            position: relative;
            max-width: 1200px;
            margin: 50px auto 0;
        }
        
        .timeline::after {
            content: '';
            position: absolute;
            width: 6px;
            background: linear-gradient(to bottom, #00c6ff, #0072ff);
            top: 0;
            bottom: 0;
            left: 50%;
            margin-left: -3px;
            border-radius: 10px;
        }
        
        .timeline-item {
            padding: 10px 40px;
            position: relative;
            width: 50%;
            box-sizing: border-box;
            opacity: 0;
            transform: translateY(30px);
            transition: all 0.5s ease;
        }
        
        .timeline-item.visible {
            opacity: 1;
            transform: translateY(0);
        }
        
        .timeline-item:nth-child(odd) {
            left: 0;
        }
        
        .timeline-item:nth-child(even) {
            left: 50%;
        }
        
        .timeline-content {
            padding: 20px 30px;
            background: rgba(30, 30, 30, 0.8);
            position: relative;
            border-radius: 10px;
            box-shadow: 0 5px 25px rgba(0,0,0,0.3);
            border-left: 4px solid #00c6ff;
        }
        
        .timeline-item:nth-child(even) .timeline-content {
            border-left: none;
            border-right: 4px solid #0072ff;
        }
        
        .timeline-item::after {
            content: '';
            position: absolute;
            width: 25px;
            height: 25px;
            right: -17px;
            background-color: #00c6ff;
            border: 4px solid #0072ff;
            top: 15px;
            border-radius: 50%;
            z-index: 1;
        }
        
        .timeline-item:nth-child(even)::after {
            left: -17px;
        }
        
        .timeline-item h3 {
            color: #00c6ff;
            margin-bottom: 10px;
        }
        
        .timeline-item p {
            color: rgba(255,255,255,0.8);
            font-size: 0.95rem;
        }
        
        /* New Testimonials Section */
        .testimonials {
            padding: 100px 0;
            background-color: #111;
            position: relative;
        }
        
        .testimonials::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, #00c6ff, #0072ff, #00c6ff, #0072ff, #00c6ff);
        }
        
        .testimonial-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
            margin-top: 50px;
        }
        
        .testimonial-card {
            background: rgba(30, 30, 30, 0.8);
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            position: relative;
            border: 1px solid rgba(0, 198, 255, 0.2);
        }
        
        .testimonial-card::before {
            content: '"';
            position: absolute;
            top: 20px;
            left: 20px;
            font-size: 60px;
            color: rgba(0, 198, 255, 0.1);
            font-family: serif;
            line-height: 1;
        }
        
        .testimonial-content {
            margin-bottom: 20px;
            color: rgba(255,255,255,0.9);
            font-style: italic;
            position: relative;
            z-index: 1;
        }
        
        .testimonial-author {
            display: flex;
            align-items: center;
        }
        
        .author-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background-color: #0072ff;
            margin-right: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            font-size: 1.2rem;
        }
        
        .author-info h4 {
            color: #00c6ff;
            margin-bottom: 5px;
        }
        
        .author-info p {
            color: rgba(255,255,255,0.6);
            font-size: 0.8rem;
        }
        
        .rating {
            color: #ffc107;
            margin-top: 5px;
        }
        
        /* New FAQ Section */
        .faq {
            padding: 100px 0;
            background-color: #111;
            position: relative;
        }
        
        .faq::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, #00c6ff, #0072ff, #00c6ff, #0072ff, #00c6ff);
        }
        
        .faq-container {
            max-width: 800px;
            margin: 50px auto 0;
        }
        
        .faq-item {
            margin-bottom: 15px;
            border-radius: 8px;
            overflow: hidden;
            background: rgba(30, 30, 30, 0.8);
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }
        
        .faq-question {
            padding: 20px;
            background: rgba(0, 198, 255, 0.1);
            color: white;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-weight: 500;
            transition: background 0.3s;
        }
        
        .faq-question:hover {
            background: rgba(0, 198, 255, 0.2);
        }
        
        .faq-question::after {
            content: '+';
            font-size: 1.5rem;
            transition: transform 0.3s;
        }
        
        .faq-item.active .faq-question::after {
            transform: rotate(45deg);
        }
        
        .faq-answer {
            padding: 0;
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s ease-out, padding 0.3s ease;
        }
        
        .faq-item.active .faq-answer {
            padding: 20px;
            max-height: 500px;
        }
        
        .faq-answer p {
            color: rgba(255,255,255,0.8);
            line-height: 1.6;
        }
        
        /* New Contact Section */
        .contact {
            padding: 100px 0;
            background-color: #0a0a0a;
            position: relative;
        }
        
        .contact::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, #00c6ff, #0072ff, #00c6ff, #0072ff, #00c6ff);
        }
        
        .contact-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 50px;
            margin-top: 50px;
        }
        
        .contact-info {
            display: flex;
            flex-direction: column;
            gap: 30px;
        }
        
        .contact-card {
            background: rgba(30, 30, 30, 0.8);
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            border-left: 4px solid #00c6ff;
        }
        
        .contact-card h3 {
            color: #00c6ff;
            margin-bottom: 20px;
            font-size: 1.3rem;
        }
        
        .contact-details {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .contact-detail {
            display: flex;
            align-items: flex-start;
            gap: 15px;
        }
        
        .contact-icon {
            color: #00c6ff;
            font-size: 1.2rem;
            margin-top: 3px;
        }
        
        .contact-text h4 {
            color: white;
            margin-bottom: 5px;
        }
        
        .contact-text p, .contact-text a {
            color: rgba(255,255,255,0.7);
            text-decoration: none;
            transition: color 0.3s;
        }
        
        .contact-text a:hover {
            color: #00c6ff;
        }
        
        .contact-form {
            background: rgba(30, 30, 30, 0.8);
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 8px;
            color: rgba(255,255,255,0.9);
        }
        
        .form-control {
            width: 100%;
            padding: 12px 15px;
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 5px;
            color: white;
            font-size: 1rem;
            transition: all 0.3s;
        }
        
        .form-control:focus {
            outline: none;
            border-color: #00c6ff;
            background: rgba(0, 198, 255, 0.1);
        }
        
        textarea.form-control {
            min-height: 150px;
            resize: vertical;
        }
        
        .submit-btn {
            background: linear-gradient(45deg, #00c6ff, #0072ff);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            font-size: 1rem;
            transition: all 0.3s;
        }
        
        .submit-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 198, 255, 0.4);
        }
        
        /* Pricing Section (existing but moved down in CSS) */
        .pricing {
            padding: 100px 0;
            background-color: #0a0a0a;
            position: relative;
        }
        
        .pricing::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, #00c6ff, #0072ff, #00c6ff, #0072ff, #00c6ff);
        }
        
        .section-title {
            text-align: center;
            margin-bottom: 70px;
        }
        
        .section-title h2 {
            font-size: 3rem;
            background: linear-gradient(90deg, #00c6ff, #0072ff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 15px;
        }
        
        .section-title p {
            font-size: 1.2rem;
            color: rgba(255,255,255,0.7);
        }
        
        .pricing-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 25px;
            margin-top: 50px;
        }
        
        .pricing-card {
            background: rgba(30, 30, 30, 0.8);
            border-radius: 15px;
            padding: 30px 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            text-align: center;
            position: relative;
            overflow: hidden;
            border: 1px solid rgba(255,255,255,0.1);
            transition: transform 0.3s, box-shadow 0.3s;
        }
        
        .pricing-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 15px 35px rgba(0, 198, 255, 0.2);
        }
        
        .pricing-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, #00c6ff, #0072ff);
        }
        
        .free-plan::before {
            background: linear-gradient(90deg, #2ecc71, #27ae60);
        }
        
        .popular-plan::before {
            background: linear-gradient(90deg, #00c6ff, #0072ff);
        }
        
        .plan-tag {
            position: absolute;
            top: 15px;
            right: 15px;
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.8rem;
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
        }
        
        .pricing-card h3 {
            font-size: 1.5rem;
            margin-bottom: 15px;
            color: white;
        }
        
        .price {
            font-size: 2.5rem;
            font-weight: bold;
            margin-bottom: 20px;
            position: relative;
            display: inline-block;
        }
        
        .price::after {
            content: '';
            position: absolute;
            bottom: -10px;
            left: 0;
            width: 100%;
            height: 2px;
            background: rgba(255,255,255,0.1);
        }
        
        .price span {
            font-size: 1rem;
            color: rgba(255,255,255,0.6);
        }
        
        .pricing-features {
            list-style: none;
            margin-bottom: 25px;
            text-align: left;
            font-size: 0.9rem;
        }
        
        .pricing-features li {
            margin-bottom: 12px;
            padding-bottom: 12px;
            border-bottom: 1px solid rgba(255,255,255,0.05);
            position: relative;
            padding-left: 25px;
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
            padding: 12px 25px;
            border-radius: 30px;
            text-decoration: none;
            font-weight: bold;
            font-size: 0.9rem;
            border: none;
            cursor: pointer;
            width: 100%;
            text-transform: uppercase;
            letter-spacing: 1px;
            position: relative;
            overflow: hidden;
            transition: all 0.3s;
        }
        
        .select-plan:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 198, 255, 0.4);
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
            padding: 60px 0 30px;
            text-align: center;
            position: relative;
        }
        
        footer::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, #00c6ff, #0072ff, #00c6ff, #0072ff, #00c6ff);
        }
        
        .footer-links {
            display: flex;
            justify-content: center;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }
        
        .footer-links a {
            color: rgba(255,255,255,0.7);
            text-decoration: none;
            margin: 0 15px 15px;
            position: relative;
            transition: color 0.3s;
        }
        
        .footer-links a:hover {
            color: #00c6ff;
        }
        
        .footer-links a::after {
            content: '•';
            position: absolute;
            right: -20px;
            color: rgba(255,255,255,0.3);
        }
        
        .footer-links a:last-child::after {
            display: none;
        }
        
        .social-footer {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .social-footer a {
            color: white;
            width: 45px;
            height: 45px;
            border-radius: 50%;
            background: rgba(255,255,255,0.1);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
            transition: all 0.3s;
        }
        
        .social-footer a:hover {
            background: #00c6ff;
            transform: translateY(-3px);
        }
        
        .copyright {
            color: rgba(255,255,255,0.5);
            font-size: 0.9rem;
            margin-top: 30px;
        }
        
        /* RGB floating elements */
        .rgb-circle {
            position: absolute;
            border-radius: 50%;
            filter: blur(60px);
            opacity: 0.3;
            z-index: -1;
            animation: float 15s infinite ease-in-out;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0) translateX(0); }
            50% { transform: translateY(-20px) translateX(20px); }
        }
        
        .rgb-circle:nth-child(1) {
            width: 300px;
            height: 300px;
            background: #00c6ff;
            top: 20%;
            left: 10%;
            animation-delay: 0s;
        }
        
        .rgb-circle:nth-child(2) {
            width: 400px;
            height: 400px;
            background: #0072ff;
            bottom: 10%;
            right: 10%;
            animation-delay: 2s;
        }
        
        .rgb-circle:nth-child(3) {
            width: 200px;
            height: 200px;
            background: #00c6ff;
            top: 60%;
            left: 30%;
            animation-delay: 4s;
        }
        
        /* Back to top button */
        .back-to-top {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 50px;
            height: 50px;
            background: linear-gradient(45deg, #00c6ff, #0072ff);
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
            cursor: pointer;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(0, 198, 255, 0.4);
            z-index: 99;
        }
        
        .back-to-top.visible {
            opacity: 1;
            visibility: visible;
        }
        
        .back-to-top:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 20px rgba(0, 198, 255, 0.6);
        }
        
        @media (max-width: 768px) {
            .hero h1 {
                font-size: 2.5rem;
            }
            
            .hero p {
                font-size: 1.1rem;
            }
            
            .pricing-grid {
                grid-template-columns: 1fr;
            }
            
            .section-title h2 {
                font-size: 2rem;
            }
            
            .timeline::after {
                left: 31px;
            }
            
            .timeline-item {
                width: 100%;
                padding-left: 70px;
                padding-right: 25px;
            }
            
            .timeline-item:nth-child(even) {
                left: 0;
            }
            
            .timeline-item::after {
                left: 18px;
            }
            
            .timeline-item:nth-child(even)::after {
                left: 18px;
            }
            
            .timeline-content {
                padding: 15px;
            }
            
            .contact-container {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <!-- Netflix-style intro animation -->
    <div class="intro-overlay" id="introOverlay">
        <div class="intro-logo">
            <svg viewBox="0 0 500 200">
                <path fill="#00c6ff" d="M50,100 L150,100 L150,50 L250,150 L250,100 L350,100 L350,150 L450,50 L450,150 L400,150 L400,100 L300,100 L300,150 L200,50 L200,100 L100,100 L100,150 L50,150 Z"/>
            </svg>
        </div>
        <div class="loading-bar">
            <div class="loading-progress"></div>
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
                <div class="nav-links">
                    <a href="#showcase">Showcase</a>
                    <a href="#process">Process</a>
                    <a href="#pricing">Pricing</a>
                    <a href="#testimonials">Testimonials</a>
                    <a href="#faq">FAQ</a>
                    <a href="#contact">Contact</a>
                </div>
            </nav>
        </div>
    </header>
    
    <section class="hero">
        <div class="hero-content">
            <div class="container">
                <h1>Professional Script Writing Services</h1>
                <p>Our expert writers craft compelling scripts for films, TV shows, commercials, and more. No AI - just human creativity and expertise.</p>
                <a href="#pricing" class="cta-button">View All Plans</a>
            </div>
        </div>
    </section>
    
    <!-- New Script Showcase Section -->
    <section class="showcase" id="showcase">
        <div class="container">
            <div class="section-title">
                <h2>Our Script Showcase</h2>
                <p>Explore samples of our professional script writing work</p>
            </div>
            
            <div class="script-grid">
                <div class="script-card">
                    <div class="script-image" style="background-image: url('https://images.unsplash.com/photo-1485846234645-a62644f84728?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80')">
                        <div class="script-overlay">
                            <div>
                                <span class="script-tag">Feature Film</span>
                                <span class="script-tag">Drama</span>
                            </div>
                        </div>
                    </div>
                    <div class="script-info">
                        <h3>The Last Sunset</h3>
                        <p>A gripping drama about redemption and second chances set in the American Southwest.</p>
                    </div>
                </div>
                
                <div class="script-card">
                    <div class="script-image" style="background-image: url('https://images.unsplash.com/photo-1542204165-65bf26472b9b?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80')">
                        <div class="script-overlay">
                            <div>
                                <span class="script-tag">TV Pilot</span>
                                <span class="script-tag">Sci-Fi</span>
                            </div>
                        </div>
                    </div>
                    <div class="script-info">
                        <h3>Quantum Paradox</h3>
                        <p>A mind-bending sci-fi series exploring parallel universes and their consequences.</p>
                    </div>
                </div>
                
                <div class="script-card">
                    <div class="script-image" style="background-image: url('https://images.unsplash.com/photo-1517604931442-7e0c8ed2963c?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80')">
                        <div class="script-overlay">
                            <div>
                                <span class="script-tag">Commercial</span>
                                <span class="script-tag">Tech</span>
                            </div>
                        </div>
                    </div>
                    <div class="script-info">
                        <h3>Future Now</h3>
                        <p>A high-energy commercial script for a cutting-edge tech product launch.</p>
                    </div>
                </div>
            </div>
        </div>
    </section>
    
    <!-- New Process Section -->
    <section class="process" id="process">
        <div class="container">
            <div class="section-title">
                <h2>Our Writing Process</h2>
                <p>Transparent workflow from concept to final draft</p>
            </div>
            
            <div class="timeline">
                <div class="timeline-item">
                    <div class="timeline-content">
                        <h3>Initial Consultation</h3>
                        <p>We discuss your vision, goals, and requirements for the script. This helps us understand your unique needs and expectations.</p>
                    </div>
                </div>
                
                <div class="timeline-item">
                    <div class="timeline-content">
                        <h3>Concept Development</h3>
                        <p>Our team brainstorms ideas and develops a compelling concept that aligns with your vision and target audience.</p>
                    </div>
                </div>
                
                <div class="timeline-item">
                    <div class="timeline-content">
                        <h3>Outline Creation</h3>
                        <p>We create a detailed outline including plot points, character arcs, and key scenes for your approval.</p>
                    </div>
                </div>
                
                <div class="timeline-item">
                    <div class="timeline-content">
                        <h3>First Draft</h3>
                        <p>Our writers craft the first draft of your script, bringing the outline to life with dialogue and action.</p>
                    </div>
                </div>
                
                <div class="timeline-item">
                    <div class="timeline-content">
                        <h3>Review & Feedback</h3>
                        <p>You review the first draft and provide feedback. We schedule a call to discuss your thoughts in detail.</p>
                    </div>
                </div>
                
                <div class="timeline-item">
                    <div class="timeline-content">
                        <h3>Revisions</h3>
                        <p>We refine the script based on your feedback, making adjustments to characters, plot, and dialogue.</p>
                    </div>
                </div>
                
                <div class="timeline-item">
                    <div class="timeline-content">
                        <h3>Final Polish</h3>
                        <p>Our senior editors perform a final polish, ensuring professional formatting and flawless execution.</p>
                    </div>
                </div>
                
                <div class="timeline-item">
                    <div class="timeline-content">
                        <h3>Delivery</h3>
                        <p>You receive the final script in your preferred format, ready for production or submission.</p>
                    </div>
                </div>
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
                    <button class="select-plan free-plan-btn" data-plan="Synex Trial">Try For Free</button>
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
                    <button class="select-plan synex-starter-btn" data-plan="Synex Starter">Get Started</button>
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
                    <button class="select-plan synex-basic-btn" data-plan="Synex Basic">Select Plan</button>
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
                    <button class="select-plan synex-standard-btn" data-plan="Synex Standard">Select Plan</button>
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
                    <button class="select-plan synex-pro-btn" data-plan="Synex Pro">Select Plan</button>
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
                    <button class="select-plan synex-premium-btn" data-plan="Synex Premium">Select Plan</button>
                </div>
                
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
                    <button class="select-plan synex-business-btn" data-plan="Synex Business">Select Plan</button>
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
                    <button class="select-plan synex-enterprise-btn" data-plan="Synex Enterprise">Select Plan</button>
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
                    <button class="select-plan synex-elite-btn" data-plan="Synex Elite">Select Plan</button>
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
                    <button class="select-plan synex-platinum-btn" data-plan="Synex Platinum">Select Plan</button>
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
                    <button class="select-plan synex-vip-btn" data-plan="Synex VIP">Select Plan</button>
                </div>
            </div>
        </div>
    </section>
    
    <!-- New Testimonials Section -->
    <section class="testimonials" id="testimonials">
        <div class="container">
            <div class="section-title">
                <h2>Client Testimonials</h2>
                <p>What our clients say about our script writing services</p>
            </div>
            
            <div class="testimonial-grid">
                <div class="testimonial-card">
                    <div class="testimonial-content">
                        <p>Synex transformed my vague idea into a compelling screenplay that got me my first production deal. Their attention to character development and plot structure is unmatched.</p>
                    </div>
                    <div class="testimonial-author">
                        <div class="author-avatar">JD</div>
                        <div class="author-info">
                            <h4>James Donovan</h4>
                            <p>Independent Filmmaker</p>
                            <div class="rating">
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="testimonial-card">
                    <div class="testimonial-content">
                        <p>The team at Synex understood my vision perfectly. They delivered a commercial script that exceeded our expectations and helped our campaign achieve a 30% increase in engagement.</p>
                    </div>
                    <div class="testimonial-author">
                        <div class="author-avatar">SM</div>
                        <div class="author-info">
                            <h4>Sarah Mitchell</h4>
                            <p>Marketing Director</p>
                            <div class="rating">
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="testimonial-card">
                    <div class="testimonial-content">
                        <p>As a first-time screenwriter, I was nervous about the process. Synex guided me through every step and delivered a script that's now being considered by several production companies.</p>
                    </div>
                    <div class="testimonial-author">
                        <div class="author-avatar">TW</div>
                        <div class="author-info">
                            <h4>Thomas Wright</h4>
                            <p>Aspiring Screenwriter</p>
                            <div class="rating">
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star"></i>
                                <i class="fas fa-star-half-alt"></i>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    
    <!-- New FAQ Section -->
    <section class="faq" id="faq">
        <div class="container">
            <div class="section-title">
                <h2>Frequently Asked Questions</h2>
                <p>Everything you need to know about our services</p>
            </div>
            
            <div class="faq-container">
                <div class="faq-item">
                    <div class="faq-question">
                        <h3>How does the script writing process work?</h3>
                    </div>
                    <div class="faq-answer">
                        <p>Our process begins with an in-depth consultation to understand your vision. We then develop a concept, create an outline for your approval, write the first draft, incorporate your feedback through revisions, and deliver a polished final script. The exact timeline depends on the project scope and your selected plan.</p>
                    </div>
                </div>
                
                <div class="faq-item">
                    <div class="faq-question">
                        <h3>What industries do you specialize in?</h3>
                    </div>
                    <div class="faq-answer">
                        <p>Our team has experience across multiple industries including feature films, television (both drama and comedy), commercials, corporate videos, web series, and theater. We have specialists in different genres from sci-fi and fantasy to romance and documentary.</p>
                    </div>
                </div>
                
                <div class="faq-item">
                    <div class="faq-question">
                        <h3>Can I see samples of your previous work?</h3>
                    </div>
                    <div class="faq-answer">
                        <p>Absolutely! We have a showcase section on our website with samples of our work (with client permission). We can also provide specific samples relevant to your project upon request after signing an NDA if needed.</p>
                    </div>
                </div>
                
                <div class="faq-item">
                    <div class="faq-question">
                        <h3>How many revisions are included?</h3>
                    </div>
                    <div class="faq-answer">
                        <p>The number of revisions depends on your selected plan. Our Trial plan includes 1 revision, Starter includes 2 per script, while Pro and higher plans include unlimited revisions. We work closely with you to ensure the script meets your expectations within the agreed revision scope.</p>
                    </div>
                </div>
                
                <div class="faq-item">
                    <div class="faq-question">
                        <h3>What file formats do you deliver?</h3>
                    </div>
                    <div class="faq-answer">
                        <p>We typically deliver scripts in Final Draft (.fdx) and PDF formats, but can provide other formats upon request including Celtx, Microsoft Word, and Fountain. We ensure all scripts follow industry-standard formatting.</p>
                    </div>
                </div>
                
                <div class="faq-item">
                    <div class="faq-question">
                        <h3>Who owns the rights to the script?</h3>
                    </div>
                    <div class="faq-answer">
                        <p>You retain all rights to the script. We work on a work-for-hire basis, meaning all creative work we produce for you belongs entirely to you. We're happy to sign any necessary agreements to confirm this arrangement.</p>
                    </div>
                </div>
            </div>
        </div>
    </section>
    
    <!-- New Contact Section -->
    <section class="contact" id="contact">
        <div class="container">
            <div class="section-title">
                <h2>Get In Touch</h2>
                <p>Have questions? We're here to help</p>
            </div>
            
            <div class="contact-container">
                <div class="contact-info">
                    <div class="contact-card">
                        <h3>Contact Information</h3>
                        <div class="contact-details">
                            <div class="contact-detail">
                                <div class="contact-icon">
                                    <i class="fas fa-map-marker-alt"></i>
                                </div>
                                <div class="contact-text">
                                    <h4>Location</h4>
                                    <p>123 Screenplay Lane, Hollywood, CA 90210</p>
                                </div>
                            </div>
                            
                            <div class="contact-detail">
                                <div class="contact-icon">
                                    <i class="fas fa-phone-alt"></i>
                                </div>
                                <div class="contact-text">
                                    <h4>Phone</h4>
                                    <a href="tel:+1234567890">(123) 456-7890</a>
                                </div>
                            </div>
                            
                            <div class="contact-detail">
                                <div class="contact-icon">
                                    <i class="fas fa-envelope"></i>
                                </div>
                                <div class="contact-text">
                                    <h4>Email</h4>
                                    <a href="mailto:contact@synex.com">contact@synex.com</a>
                                </div>
                            </div>
                            
                            <div class="contact-detail">
                                <div class="contact-icon">
                                    <i class="fas fa-clock"></i>
                                </div>
                                <div class="contact-text">
                                    <h4>Working Hours</h4>
                                    <p>Monday - Friday: 9am - 6pm PST</p>
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="contact-card">
                        <h3>Why Choose Synex?</h3>
                        <div class="contact-details">
                            <div class="contact-detail">
                                <div class="contact-icon">
                                    <i class="fas fa-pen-fancy"></i>
                                </div>
                                <div class="contact-text">
                                    <h4>Professional Writers</h4>
                                    <p>Industry-experienced scriptwriters with proven track records</p>
                                </div>
                            </div>
                            
                            <div class="contact-detail">
                                <div class="contact-icon">
                                    <i class="fas fa-certificate"></i>
                                </div>
                                <div class="contact-text">
                                    <h4>Quality Guarantee</h4>
                                    <p>We stand behind our work with satisfaction guarantees</p>
                                </div>
                            </div>
                            
                            <div class="contact-detail">
                                <div class="contact-icon">
                                    <i class="fas fa-lock"></i>
                                </div>
                                <div class="contact-text">
                                    <h4>Confidentiality</h4>
                                    <p>Your ideas and projects are kept completely secure</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="contact-form">
                    <form id="contactForm">
                        <div class="form-group">
                            <label for="name">Your Name</label>
                            <input type="text" id="name" class="form-control" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="email">Email Address</label>
                            <input type="email" id="email" class="form-control" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="subject">Subject</label>
                            <input type="text" id="subject" class="form-control" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="message">Your Message</label>
                            <textarea id="message" class="form-control" required></textarea>
                        </div>
                        
                        <button type="submit" class="submit-btn">Send Message</button>
                    </form>
                </div>
            </div>
        </div>
    </section>
    
    <footer>
        <div class="container">
            <div class="footer-links">
                <a href="#">Privacy Policy</a>
                <a href="#">Terms of Service</a>
                <a href="mailto:contact@synex.com">Contact Us</a>
                <a href="#">FAQ</a>
                <a href="#">Refund Policy</a>
            </div>
            
            <div class="social-footer">
                <a href="#"><i class="fab fa-facebook-f"></i></a>
                <a href="#"><i class="fab fa-twitter"></i></a>
                <a href="#"><i class="fab fa-instagram"></i></a>
                <a href="#"><i class="fab fa-linkedin-in"></i></a>
            </div>
            
            <p class="copyright">© 2023 Synex Script Writing. All rights reserved.</p>
        </div>
    </footer>

    <!-- Back to top button -->
    <div class="back-to-top" id="backToTop">
        <i class="fas fa-arrow-up"></i>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Intro animation
            const introOverlay = document.getElementById('introOverlay');
            
            // Hide intro after 3 seconds
            setTimeout(() => {
                introOverlay.style.opacity = '0';
                setTimeout(() => {
                    introOverlay.style.display = 'none';
                }, 1000);
            }, 3000);
            
            // Header scroll effect
            const header = document.getElementById('main-header');
            window.addEventListener('scroll', function() {
                if (window.scrollY > 50) {
                    header.classList.add('scrolled');
                } else {
                    header.classList.remove('scrolled');
                }
            });
            
            // Smooth scrolling for anchor links
            document.querySelectorAll('a[href^="#"]').forEach(anchor => {
                anchor.addEventListener('click', function(e) {
                    e.preventDefault();
                    
                    const targetId = this.getAttribute('href');
                    if (targetId === '#') return;
                    
                    const targetElement = document.querySelector(targetId);
                    if (targetElement) {