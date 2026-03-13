<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Computedocean | Michael Dallman</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Syne:wght@400;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --cyan: #00F7FF;
    --cyan-dim: rgba(0,247,255,0.12);
    --cyan-glow: rgba(0,247,255,0.35);
    --magenta: #FF2D78;
    --mag-dim: rgba(255,45,120,0.12);
    --amber: #FFB800;
    --bg: #060912;
    --bg2: #0c1120;
    --bg3: #111827;
    --text: #E8EDF5;
    --muted: #6b7a96;
    --border: rgba(0,247,255,0.15);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Space Mono', monospace;
    overflow-x: hidden;
    cursor: none;
  }

  /* Custom cursor */
  #cursor {
    width: 10px; height: 10px;
    background: var(--cyan);
    border-radius: 50%;
    position: fixed; top: 0; left: 0;
    pointer-events: none;
    z-index: 9999;
    transform: translate(-50%,-50%);
    transition: width .15s, height .15s, background .15s;
    mix-blend-mode: screen;
  }
  #cursor-ring {
    width: 36px; height: 36px;
    border: 1.5px solid var(--cyan);
    border-radius: 50%;
    position: fixed; top: 0; left: 0;
    pointer-events: none;
    z-index: 9998;
    transform: translate(-50%,-50%);
    transition: all .08s ease;
    opacity: 0.5;
  }
  body:has(a:hover) #cursor, body:has(button:hover) #cursor { width: 20px; height: 20px; background: var(--magenta); }

  /* Scanline overlay */
  body::before {
    content: '';
    position: fixed; inset: 0;
    background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.04) 2px, rgba(0,0,0,0.04) 4px);
    pointer-events: none;
    z-index: 1000;
  }

  /* Grid noise bg */
  body::after {
    content: '';
    position: fixed; inset: 0;
    background-image:
      linear-gradient(rgba(0,247,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,247,255,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  /* NAV */
  nav {
    position: fixed; top: 0; left: 0; right: 0;
    z-index: 100;
    display: flex; align-items: center; justify-content: space-between;
    padding: 1.25rem 4rem;
    background: rgba(6,9,18,0.85);
    backdrop-filter: blur(16px);
    border-bottom: 1px solid var(--border);
  }

  .nav-logo {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 1.1rem;
    color: var(--cyan);
    letter-spacing: 0.1em;
    text-decoration: none;
  }

  .nav-links { display: flex; gap: 2rem; list-style: none; }
  .nav-links a {
    color: var(--muted);
    text-decoration: none;
    font-size: 0.75rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    transition: color .2s;
  }
  .nav-links a:hover { color: var(--cyan); }

  .nav-status {
    display: flex; align-items: center; gap: 8px;
    font-size: 0.7rem;
    color: var(--muted);
    letter-spacing: 0.1em;
  }
  .status-dot {
    width: 7px; height: 7px;
    background: #00ff88;
    border-radius: 50%;
    animation: pulse-dot 2s ease-in-out infinite;
  }
  @keyframes pulse-dot {
    0%,100% { opacity: 1; box-shadow: 0 0 0 0 rgba(0,255,136,0.4); }
    50% { opacity: .8; box-shadow: 0 0 0 5px rgba(0,255,136,0); }
  }

  /* HERO */
  .hero {
    position: relative;
    min-height: 100vh;
    display: flex; align-items: center;
    padding: 8rem 4rem 4rem;
    overflow: hidden;
    z-index: 1;
  }

  .hero-bg-glow {
    position: absolute;
    width: 600px; height: 600px;
    background: radial-gradient(circle, rgba(0,247,255,0.08) 0%, transparent 70%);
    top: 50%; left: 50%;
    transform: translate(-50%,-50%);
    pointer-events: none;
  }

  .hero-content { position: relative; max-width: 900px; }

  .hero-eyebrow {
    font-size: 0.7rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--cyan);
    margin-bottom: 1.5rem;
    display: flex; align-items: center; gap: 12px;
  }
  .hero-eyebrow::before {
    content: '';
    width: 40px; height: 1px;
    background: var(--cyan);
  }

  .hero-title {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: clamp(3.5rem, 7vw, 6.5rem);
    line-height: 0.95;
    letter-spacing: -0.02em;
    margin-bottom: 1.5rem;
  }

  .hero-title .line1 { display: block; color: var(--text); }
  .hero-title .line2 {
    display: block;
    -webkit-text-stroke: 1.5px var(--cyan);
    color: transparent;
    position: relative;
  }

  .hero-handle {
    display: inline-block;
    font-size: 0.85rem;
    font-family: 'Space Mono', monospace;
    color: var(--cyan);
    background: var(--cyan-dim);
    border: 1px solid var(--border);
    padding: 0.4rem 1rem;
    border-radius: 2px;
    margin-bottom: 2rem;
    letter-spacing: 0.05em;
  }
  .hero-handle::before { content: '> '; color: var(--muted); }

  .hero-desc {
    font-size: 0.95rem;
    color: var(--muted);
    line-height: 1.8;
    max-width: 560px;
    margin-bottom: 3rem;
    font-family: 'Space Mono', monospace;
  }

  .hero-stats {
    display: flex; gap: 3rem; flex-wrap: wrap;
    margin-bottom: 3rem;
  }
  .stat { }
  .stat-num {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 2.5rem;
    color: var(--cyan);
    line-height: 1;
    display: block;
  }
  .stat-label {
    font-size: 0.7rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--muted);
  }

  .hero-ctas { display: flex; gap: 1rem; flex-wrap: wrap; }

  .btn-primary {
    display: inline-flex; align-items: center; gap: 8px;
    background: var(--cyan);
    color: var(--bg);
    padding: 0.8rem 1.8rem;
    font-family: 'Space Mono', monospace;
    font-size: 0.75rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    text-decoration: none;
    border: none;
    cursor: pointer;
    transition: all .2s;
    clip-path: polygon(0 0, calc(100% - 10px) 0, 100% 10px, 100% 100%, 10px 100%, 0 calc(100% - 10px));
  }
  .btn-primary:hover {
    background: transparent;
    color: var(--cyan);
    box-shadow: inset 0 0 0 1.5px var(--cyan), 0 0 20px var(--cyan-glow);
  }

  .btn-secondary {
    display: inline-flex; align-items: center; gap: 8px;
    background: transparent;
    color: var(--text);
    padding: 0.8rem 1.8rem;
    font-family: 'Space Mono', monospace;
    font-size: 0.75rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    text-decoration: none;
    border: 1px solid var(--border);
    cursor: pointer;
    transition: all .2s;
    clip-path: polygon(0 0, calc(100% - 10px) 0, 100% 10px, 100% 100%, 10px 100%, 0 calc(100% - 10px));
  }
  .btn-secondary:hover {
    border-color: var(--cyan);
    color: var(--cyan);
    box-shadow: 0 0 20px var(--cyan-glow);
  }

  /* Floating terminal */
  .hero-terminal {
    position: absolute;
    right: 4rem; top: 50%;
    transform: translateY(-50%);
    width: 380px;
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 4px;
    overflow: hidden;
    box-shadow: 0 0 40px rgba(0,247,255,0.1);
    display: none;
  }
  @media(min-width: 1100px) { .hero-terminal { display: block; } }

  .terminal-bar {
    background: var(--bg3);
    padding: 0.6rem 1rem;
    display: flex; align-items: center; gap: 6px;
    border-bottom: 1px solid var(--border);
  }
  .t-dot { width: 10px; height: 10px; border-radius: 50%; }
  .t-dot.r { background: #ff5f57; }
  .t-dot.y { background: #febc2e; }
  .t-dot.g { background: #28c840; }
  .terminal-title {
    font-size: 0.7rem;
    color: var(--muted);
    margin-left: 6px;
    letter-spacing: 0.05em;
  }

  .terminal-body {
    padding: 1rem 1.2rem;
    font-size: 0.75rem;
    line-height: 1.9;
    color: var(--muted);
  }
  .terminal-body .cmd { color: var(--cyan); }
  .terminal-body .out { color: #a8ff78; }
  .terminal-body .err { color: var(--magenta); }
  .terminal-body .comment { color: #4a5a78; }
  .cursor-blink {
    display: inline-block;
    width: 8px; height: 14px;
    background: var(--cyan);
    vertical-align: middle;
    animation: blink .8s step-end infinite;
  }
  @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }

  /* SECTIONS */
  section {
    position: relative;
    z-index: 1;
  }

  .section-inner {
    max-width: 1100px;
    margin: 0 auto;
    padding: 6rem 4rem;
  }

  .section-label {
    font-size: 0.65rem;
    letter-spacing: 0.4em;
    text-transform: uppercase;
    color: var(--cyan);
    margin-bottom: 0.75rem;
    display: flex; align-items: center; gap: 10px;
  }
  .section-label::before {
    content: '';
    width: 28px; height: 1px;
    background: var(--cyan);
  }

  .section-title {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: clamp(2rem, 4vw, 3.2rem);
    margin-bottom: 3rem;
    line-height: 1.1;
  }

  /* SKILLS */
  #skills { background: var(--bg2); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }

  .skills-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 1.5px;
    background: var(--border);
    margin-top: 2rem;
  }

  .skill-group {
    background: var(--bg2);
    padding: 2rem;
    transition: background .2s;
  }
  .skill-group:hover { background: var(--bg3); }

  .skill-group-title {
    font-size: 0.65rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--cyan);
    margin-bottom: 1.25rem;
    padding-bottom: 0.75rem;
    border-bottom: 1px solid var(--border);
  }

  .badge-cloud { display: flex; flex-wrap: wrap; gap: 8px; }

  .badge {
    display: inline-flex; align-items: center; gap: 6px;
    background: rgba(255,255,255,0.03);
    border: 1px solid var(--border);
    padding: 0.35rem 0.85rem;
    font-size: 0.72rem;
    letter-spacing: 0.05em;
    color: var(--text);
    transition: all .2s;
    clip-path: polygon(0 0, calc(100% - 6px) 0, 100% 6px, 100% 100%, 6px 100%, 0 calc(100% - 6px));
  }
  .badge:hover {
    border-color: var(--cyan);
    color: var(--cyan);
    background: var(--cyan-dim);
    box-shadow: 0 0 10px var(--cyan-glow);
  }
  .badge-icon { font-size: 0.8rem; }

  /* ABOUT */
  #about { background: var(--bg); }

  .about-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 6rem;
    align-items: center;
  }
  @media(max-width: 768px) { .about-grid { grid-template-columns: 1fr; gap: 3rem; } }

  .about-text p {
    color: var(--muted);
    line-height: 1.9;
    font-size: 0.88rem;
    margin-bottom: 1.25rem;
  }
  .about-text .highlight { color: var(--cyan); }
  .about-text .highlight-mag { color: var(--magenta); }

  .about-facts {
    display: flex;
    flex-direction: column;
    gap: 0;
    border: 1px solid var(--border);
  }

  .fact-row {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 1rem 1.25rem;
    border-bottom: 1px solid var(--border);
    transition: background .2s;
  }
  .fact-row:last-child { border-bottom: none; }
  .fact-row:hover { background: var(--cyan-dim); }

  .fact-icon {
    font-size: 1.1rem;
    width: 32px;
    text-align: center;
    flex-shrink: 0;
  }

  .fact-content { }
  .fact-label {
    font-size: 0.65rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 2px;
  }
  .fact-value {
    font-size: 0.82rem;
    color: var(--text);
  }

  /* EXPERIENCE TIMELINE */
  #experience { background: var(--bg2); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }

  .timeline { position: relative; padding-left: 3rem; }

  .timeline::before {
    content: '';
    position: absolute;
    left: 8px; top: 0; bottom: 0;
    width: 1px;
    background: linear-gradient(to bottom, var(--cyan), transparent);
  }

  .timeline-item {
    position: relative;
    margin-bottom: 3rem;
  }

  .timeline-dot {
    position: absolute;
    left: -3rem;
    top: 4px;
    width: 14px; height: 14px;
    border: 1.5px solid var(--cyan);
    border-radius: 50%;
    background: var(--bg);
    box-shadow: 0 0 10px var(--cyan-glow);
  }
  .timeline-item.current .timeline-dot {
    background: var(--cyan);
  }

  .timeline-date {
    font-size: 0.65rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--cyan);
    margin-bottom: 0.4rem;
  }

  .timeline-title {
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 1.15rem;
    margin-bottom: 0.25rem;
  }

  .timeline-company {
    font-size: 0.78rem;
    color: var(--muted);
    margin-bottom: 0.75rem;
    letter-spacing: 0.05em;
  }

  .timeline-desc {
    font-size: 0.82rem;
    color: var(--muted);
    line-height: 1.8;
    max-width: 580px;
  }

  /* CONNECT */
  #connect { background: var(--bg); }

  .connect-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 1px;
    background: var(--border);
    margin-top: 2.5rem;
  }

  .connect-card {
    background: var(--bg);
    padding: 2rem;
    text-decoration: none;
    display: flex; align-items: center; gap: 1.25rem;
    transition: background .2s;
  }
  .connect-card:hover { background: var(--cyan-dim); }
  .connect-card:hover .connect-arrow { transform: translate(4px, -4px); }

  .connect-icon-wrap {
    width: 48px; height: 48px;
    border: 1px solid var(--border);
    display: flex; align-items: center; justify-content: center;
    font-size: 1.3rem;
    flex-shrink: 0;
    transition: border-color .2s;
  }
  .connect-card:hover .connect-icon-wrap { border-color: var(--cyan); }

  .connect-info { flex: 1; }
  .connect-platform {
    font-size: 0.65rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 2px;
  }
  .connect-handle {
    font-size: 0.88rem;
    color: var(--text);
  }

  .connect-arrow {
    font-size: 1rem;
    color: var(--muted);
    transition: transform .2s, color .2s;
    flex-shrink: 0;
  }
  .connect-card:hover .connect-arrow { color: var(--cyan); }

  /* CERT BADGE */
  .cert-badge {
    display: inline-flex; align-items: center; gap: 10px;
    background: linear-gradient(135deg, rgba(0,114,198,0.15), rgba(0,247,255,0.08));
    border: 1px solid rgba(0,114,198,0.4);
    padding: 0.6rem 1.2rem;
    font-size: 0.78rem;
    border-radius: 2px;
    color: #5bc0eb;
    margin-top: 1.5rem;
  }

  /* FUN FACT */
  .fun-fact-box {
    background: var(--mag-dim);
    border: 1px solid rgba(255,45,120,0.3);
    padding: 1.25rem 1.5rem;
    margin-top: 1.5rem;
    clip-path: polygon(0 0, calc(100% - 12px) 0, 100% 12px, 100% 100%, 12px 100%, 0 calc(100% - 12px));
    font-size: 0.82rem;
    color: var(--muted);
    line-height: 1.7;
  }
  .fun-fact-box span.ff-label {
    font-size: 0.65rem;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    color: var(--magenta);
    display: block;
    margin-bottom: 6px;
  }

  /* FOOTER */
  footer {
    border-top: 1px solid var(--border);
    padding: 2rem 4rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: relative;
    z-index: 1;
    background: var(--bg);
  }
  footer p {
    font-size: 0.7rem;
    color: var(--muted);
    letter-spacing: 0.1em;
  }
  footer .footer-mark {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    color: var(--cyan);
    font-size: 0.85rem;
  }

  /* SCROLL REVEAL */
  .reveal {
    opacity: 0;
    transform: translateY(28px);
    transition: opacity .6s ease, transform .6s ease;
  }
  .reveal.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* TICKER */
  .ticker-wrap {
    overflow: hidden;
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
    background: var(--bg3);
    padding: 0.7rem 0;
    position: relative;
    z-index: 1;
  }
  .ticker {
    display: flex;
    white-space: nowrap;
    animation: ticker 30s linear infinite;
  }
  .ticker-item {
    font-size: 0.65rem;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    color: var(--muted);
    padding: 0 2rem;
  }
  .ticker-item .sep { color: var(--cyan); margin: 0 0.5rem; }
  @keyframes ticker {
    0% { transform: translateX(0); }
    100% { transform: translateX(-50%); }
  }

  @media(max-width: 768px) {
    nav { padding: 1rem 1.5rem; }
    .nav-links { display: none; }
    .hero { padding: 7rem 1.5rem 3rem; }
    .section-inner { padding: 4rem 1.5rem; }
    footer { padding: 1.5rem; flex-direction: column; gap: 0.5rem; text-align: center; }
  }
