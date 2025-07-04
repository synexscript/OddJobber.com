<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Synex - Script Writing Agency</title>
  <meta name="description" content="India's Best Script Writing Agency for YouTubers, Gamers, and Content Creators." />
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;900&display=swap" rel="stylesheet"/>
  <link href="https://unpkg.com/aos@2.3.1/dist/aos.css" rel="stylesheet" />
  <script src="https://cdn.jsdelivr.net/npm/particles.js"></script>
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
    section { padding: 100px 50px; text-align: center; }
    .cards {
      display: grid;
      grid-template-columns: repeat(auto-fit,minmax(280px,1fr));
      gap: 30px;
      margin-top: 50px;
      transition: opacity 0.4s ease;
    }
    $1
.card:hover {
  animation: glow-bounce 1s ease forwards;
}
    .card:hover { transform: scale(1.05); }
    .card h3 { color: #00f2ff; }
    .card p {
      color: #ccc;
      min-height: 100px;
      margin: 20px 0;
    }
    input[type="text"] {
      padding: 8px;
      border: none;
      border-radius: 10px;
      margin-bottom: 10px;
    }
    .card a, .card button {
      display: inline-block;
      margin-top: 10px;
      padding: 10px 20px;
      background: linear-gradient(90deg,#00f2ff,#ff00ff);
      color: white;
      text-decoration: none;
      border: none;
      border-radius: 30px;
      box-shadow: 0 0 10px #00f2ff;
      transition: 0.3s;
      cursor: pointer;
    }
    .card a:hover, .card button:hover {
      transform: scale(1.1);
      box-shadow: 0 0 20px #00f2ff;
    }
    .testimonials, .portfolio {
      background: #121212;
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
    }
  #main-header {
  transition: top 0.4s ease-in-out;
  top: 10px;
}
@keyframes glow-bounce {
  0% {
    box-shadow: 0 0 0px #00f2ff;
    transform: scale(1);
  }
  50% {
    box-shadow: 0 0 20px #00f2ff;
    transform: scale(1.05);
  }
  100% {
    box-shadow: 0 0 10px #00f2ff;
    transform: scale(1);
  }
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

  <header id="main-header">
    <div class="logo" id="synex-logo">SYNEX</div>
    <nav>
      <a href="#">Home</a>
      <a href="#pricing">Pricing</a>
      <a href="#testimonials">Reviews</a>
      <a href="#portfolio">Portfolio</a>
    </nav>
    <div class="toggle" onclick="toggleTheme()">Toggle</div>
  </header>

  <section class="hero" data-aos="fade-up" data-aos-duration="1500">
    <h1 data-tilt>Welcome to Synex</h1>
    <p data-tilt>India's Best Script Writing for Gamers & Creators</p>
  </section>

  <section id="pricing" data-aos="fade-up" data-aos-duration="1500">
    <h2>Pricing & Buy</h2>
    <div class="cards">
  <div class="card" data-aos="zoom-in-up" data-aos-duration="1000" data-aos-delay="100" data-tilt><h3>₹49 - Caption Pro</h3><p>Hooks & captions for reels & shorts.</p><input id="name49" type="text" placeholder="Enter Your Name"><br><button onclick="redirectToWhatsApp('Caption Pro', '49', 'name49')">Pay Now</button></div>
  <div class="card" data-aos="fade-up" data-tilt><h3>₹99 - Reel Script</h3><p>Short-form skits & trending reels.</p><input id="name99" type="text" placeholder="Enter Your Name"><br><button onclick="redirectToWhatsApp('Reel Script', '99', 'name99')">Pay Now</button></div>
  <div class="card" data-aos="fade-up" data-tilt><h3>₹149 - YT Shorts</h3><p>Punchy YouTube Shorts scripting.</p><input id="name149" type="text" placeholder="Enter Your Name"><br><button onclick="redirectToWhatsApp('YT Shorts', '149', 'name149')">Pay Now</button></div>
  <div class="card" data-aos="fade-up" data-tilt><h3>₹199 - Gaming Roast</h3><p>Funny roast script with memes.</p><input id="name199" type="text" placeholder="Enter Your Name"><br><button onclick="redirectToWhatsApp('Gaming Roast', '199', 'name199')">Pay Now</button></div>
  <div class="card" data-aos="fade-up" data-tilt><h3>₹249 - Reaction Script</h3><p>Engaging scripts for reaction content.</p><input id="name249" type="text" placeholder="Enter Your Name"><br><button onclick="redirectToWhatsApp('Reaction Script', '249', 'name249')">Pay Now</button></div>
  <div class="card" data-aos="fade-up" data-tilt><h3>₹299 - Full YT Video</h3><p>Complete script from intro to outro.</p><input id="name299" type="text" placeholder="Enter Your Name"><br><button onclick="redirectToWhatsApp('Full YT Video', '299', 'name299')">Pay Now</button></div>
  <div class="card" data-aos="fade-up" data-tilt><h3>₹349 - Cinematic Story</h3><p>Storytelling scripts for cinematic content.</p><input id="name349" type="text" placeholder="Enter Your Name"><br><button onclick="redirectToWhatsApp('Cinematic Story', '349', 'name349')">Pay Now</button></div>
  <div class="card" data-aos="fade-up" data-tilt><h3>₹399 - Gaming Storyline</h3><p>Immersive lore for games & characters.</p><input id="name399" type="text" placeholder="Enter Your Name"><br><button onclick="redirectToWhatsApp('Gaming Storyline', '399', 'name399')">Pay Now</button></div>
  <div class="card" data-aos="fade-up" data-tilt><h3>₹449 - Brand Script</h3><p>Ad & promo script for brand videos.</p><input id="name449" type="text" placeholder="Enter Your Name"><br><button onclick="redirectToWhatsApp('Brand Script', '449', 'name449')">Pay Now</button></div>
  <div class="card" data-aos="fade-up" data-tilt><h3>₹499 - Ultimate Series</h3><p>Long-form series script with episodes.</p><input id="name499" type="text" placeholder="Enter Your Name"><br><button onclick="redirectToWhatsApp('Ultimate Series', '499', 'name499')">Pay Now</button></div>
</div>
</section>

  <section id="testimonials" class="testimonials" data-aos="fade-up" data-aos-duration="1500">
    <h2>Client Reviews</h2>
    <div class="testimonial-card">"Amazing script quality! My videos blew up. - GamerX"</div>
    <div class="testimonial-card">"Quick, creative and on point. - CreatorY"</div>
    <div class="testimonial-card">"Affordable premium work. Highly recommend. - VloggerZ"</div>
  </section>

  <section id="portfolio" class="portfolio" data-aos="fade-up" data-aos-duration="1500">
    <h2>Our Portfolio</h2>
    <div class="portfolio-item">✔ Gaming Roast for GamerX</div>
    <div class="portfolio-item">✔ Brand Ad for ABC Brand</div>
    <div class="portfolio-item">✔ YouTube Series for CreatorZ</div>
  </section>

  <footer>
    &copy; 2025 Synex | All Rights Reserved
  </footer>

  <script>
    AOS.init();
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
    VanillaTilt.init(document.querySelectorAll("[data-tilt]"), {
      max: 15,
      speed: 400,
      glare: true,
      "max-glare": 0.5
    });

    let lastScrollTop = 0;
window.addEventListener('scroll', function () {
  const header = document.getElementById('main-header');
  let currentScrollTop = window.scrollY;
  if (currentScrollTop > lastScrollTop) {
    header.style.top = '-120px'; // hide when scrolling down
  } else {
    header.style.top = '10px'; // show when scrolling up
  }
  lastScrollTop = currentScrollTop <= 0 ? 0 : currentScrollTop;
      const logo = document.getElementById('synex-logo');
      const cardsSection = document.querySelector('.cards');
      const rect = cardsSection.getBoundingClientRect();
      const middle = window.innerHeight / 2;

      if (rect.top < middle && rect.bottom > middle) {
        cardsSection.style.opacity = '1';
        logo.style.opacity = '1';
      } else {
        cardsSection.style.opacity = '0.4';
        logo.style.opacity = '0.4';
      }
    });

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

    function redirectToWhatsApp(plan, price, inputId) {
  let message = '';
  const inputElement = document.getElementById(inputId);
  if (inputElement) {
    const name = inputElement.value.trim();
    if (name === "") {
      alert("Please enter your name.");
      return;
    }
    message = `Hello, my name is ${name}. I would like to purchase the ${plan} Plan for ₹${price}. Please proceed.`;
  } else {
    message = `Hi, I'm interested in the ${plan} Plan for ₹${price}. Please proceed.`;
  }
  const url = "https://wa.me/918618365136?text=" + encodeURIComponent(message);
  window.open(url, '_blank');
    }
  </script>
</body>
</html>