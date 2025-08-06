8<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Synex - Script Writing Agency</title>
  <meta name="description" content="India's Best Script Writing Agency for YouTubers, Gamers, and Content Creators." />
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;900&display=swap" rel="stylesheet"/>
  <link href="https://unpkg.com/aos@2.3.1/dist/aos.css" rel="stylesheet" />
  <script src="https://cdn.jsdelivr.net/npm/particles.js@2.0.0/particles.min.js"></script>
  <script src="https://unpkg.com/aos@2.3.1/dist/aos.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/vanilla-tilt/1.7.0/vanilla-tilt.min.js"></script>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      background: #0f0f0f;
      color: white;
      font-family: 'Poppins', sans-serif;
      overflow-x: hidden;
    }
    video.bg-video {
      position: fixed;
      top: 0; left: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
      z-index: -10;
      opacity: 0.1;
    }
    header {
      transition: opacity 0.5s ease;
      position: fixed;
      width: 100%;
      z-index: 999;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px 50px;
      background: rgba(0,0,0,0.4);
      backdrop-filter: blur(20px);
      border-radius: 20px;
      margin: 10px;
    }
    .logo {
      font-size: 2rem;
      font-weight: 900;
      background: linear-gradient(90deg, #00f2ff, #ff00ff);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }
    nav a {
      color: white;
      text-decoration: none;
      margin: 0 20px;
      font-weight: 600;
    }
    nav a:hover { color: #00f2ff; }
    .toggle {
      cursor: pointer;
      border: 2px solid white;
      padding: 5px 10px;
      border-radius: 30px;
    }
    .toggle:hover {
      background-color: #00f2ff;
      color: black;
    }
    .hero {
      height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      text-align: center;
    }
    .hero h1 {
      font-size: 4rem;
      background: linear-gradient(90deg,#00f2ff,#ff00ff,#00f2ff);
      background-size: 200% auto;
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      animation: shine 3s linear infinite;
    }
    @keyframes shine {
      to { background-position: 200% center; }
    }
    .hero p {
      margin-top: 20px;
      max-width: 600px;
      color: #ccc;
    }
    section { 
      padding: 100px 50px; 
      text-align: center;
      max-width: 1200px;
      margin: 0 auto;
    }
    .section-title {
      font-size: 2.5rem;
      margin-bottom: 50px;
      background: linear-gradient(90deg, #00f2ff, #ff00ff);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      text-transform: uppercase;
      letter-spacing: 2px;
    }
    .section-subtitle {
      font-size: 1.2rem;
      color: #ccc;
      margin-bottom: 30px;
    }
    .pricing-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 30px;
      margin-top: 50px;
    }
    .pricing-card {
      background: rgba(255,255,255,0.05);
      border-radius: 20px;
      padding: 30px;
      transition: all 0.3s ease;
      border: 1px solid rgba(255,255,255,0.1);
    }
    .pricing-card:hover {
      transform: translateY(-10px);
      box-shadow: 0 10px 20px rgba(0, 242, 255, 0.2);
      border: 1px solid #00f2ff;
    }
    .pricing-card h3 {
      font-size: 1.8rem;
      margin-bottom: 15px;
      color: #00f2ff;
    }
    .pricing-features {
      list-style: none;
      margin: 20px 0;
      text-align: left;
    }
    .pricing-features li {
      margin-bottom: 10px;
      position: relative;
      padding-left: 25px;
      color: #ccc;
    }
    .pricing-features li:before {
      content: "✓";
      color: #00f2ff;
      position: absolute;
      left: 0;
    }
    .price-tag {
      font-size: 2.5rem;
      font-weight: 900;
      margin: 20px 0;
      background: linear-gradient(90deg, #00f2ff, #ff00ff);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }
    .buy-btn {
      display: inline-block;
      padding: 12px 30px;
      background: linear-gradient(90deg,#00f2ff,#ff00ff);
      color: white;
      text-decoration: none;
      border: none;
      border-radius: 30px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
      width: 100%;
      max-width: 200px;
    }
    .buy-btn:hover {
      transform: scale(1.05);
      box-shadow: 0 0 20px rgba(0, 242, 255, 0.5);
    }
    .testimonials, .portfolio {
      background: #121212;
      border-radius: 20px;
      margin: 50px auto;
    }
    .testimonial-card, .portfolio-item {
      background: rgba(255,255,255,0.05);
      border-radius: 20px;
      padding: 20px;
      margin: 20px auto;
      max-width: 600px;
    }
    footer {
      padding: 30px;
      background: #000;
      text-align: center;
      color: #888;
    }
    .floating-whatsapp {
      position: fixed;
      bottom: 20px;
      right: 20px;
      z-index: 9999;
    }
    .floating-whatsapp img {
      width: 60px;
      transition: 0.3s;
    }
    .floating-whatsapp img:hover {
      transform: scale(1.1);
    }
    @media(max-width:768px) {
      .hero h1 { font-size: 2.5rem; }
      header { flex-direction: column; gap: 10px; }
      .section-title { font-size: 2rem; }
      .pricing-grid { grid-template-columns: 1fr; }
    }
    #main-header {
      transition: top 0.4s ease-in-out;
      top: 10px;
    }
    /* Modal styles */
    .modal {
      display: none;
      position: fixed;
      z-index: 99999;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0,0,0,0.8);
    }
    .modal-content {
      background-color: #121212;
      margin: 15% auto;
      padding: 30px;
      border: 1px solid #00f2ff;
      border-radius: 20px;
      width: 80%;
      max-width: 500px;
      position: relative;
    }
    .close {
      color: #aaa;
      position: absolute;
      top: 10px;
      right: 20px;
      font-size: 28px;
      font-weight: bold;
      cursor: pointer;
    }
    .close:hover {
      color: #00f2ff;
    }
    .form-group {
      margin-bottom: 20px;
    }
    .form-group label {
      display: block;
      margin-bottom: 8px;
      color: #00f2ff;
    }
    .form-group input {
      width: 100%;
      padding: 12px;
      border-radius: 10px;
      border: 1px solid #333;
      background: #222;
      color: white;
    }
    .submit-btn {
      background: linear-gradient(90deg,#00f2ff,#ff00ff);
      color: white;
      border: none;
      padding: 12px 30px;
      border-radius: 30px;
      cursor: pointer;
      font-weight: 600;
      width: 100%;
      transition: all 0.3s ease;
    }
    .submit-btn:hover {
      transform: scale(1.02);
      box-shadow: 0 0 15px rgba(0, 242, 255, 0.5);
    }
  </style>
</head>
<body>
  <video class="bg-video" autoplay muted loop>
    <source src="https://cdn.coverr.co/videos/coverr-cloudy-sky-8170/1080p.mp4" type="video/mp4" />
  </video>
  <div id="particles-js"></div>

  <div class="floating-whatsapp">
    <a href="https://wa.me/918618365136?text=Hello%20Synex!%20I%20have%20a%20query." target="_blank">
      <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" alt="WhatsApp" />
    </a>
  </div>

  <!-- The Modal -->
  <div id="orderModal" class="modal">
    <div class="modal-content">
      <span class="close">&times;</span>
      <h2 style="margin-bottom: 20px; color: #00f2ff;">Complete Your Order</h2>
      <form id="orderForm">
        <input type="hidden" id="selectedPlan">
        <input type="hidden" id="selectedPrice">
        <div class="form-group">
          <label for="userName">Your Name</label>
          <input type="text" id="userName" required placeholder="Enter your full name">
        </div>
        <div class="form-group">
          <label for="userEmail">Topic for yout topic(compalsary in 100 words)</label>
          <input type="text" id="topic" placeholder="Enter your topic">
        </div>
        <div class="form-group">
          <label for="userPhone">WhatsApp Number</label>
          <input type="text" id="userPhone" required placeholder="Enter your WhatsApp number">
        </div>
        <button type="submit" class="submit-btn">PROCEED TO WHATSAPP</button>
      </form>
    </div>
  </div>

  <header id="main-header">
    <div class="logo" id="synex-logo">SYNEX</div>
    <nav>
      <a href="#">Home</a>
      <a href="#youtube-hooks">YouTube Hooks</a>
      <a href="#reels-shorts">Reels & Youtube Shorts</a>
      <a href="#long-videos">Long Videos</a>
      <a href="#brand-ads">Brand Ads</a>
    </nav>
    <div class="toggle" onclick="toggleTheme()">Toggle</div>
  </header>

  <section class="hero" data-aos="fade-up" data-aos-duration="1500">
    <h1 data-tilt>Professional Scripts for Creators</h1>
    <p data-tilt>Elevate your content with our expertly crafted scripts</p>
  </section>

  <!-- YouTube Hooks Section -->
  <section id="youtube-hooks" class="pricing-section" data-aos="fade-up">
    <h2 class="section-title">YOUTUBE CHANNEL HOOKS</h2>
    <p class="section-subtitle">5-second attention grabbers to boost your CTR</p>
    
    <div class="pricing-grid">
      <div class="pricing-card" data-aos="zoom-in">
        <h3>5-second Hook</h3>
        <ul class="pricing-features">
          <li>15 SEC HOOK SCRIPT</li>
          <li>Hooks viewers instantly</li>
          <li>Attention in first 3 seconds</li>
          <li>Perfect for any niche</li>
        </ul>
        <div class="price-tag">₹11</div>
        <button class="buy-btn" onclick="openOrderModal('5-second Hook', '11')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="100">
        <h3>Hook Pack</h3>
        <ul class="pricing-features">
          <li>5 unique hooks</li>
          <li>Variations for different videos</li>
          <li>Tested formulas</li>
          <li>High retention guaranteed</li>
        </ul>
        <div class="price-tag">₹49</div>
        <button class="buy-btn" onclick="openOrderModal('Hook Pack', '49')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="200">
        <h3>Premium Hooks</h3>
        <ul class="pricing-features">
          <li>10 premium hooks</li>
          <li> 15 SECONDS EACH </li>
          <li>Storytelling hooks</li>
          <li>Curiosity gap techniques</li>
        </ul>
        <div class="price-tag">₹99</div>
        <button class="buy-btn" onclick="openOrderModal('Premium Hook Series', '99')">BUY NOW</button>
      </div>
    </div>
  </section>

  <!-- Reels & Youtube Shorts Section -->
  <section id="reels-shorts" class="pricing-section test" data-aos="fade-up">
    <h2 class="section-title">REELS & SHORTS SCRIPTS</h2>
    <p class="section-subtitle">Viral-ready scripts for short-form content</p>
    
    <div class="pricing-grid">
      <div class="pricing-card" data-aos="zoom-in">
        <h3>Quick Shot</h3>
        <ul class="pricing-features">
          <li>45 second script</li>
          <li>Trending concept</li>
          <li>High shareability</li>
          <li>Simple execution</li>
          <li>Any Niech</li>
        </ul>
        <div class="price-tag">₹149</div>
        <button class="buy-btn" onclick="openOrderModal('Quick Shot', '149')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="100">
        <h3>Engage Mini</h3>
        <ul class="pricing-features">
          <li>60 second script</li>
          <li>Story arc included</li>
          <li>2 emotional triggers</li>
          <li>Call-to-action</li>
          <li> Mainly-For-Episodes </li>
        </ul>
        <div class="price-tag">₹199</div>
        <button class="buy-btn" onclick="openOrderModal('Engage Mini', '199')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="200">
        <h3>Dual Boost</h3>
        <ul class="pricing-features">
          <li>2 viral hooks</li>
          <li>90 second script</li>
          <li>Multi-platform ready</li>
          <li>Trend analysis</li>
          <li> 3 Part Reels </li>
        </ul>
        <div class="price-tag">₹249</div>
        <button class="buy-btn" onclick="openOrderModal('Dual Boost', '249')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="300">
        <h3>Power Series</h3>
        <ul class="pricing-features">
          <li>150 second script</li>
          <li>4 part structure</li>
          <li>Value-packed</li>
          <li>High retention flow</li>
        </ul>
        <div class="price-tag">₹299</div>
        <button class="buy-btn" onclick="openOrderModal('Power Series', '349')">BUY NOW</button>
      </div>

      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="400">
        <h3>Pro Reel Pack</h3>
        <ul class="pricing-features">
          <li>180 second script</li>
          <li>6 part structure</li>
          <li>Advanced storytelling</li>
          <li>Multi-hook approach</li>
        </ul>
        <div class="price-tag">₹349</div>
        <button class="buy-btn" onclick="openOrderModal('Pro Reel Pack', '449')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="400">
        <h3>Elite Reel Pack</h3>
        <ul class="pricing-features">
          <li> 10 Parts Structure </li>
          <li> 30 Seconds Each </li>
          <li>Advanced storytelling</li>
           <li>With #tags</li>
        </ul>
        <div class="price-tag">₹399</div>
        <button class="buy-btn" onclick="openOrderModal('Pro Reel Pack', '399')">BUY NOW</button>
      </div>
    </div>
  </section>

  <!-- Long Videos Section -->
  <section id="long-videos" class="pricing-section" data-aos="fade-up">
    <h2 class="section-title">YOUTUBE LONG VIDEO SCRIPTS</h2>
    <p class="section-subtitle">Complete scripts for engaging long-form content</p>
    
    <div class="pricing-grid">
      <div class="pricing-card" data-aos="zoom-in">
        <h3>Basic</h3>
        <ul class="pricing-features">
          <li>3 minute script</li>
          <li>Clear structure</li>
          <li>1 main point</li>
          <li>Simple CTA</li>
        </ul>
        <div class="price-tag">₹49</div>
        <button class="buy-btn" onclick="openOrderModal('Basic Long Video', '49')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="100">
        <h3>Starter Plus</h3>
        <ul class="pricing-features">
          <li>4 minute script</li>
          <li>2 main points</li>
          <li>Transition phrases</li>
          <li>Basic storytelling</li>
        </ul>
        <div class="price-tag">₹79</div>
        <button class="buy-btn" onclick="openOrderModal('Starter Plus Long Video', '79')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="200">
        <h3>Advanced</h3>
        <ul class="pricing-features">
          <li>6 minute script</li>
          <li>3 main points</li>
          <li>Engaging hooks</li>
          <li>Visual descriptions</li>
        </ul>
        <div class="price-tag">₹99</div>
        <button class="buy-btn" onclick="openOrderModal('Advanced Long Video', '99')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="300">
        <h3>Value Pack</h3>
        <ul class="pricing-features">
          <li>8.5 minute script</li>
          <li>Comprehensive content</li>
          <li>Multiple CTAs</li>
          <li>Audience engagement</li>
        </ul>
        <div class="price-tag">₹129</div>
        <button class="buy-btn" onclick="openOrderModal('Value Pack Long Video', '129')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="400">
        <h3>Pro</h3>
        <ul class="pricing-features">
          <li>10 minute script</li>
          <li>Professional structure</li>
          <li>Multiple hooks</li>
          <li>SEO optimized</li>
        </ul>
        <div class="price-tag">₹179</div>
        <button class="buy-btn" onclick="openOrderModal('Pro Long Video', '179')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="500">
        <h3>Pro Plus</h3>
        <ul class="pricing-features">
          <li>15 minute script</li>
          <li>In-depth content</li>
          <li>Storytelling elements</li>
          <li>Multiple segments</li>
        </ul>
        <div class="price-tag">₹249</div>
        <button class="buy-btn" onclick="openOrderModal('Pro Plus Long Video', '249')">BUY NOW</button>
      </div>
    </div>
  </section>

  <!-- Brand Ads Section -->
  <section id="brand-ads" class="pricing-section" data-aos="fade-up">
    <h2 class="section-title">BRANDED AD SCRIPTS</h2>
    <p class="section-subtitle">Professional scripts for brand promotions</p>
    
    <div class="pricing-grid">
      <div class="pricing-card" data-aos="zoom-in">
        <h3>45-sec Brand Ad</h3>
        <ul class="pricing-features">
          <li>Hook + Benefits + CTA</li>
          <li>Emotional appeal</li>
          <li>Brand messaging</li>
          <li>Clear conversion path</li>
        </ul>
        <div class="price-tag">₹299</div>
        <button class="buy-btn" onclick="openOrderModal('45-sec Brand Ad', '299')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="100">
        <h3>60-sec Story Ad</h3>
        <ul class="pricing-features">
          <li>Mini-story format</li>
          <li>Problem-solution</li>
          <li>Emotional journey</li>
          <li>Strong CTA</li>
        </ul>
        <div class="price-tag">₹399</div>
        <button class="buy-btn" onclick="openOrderModal('60-sec Story Ad', '399')">BUY NOW</button>
      </div>
      
      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="200">
        <h3>Product Demo</h3>
        <ul class="pricing-features">
          <li>Feature highlights</li>
          <li>Benefits showcase</li>
          <li>Comparison points</li>
          <li>Urgency creation</li>
        </ul>
        <div class="price-tag">₹499</div>
        <button class="buy-btn" onclick="openOrderModal('Product Demo Ad', '499')">BUY NOW</button>
      </div>
    </div>
  </section>

  <!-- Buy Website  -->
  <section id="buy-website" class="pricing-section" data-aos="fade-up">
    <h2 class="section-title">Buy Website</h2>
    <p class="section-subtitle">Buy a website like synex</p>

      <div class="pricing-card" data-aos="zoom-in" data-aos-delay="200">
        <h3>Buy Website</h3>
        <ul class="pricing-features">
          <li>Manual Control</li>
          <li>Whatsapp Based</li>
          <li>No Database</li>
          <li>Easy To Access</li>
          <li>Smooth UI</li>
          <li>Only HTML</li>
        </ul>
        <div class="price-tag">₹999</div>
        <button class="buy-btn" onclick="openOrderModal('Buy A Website at', '999')">BUY NOW</button>
      </div>
  </section>

  <section id="testimonials" class="testimonials" data-aos="fade-up" data-aos-duration="1500">
    <h2 class="section-title">Client Reviews</h2>
    <div class="testimonial-card">"Amazing script quality! My videos blew up. - GamerX"</div>
    <div class="testimonial-card">"Quick, creative and on point. - CreatorY"</div>
    <div class="testimonial-card">"Affordable premium work. Highly recommend. - VloggerZ"</div>
  </section>

  <footer>
    &copy; 2025 Synex | All Rights Reserved
  </footer>

  <script>
    // Initialize animations
    AOS.init();
    
    // Initialize particles.js
    particlesJS('particles-js', {
      particles: {
        number: { value: 80 },
        color: { value: "#00f2ff" },
        shape: { type: "circle" },
        opacity: { value: 0.3 },
        size: { value: 4 },
        line_linked: {
          enable: true,
          distance: 150,
          color: "#00f2ff",
          opacity: 0.4,
          width: 1
        },
        move: { enable: true, speed: 3 }
      }
    });
    
    // Initialize tilt effects
    VanillaTilt.init(document.querySelectorAll("[data-tilt]"), {
      max: 15,
      speed: 400,
      glare: true,
      "max-glare": 0.5
    });

    // Header scroll effect
    let lastScrollTop = 0;
    window.addEventListener('scroll', function() {
      const header = document.getElementById('main-header');
      let currentScrollTop = window.scrollY;
      if (currentScrollTop > lastScrollTop) {
        header.style.top = '-120px'; // hide when scrolling down
      } else {
        header.style.top = '10px'; // show when scrolling up
      }
      lastScrollTop = currentScrollTop <= 0 ? 0 : currentScrollTop;
    });

    // Theme toggle
    function toggleTheme() {
      document.body.classList.toggle('light');
      if (document.body.classList.contains('light')) {
        document.body.style.backgroundColor = '#fff';
        document.body.style.color = '#000';
      } else {
        document.body.style.backgroundColor = '#0f0f0f';
        document.body.style.color = '#fff';
      }
    }

    // Modal functionality
    const modal = document.getElementById("orderModal");
    const span = document.getElementsByClassName("close")[0];
    
    // Open modal function
    function openOrderModal(plan, price) {
      document.getElementById("selectedPlan").value = plan;
      document.getElementById("selectedPrice").value = price;
      modal.style.display = "block";
    }
    
    // Close modal when clicking X
    span.onclick = function() {
      modal.style.display = "none";
    }
    
    // Close modal when clicking outside
    window.onclick = function(event) {
      if (event.target == modal) {
        modal.style.display = "none";
      }
    }
    
    // Form submission
    document.getElementById("orderForm").addEventListener("submit", function(e) {
      e.preventDefault();
      
      const plan = document.getElementById("selectedPlan").value;
      const price = document.getElementById("selectedPrice").value;
      const name = document.getElementById("userName").value;
      const phone = document.getElementById("userPhone").value;
      const email = document.getElementById("userEmail").value;
      
      // Validate phone number
      if (!phone.match(/^[0-9]{10}$/)) {
        alert("Please enter a valid 10-digit phone number");
        return;
      }
      
      // Create WhatsApp message
      let message = `Hello Synex Team!%0A%0A`;
      message += `I want to order: *${plan} (₹${price})*%0A`;
      message += `Name: *${name}*%0A`;
      message += `Phone: *${phone}*%0A`;
      if (email) message += `Email: *${email}*%0A`;
      message += `%0APlease confirm my order and provide payment details.`;
      
      // Open WhatsApp
      window.open(`https://wa.me/918618365136?text=${message}`, '_blank');
      
      // Close modal
      modal.style.display = "none";
      
      // Reset form
      document.getElementById("orderForm").reset();
    });
  </script>
</body>
</html>