</style>
</head>
<body>

<div id="cursor"></div>
<div id="cursor-ring"></div>

<!-- NAV -->
<nav>
  <a href="#" class="nav-logo">COMPUTEDOCEAN</a>
  <ul class="nav-links">
    <li><a href="#skills">Stack</a></li>
    <li><a href="#about">About</a></li>
    <li><a href="#experience">Experience</a></li>
    <li><a href="#connect">Connect</a></li>
  </ul>
  <div class="nav-status">
    <div class="status-dot"></div>
    Available for projects
  </div>
</nav>

<!-- TICKER -->
<div class="ticker-wrap" style="margin-top:60px">
  <div class="ticker" id="ticker">
    <span class="ticker-item">Azure 104 Certified<span class="sep">◆</span></span>
    <span class="ticker-item">16+ Years in IT Infrastructure<span class="sep">◆</span></span>
    <span class="ticker-item">Cloud &amp; Systems Engineer<span class="sep">◆</span></span>
    <span class="ticker-item">Automation Architect<span class="sep">◆</span></span>
    <span class="ticker-item">Rackspace Alumni<span class="sep">◆</span></span>
    <span class="ticker-item">Windows Admin IV<span class="sep">◆</span></span>
    <span class="ticker-item">Terraform · Docker · Python<span class="sep">◆</span></span>
    <span class="ticker-item">Hybrid Azure Infrastructure<span class="sep">◆</span></span>
    <span class="ticker-item">Computedocean on Twitch 🎮<span class="sep">◆</span></span>
    <span class="ticker-item">Azure 104 Certified<span class="sep">◆</span></span>
    <span class="ticker-item">16+ Years in IT Infrastructure<span class="sep">◆</span></span>
    <span class="ticker-item">Cloud &amp; Systems Engineer<span class="sep">◆</span></span>
    <span class="ticker-item">Automation Architect<span class="sep">◆</span></span>
    <span class="ticker-item">Rackspace Alumni<span class="sep">◆</span></span>
    <span class="ticker-item">Windows Admin IV<span class="sep">◆</span></span>
    <span class="ticker-item">Terraform · Docker · Python<span class="sep">◆</span></span>
    <span class="ticker-item">Hybrid Azure Infrastructure<span class="sep">◆</span></span>
    <span class="ticker-item">Computedocean on Twitch 🎮<span class="sep">◆</span></span>
  </div>
