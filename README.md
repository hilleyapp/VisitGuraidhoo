Welcome to Guraidhoo Island
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Ferry Schedule • Guraidhoo ↔ Male</title>
  
  <!-- Open Graph / Social Media Preview Meta Tags -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://hilleyapp.github.io/VisitGuraidhoo" />
  <meta property="og:title" content="Ferry Schedule • Guraidhoo ↔ Male" />
  <meta property="og:description" content="Check live ferry schedules, live tracking, weather, and onboard entertainment for Guraidhoo and Malé." />
  <meta property="og:image" content="https://hilleyapp.github.io/VisitGuraidhoo/preview.jpg" />

  <!-- Twitter Card Meta Tags -->
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:url" content="https://hilleyapp.github.io/VisitGuraidhoo" />
  <meta name="twitter:title" content="Ferry Schedule • Guraidhoo ↔ Male" />
  <meta name="twitter:description" content="Check live ferry schedules, live tracking, weather, and onboard entertainment for Guraidhoo and Malé." />
  <meta name="twitter:image" content="https://hilleyapp.github.io/VisitGuraidhoo/preview.jpg" />

  <!-- Leaflet CSS -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
        integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY="
        crossorigin="" />
  <style>
    :root {
      --primary: #0288D1;
      --accent: #00ACC1;
      --escape: #1E88E5;
      --vmarine: #00897B;
      --hawks: #E65100;
      --buggy: #43A047;
      --dining: #D81B60;
      --bg: #F4F8FB;
      --card: #FFFFFF;
      --text: #1A2A3A;
      --muted: #5A6A7A;
      --border: #D0DCE8;
      --shadow: 0 4px 20px rgba(11, 61, 92, 0.08);
    }

    [data-theme="dark"] {
      --primary: #0288D1;
      --accent: #26A69A;
      --bg: #0F172A;
      --card: #1E293B;
      --text: #F8FAFC;
      --muted: #94A3B8;
      --border: #334155;
      --shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
    }

    * { box-sizing: border-box; margin: 0; padding: 0; transition: background 0.2s, color 0.2s, border-color 0.2s; }

    body {
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
      background: var(--bg);
      color: var(--text);
      line-height: 1.5;
      min-height: 100vh;
    }

    /* Header Photo Banner */
    header {
      background: linear-gradient(135deg, rgba(2, 119, 189, 0.75) 0%, rgba(0, 131, 143, 0.75) 50%, rgba(0, 105, 92, 0.75) 100%),
                  url('Top-banner.jpg') center/cover no-repeat;
      color: white;
      padding: 3rem 1.5rem 2.5rem;
      text-align: center;
      box-shadow: 0 4px 16px rgba(0,0,0,0.2);
      position: relative;
    }
    header h1 {
      font-size: 2.2rem;
      font-weight: 800;
      letter-spacing: 0.5px;
      margin-bottom: 0.2rem;
      text-shadow: 0 2px 6px rgba(0,0,0,0.5);
    }
    header p {
      font-size: 1.15rem;
      opacity: 0.95;
      font-weight: 500;
      margin-bottom: 0.9rem;
      text-shadow: 0 1px 4px rgba(0,0,0,0.5);
    }
    .live-clock {
      display: inline-flex;
      align-items: center;
      gap: 0.4rem;
      background: rgba(0, 0, 0, 0.45);
      backdrop-filter: blur(4px);
      padding: 0.35rem 0.9rem;
      border-radius: 20px;
      font-size: 0.82rem;
      font-weight: 600;
      letter-spacing: 0.3px;
      border: 1px solid rgba(255, 255, 255, 0.25);
    }

    /* Theme Toggle */
    .theme-toggle {
      position: absolute;
      top: 1.2rem;
      right: 1.2rem;
      background: rgba(0, 0, 0, 0.45);
      backdrop-filter: blur(4px);
      border: 1px solid rgba(255, 255, 255, 0.25);
      color: white;
      padding: 0.4rem 0.8rem;
      border-radius: 20px;
      cursor: pointer;
      font-size: 0.8rem;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: 0.3rem;
    }
    .theme-toggle:hover { background: rgba(0, 0, 0, 0.65); }

    /* Weather Widget */
    .weather-widget {
      max-width: 960px;
      margin: -1.2rem auto 1.2rem;
      padding: 0 1rem;
      position: relative;
      z-index: 11;
    }
    .weather-card {
      background: var(--card);
      border-radius: 14px;
      box-shadow: var(--shadow);
      padding: 0.9rem 1.2rem;
      border: 1px solid var(--border);
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 0.8rem;
    }
    .weather-left {
      display: flex;
      align-items: center;
      gap: 0.7rem;
    }
    .weather-icon {
      font-size: 1.8rem;
    }
    .weather-info strong {
      display: block;
      font-size: 0.9rem;
      color: var(--text);
    }
    .weather-info span {
      font-size: 0.78rem;
      color: var(--muted);
    }
    .weather-stats {
      display: flex;
      gap: 1.2rem;
      font-size: 0.85rem;
      color: var(--muted);
      font-weight: 500;
    }
    .weather-stats strong {
      color: var(--text);
    }

    /* Controls */
    .controls {
      max-width: 960px;
      margin: 0 auto;
      padding: 0 1rem;
      position: relative;
      z-index: 10;
    }
    .controls-inner {
      background: var(--card);
      border-radius: 14px;
      box-shadow: var(--shadow);
      padding: 1.1rem 1.3rem;
      display: flex;
      flex-wrap: wrap;
      gap: 1rem;
      align-items: center;
      justify-content: center;
      border: 1px solid var(--border);
    }
    .control-group {
      display: flex;
      flex-direction: column;
      gap: 0.3rem;
    }
    .control-group label {
      font-size: 0.7rem;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.6px;
      color: var(--muted);
    }
    select, input[type="text"], .day-btns {
      display: flex;
      gap: 0.35rem;
      flex-wrap: wrap;
    }
    select, input[type="text"] {
      appearance: none;
      background: var(--bg);
      border: 1.5px solid var(--border);
      border-radius: 8px;
      padding: 0.45rem 2rem 0.45rem 0.8rem;
      font-size: 0.9rem;
      color: var(--text);
      cursor: pointer;
      background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 12 12'%3E%3Cpath fill='%235A6A7A' d='M6 8L1 3h10z'/%3E%3C/svg%3E");
      background-repeat: no-repeat;
      background-position: right 0.7rem center;
      min-width: 140px;
    }
    input[type="text"] {
      background-image: none;
      padding-right: 0.8rem;
      min-width: 160px;
    }
    select:focus, input[type="text"]:focus {
      outline: none;
      border-color: var(--accent);
      box-shadow: 0 0 0 3px rgba(26, 107, 154, 0.15);
    }

    .day-btn {
      border: 1.5px solid var(--border);
      background: var(--card);
      color: var(--text);
      border-radius: 8px;
      padding: 0.4rem 0.7rem;
      font-size: 0.8rem;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.15s ease;
      min-width: 42px;
    }
    .day-btn:hover { border-color: var(--accent); color: var(--accent); }
    .day-btn.active {
      background: var(--primary);
      border-color: var(--primary);
      color: white;
    }
    .day-btn.all-btn { min-width: 52px; }

    /* Main content */
    main {
      max-width: 960px;
      margin: 1.5rem auto 2rem;
      padding: 0 1rem;
    }

    /* Map section */
    .map-section {
      margin-bottom: 1.8rem;
    }
    .map-section h2 {
      font-size: 0.85rem;
      text-transform: uppercase;
      letter-spacing: 0.8px;
      color: var(--muted);
      margin-bottom: 0.7rem;
      text-align: center;
    }
    #map {
      height: 320px;
      border-radius: 14px;
      box-shadow: var(--shadow);
      border: 2px solid var(--border);
      z-index: 1;
    }
    .map-controls {
      display: flex;
      justify-content: center;
      gap: 1rem;
      margin-top: 0.8rem;
    }
    .sim-btn {
      background: var(--primary);
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 8px;
      font-size: 0.85rem;
      font-weight: 600;
      cursor: pointer;
      box-shadow: 0 2px 8px rgba(0,0,0,0.15);
    }
    .sim-btn:hover { background: var(--accent); }

    .map-info {
      display: flex;
      justify-content: center;
      gap: 1.5rem;
      margin-top: 0.7rem;
      flex-wrap: wrap;
      font-size: 0.82rem;
      color: var(--muted);
    }
    .map-info span {
      display: flex;
      align-items: center;
      gap: 0.35rem;
    }
    .map-info .dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
    }
    .map-info .dot.male { background: #E53935; }
    .map-info .dot.guraidhoo { background: #1E88E5; }
    .map-info .dot.route { background: #FFB300; width: 18px; height: 3px; border-radius: 2px; }

    /* Live Tracking & Entertainment Grid */
    .tracking-entertainment-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 1rem;
      margin: 1.4rem 0 0.5rem;
      align-items: stretch;
    }
    @media (max-width: 768px) {
      .tracking-entertainment-grid {
        grid-template-columns: 1fr;
      }
    }

    .live-tracking, .entertainment-box {
      background: var(--card);
      border-radius: 14px;
      box-shadow: var(--shadow);
      padding: 1.2rem 1.3rem;
      border: 1.5px solid var(--border);
      display: flex;
      flex-direction: column;
    }
    .live-tracking h2, .entertainment-box h2 {
      font-size: 0.85rem;
      text-transform: uppercase;
      letter-spacing: 0.8px;
      color: var(--muted);
      margin-bottom: 0.85rem;
      text-align: center;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 0.45rem;
    }
    .live-dot {
      width: 9px;
      height: 9px;
      background: #E53935;
      border-radius: 50%;
      animation: pulse 1.4s ease-in-out infinite;
    }
    @keyframes pulse {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.45; transform: scale(0.85); }
    }
    .live-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 0.7rem;
      flex-grow: 1;
    }
    .live-btn {
      display: flex;
      align-items: center;
      gap: 0.6rem;
      padding: 0.65rem 0.8rem;
      border-radius: 10px;
      text-decoration: none;
      font-weight: 600;
      font-size: 0.85rem;
      border: 1.5px solid var(--border);
      background: var(--bg);
      color: var(--text);
    }
    .live-btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 14px rgba(0,0,0,0.1);
      border-color: var(--accent);
    }
    .live-btn .icon, .live-btn .logo-img {
      width: 28px;
      height: 28px;
      border-radius: 6px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-size: 0.75rem;
      font-weight: 700;
      flex-shrink: 0;
      object-fit: cover;
      background: white;
    }
    .live-btn.all .icon { background: #7B1FA2; }
    .live-btn small {
      display: block;
      font-weight: 400;
      font-size: 0.68rem;
      opacity: 0.8;
      margin-top: 1px;
    }

    /* Stream Button styling */
    .tv-promo-wrapper {
      display: flex;
      flex-direction: column;
      flex-grow: 1;
      justify-content: center;
    }
    .tv-promo-btn {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 1.2rem 1rem;
      background: linear-gradient(135deg, rgba(230, 81, 0, 0.08), rgba(26, 107, 154, 0.08));
      border: 2px dashed var(--accent);
      border-radius: 12px;
      text-decoration: none;
      color: var(--text);
      transition: transform 0.15s, background 0.15s;
      width: 100%;
      height: 100%;
    }
    .tv-promo-btn:hover {
      transform: translateY(-2px);
      background: linear-gradient(135deg, rgba(230, 81, 0, 0.12), rgba(26, 107, 154, 0.12));
    }
    .tv-promo-btn .tv-logo-img {
      width: 44px;
      height: 44px;
      object-fit: contain;
      margin-bottom: 0.4rem;
    }
    .tv-promo-btn strong {
      font-size: 1rem;
      color: var(--primary);
      margin-bottom: 0.2rem;
    }
    .tv-promo-btn span {
      font-size: 0.78rem;
      color: var(--muted);
    }

    .section-title {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin: 1.5rem 0 0.9rem;
      font-size: 1.05rem;
      font-weight: 700;
      color: var(--primary);
    }
    .section-title-left {
      display: flex;
      align-items: center;
      gap: 0.6rem;
    }
    .section-title .arrow {
      background: var(--primary);
      color: white;
      font-size: 0.75rem;
      padding: 0.2rem 0.55rem;
      border-radius: 6px;
      font-weight: 600;
    }
    .count-badge {
      font-size: 0.75rem;
      background: var(--border);
      color: var(--muted);
      padding: 0.15rem 0.5rem;
      border-radius: 10px;
    }

    .cards {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
      gap: 1rem;
    }

    .card {
      background: var(--card);
      border-radius: 12px;
      box-shadow: var(--shadow);
      overflow: hidden;
      border: 1px solid var(--border);
      border-top: 4px solid transparent;
      transition: transform 0.15s ease, box-shadow 0.15s ease;
    }
    .card:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 28px rgba(11, 61, 92, 0.12);
    }
    .card.escape { border-top-color: var(--escape); }
    .card.vmarine { border-top-color: var(--vmarine); }
    .card.hawks { border-top-color: var(--hawks); }

    .card-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0.85rem 1rem 0.5rem;
    }
    .operator {
      font-weight: 700;
      font-size: 0.95rem;
      display: flex;
      align-items: center;
      gap: 0.5rem;
    }
    .operator img {
      width: 24px;
      height: 24px;
      border-radius: 50%;
      object-fit: cover;
      background: white;
    }
    .card.escape .operator { color: var(--escape); }
    .card.vmarine .operator { color: var(--vmarine); }
    .card.hawks .operator { color: var(--hawks); }

    .time {
      font-size: 1.55rem;
      font-weight: 700;
      color: var(--text);
      letter-spacing: -0.5px;
    }

    .card-body {
      padding: 0 1rem 1rem;
    }
    .days-badge {
      display: inline-block;
      background: var(--bg);
      color: var(--muted);
      font-size: 0.78rem;
      font-weight: 600;
      padding: 0.25rem 0.6rem;
      border-radius: 6px;
      margin-top: 0.35rem;
      border: 1px solid var(--border);
    }
    .days-badge.daily {
      background: rgba(46, 125, 50, 0.1);
      color: #2E7D32;
      border-color: rgba(46, 125, 50, 0.2);
    }
    .card.next-ferry {
      border-top-width: 5px;
      box-shadow: 0 0 0 2px #FF6F00, 0 8px 28px rgba(255, 111, 0, 0.2);
    }
    .next-badge {
      display: inline-block;
      background: #FF6F00;
      color: white;
      font-size: 0.7rem;
      font-weight: 700;
      padding: 0.2rem 0.55rem;
      border-radius: 5px;
      margin-left: 0.4rem;
      text-transform: uppercase;
      letter-spacing: 0.4px;
    }
    .next-ferry-banner {
      background: linear-gradient(135deg, #FF6F00, #FF8F00);
      color: white;
      border-radius: 12px;
      padding: 0.9rem 1.2rem;
      margin-bottom: 1rem;
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 0.8rem;
      box-shadow: 0 4px 16px rgba(255, 111, 0, 0.25);
    }
    .next-ferry-banner .banner-col {
      flex: 1;
      min-width: 200px;
    }
    .next-ferry-banner .label {
      font-size: 0.72rem;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.6px;
      opacity: 0.9;
    }
    .next-ferry-banner .details {
      font-size: 1.05rem;
      font-weight: 700;
    }
    .next-ferry-banner.hidden { display: none; }

    .empty-state {
      text-align: center;
      padding: 3rem 1rem;
      color: var(--muted);
      grid-column: 1 / -1;
      background: var(--card);
      border-radius: 12px;
      border: 1px solid var(--border);
    }
    .empty-state svg {
      width: 48px;
      height: 48px;
      margin-bottom: 0.8rem;
      opacity: 0.4;
    }
    .reset-filter-btn {
      margin-top: 0.8rem;
      background: var(--accent);
      color: white;
      border: none;
      padding: 0.4rem 0.9rem;
      border-radius: 6px;
      font-size: 0.8rem;
      cursor: pointer;
      font-weight: 600;
    }

    /* Contacts, Buggy Service & Dining Section */
    .contacts, .buggy-service, .dining-service {
      margin-top: 1.5rem;
      background: var(--card);
      border-radius: 14px;
      box-shadow: var(--shadow);
      padding: 1.3rem;
      border: 1px solid var(--border);
    }
    .contacts { margin-top: 2.5rem; }
    
    .contacts h3, .buggy-service h3, .dining-service h3 {
      font-size: 0.85rem;
      text-transform: uppercase;
      letter-spacing: 0.8px;
      color: var(--muted);
      margin-bottom: 1rem;
      text-align: center;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 0.4rem;
    }
    .contact-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 0.8rem;
    }
    .contact-item {
      display: flex;
      align-items: center;
      gap: 0.7rem;
      padding: 0.7rem 0.9rem;
      border-radius: 10px;
      text-decoration: none;
      border: 1px solid var(--border);
      background: var(--bg);
    }
    .contact-item:hover { border-color: var(--accent); }
    .contact-item img, .contact-item .icon-avatar {
      width: 36px;
      height: 36px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-shrink: 0;
      object-fit: cover;
      background: white;
    }
    .contact-item .icon-avatar.buggy-avatar {
      background: var(--buggy);
      color: white;
      font-size: 1.2rem;
    }
    .contact-item .icon-avatar.dining-avatar {
      background: var(--dining);
      color: white;
      font-size: 1.1rem;
    }
    .contact-item .info strong {
      display: block;
      font-size: 0.88rem;
      color: var(--text);
    }
    .contact-item .info span {
      font-size: 0.82rem;
      color: var(--accent);
      font-weight: 500;
    }
    .contact-item.buggy-item:hover {
      border-color: var(--buggy);
    }
    .contact-item.buggy-item .info span {
      color: var(--buggy);
    }
    .contact-item.dining-item:hover {
      border-color: var(--dining);
    }
    .contact-item.dining-item .info span {
      color: var(--dining);
    }

    footer {
      text-align: center;
      padding: 1.5rem 1rem 2rem;
      font-size: 0.78rem;
      color: var(--muted);
    }

    @media (max-width: 600px) {
      header h1 { font-size: 1.4rem; }
      .controls-inner { flex-direction: column; align-items: stretch; }
      .day-btns { justify-content: center; }
      .cards { grid-template-columns: 1fr; }
      #map { height: 260px; }
    }
  </style>
