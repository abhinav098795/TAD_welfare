# TAD_welfare
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>TAD - Tiwari Alokik Dal</title>

  <style>
    /* General Reset */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Segoe UI', sans-serif;
    }

    body {
      color: #fff;
      text-align: center;
      overflow-x: hidden;
      position: relative;
      min-height: 100vh;
    }

    /* 🔥 Animated Gradient Background */
    body::before {
      content: "";
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: linear-gradient(-45deg, #ff9a9e, #fad0c4, #fad0c4, #fbc2eb, #a18cd1, #fbc2eb);
      background-size: 400% 400%;
      animation: gradient 15s ease infinite;
      z-index: -1;
    }

    @keyframes gradient {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    /* Header */
    header {
      padding: 60px 20px 40px;
    }

    header h1 {
      font-size: 4rem;
      letter-spacing: 4px;
    }

    .fullform {
      font-size: 1.3rem;
      margin-top: 5px;
      opacity: 0.9;
    }

    .tagline {
      font-size: 1.2rem;
      margin: 10px 0 20px;
    }

    /* Buttons */
    .btn {
      display: inline-block;
      padding: 12px 25px;
      margin-top: 10px;
      border: none;
      border-radius: 30px;
      background: #ff6a88;
      color: #fff;
      font-size: 1rem;
      cursor: pointer;
      transition: transform 0.3s ease, background 0.3s ease;
      text-decoration: none;
    }

    .btn:hover {
      transform: scale(1.1);
      background: #ff3d68;
    }

    /* Activities Section */
    #activities {
      padding: 50px 20px;
    }

    #activities h2 {
      font-size: 2rem;
      margin-bottom: 30px;
    }

    .activities {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 20px;
    }

    .card {
      background: rgba(255, 255, 255, 0.1);
      padding: 25px;
      border-radius: 20px;
      width: 280px;
      backdrop-filter: blur(10px);
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }

    .card:hover {
      transform: translateY(-10px);
      box-shadow: 0px 8px 20px rgba(0, 0, 0, 0.3);
    }

    .card h3 {
      font-size: 1.4rem;
      margin-bottom: 10px;
    }

    /* Donation Popup */
    .donation-popup {
      display: none;
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: rgba(0,0,0,0.7);
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }

    .donation-content {
      background: #fff;
      color: #333;
      padding: 30px;
      border-radius: 20px;
      width: 320px;
      text-align: center;
      position: relative;
    }

    .donation-content h2 {
      margin-bottom: 20px;
    }

    .donation-content input {
      width: 100%;
      padding: 12px;
      margin: 10px 0;
      border: 2px solid #ddd;
      border-radius: 10px;
    }

    .close-btn {
      position: absolute;
      top: 15px;
      right: 20px;
      font-size: 25px;
      cursor: pointer;
      color: #333;
    }

    .donation-content .btn {
      background: #28a745;
    }

    .donation-content .btn:hover {
      background: #1e7e34;
    }

    /* Footer */
    footer {
      margin-top: 50px;
      padding: 20px;
      font-size: 0.9rem;
      background: rgba(0,0,0,0.4);
    }

    /* Confetti Canvas */
    #confetti {
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      pointer-events: none;
      z-index: 2000;
    }
  </style>
</head>
<body>

  <header>
    <h1>TAD</h1>
    <p class="fullform">Tiwari Alokik Dal</p>
    <p class="tagline">Together for a Better Society 🌍</p>
    <a href="#activities" class="btn">Join the Movement</a>
  </header>

  <section id="activities">
    <h2>✨ Upcoming Activities ✨</h2>
    <div class="activities">
      <div class="card">
        <h3>🌱 Tree Plantation</h3>
        <p>Join us in planting trees and making our city greener.</p>
        <button class="btn join-btn">Join Now</button>
      </div>
      <div class="card">
        <h3>🧹 Clean-Up Drive</h3>
        <p>Be part of our mission to clean and beautify public spaces.</p>
        <button class="btn join-btn">Join Now</button>
      </div>
      <div class="card">
        <h3>💰 Money Donation</h3>
        <p>Support our social projects by contributing funds for welfare activities.</p>
        <button class="btn donate-btn">Donate Now</button>
      </div>
    </div>
  </section>

  <!-- Donation Form (hidden by default) -->
  <div id="donationForm" class="donation-popup">
    <div class="donation-content">
      <span class="close-btn">&times;</span>
      <h2>💰 Donate to TAD</h2>
      <form id="donation">
        <input type="text" id="donorName" placeholder="Your Name" required>
        <input type="number" id="donationAmount" placeholder="Donation Amount (₹)" required>
        <button type="submit" class="btn">Donate</button>
      </form>
    </div>
  </div>

  <footer>
    <p>© 2025 TAD - Tiwari Alokik Dal | Stay Safe & United 🤝</p>
  </footer>

  <canvas id="confetti"></canvas>

  <script>
    // Elements
    const donateBtn = document.querySelector(".donate-btn");
    const donationPopup = document.getElementById("donationForm");
    const closeBtn = document.querySelector(".close-btn");
    const donationForm = document.getElementById("donation");
    const confettiCanvas = document.getElementById("confetti");
    const ctx = confettiCanvas.getContext("2d");

    // Resize canvas
    confettiCanvas.width = window.innerWidth;
    confettiCanvas.height = window.innerHeight;

    // Open popup
    donateBtn.addEventListener("click", () => {
      donationPopup.style.display = "flex";
    });

    // Close popup
    closeBtn.addEventListener("click", () => {
      donationPopup.style.display = "none";
    });

    // Close on outside click
    window.addEventListener("click", (e) => {
      if (e.target === donationPopup) {
        donationPopup.style.display = "none";
      }
    });

    // Confetti
    let confetti = [];
    function createConfetti() {
      for (let i = 0; i < 200; i++) {
        confetti.push({
          x: Math.random() * confettiCanvas.width,
          y: Math.random() * confettiCanvas.height - confettiCanvas.height,
          r: Math.random() * 6 + 2,
          d: Math.random() * 0.5 + 0.5,
          color: `hsl(${Math.random() * 360}, 100%, 50%)`
        });
      }
    }

    function drawConfetti() {
      ctx.clearRect(0, 0, confettiCanvas.width, confettiCanvas.height);
      confetti.forEach(c => {
        ctx.beginPath();
        ctx.arc(c.x, c.y, c.r, 0, 2 * Math.PI);
        ctx.fillStyle = c.color;
        ctx.fill();
      });
      updateConfetti();
    }

    function updateConfetti() {
      confetti.forEach(c => {
        c.y += c.d * 5;
        if (c.y > confettiCanvas.height) {
          c.y = -10;
          c.x = Math.random() * confettiCanvas.width;
        }
      });
    }

    function startConfetti() {
      createConfetti();
      let interval = setInterval(drawConfetti, 20);
      setTimeout(() => {
        clearInterval(interval);
        ctx.clearRect(0, 0, confettiCanvas.width, confettiCanvas.height);
        confetti = [];
      }, 5000);
    }

    // Handle Donation
    donationForm.addEventListener("submit", (e) => {
      e.preventDefault();
      donationPopup.style.display = "none";
      startConfetti();
      alert("🎉 Thank you for your kind donation!");
    });

    // Smooth Scroll for "Join the Movement"
    document.querySelector('a[href="#activities"]').addEventListener("click", (e) => {
      e.preventDefault();
      document.getElementById("activities").scrollIntoView({ behavior: "smooth" });
    });
  </script>
</body>
</html>