</div>

<!-- HERO -->
<section class="hero">
  <div class="hero-bg-glow"></div>

  <div class="hero-content">
    <div class="hero-eyebrow reveal">Cloud &amp; Systems Engineer</div>

    <h1 class="hero-title reveal">
      <span class="line1">MICHAEL</span>
      <span class="line2">DALLMAN</span>
    </h1>

    <div class="hero-handle reveal">computedocean</div>

    <p class="hero-desc reveal">
      Seasoned engineer powering innovation at Cavender Auto Group.<br>
      16+ years at Rackspace. Now building hybrid Azure infrastructures,<br>
      secure automation pipelines &amp; SSO integrations.
    </p>

    <div class="hero-stats reveal">
      <div class="stat">
        <span class="stat-num">16+</span>
        <span class="stat-label">Years Experience</span>
      </div>
      <div class="stat">
        <span class="stat-num">5</span>
        <span class="stat-label">Firewalls in a Weekend</span>
      </div>
      <div class="stat">
        <span class="stat-num">3</span>
        <span class="stat-label">Cloud Platforms</span>
      </div>
    </div>

    <div class="hero-ctas reveal">
      <a href="#connect" class="btn-primary">Get in touch ↗</a>
      <a href="#experience" class="btn-secondary">View experience</a>
    </div>
  </div>

  <!-- Terminal card -->
  <div class="hero-terminal reveal">
    <div class="terminal-bar">
      <div class="t-dot r"></div>
      <div class="t-dot y"></div>
      <div class="t-dot g"></div>
      <span class="terminal-title">michael@azure-vm ~ bash</span>
    </div>
    <div class="terminal-body">
      <div><span class="comment"># whoami</span></div>
      <div><span class="cmd">$</span> az account show --query name</div>
      <div><span class="out">"Cavender Auto Group - Prod"</span></div>
      <br>
      <div><span class="cmd">$</span> terraform plan -out=infra.tfplan</div>
      <div><span class="out">Plan: 12 to add, 3 to change, 0 to destroy.</span></div>
      <br>
      <div><span class="cmd">$</span> git log --oneline -3</div>
      <div><span class="out">a3f91c2 feat: hybrid vpn gateway config</span></div>
      <div><span class="out">e8d2104 fix: sso token rotation policy</span></div>
      <div><span class="out">c72ab3f chore: update docker compose</span></div>
      <br>
      <div><span class="cmd">$</span> echo "certified."</div>
      <div><span class="out">AZ-104 ✓</span></div>
      <br>
      <div><span class="cmd">$</span> <span class="cursor-blink"></span></div>
    </div>
  </div>