</head>
<body>
  <header>
    <button class="theme-toggle" id="theme-toggle">🌙 Dark</button>
    <h1>Ferry Schedule</h1>
    <p>Guraidhoo ↔ Male ↔ Guraidhoo</p>
    <div class="live-clock" id="live-clock">🕒 Loading local time...</div>
  </header>

  <!-- Weather Widget -->
  <div class="weather-widget">
    <div class="weather-card">
      <div class="weather-left">
        <span class="weather-icon">⛅</span>
        <div class="weather-info">
          <strong>Guraidhoo Weather</strong>
          <span>Scattered Clouds / Afternoon Showers</span>
        </div>
      </div>
      <div class="weather-stats">
        <div>Temp: <strong>29°C</strong></div>
        <div>Wind: <strong>Gentle breeze</strong></div>
      </div>
    </div>
  </div>

  <div class="controls">
    <div class="controls-inner">
      <div class="control-group">
        <label>Route</label>
        <select id="direction">
          <option value="all">Both Routes</option>
          <option value="to-male">Guraidhoo → Male</option>
          <option value="to-guraidhoo">Male → Guraidhoo</option>
        </select>
      </div>
      <div class="control-group">
        <label>Operator</label>
        <select id="operator">
          <option value="all">All Operators</option>
          <option value="escape">Escape</option>
          <option value="vmarine">V Marine</option>
          <option value="hawks">Hawks</option>
        </select>
      </div>
      <div class="control-group">
        <label>Quick Search</label>
        <input type="text" id="search-input" placeholder="Search time or operator..." />
      </div>
      <div class="control-group">
        <label>Day</label>
        <div class="day-btns" id="day-btns">
          <button class="day-btn all-btn active" data-day="all">All</button>
          <button class="day-btn" data-day="Sun">Sun</button>
          <button class="day-btn" data-day="Mon">Mon</button>
          <button class="day-btn" data-day="Tue">Tue</button>
          <button class="day-btn" data-day="Wed">Wed</button>
          <button class="day-btn" data-day="Thu">Thu</button>
          <button class="day-btn" data-day="Fri">Fri</button>
          <button class="day-btn" data-day="Sat">Sat</button>
        </div>
      </div>
    </div>
  </div>

  <main>
    <div id="next-ferry-banner" class="next-ferry-banner hidden"></div>
    
    <!-- Interactive Map -->
    <div class="map-section">
      <h2>Ferry Route Map</h2>
      <div id="map"></div>
      <div class="map-controls">
        <button class="sim-btn" id="sim-btn">▶ Simulate Ferry Trip</button>
      </div>
      <div class="map-info">
        <span><span class="dot male"></span> Malé</span>
        <span><span class="dot guraidhoo"></span> Guraidhoo</span>
        <span><span class="dot route"></span> Ferry Route (~32 km)</span>
      </div>
    </div>

    <!-- Live Tracking & Entertainment Grid -->
    <div class="tracking-entertainment-grid">
      <!-- Live Tracking -->
      <div class="live-tracking">
        <h2><span class="live-dot"></span> Live Tracking</h2>
        <div class="live-grid">
          <a class="live-btn escape" href="https://m.followme.mv/public/?s=ESCAPE" target="_blank" rel="noopener">
            <img src="escape-logo.jpg" alt="Escape" class="logo-img" />
            <div>Escape<small>Queen · Wing</small></div>
          </a>
          <a class="live-btn vmarine" href="https://m.followme.mv/public/?s=V%20Marine" target="_blank" rel="noopener">
            <img src="vmarine-logo.jpg" alt="V Marine" class="logo-img" />
            <div>V Marine<small>V Marine 1-3</small></div>
          </a>
          <a class="live-btn hawks" href="https://m.followme.mv/public/?s=HAWKS" target="_blank" rel="noopener">
            <img src="hawks-logo.jpg" alt="Hawks" class="logo-img" />
            <div>Hawks<small>Daisy · Ivy</small></div>
          </a>
          <a class="live-btn all" href="https://m.followme.mv/public/" target="_blank" rel="noopener">
            <div class="icon">✈</div>
            <div>All Vessels<small>FollowMe map</small></div>
          </a>
        </div>
      </div>

      <!-- Onboard Entertainment / Live Stream -->
      <div class="entertainment-box">
        <h2>📺 Onboard Entertainment</h2>
        <div class="tv-promo-wrapper">
          <a class="tv-promo-btn" href="https://hilaytv.xyz/" target="_blank" rel="noopener">
            <img src="hilaytv-logo.png" alt="Hilay TV Play Icon" class="tv-logo-img" />
            <strong>Enjoy Live TV while on the Go</strong>
            <span>Stream your favorite channels & shows live on board</span>
            <span style="font-size: 0.68rem; opacity: 0.75; margin-top: 0.3rem;">Powered by HilayTV</span>
          </a>
        </div>
      </div>
    </div>

    <div id="to-male-section">
      <div class="section-title">
        <div class="section-title-left"><span class="arrow">→</span> Guraidhoo → Male</div>
        <span class="count-badge" id="to-male-count">0 ferries</span>
      </div>
      <div class="cards" id="to-male-cards"></div>
    </div>

    <div id="to-guraidhoo-section">
      <div class="section-title">
        <div class="section-title-left"><span class="arrow">→</span> Male → Guraidhoo</div>
        <span class="count-badge" id="to-guraidhoo-count">0 ferries</span>
      </div>
      <div class="cards" id="to-guraidhoo-cards"></div>
    </div>

    <!-- Contact Ferry Operators -->
    <div class="contacts">
      <h3>Contact Ferry Operators</h3>
      <div class="contact-grid">
        <a class="contact-item escape" href="tel:+9607994699">
          <img src="escape-logo.jpg" alt="Escape" />
          <div class="info"><strong>Escape</strong><span>+960 799 4699</span></div>
        </a>
        <a class="contact-item vmarine" href="tel:+9607407777">
          <img src="vmarine-logo.jpg" alt="V Marine" />
          <div class="info"><strong>V Marine</strong><span>+960 740 7777</span></div>
        </a>
        <a class="contact-item hawks" href="tel:+9607216816">
          <img src="hawks-logo.jpg" alt="Hawks" />
          <div class="info"><strong>Hawks</strong><span>+960 721 6816</span></div>
        </a>
      </div>
    </div>

    <!-- Buggy Service Booking -->
    <div class="buggy-service">
      <h3>🛺 Island Buggy Hiring Service</h3>
      <div class="contact-grid">
        <a class="contact-item buggy-item" href="tel:+9607994699">
          <div class="icon-avatar buggy-avatar">🛺</div>
          <div class="info"><strong>Coming soon</strong><span>Ready to Book Your Ride</span></div>
        </a>
      </div>
    </div>

    <!-- Cafés & Dining Section -->
    <div class="dining-service">
      <h3>☕ Cafés & Dining</h3>
      <div class="contact-grid">
        <a class="contact-item dining-item" href="tel:+9607257878">
          <img src="muranga-logo.jpg" alt="Muranga Restaurant" onerror="this.style.display='none'; this.nextElementSibling.style.display='flex';" />
          <div class="icon-avatar dining-avatar" style="display:none;">🍽️</div>
          <div class="info"><strong>Muranga Restaurant</strong><span>+960 725 7878</span></div>
        </a>
        <a class="contact-item dining-item" href="tel:+9609888339">
          <img src="amore-logo.jpg" alt="Amore Cafe" onerror="this.style.display='none'; this.nextElementSibling.style.display='flex';" />
          <div class="icon-avatar dining-avatar" style="display:none;">☕</div>
          <div class="info"><strong>Amore - The Island Cafe</strong><span>+960 988 8339</span></div>
        </a>
        <a class="contact-item dining-item" href="tel:+9607372999">
          <img src="chambao-logo.jpg" alt="Chambao Cafe" onerror="this.style.display='none'; this.nextElementSibling.style.display='flex';" />
          <div class="icon-avatar dining-avatar" style="display:none;">🍹</div>
          <div class="info"><strong>Chambao - Cafe & Retreat</strong><span>+960 737 2999</span></div>
        </a>
        <a class="contact-item dining-item" href="tel:+9609663694">
          <img src="thundi-logo.jpg" alt="Thundi Cafe" onerror="this.style.display='none'; this.nextElementSibling.style.display='flex';" />
          <div class="icon-avatar dining-avatar" style="display:none;">🏝️</div>
          <div class="info"><strong>Thundi Cafe</strong><span>+960 966 3694</span></div>
        </a>
      </div>
    </div>
  </main>

  <footer>
    Schedules may change without prior notice. Please confirm with the operator before travel.
  </footer>

  <!-- Leaflet JS -->
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
          integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo="
          crossorigin=""></script>

  <script>
    // Theme Toggle Logic
    const themeToggle = document.getElementById("theme-toggle");
    themeToggle.addEventListener("click", () => {
      const currentTheme = document.documentElement.getAttribute("data-theme");
      if (currentTheme === "dark") {
        document.documentElement.removeAttribute("data-theme");
        themeToggle.textContent = "🌙 Dark";
      } else {
        document.documentElement.setAttribute("data-theme", "dark");
        themeToggle.textContent = "☀️ Light";
      }
    });

    // Live Clock Logic (Maldives Time UTC+5)
    function updateClock() {
      const now = new Date();
      const utc = now.getTime() + (now.getTimezoneOffset() * 60000);
      const maldivesTime = new Date(utc + (5 * 3600000));
      
      const options = { weekday: 'short', month: 'short', day: 'numeric', hour: '2-digit', minute: '2-digit', second: '2-digit', hour12: true };
      document.getElementById("live-clock").textContent = "🕒 " + maldivesTime.toLocaleString('en-US', options) + " (Maldives)";
    }
    setInterval(updateClock, 1000);
    updateClock();

    // ========== MAP & SIMULATION ==========
    const maleCoords = [4.17521, 73.50916];
    const guraidhooCoords = [3.90045, 73.46623];

    const map = L.map('map', {
      scrollWheelZoom: false,
      zoomControl: true
    }).setView([4.04, 73.49], 10);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 18,
      attribution: '© OpenStreetMap'
    }).addTo(map);

    L.marker(maleCoords, {
      icon: L.divIcon({
        className: '',
        html: `<div style="background:#E53935;width:16px;height:16px;border-radius:50%;border:3px solid white;box-shadow:0 2px 6px rgba(0,0,0,0.35);"></div>`,
        iconSize: [16, 16],
        iconAnchor: [8, 8]
      })
    }).addTo(map).bindPopup("<strong>Malé</strong><br>Capital");

    L.marker(guraidhooCoords, {
      icon: L.divIcon({
        className: '',
        html: `<div style="background:#1E88E5;width:16px;height:16px;border-radius:50%;border:3px solid white;box-shadow:0 2px 6px rgba(0,0,0,0.35);"></div>`,
        iconSize: [16, 16],
        iconAnchor: [8, 8]
      })
    }).addTo(map).bindPopup("<strong>Guraidhoo</strong><br>Kaafu Atoll");

    L.polyline([guraidhooCoords, maleCoords], {
      color: '#FFB300',
      weight: 4,
      opacity: 0.85,
      dashArray: '8, 8',
      lineCap: 'round'
    }).addTo(map);

    map.fitBounds([maleCoords, guraidhooCoords], { padding: [40, 40] });
    map.on('click', () => map.scrollWheelZoom.enable());
    map.on('mouseout', () => map.scrollWheelZoom.disable());

    // Ferry Simulation Marker
    let simMarker = null;
    document.getElementById("sim-btn").addEventListener("click", () => {
      if (simMarker) map.removeLayer(simMarker);
      
      const boatIcon = L.divIcon({
        className: '',
        html: `<div style="background:#FF6F00;width:22px;height:22px;border-radius:50%;border:3px solid white;display:flex;align-items:center;justify-content:center;color:white;font-size:10px;box-shadow:0 0 10px #FF6F00;">🚤</div>`,
        iconSize: [22, 22],
        iconAnchor: [11, 11]
      });

      simMarker = L.marker(guraidhooCoords, { icon: boatIcon }).addTo(map);
      simMarker.bindPopup("<strong>Simulated Ferry</strong><br>En route to Malé...").openPopup();

      let steps = 100;
      let currentStep = 0;
      let latStep = (maleCoords[0] - guraidhooCoords[0]) / steps;
      let lonStep = (maleCoords[1] - guraidhooCoords[1]) / steps;

      let interval = setInterval(() => {
        currentStep++;
        let newLat = guraidhooCoords[0] + (latStep * currentStep);
        let newLon = guraidhooCoords[1] + (lonStep * currentStep);
        simMarker.setLatLng([newLat, newLon]);

        if (currentStep >= steps) {
          clearInterval(interval);
          simMarker.bindPopup("<strong>Simulated Ferry</strong><br>Arrived in Malé!").openPopup();
        }
      }, 40);
    });

    // ========== SCHEDULE DATA & FILTERING ==========
    const schedule = {
      "to-male": [
        { time: "05:55", days: "Sun-Sat", operator: "escape" },
        { time: "07:00", days: "Sun-Sat", operator: "vmarine" },
        { time: "08:00", days: "Sun-Sat", operator: "escape" },
        { time: "08:00", days: "Sun-Sat", operator: "hawks" },
        { time: "11:00", days: "Sun-Sat", operator: "vmarine" },
        { time: "11:10", days: "Sun-Sat", operator: "escape" },
        { time: "13:00", days: "Sat-Thur", operator: "vmarine" },
        { time: "14:30", days: "Sun-Sat", operator: "escape" },
        { time: "14:30", days: "Sun-Sat", operator: "hawks" },
        { time: "16:30", days: "Sun-Sat", operator: "vmarine" },
        { time: "17:30", days: "Sun-Sat", operator: "escape" },
        { time: "17:30", days: "Sun-Sat", operator: "hawks" },
        { time: "20:40", days: "Sun-Sat", operator: "escape" },
        { time: "23:15", days: "Thu-Sat", operator: "escape" },
      ],
      "to-guraidhoo": [
        { time: "06:30", days: "Fri-Sun", operator: "escape" },
        { time: "09:30", days: "Fri", operator: "escape" },
        { time: "09:30", days: "Sun-Sat", operator: "vmarine" },
        { time: "09:45", days: "Sat-Thur", operator: "escape" },
        { time: "09:45", days: "Fri", operator: "hawks" },
        { time: "12:00", days: "Sat-Thur", operator: "vmarine" },
        { time: "12:30", days: "Sat-Thur", operator: "escape" },
        { time: "12:30", days: "Sat-Thur", operator: "hawks" },
        { time: "14:30", days: "Sun-Sat", operator: "vmarine" },
        { time: "16:00", days: "Sun-Sat", operator: "escape" },
        { time: "16:00", days: "Sun-Sat", operator: "hawks" },
        { time: "18:30", days: "Sun-Sat", operator: "vmarine" },
        { time: "19:30", days: "Sun-Sat", operator: "escape" },
        { time: "22:00", days: "Sun-Sat", operator: "escape" },
        { time: "22:00", days: "Sun-Sat", operator: "hawks" },
        { time: "22:30", days: "Sun-Sat", operator: "escape" },
      ]
    };

    const operatorNames = { escape: "Escape", vmarine: "V Marine", hawks: "Hawks" };

    function daysInclude(dayRange, day) {
      if (day === "all") return true;
      const map = {
        "Sun-Sat": ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
        "Sat-Thur": ["Sat", "Sun", "Mon", "Tue", "Wed", "Thu"],
        "Thu-Sat": ["Thu", "Fri", "Sat"],
        "Fri-Sun": ["Fri", "Sat", "Sun"],
        "Fri": ["Fri"],
      };
      return (map[dayRange] || [dayRange]).includes(day);
    }

    function isDaily(days) { return days === "Sun-Sat"; }

    function formatDays(days) {
      const labels = {
        "Sun-Sat": "Daily", "Sat-Thur": "Sat – Thu",
        "Thu-Sat": "Thu – Sat", "Fri-Sun": "Fri – Sun", "Fri": "Friday only"
      };
      return labels[days] || days;
    }

    function getCurrentDayAndTime() {
      const now = new Date();
      const utc = now.getTime() + (now.getTimezoneOffset() * 60000);
      const maldives = new Date(utc + (5 * 3600000));
      const days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
      return { day: days[maldives.getDay()], time: String(maldives.getHours()).padStart(2, "0") + ":" + String(maldives.getMinutes()).padStart(2, "0") };
    }

    function render() {
      const direction = document.getElementById("direction").value;
      const operator = document.getElementById("operator").value;
      const searchQuery = document.getElementById("search-input").value.toLowerCase();
      const activeDayBtn = document.querySelector(".day-btn.active");
      let day = activeDayBtn ? activeDayBtn.dataset.day : "all";

      const { day: today, time: nowTime } = getCurrentDayAndTime();
      
      let nextFerries = {
        "to-male": [],
        "to-guraidhoo": []
      };

      ["to-male", "to-guraidhoo"].forEach(dir => {
        if (day === "all" || day === today) {
          const sortedDirection = [...schedule[dir]].sort((a, b) => a.time.localeCompare(b.time));
          let minTime = null;
          
          for (const item of sortedDirection) {
            if (item.time > nowTime && daysInclude(item.days, today)) {
              if (!minTime) {
                minTime = item.time;
              }
              if (item.time === minTime) {
                nextFerries[dir].push(item);
              }
            }
          }
        }
      });

      ["to-male", "to-guraidhoo"].forEach(dir => {
        const section = document.getElementById(`${dir}-section`);
        const container = document.getElementById(`${dir}-cards`);
        const countBadge = document.getElementById(`${dir}-count`);

        if (direction !== "all" && direction !== dir) {
          section.style.display = "none";
          return;
        }
        section.style.display = "block";

        let items = schedule[dir].filter(item => {
          if (operator !== "all" && item.operator !== operator) return false;
          if (!daysInclude(item.days, day)) return false;
          if (searchQuery && !item.time.includes(searchQuery) && !operatorNames[item.operator].toLowerCase().includes(searchQuery)) return false;
          return true;
        });

        items.sort((a, b) => a.time.localeCompare(b.time));
        countBadge.textContent = `${items.length} ${items.length === 1 ? 'ferry' : 'ferries'}`;

        if (items.length === 0) {
          container.innerHTML = `
            <div class="empty-state">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <circle cx="12" cy="12" r="10"/><path d="M12 8v4M12 16h.01"/>
              </svg>
              <p>No ferries match your filter criteria</p>
              <button class="reset-filter-btn" onclick="resetFilters()">Reset Filters</button>
            </div>
          `;
        } else {
          container.innerHTML = items.map(item => {
            const isNext = nextFerries[dir].some(nf => nf.time === item.time && nf.operator === item.operator);
            let iconMarkup = `<img src="${item.operator}-logo.jpg" alt="${operatorNames[item.operator]}" />`;
            return `
              <div class="card ${item.operator}${isNext ? ' next-ferry' : ''}">
                <div class="card-header">
                  <div class="operator">${iconMarkup}${operatorNames[item.operator]}${isNext ? '<span class="next-badge">Next</span>' : ''}</div>
                  <div class="time">${item.time}</div>
                </div>
                <div class="card-body">
                  <span class="days-badge ${isDaily(item.days) ? 'daily' : ''}">${formatDays(item.days)}</span>
                </div>
              </div>
            `;
          }).join("");
        }
      });

      const banner = document.getElementById("next-ferry-banner");
      let bannerContent = "";

      if (nextFerries["to-male"].length > 0) {
        const timeVal = nextFerries["to-male"][0].time;
        const opNames = nextFerries["to-male"].map(nf => operatorNames[nf.operator]).join(" & ");
        bannerContent += `
          <div class="banner-col">
            <div class="label">Next: Guraidhoo → Male</div>
            <div class="details">${timeVal} · ${opNames}</div>
          </div>
        `;
      }
      if (nextFerries["to-guraidhoo"].length > 0) {
        const timeVal = nextFerries["to-guraidhoo"][0].time;
        const opNames = nextFerries["to-guraidhoo"].map(nf => operatorNames[nf.operator]).join(" & ");
        bannerContent += `
          <div class="banner-col">
            <div class="label">Next: Male → Guraidhoo</div>
            <div class="details">${timeVal} · ${opNames}</div>
          </div>
        `;
      }

      if (bannerContent) {
        banner.classList.remove("hidden");
        banner.innerHTML = bannerContent;
      } else {
        banner.classList.add("hidden");
      }
    }

    function resetFilters() {
      document.getElementById("direction").value = "all";
      document.getElementById("operator").value = "all";
      document.getElementById("search-input").value = "";
      document.querySelectorAll(".day-btn").forEach(b => b.classList.remove("active"));
      document.querySelector(".day-btn.all-btn").classList.add("active");
      render();
    }

    document.getElementById("direction").addEventListener("change", render);
    document.getElementById("operator").addEventListener("change", render);
    document.getElementById("search-input").addEventListener("input", render);
    document.getElementById("day-btns").addEventListener("click", (e) => {
      if (e.target.classList.contains("day-btn")) {
        document.querySelectorAll(".day-btn").forEach(b => b.classList.remove("active"));
        e.target.classList.add("active");
        render();
      }
    });

    render();
  </script>
</body>
</html>
