<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>FrameCraft — Video Editing & Production</title>
  <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,300&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #0a0a0a;
      --surface: #111111;
      --surface2: #191919;
      --accent: #e8ff47;
      --accent2: #ff4747;
      --text: #f0ede6;
      --muted: #6b6b6b;
      --border: #222222;
      --font-display: 'Bebas Neue', sans-serif;
      --font-body: 'DM Sans', sans-serif;
      --font-mono: 'Space Mono', monospace;
    }

    html { scroll-behavior: smooth; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: var(--font-body);
      overflow-x: hidden;
      cursor: none;
    }

    /* CUSTOM CURSOR */
    .cursor {
      width: 12px; height: 12px;
      background: var(--accent);
      border-radius: 50%;
      position: fixed;
      top: 0; left: 0;
      pointer-events: none;
      z-index: 9999;
      transform: translate(-50%, -50%);
      transition: transform 0.1s ease, width 0.2s ease, height 0.2s ease, background 0.2s ease;
      mix-blend-mode: difference;
    }
    .cursor-ring {
      width: 36px; height: 36px;
      border: 1px solid var(--accent);
      border-radius: 50%;
      position: fixed;
      top: 0; left: 0;
      pointer-events: none;
      z-index: 9998;
      transform: translate(-50%, -50%);
      transition: all 0.15s ease;
      mix-blend-mode: difference;
    }

    /* NAV */
    nav {
      position: fixed; top: 0; left: 0; right: 0;
      z-index: 100;
      display: flex; align-items: center; justify-content: space-between;
      padding: 1.5rem 3rem;
      backdrop-filter: blur(12px);
      border-bottom: 1px solid transparent;
      transition: border-color 0.3s ease, background 0.3s ease;
    }
    nav.scrolled {
      background: rgba(10,10,10,0.92);
      border-color: var(--border);
    }
    .nav-logo {
      font-family: var(--font-display);
      font-size: 1.6rem;
      letter-spacing: 0.05em;
      color: var(--text);
      text-decoration: none;
    }
    .nav-logo span { color: var(--accent); }
    .nav-links { display: flex; gap: 2rem; list-style: none; }
    .nav-links a {
      font-family: var(--font-mono);
      font-size: 0.7rem;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      color: var(--muted);
      text-decoration: none;
      transition: color 0.2s;
    }
    .nav-links a:hover { color: var(--accent); }

    /* HERO */
    #hero {
      min-height: 100vh;
      display: flex; flex-direction: column; justify-content: center;
      padding: 0 3rem;
      position: relative;
      overflow: hidden;
    }
    .hero-noise {
      position: absolute; inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none; z-index: 0;
    }
    .hero-grid {
      position: absolute; inset: 0;
      background-image: linear-gradient(var(--border) 1px, transparent 1px), linear-gradient(90deg, var(--border) 1px, transparent 1px);
      background-size: 60px 60px;
      opacity: 0.35; z-index: 0;
      mask-image: radial-gradient(ellipse 80% 80% at 50% 50%, black 40%, transparent 100%);
    }
    .hero-glow {
      position: absolute; width: 600px; height: 600px;
      border-radius: 50%;
      background: radial-gradient(circle, rgba(232,255,71,0.08) 0%, transparent 70%);
      top: 50%; left: 50%; transform: translate(-50%, -50%);
      pointer-events: none; z-index: 0;
      animation: pulse 4s ease-in-out infinite;
    }
    @keyframes pulse {
      0%, 100% { transform: translate(-50%, -50%) scale(1); opacity: 0.6; }
      50% { transform: translate(-50%, -50%) scale(1.15); opacity: 1; }
    }
    .hero-content { position: relative; z-index: 1; max-width: 1000px; }
    .hero-tag {
      font-family: var(--font-mono);
      font-size: 0.72rem;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 1.5rem;
      opacity: 0; animation: fadeUp 0.8s 0.2s ease forwards;
    }
    .hero-title {
      font-family: var(--font-display);
      font-size: clamp(5rem, 13vw, 13rem);
      line-height: 0.9;
      letter-spacing: -0.01em;
      opacity: 0; animation: fadeUp 0.8s 0.4s ease forwards;
    }
    .hero-title .line2 { color: var(--accent); display: block; }
    .hero-title .line3 { color: var(--muted); display: block; font-size: 0.45em; letter-spacing: 0.04em; margin-top: 0.4em; }
    .hero-sub {
      max-width: 480px;
      margin-top: 2rem;
      font-size: 1rem;
      font-weight: 300;
      line-height: 1.7;
      color: rgba(240,237,230,0.6);
      opacity: 0; animation: fadeUp 0.8s 0.6s ease forwards;
    }
    .hero-actions {
      display: flex; gap: 1rem; margin-top: 2.5rem; align-items: center;
      opacity: 0; animation: fadeUp 0.8s 0.8s ease forwards;
    }
    .btn-primary {
      background: var(--accent);
      color: #0a0a0a;
      padding: 1rem 2.2rem;
      font-family: var(--font-mono);
      font-size: 0.78rem;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      font-weight: 700;
      text-decoration: none;
      border: none;
      cursor: none;
      transition: all 0.2s ease;
      clip-path: polygon(0 0, calc(100% - 12px) 0, 100% 12px, 100% 100%, 12px 100%, 0 calc(100% - 12px));
    }
    .btn-primary:hover { background: #fff; transform: translateY(-2px); box-shadow: 0 12px 40px rgba(232,255,71,0.25); }
    .btn-secondary {
      font-family: var(--font-mono);
      font-size: 0.72rem;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--muted);
      text-decoration: none;
      transition: color 0.2s;
      display: flex; align-items: center; gap: 0.5rem;
    }
    .btn-secondary:hover { color: var(--text); }
    .btn-secondary::before { content: '↓'; font-size: 1rem; }
    .hero-scroll-hint {
      position: absolute; bottom: 2.5rem; right: 3rem;
      font-family: var(--font-mono); font-size: 0.65rem;
      letter-spacing: 0.15em; text-transform: uppercase;
      color: var(--muted);
      writing-mode: vertical-rl;
      display: flex; align-items: center; gap: 0.8rem;
      opacity: 0; animation: fadeIn 1s 1.2s ease forwards;
    }
    .hero-scroll-hint::after {
      content: ''; display: block;
      width: 1px; height: 60px;
      background: linear-gradient(to bottom, var(--muted), transparent);
    }
    .hero-counter {
      position: absolute; bottom: 2.5rem; left: 3rem;
      font-family: var(--font-display);
      font-size: 5rem; color: var(--border);
      pointer-events: none;
      opacity: 0; animation: fadeIn 1s 1s ease forwards;
    }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(30px); }
      to { opacity: 1; transform: translateY(0); }
    }
    @keyframes fadeIn {
      from { opacity: 0; } to { opacity: 1; }
    }

    /* SECTION SHARED */
    section { padding: 7rem 3rem; }
    .section-label {
      font-family: var(--font-mono);
      font-size: 0.68rem;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: var(--accent);
      display: flex; align-items: center; gap: 1rem;
      margin-bottom: 1.5rem;
    }
    .section-label::before {
      content: ''; display: block; width: 30px; height: 1px; background: var(--accent);
    }
    .section-title {
      font-family: var(--font-display);
      font-size: clamp(3rem, 6vw, 6rem);
      line-height: 1;
      letter-spacing: 0.02em;
    }

    /* ABOUT */
    #about { background: var(--surface); position: relative; overflow: hidden; }
    .about-accent-line {
      position: absolute; top: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, transparent, var(--accent), transparent);
    }
    .about-inner {
      display: grid; grid-template-columns: 1fr 1fr;
      gap: 5rem; align-items: center; max-width: 1200px; margin: 0 auto;
    }
    .about-left .section-title { margin-bottom: 1.5rem; }
    .about-text {
      font-size: 1.05rem; line-height: 1.8; font-weight: 300;
      color: rgba(240,237,230,0.75);
    }
    .about-text strong { color: var(--accent); font-weight: 500; }
    .about-text p + p { margin-top: 1rem; }
    .about-stats {
      display: grid; grid-template-columns: 1fr 1fr;
      gap: 1px; background: var(--border); margin-top: 2.5rem;
    }
    .stat {
      background: var(--surface2);
      padding: 1.5rem;
      transition: background 0.2s;
    }
    .stat:hover { background: #1f1f1f; }
    .stat-num {
      font-family: var(--font-display);
      font-size: 3rem; color: var(--accent); line-height: 1;
    }
    .stat-desc {
      font-family: var(--font-mono);
      font-size: 0.68rem; letter-spacing: 0.1em;
      text-transform: uppercase; color: var(--muted); margin-top: 0.4rem;
    }
    .about-right {
      display: flex; flex-direction: column; gap: 1px;
      background: var(--border);
    }
    .about-card {
      background: var(--surface2);
      padding: 1.8rem;
      transition: background 0.2s, transform 0.2s;
      border-left: 2px solid transparent;
    }
    .about-card:hover { background: #1a1a1a; border-left-color: var(--accent); transform: translateX(4px); }
    .about-card-icon { font-size: 1.6rem; margin-bottom: 0.8rem; }
    .about-card h3 {
      font-family: var(--font-display); font-size: 1.4rem; letter-spacing: 0.05em; margin-bottom: 0.4rem;
    }
    .about-card p { font-size: 0.88rem; color: var(--muted); line-height: 1.6; }

    /* SERVICES */
    #services { background: var(--bg); }
    .services-inner { max-width: 1200px; margin: 0 auto; }
    .services-header { display: flex; align-items: flex-end; justify-content: space-between; margin-bottom: 3rem; }
    .services-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 1px; background: var(--border);
    }
    .service-card {
      background: var(--bg);
      padding: 2.5rem;
      position: relative;
      overflow: hidden;
      transition: background 0.3s;
      cursor: none;
    }
    .service-card::before {
      content: ''; position: absolute; inset: 0;
      background: linear-gradient(135deg, var(--accent) 0%, transparent 60%);
      opacity: 0; transition: opacity 0.4s;
    }
    .service-card:hover { background: var(--surface); }
    .service-card:hover::before { opacity: 0.04; }
    .service-num {
      font-family: var(--font-display); font-size: 5rem;
      color: var(--border); line-height: 1;
      position: absolute; top: 1.5rem; right: 1.5rem;
      transition: color 0.3s;
    }
    .service-card:hover .service-num { color: rgba(232,255,71,0.12); }
    .service-icon { font-size: 2rem; margin-bottom: 1.5rem; position: relative; }
    .service-name {
      font-family: var(--font-display);
      font-size: 1.8rem; letter-spacing: 0.05em; margin-bottom: 0.8rem;
      position: relative;
    }
    .service-desc {
      font-size: 0.88rem; line-height: 1.65;
      color: var(--muted); position: relative;
    }
    .service-tags {
      display: flex; flex-wrap: wrap; gap: 0.4rem; margin-top: 1.2rem;
      position: relative;
    }
    .tag {
      font-family: var(--font-mono);
      font-size: 0.6rem; letter-spacing: 0.1em;
      text-transform: uppercase; padding: 0.25rem 0.6rem;
      border: 1px solid var(--border); color: var(--muted);
      transition: all 0.2s;
    }
    .service-card:hover .tag { border-color: var(--accent); color: var(--accent); }

    /* PORTFOLIO */
    #portfolio { background: var(--surface); }
    .portfolio-inner { max-width: 1200px; margin: 0 auto; }
    .portfolio-header { margin-bottom: 3rem; }
    .portfolio-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 1px; background: var(--border);
    }
    .video-card {
      position: relative; aspect-ratio: 16/9;
      background: var(--surface2);
      overflow: hidden; cursor: none;
    }
    .video-card:first-child { grid-column: span 2; }
    .video-card iframe {
      width: 100%; height: 100%;
      border: none; display: block;
    }
    .video-placeholder {
      width: 100%; height: 100%;
      display: flex; flex-direction: column; align-items: center; justify-content: center;
      gap: 0.8rem;
      background: linear-gradient(135deg, var(--surface2) 0%, var(--bg) 100%);
      transition: background 0.3s;
      cursor: none;
    }
    .video-placeholder:hover { background: linear-gradient(135deg, #1f1f1f 0%, #111 100%); }
    .play-btn {
      width: 56px; height: 56px;
      border: 2px solid var(--accent);
      border-radius: 50%;
      display: flex; align-items: center; justify-content: center;
      font-size: 1.2rem;
      transition: all 0.2s;
    }
    .video-placeholder:hover .play-btn {
      background: var(--accent); color: #0a0a0a; transform: scale(1.1);
    }
    .video-label {
      font-family: var(--font-mono);
      font-size: 0.65rem; letter-spacing: 0.15em; text-transform: uppercase;
      color: var(--muted);
    }
    .portfolio-note {
      text-align: center; margin-top: 2rem;
      font-family: var(--font-mono); font-size: 0.7rem;
      letter-spacing: 0.1em; color: var(--muted);
    }
    .portfolio-note a { color: var(--accent); text-decoration: none; }

    /* CONTACT */
    #contact { background: var(--bg); position: relative; overflow: hidden; }
    .contact-bg-text {
      position: absolute; bottom: -3rem; left: -1rem;
      font-family: var(--font-display); font-size: 22vw;
      color: var(--surface); pointer-events: none; line-height: 1;
      white-space: nowrap; z-index: 0;
    }
    .contact-inner {
      max-width: 1200px; margin: 0 auto;
      display: grid; grid-template-columns: 1fr 1fr;
      gap: 5rem; position: relative; z-index: 1;
    }
    .contact-left .section-title { margin-bottom: 1rem; }
    .contact-tagline {
      font-size: 1rem; line-height: 1.7; color: rgba(240,237,230,0.6);
      font-weight: 300; margin-bottom: 2.5rem;
    }
    .contact-links { display: flex; flex-direction: column; gap: 0; }
    .contact-link {
      display: flex; align-items: center; gap: 1rem;
      padding: 1.2rem 0;
      border-bottom: 1px solid var(--border);
      text-decoration: none; color: var(--text);
      transition: all 0.2s;
      font-family: var(--font-mono); font-size: 0.8rem;
    }
    .contact-link:hover { color: var(--accent); padding-left: 0.5rem; }
    .contact-link-icon { font-size: 1.1rem; width: 24px; text-align: center; }
    .contact-link-label { flex: 1; }
    .contact-link-arrow { color: var(--muted); transition: transform 0.2s; }
    .contact-link:hover .contact-link-arrow { transform: translateX(4px); color: var(--accent); }
    .contact-form { display: flex; flex-direction: column; gap: 0; }
    .form-row { position: relative; }
    .form-row + .form-row { margin-top: 1px; }
    .form-row input, .form-row textarea, .form-row select {
      width: 100%; background: var(--surface);
      border: none; border-bottom: 1px solid var(--border);
      padding: 1.3rem 1rem;
      color: var(--text); font-family: var(--font-body); font-size: 0.95rem;
      outline: none; transition: border-color 0.2s, background 0.2s;
      resize: none; cursor: none;
    }
    .form-row input::placeholder, .form-row textarea::placeholder {
      color: var(--muted); font-size: 0.85rem;
    }
    .form-row input:focus, .form-row textarea:focus {
      background: var(--surface2); border-color: var(--accent);
    }
    .form-row textarea { min-height: 120px; }
    .form-submit {
      margin-top: 1.5rem;
      background: var(--accent);
      color: #0a0a0a;
      padding: 1.1rem 2rem;
      font-family: var(--font-mono);
      font-size: 0.78rem; font-weight: 700;
      letter-spacing: 0.12em; text-transform: uppercase;
      border: none; cursor: none;
      transition: all 0.2s;
      clip-path: polygon(0 0, calc(100% - 10px) 0, 100% 10px, 100% 100%, 10px 100%, 0 calc(100% - 10px));
    }
    .form-submit:hover { background: #fff; transform: translateY(-2px); box-shadow: 0 12px 40px rgba(232,255,71,0.2); }

    /* FOOTER */
    footer {
      background: var(--surface);
      border-top: 1px solid var(--border);
      padding: 2rem 3rem;
      display: flex; align-items: center; justify-content: space-between;
    }
    .footer-logo {
      font-family: var(--font-display); font-size: 1.3rem;
      color: var(--muted); letter-spacing: 0.05em;
      text-decoration: none;
    }
    .footer-logo span { color: var(--accent); }
    .footer-copy {
      font-family: var(--font-mono); font-size: 0.65rem;
      letter-spacing: 0.1em; color: var(--muted);
    }
    .footer-top {
      font-family: var(--font-mono); font-size: 0.65rem;
      letter-spacing: 0.1em; color: var(--muted);
      text-decoration: none; transition: color 0.2s;
    }
    .footer-top:hover { color: var(--accent); }

    /* REVEAL ANIMATIONS */
    .reveal {
      opacity: 0; transform: translateY(24px);
      transition: opacity 0.7s ease, transform 0.7s ease;
    }
    .reveal.visible { opacity: 1; transform: translateY(0); }

    /* RESPONSIVE */
    @media (max-width: 900px) {
      nav { padding: 1.2rem 1.5rem; }
      .nav-links { display: none; }
      section { padding: 5rem 1.5rem; }
      #hero { padding: 0 1.5rem; }
      .about-inner { grid-template-columns: 1fr; gap: 2.5rem; }
      .services-grid { grid-template-columns: 1fr; }
      .portfolio-grid { grid-template-columns: 1fr 1fr; }
      .video-card:first-child { grid-column: span 2; }
      .contact-inner { grid-template-columns: 1fr; gap: 3rem; }
      footer { flex-direction: column; gap: 1rem; text-align: center; }
      .services-header { flex-direction: column; align-items: flex-start; gap: 1rem; }
      body { cursor: auto; }
      .cursor, .cursor-ring { display: none; }
    }
    @media (max-width: 600px) {
      .portfolio-grid { grid-template-columns: 1fr; }
      .video-card:first-child { grid-column: span 1; }
      .about-stats { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>

  <div class="cursor" id="cursor"></div>
  <div class="cursor-ring" id="cursorRing"></div>

  <!-- NAV -->
  <nav id="nav">
    <a href="#hero" class="nav-logo">Frame<span>Craft</span></a>
    <ul class="nav-links">
      <li><a href="#about">About</a></li>
      <li><a href="#services">Services</a></li>
      <li><a href="#portfolio">Portfolio</a></li>
      <li><a href="#contact">Contact</a></li>
    </ul>
  </nav>

  <!-- HERO -->
  <section id="hero">
    <div class="hero-noise"></div>
    <div class="hero-grid"></div>
    <div class="hero-glow"></div>
    <div class="hero-content">
      <div class="hero-tag">Video Editing & Production · Available Now</div>
      <h1 class="hero-title">
        HIGH<br>
        <span class="line2">QUALITY</span>
        <span class="line3">Video Editing. Affordable Price. Real Results.</span>
      </h1>
      <p class="hero-sub">
        Cinematic cuts, scroll-stopping Reels, and brand-defining content — delivered fast, without the agency price tag.
      </p>
      <div class="hero-actions">
        <a href="#contact" class="btn-primary">Work With Me</a>
        <a href="#portfolio" class="btn-secondary">See the Work</a>
      </div>
    </div>
    <div class="hero-scroll-hint">Scroll</div>
    <div class="hero-counter">01</div>
  </section>

  <!-- ABOUT -->
  <section id="about">
    <div class="about-accent-line"></div>
    <div class="about-inner">
      <div class="about-left reveal">
        <div class="section-label">About Me</div>
        <h2 class="section-title">PASSION MEETS PRECISION</h2>
        <div class="about-text">
          <p>I'm a <strong>dedicated student editor</strong> obsessed with the craft of visual storytelling. While others see raw footage, I see potential — moments waiting to hit, beats waiting to land, stories waiting to connect.</p>
          <p>I work directly with <strong>creators and brands</strong> who want content that actually performs — not just looks nice. No bloated agency fees, no weeks of waiting. Just clean communication and work that delivers.</p>
        </div>
        <div class="about-stats">
          <div class="stat">
            <div class="stat-num">50+</div>
            <div class="stat-desc">Videos Edited</div>
          </div>
          <div class="stat">
            <div class="stat-num">24h</div>
            <div class="stat-desc">Avg. Turnaround</div>
          </div>
          <div class="stat">
            <div class="stat-num">100%</div>
            <div class="stat-desc">Client Satisfaction</div>
          </div>
          <div class="stat">
            <div class="stat-num">∞</div>
            <div class="stat-desc">Revisions Available</div>
          </div>
        </div>
      </div>
      <div class="about-right reveal">
        <div class="about-card">
          <div class="about-card-icon">🎬</div>
          <h3>STORY-FIRST EDITING</h3>
          <p>Every cut serves the narrative. I edit with emotion and intention, not just technique.</p>
        </div>
        <div class="about-card">
          <div class="about-card-icon">⚡</div>
          <h3>FAST TURNAROUND</h3>
          <p>Rush deadlines? No problem. I work fast without sacrificing quality.</p>
        </div>
        <div class="about-card">
          <div class="about-card-icon">💬</div>
          <h3>CLEAR COMMUNICATION</h3>
          <p>You'll always know where your project stands. No ghosting, no guessing.</p>
        </div>
        <div class="about-card">
          <div class="about-card-icon">💰</div>
          <h3>STUDENT RATES</h3>
          <p>Premium editing quality at a price that actually makes sense for your budget.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- SERVICES -->
  <section id="services">
    <div class="services-inner">
      <div class="services-header reveal">
        <div>
          <div class="section-label">What I Do</div>
          <h2 class="section-title">SERVICES</h2>
        </div>
      </div>
      <div class="services-grid">
        <div class="service-card reveal">
          <div class="service-num">01</div>
          <div class="service-icon">📱</div>
          <div class="service-name">SHORT-FORM CONTENT</div>
          <div class="service-desc">Reels, TikToks, and Shorts engineered to stop the scroll. Punchy pacing, trending formats, and hooks that convert viewers into followers.</div>
          <div class="service-tags">
            <span class="tag">Instagram Reels</span>
            <span class="tag">TikTok</span>
            <span class="tag">YouTube Shorts</span>
            <span class="tag">Subtitles</span>
          </div>
        </div>
        <div class="service-card reveal">
          <div class="service-num">02</div>
          <div class="service-icon">🎥</div>
          <div class="service-name">YOUTUBE EDITING</div>
          <div class="service-desc">Long-form videos that keep viewers watching. Color grading, sound design, motion graphics, and retention-focused cuts built for growth.</div>
          <div class="service-tags">
            <span class="tag">Vlogs</span>
            <span class="tag">Tutorials</span>
            <span class="tag">Podcasts</span>
            <span class="tag">Commentary</span>
          </div>
        </div>
        <div class="service-card reveal">
          <div class="service-num">03</div>
          <div class="service-icon">🏢</div>
          <div class="service-name">BRAND VIDEOS</div>
          <div class="service-desc">Promotional content that makes brands look professional and polished. Product showcases, ads, and brand stories that convert.</div>
          <div class="service-tags">
            <span class="tag">Product Promos</span>
            <span class="tag">Brand Films</span>
            <span class="tag">Ad Creatives</span>
          </div>
        </div>
        <div class="service-card reveal">
          <div class="service-num">04</div>
          <div class="service-icon">✨</div>
          <div class="service-name">MOTION GRAPHICS</div>
          <div class="service-desc">Animated text, lower thirds, transitions, and visual effects that elevate your content from good to unforgettable.</div>
          <div class="service-tags">
            <span class="tag">Titles</span>
            <span class="tag">Lower Thirds</span>
            <span class="tag">Transitions</span>
          </div>
        </div>
        <div class="service-card reveal">
          <div class="service-num">05</div>
          <div class="service-icon">🎨</div>
          <div class="service-name">COLOR GRADING</div>
          <div class="service-desc">Transform flat footage into a cinematic visual language with professional color correction and custom LUT development for your brand.</div>
          <div class="service-tags">
            <span class="tag">LUTs</span>
            <span class="tag">Correction</span>
            <span class="tag">Cinematic</span>
          </div>
        </div>
        <div class="service-card reveal">
          <div class="service-num">06</div>
          <div class="service-icon">🎙️</div>
          <div class="service-name">AUDIO CLEANUP</div>
          <div class="service-desc">Noise reduction, mixing, music syncing, and SFX. Clean, balanced audio that makes your video sound as good as it looks.</div>
          <div class="service-tags">
            <span class="tag">Noise Removal</span>
            <span class="tag">Music Sync</span>
            <span class="tag">SFX</span>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- PORTFOLIO -->
  <section id="portfolio">
    <div class="portfolio-inner">
      <div class="portfolio-header reveal">
        <div class="section-label">My Work</div>
        <h2 class="section-title">PORTFOLIO</h2>
      </div>
      <div class="portfolio-grid">
        <!-- Featured slot — paste your YouTube embed URL -->
        <div class="video-card">
          <div class="video-placeholder">
            <div class="play-btn">▶</div>
            <div class="video-label">Featured · Short-Form Reel</div>
          </div>
        </div>
        <div class="video-card">
          <div class="video-placeholder">
            <div class="play-btn">▶</div>
            <div class="video-label">YouTube Edit</div>
          </div>
        </div>
        <div class="video-card">
          <div class="video-placeholder">
            <div class="play-btn">▶</div>
            <div class="video-label">Brand Promo</div>
          </div>
        </div>
        <div class="video-card">
          <div class="video-placeholder">
            <div class="play-btn">▶</div>
            <div class="video-label">TikTok / Reel</div>
          </div>
        </div>
        <div class="video-card">
          <div class="video-placeholder">
            <div class="play-btn">▶</div>
            <div class="video-label">Motion Graphics</div>
          </div>
        </div>
        <div class="video-card">
          <div class="video-placeholder">
            <div class="play-btn">▶</div>
            <div class="video-label">Color Grade Reel</div>
          </div>
        </div>
      </div>
      <div class="portfolio-note reveal">
        Replace placeholders with iframe embeds from YouTube or Vimeo. &nbsp;·&nbsp;
        <a href="#contact">Request a private reel →</a>
      </div>
    </div>
  </section>

  <!-- CONTACT -->
  <section id="contact">
    <div class="contact-bg-text">LET'S TALK</div>
    <div class="contact-inner">
      <div class="contact-left reveal">
        <div class="section-label">Get In Touch</div>
        <h2 class="section-title">LET'S<br>CREATE.</h2>
        <p class="contact-tagline">Ready to level up your content? Drop me a message and I'll get back to you within 24 hours.</p>
        <div class="contact-links">
          <a href="mailto:hello@youremail.com" class="contact-link">
            <span class="contact-link-icon">✉</span>
            <span class="contact-link-label">hello@youremail.com</span>
            <span class="contact-link-arrow">→</span>
          </a>
          <a href="https://instagram.com/yourhandle" target="_blank" class="contact-link">
            <span class="contact-link-icon">📸</span>
            <span class="contact-link-label">@yourhandle · Instagram</span>
            <span class="contact-link-arrow">→</span>
          </a>
          <a href="https://tiktok.com/@yourhandle" target="_blank" class="contact-link">
            <span class="contact-link-icon">🎵</span>
            <span class="contact-link-label">@yourhandle · TikTok</span>
            <span class="contact-link-arrow">→</span>
          </a>
          <a href="https://twitter.com/yourhandle" target="_blank" class="contact-link">
            <span class="contact-link-icon">𝕏</span>
            <span class="contact-link-label">@yourhandle · X / Twitter</span>
            <span class="contact-link-arrow">→</span>
          </a>
        </div>
      </div>
      <div class="contact-right reveal">
        <form class="contact-form" onsubmit="handleSubmit(event)">
          <div class="form-row">
            <input type="text" placeholder="Your Name *" required />
          </div>
          <div class="form-row">
            <input type="email" placeholder="Email Address *" required />
          </div>
          <div class="form-row">
            <input type="text" placeholder="Project Type (e.g. Reels, YouTube, Brand Video)" />
          </div>
          <div class="form-row">
            <input type="text" placeholder="Budget Range (e.g. $50–$200)" />
          </div>
          <div class="form-row">
            <textarea placeholder="Tell me about your project — the more detail, the better!"></textarea>
          </div>
          <button type="submit" class="form-submit">Send Message →</button>
        </form>
      </div>
    </div>
  </section>

  <!-- FOOTER -->
  <footer>
    <a href="#hero" class="footer-logo">Frame<span>Craft</span></a>
    <span class="footer-copy">© 2026 — All rights reserved</span>
    <a href="#hero" class="footer-top">Back to top ↑</a>
  </footer>

  <script>
    // CURSOR
    const cursor = document.getElementById('cursor');
    const ring = document.getElementById('cursorRing');
    document.addEventListener('mousemove', e => {
      cursor.style.left = e.clientX + 'px';
      cursor.style.top = e.clientY + 'px';
      setTimeout(() => {
        ring.style.left = e.clientX + 'px';
        ring.style.top = e.clientY + 'px';
      }, 60);
    });
    document.querySelectorAll('a, button, .service-card, .about-card, .video-card, .stat').forEach(el => {
      el.addEventListener('mouseenter', () => {
        cursor.style.width = '20px'; cursor.style.height = '20px';
        ring.style.width = '50px'; ring.style.height = '50px';
      });
      el.addEventListener('mouseleave', () => {
        cursor.style.width = '12px'; cursor.style.height = '12px';
        ring.style.width = '36px'; ring.style.height = '36px';
      });
    });

    // NAV SCROLL
    const nav = document.getElementById('nav');
    window.addEventListener('scroll', () => {
      nav.classList.toggle('scrolled', window.scrollY > 40);
    });

    // REVEAL ON SCROLL
    const revealEls = document.querySelectorAll('.reveal');
    const io = new IntersectionObserver((entries) => {
      entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('visible'); io.unobserve(e.target); } });
    }, { threshold: 0.12 });
    revealEls.forEach(el => io.observe(el));

    // FORM SUBMIT
    function handleSubmit(e) {
      e.preventDefault();
      const btn = e.target.querySelector('.form-submit');
      btn.textContent = '✓ Message Sent!';
      btn.style.background = '#2dff8a';
      setTimeout(() => {
        btn.textContent = 'Send Message →';
        btn.style.background = '';
        e.target.reset();
      }, 3000);
    }
  </script>
</body>
</html>