</section>

<!-- SKILLS -->
<section id="skills">
  <div class="section-inner">
    <div class="section-label reveal">Tech Stack</div>
    <h2 class="section-title reveal">Tools &amp; Technologies</h2>

    <div class="skills-grid reveal">
      <div class="skill-group">
        <div class="skill-group-title">☁ Cloud &amp; Infrastructure</div>
        <div class="badge-cloud">
          <span class="badge"><span class="badge-icon">⬡</span> Azure</span>
          <span class="badge"><span class="badge-icon">⬡</span> AWS</span>
          <span class="badge"><span class="badge-icon">⬡</span> Google Cloud</span>
          <span class="badge">Hybrid Cloud</span>
          <span class="badge">VPN Gateway</span>
          <span class="badge">Azure AD / SSO</span>
        </div>
      </div>
      <div class="skill-group">
        <div class="skill-group-title">⚙ DevOps &amp; Automation</div>
        <div class="badge-cloud">
          <span class="badge"><span class="badge-icon">🐍</span> Python</span>
          <span class="badge"><span class="badge-icon">◈</span> Terraform</span>
          <span class="badge"><span class="badge-icon">🐳</span> Docker</span>
          <span class="badge">Git</span>
          <span class="badge">CI/CD Pipelines</span>
          <span class="badge">PowerShell</span>
        </div>
      </div>
      <div class="skill-group">
        <div class="skill-group-title">💻 OS &amp; Systems</div>
        <div class="badge-cloud">
          <span class="badge">Windows Server</span>
          <span class="badge">Linux</span>
          <span class="badge">SQL Server</span>
          <span class="badge">Active Directory</span>
          <span class="badge">DNS / DHCP</span>
          <span class="badge">Firewall Mgmt</span>
        </div>
      </div>
      <div class="skill-group">
        <div class="skill-group-title">🔐 Security &amp; Networking</div>
        <div class="badge-cloud">
          <span class="badge">Zero Trust</span>
          <span class="badge">SSO Integration</span>
          <span class="badge">MFA / Conditional Access</span>
          <span class="badge">Network Architecture</span>
          <span class="badge">PKI</span>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ABOUT -->
<section id="about">
  <div class="section-inner">
    <div class="about-grid">
      <div class="about-text reveal">
        <div class="section-label">About</div>
        <h2 class="section-title">Building things<br>that don't break.</h2>

        <p>I'm <span class="highlight">Michael Dallman</span> — a Cloud &amp; Systems Engineer with a deep obsession for infrastructure that just <em>works</em>. I spent 16+ years at <span class="highlight">Rackspace</span> mastering Windows Server architecture and enterprise automation, reaching <span class="highlight">Windows Admin IV</span>.</p>

        <p>Today I'm engineering at <span class="highlight">Cavender Auto Group</span>, diving deep into <span class="highlight">Azure</span>, modern security, and DevOps pipelines. My specialty: taking complex, messy infrastructure problems and turning them into clean, automated, documented solutions.</p>

        <p>Known for solving the problems others skip past — including <span class="highlight-mag">migrating 5 firewalls in a single weekend</span> and surviving to tell the tale.</p>

        <div class="cert-badge">
          🏆 Microsoft Certified: Azure Administrator Associate (AZ-104)
        </div>

        <div class="fun-fact-box">
          <span class="ff-label">⚡ Fun fact</span>
          Once migrated 5 firewalls in a single weekend. Coffee was consumed. Chaos was managed. Everything came back up.
        </div>
      </div>

      <div class="about-facts reveal">
        <div class="fact-row">
          <div class="fact-icon">🏢</div>
          <div class="fact-content">
            <div class="fact-label">Current Role</div>
            <div class="fact-value">Cloud &amp; Systems Engineer @ Cavender Auto Group</div>
          </div>
        </div>
        <div class="fact-row">
          <div class="fact-icon">🏛</div>
          <div class="fact-content">
            <div class="fact-label">Past Life</div>
            <div class="fact-value">16+ Years @ Rackspace · Windows Admin IV</div>
          </div>
        </div>
        <div class="fact-row">
          <div class="fact-icon">☁</div>
          <div class="fact-content">
            <div class="fact-label">Currently Building</div>
            <div class="fact-value">Hybrid Azure infra, secure automation pipelines, SSO integrations</div>
          </div>
        </div>
        <div class="fact-row">
          <div class="fact-icon">📜</div>
          <div class="fact-content">
            <div class="fact-label">Certifications</div>
            <div class="fact-value">Azure AZ-104 · Deep enterprise IT experience</div>
          </div>
        </div>
        <div class="fact-row">
          <div class="fact-icon">🎮</div>
          <div class="fact-content">
            <div class="fact-label">Off-Duty</div>
            <div class="fact-value">Twitch streamer @ Computedocean · Xbox gamer</div>
          </div>
        </div>
        <div class="fact-row">
          <div class="fact-icon">📍</div>
          <div class="fact-content">
            <div class="fact-label">Location</div>
            <div class="fact-value">San Antonio, Texas, US</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- EXPERIENCE -->
<section id="experience">
  <div class="section-inner">
    <div class="section-label reveal">Experience</div>
    <h2 class="section-title reveal">Career Timeline</h2>

    <div class="timeline">
      <div class="timeline-item current reveal">
        <div class="timeline-dot"></div>
        <div class="timeline-date">2023 — Present</div>
        <div class="timeline-title">Cloud &amp; Systems Engineer</div>
        <div class="timeline-company">Cavender Auto Group · San Antonio, TX</div>
        <div class="timeline-desc">Architecting hybrid Azure infrastructures and modern DevOps pipelines. Leading SSO integrations, secure automation, and cloud-native migration efforts across the organization.</div>
      </div>

      <div class="timeline-item reveal">
        <div class="timeline-dot"></div>
        <div class="timeline-date">2005 — 2023</div>
        <div class="timeline-title">Windows Admin IV</div>
        <div class="timeline-company">Rackspace · San Antonio, TX</div>
        <div class="timeline-desc">16+ years mastering enterprise infrastructure, Windows Server architecture, and automation at scale. Progressed from systems administration to senior architecture and team leadership — all at one of the world's leading managed cloud companies.</div>
      </div>
    </div>
  </div>
</section>

<!-- CONNECT -->
<section id="connect">
  <div class="section-inner">
    <div class="section-label reveal">Connect</div>
    <h2 class="section-title reveal">Let's Build Something.</h2>

    <div class="connect-grid reveal">
      <a href="https://linkedin.com/in/michael-dallman-21b22355" target="_blank" class="connect-card">
        <div class="connect-icon-wrap">💼</div>
        <div class="connect-info">
          <div class="connect-platform">LinkedIn</div>
          <div class="connect-handle">michael-dallman</div>
        </div>
        <div class="connect-arrow">↗</div>
      </a>
      <a href="https://github.com/computedocean" target="_blank" class="connect-card">
        <div class="connect-icon-wrap">🐙</div>
        <div class="connect-info">
          <div class="connect-platform">GitHub</div>
          <div class="connect-handle">computedocean</div>
        </div>
        <div class="connect-arrow">↗</div>
      </a>
      <a href="https://twitch.tv/computedocean" target="_blank" class="connect-card">
        <div class="connect-icon-wrap">🎮</div>
        <div class="connect-info">
          <div class="connect-platform">Twitch</div>
          <div class="connect-handle">computedocean</div>
        </div>
        <div class="connect-arrow">↗</div>
      </a>
      <a href="https://twitter.com/computedocean" target="_blank" class="connect-card">
        <div class="connect-icon-wrap">𝕏</div>
        <div class="connect-info">
          <div class="connect-platform">Twitter / X</div>
          <div class="connect-handle">@computedocean</div>
        </div>
        <div class="connect-arrow">↗</div>
      </a>
      <a href="https://facebook.com/dallmanm" target="_blank" class="connect-card">
        <div class="connect-icon-wrap">📘</div>
        <div class="connect-info">
          <div class="connect-platform">Facebook</div>
          <div class="connect-handle">dallmanm</div>
        </div>
        <div class="connect-arrow">↗</div>
      </a>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <span class="footer-mark">COMPUTEDOCEAN</span>
  <p>Michael Dallman · Cloud &amp; Systems Engineer · San Antonio, TX</p>
  <p>Built with intent. Deployed with confidence.</p>
</footer>

<script>
// Custom cursor
const cursor = document.getElementById('cursor');
const ring = document.getElementById('cursor-ring');
let mx = 0, my = 0, rx = 0, ry = 0;
document.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY; });
function animCursor() {
  cursor.style.left = mx + 'px';
  cursor.style.top = my + 'px';
  rx += (mx - rx) * 0.12;
  ry += (my - ry) * 0.12;
  ring.style.left = rx + 'px';
  ring.style.top = ry + 'px';
  requestAnimationFrame(animCursor);
}
animCursor();

// Scroll reveal
const reveals = document.querySelectorAll('.reveal');
const io = new IntersectionObserver((entries) => {
  entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('visible'); } });
}, { threshold: 0.1 });
reveals.forEach(el => io.observe(el));

// Stagger children of grids
document.querySelectorAll('.skills-grid, .connect-grid').forEach(grid => {
  Array.from(grid.children).forEach((child, i) => {
    child.style.transitionDelay = (i * 0.07) + 's';
  });
});
</script>
</body>
</html>
