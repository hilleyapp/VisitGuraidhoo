
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Ferry Schedule • Guraidhoo ↔ Male</title>
  <!-- Leaflet CSS -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
        integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY="
        crossorigin="" />
  <style>
    :root {
      --primary: #0B3D5C;
      --accent: #1A6B9A;
      --escape: #1E88E5;
      --vmarine: #00897B;
      --hawks: #E65100;
      --bg: #F4F8FB;
      --card: #FFFFFF;
      --text: #1A2A3A;
      --muted: #5A6A7A;
      --border: #D0DCE8;
      --shadow: 0 4px 20px rgba(11, 61, 92, 0.08);
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
      background: var(--bg);
      color: var(--text);
      line-height: 1.5;
      min-height: 100vh;
    }

    /* Header */
    header {
      background: linear-gradient(135deg, var(--primary) 0%, var(--accent) 100%);
      color: white;
      padding: 1.8rem 1.5rem 1.4rem;
      text-align: center;
      box-shadow: 0 4px 16px rgba(0,0,0,0.15);
    }
    header h1 {
      font-size: 1.75rem;
      font-weight: 700;
      letter-spacing: 0.5px;
      margin-bottom: 0.25rem;
    }
    header p {
      font-size: 1.05rem;
      opacity: 0.9;
      font-weight: 400;
    }

    /* Controls */
    .controls {
      max-width: 960px;
      margin: -1.2rem auto 0;
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
    select, .day-btns {
      display: flex;
      gap: 0.35rem;
      flex-wrap: wrap;
    }
    select {
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
    select:focus {
      outline: none;
      border-color: var(--accent);
      box-shadow: 0 0 0 3px rgba(26, 107, 154, 0.15);
    }

    .day-btn {
      border: 1.5px solid var(--border);
      background: white;
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


    /* Live Tracking */
    .live-tracking {
      margin: 1.4rem 0 0.5rem;
      background: var(--card);
      border-radius: 14px;
      box-shadow: var(--shadow);
      padding: 1.2rem 1.3rem;
      border: 1.5px solid #E3F2FD;
    }
    .live-tracking h2 {
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
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 0.7rem;
    }
    .live-btn {
      display: flex;
      align-items: center;
      gap: 0.7rem;
      padding: 0.75rem 1rem;
      border-radius: 10px;
      text-decoration: none;
      font-weight: 600;
      font-size: 0.9rem;
      transition: transform 0.15s, box-shadow 0.15s;
      border: 1.5px solid transparent;
    }
    .live-btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 14px rgba(0,0,0,0.1);
    }
    .live-btn.escape {
      background: #E3F2FD;
      color: var(--escape);
      border-color: #90CAF9;
    }
    .live-btn.vmarine {
      background: #E0F2F1;
      color: var(--vmarine);
      border-color: #80CBC4;
    }
    .live-btn.hawks {
      background: #FFF3E0;
      color: var(--hawks);
      border-color: #FFCC80;
    }
    .live-btn.all {
      background: #F3E5F5;
      color: #7B1FA2;
      border-color: #CE93D8;
    }
    .live-btn .icon {
      width: 32px;
      height: 32px;
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-size: 0.8rem;
      font-weight: 700;
      flex-shrink: 0;
    }
    .live-btn.escape .icon { background: var(--escape); }
    .live-btn.vmarine .icon { background: var(--vmarine); }
    .live-btn.hawks .icon { background: var(--hawks); }
    .live-btn.all .icon { background: #7B1FA2; }
    .live-btn small {
      display: block;
      font-weight: 400;
      font-size: 0.72rem;
      opacity: 0.8;
      margin-top: 1px;
    }
    .live-note {
      text-align: center;
      font-size: 0.75rem;
      color: var(--muted);
      margin-top: 0.85rem;
      line-height: 1.4;
    }
    .live-note a {
      color: var(--accent);
      text-decoration: none;
    }
    .live-note a:hover { text-decoration: underline; }

    .section-title {
      display: flex;
      align-items: center;
      gap: 0.6rem;
      margin: 1.5rem 0 0.9rem;
      font-size: 1.05rem;
      font-weight: 700;
      color: var(--primary);
    }
    .section-title .arrow {
      background: var(--primary);
      color: white;
      font-size: 0.75rem;
      padding: 0.2rem 0.55rem;
      border-radius: 6px;
      font-weight: 600;
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
      gap: 0.4rem;
    }
    .operator .dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
    }
    .card.escape .operator { color: var(--escape); }
    .card.vmarine .operator { color: var(--vmarine); }
    .card.hawks .operator { color: var(--hawks); }
    .card.escape .dot { background: var(--escape); }
    .card.vmarine .dot { background: var(--vmarine); }
    .card.hawks .dot { background: var(--hawks); }

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
    }
    .days-badge.daily {
      background: #E8F5E9;
      color: #2E7D32;
    }
    .card.next-ferry {
      border-top-width: 5px;
      box-shadow: 0 0 0 2px #FF6F00, 0 8px 28px rgba(255, 111, 0, 0.2);
      position: relative;
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
      animation: pulse-badge 1.6s ease-in-out infinite;
    }
    @keyframes pulse-badge {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.7; }
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
      gap: 0.5rem;
      box-shadow: 0 4px 16px rgba(255, 111, 0, 0.25);
    }
    .next-ferry-banner .label {
      font-size: 0.75rem;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.6px;
      opacity: 0.9;
    }
    .next-ferry-banner .details {
      font-size: 1.1rem;
      font-weight: 700;
    }
    .next-ferry-banner .details span {
      opacity: 0.9;
      font-weight: 500;
      font-size: 0.95rem;
    }
    .next-ferry-banner.hidden { display: none; }

    .empty-state {
      text-align: center;
      padding: 3rem 1rem;
      color: var(--muted);
      grid-column: 1 / -1;
    }
    .empty-state svg {
      width: 48px;
      height: 48px;
      margin-bottom: 0.8rem;
      opacity: 0.4;
    }

    /* Contacts */
    .contacts {
      margin-top: 2.5rem;
      background: var(--card);
      border-radius: 14px;
      box-shadow: var(--shadow);
      padding: 1.3rem;
    }
    .contacts h3 {
      font-size: 0.85rem;
      text-transform: uppercase;
      letter-spacing: 0.8px;
      color: var(--muted);
      margin-bottom: 1rem;
      text-align: center;
    }
    .contact-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 0.8rem;
    }
    .contact-item {
      display: flex;
      align-items: center;
      gap: 0.7rem;
      padding: 0.7rem 0.9rem;
      border-radius: 10px;
      text-decoration: none;
      transition: background 0.15s;
    }
    .contact-item:hover { background: var(--bg); }
    .contact-item .icon {
      width: 36px;
      height: 36px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-size: 0.85rem;
      font-weight: 700;
      flex-shrink: 0;
    }
    .contact-item.escape .icon { background: var(--escape); }
    .contact-item.vmarine .icon { background: var(--vmarine); }
    .contact-item.hawks .icon { background: var(--hawks); }
    .contact-item .info strong {
      display: block;
      font-size: 0.9rem;
      color: var(--text);
    }
    .contact-item .info span {
      font-size: 0.85rem;
      color: var(--accent);
      font-weight: 500;
    }

    footer {
      text-align: center;
      padding: 1.5rem 1rem 2rem;
      font-size: 0.78rem;
      color: var(--muted);
    }

    /* Leaflet marker customization */
    .custom-marker {
      background: white;
      border-radius: 50%;
      border: 3px solid;
      width: 18px !important;
      height: 18px !important;
      margin-left: -9px !important;
      margin-top: -9px !important;
      box-shadow: 0 2px 6px rgba(0,0,0,0.3);
    }

    /* Responsive */
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
    <h1>Ferry Schedule</h1>
    <p>Guraidhoo ↔ Male</p>
  </header>

  <div class="controls">
    <div class="controls-inner">
      <div class="control-group">
        <label>Direction</label>
        <select id="direction">
          <option value="all">Both Directions</option>
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
      <div class="map-info">
        <span><span class="dot male"></span> Malé</span>
        <span><span class="dot guraidhoo"></span> Guraidhoo</span>
        <span><span class="dot route"></span> Ferry Route (~32 km)</span>
      </div>
    </div>

    <!-- Live Tracking via FollowMe -->
    <div class="live-tracking">
      <h2><span class="live-dot"></span> Live Tracking (FollowMe)</h2>
      <div class="live-grid">
        <a class="live-btn escape" href="https://m.followme.mv/public/?s=ESCAPE" target="_blank" rel="noopener">
          <div class="icon">E</div>
          <div>
            Escape Ferries
            <small>Queen · Wing · King</small>
          </div>
        </a>
        <a class="live-btn vmarine" href="https://m.followme.mv/public/?s=V%20Marine" target="_blank" rel="noopener">
          <div class="icon">V</div>
          <div>
            V Marine
            <small>V Marine 1 · 2 · 3</small>
          </div>
        </a>
        <a class="live-btn hawks" href="https://m.followme.mv/public/?s=HAWKS" target="_blank" rel="noopener">
          <div class="icon">H</div>
          <div>
            Hawks Ferries
            <small>Daisy · Ivy</small>
          </div>
        </a>
        <a class="live-btn all" href="https://m.followme.mv/public/" target="_blank" rel="noopener">
          <div class="icon">✈</div>
          <div>
            All Public Vessels
            <small>Full FollowMe map</small>
          </div>
        </a>
      </div>
      <p class="live-note">
        Real-time positions powered by <a href="https://followme.mv" target="_blank" rel="noopener">FollowMe Maldives</a>.
        Search for boat names (e.g. ESCAPE QUEEN) on the public map. Opens in a new tab.
      </p>
    </div>

    <div id="to-male-section">
      <div class="section-title">
        <span class="arrow">→</span> Guraidhoo → Male
      </div>
      <div class="cards" id="to-male-cards"></div>
    </div>

    <div id="to-guraidhoo-section">
      <div class="section-title">
        <span class="arrow">→</span> Male → Guraidhoo
      </div>
      <div class="cards" id="to-guraidhoo-cards"></div>
    </div>

    <div class="contacts">
      <h3>Contact Operators</h3>
      <div class="contact-grid">
        <a class="contact-item escape" href="tel:+9607994699">
          <div class="icon">E</div>
          <div class="info">
            <strong>Escape</strong>
            <span>+960 799 4699</span>
          </div>
        </a>
        <a class="contact-item vmarine" href="tel:+9607407777">
          <div class="icon">V</div>
          <div class="info">
            <strong>V Marine</strong>
            <span>+960 740 7777</span>
          </div>
        </a>
        <a class="contact-item hawks" href="tel:+9607216816">
          <div class="icon">H</div>
          <div class="info">
            <strong>Hawks</strong>
            <span>+960 721 6816</span>
          </div>
        </a>
      </div>
    </div>
  </main>

  <footer>
    Schedules may change without notice. Please confirm with the operator before travel.
  </footer>

  <!-- Leaflet JS -->
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
          integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo="
          crossorigin=""></script>

  <script>
    // ========== MAP ==========
    const maleCoords = [4.17521, 73.50916];
    const guraidhooCoords = [3.90045, 73.46623];

    const map = L.map('map', {
      scrollWheelZoom: false,   // prevent accidental zoom while scrolling page
      zoomControl: true
    }).setView([4.04, 73.49], 10);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 18,
      attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a>'
    }).addTo(map);

    // Custom markers
    const maleIcon = L.divIcon({
      className: 'custom-marker',
      html: '',
      iconSize: [18, 18]
    });
    const guraidhooIcon = L.divIcon({
      className: 'custom-marker',
      html: '',
      iconSize: [18, 18]
    });

    // Style the markers via CSS injection after creation
    const maleMarker = L.marker(maleCoords, {
      icon: L.divIcon({
        className: '',
        html: `<div style="background:#E53935;width:16px;height:16px;border-radius:50%;border:3px solid white;box-shadow:0 2px 6px rgba(0,0,0,0.35);"></div>`,
        iconSize: [16, 16],
        iconAnchor: [8, 8]
      })
    }).addTo(map).bindPopup(`
      <strong style="font-size:1rem;">Malé</strong><br>
      Capital of the Maldives<br>
      <small>Lat 4.175°, Lon 73.509°</small>
    `);

    const guraidhooMarker = L.marker(guraidhooCoords, {
      icon: L.divIcon({
        className: '',
        html: `<div style="background:#1E88E5;width:16px;height:16px;border-radius:50%;border:3px solid white;box-shadow:0 2px 6px rgba(0,0,0,0.35);"></div>`,
        iconSize: [16, 16],
        iconAnchor: [8, 8]
      })
    }).addTo(map).bindPopup(`
      <strong style="font-size:1rem;">Guraidhoo</strong><br>
      Kaafu Atoll<br>
      <small>Lat 3.900°, Lon 73.466°</small>
    `);

    // Route line
    const routeLine = L.polyline([guraidhooCoords, maleCoords], {
      color: '#FFB300',
      weight: 4,
      opacity: 0.85,
      dashArray: '8, 8',
      lineCap: 'round'
    }).addTo(map);

    // Fit bounds with some padding
    map.fitBounds([maleCoords, guraidhooCoords], { padding: [40, 40] });

    // Enable scroll zoom only when map is focused/clicked
    map.on('click', () => map.scrollWheelZoom.enable());
    map.on('mouseout', () => map.scrollWheelZoom.disable());

    // ========== SCHEDULE DATA ==========
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

    const operatorNames = {
      escape: "Escape",
      vmarine: "V Marine",
      hawks: "Hawks"
    };

    function daysInclude(dayRange, day) {
      if (day === "all") return true;
      const map = {
        "Sun-Sat": ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
        "Sat-Thur": ["Sat", "Sun", "Mon", "Tue", "Wed", "Thu"],
        "Thu-Sat": ["Thu", "Fri", "Sat"],
        "Fri-Sun": ["Fri", "Sat", "Sun"],
        "Fri": ["Fri"],
      };
      const list = map[dayRange] || [dayRange];
      return list.includes(day);
    }

    function isDaily(days) {
      return days === "Sun-Sat";
    }

    function formatDays(days) {
      const labels = {
        "Sun-Sat": "Daily",
        "Sat-Thur": "Sat – Thu",
        "Thu-Sat": "Thu – Sat",
        "Fri-Sun": "Fri – Sun",
        "Fri": "Friday only",
      };
      return labels[days] || days;
    }

    function getCurrentDayAndTime() {
      // Maldives is UTC+5
      const now = new Date();
      // Adjust to Maldives time (UTC+5)
      const utc = now.getTime() + (now.getTimezoneOffset() * 60000);
      const maldives = new Date(utc + (5 * 3600000));
      const days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
      const day = days[maldives.getDay()];
      const hours = String(maldives.getHours()).padStart(2, "0");
      const mins = String(maldives.getMinutes()).padStart(2, "0");
      return { day, time: hours + ":" + mins, full: maldives };
    }

    function createCard(item, isNext) {
      const daily = isDaily(item.days);
      const nextClass = isNext ? " next-ferry" : "";
      const nextBadge = isNext ? '<span class="next-badge">Next</span>' : "";
      return `
        <div class="card ${item.operator}${nextClass}">
          <div class="card-header">
            <div class="operator">
              <span class="dot"></span>
              ${operatorNames[item.operator]}${nextBadge}
            </div>
            <div class="time">${item.time}</div>
          </div>
          <div class="card-body">
            <span class="days-badge ${daily ? 'daily' : ''}">${formatDays(item.days)}</span>
          </div>
        </div>
      `;
    }

    function findNextFerry(items, currentTime, currentDay) {
      // items already filtered for day and sorted by time
      // Find the first departure that is still upcoming today
      for (const item of items) {
        if (item.time > currentTime && daysInclude(item.days, currentDay)) {
          return item;
        }
      }
      return null; // none left today under current filters
    }

    function render() {
      const direction = document.getElementById("direction").value;
      const operator = document.getElementById("operator").value;
      const activeDayBtn = document.querySelector(".day-btn.active");
      let day = activeDayBtn ? activeDayBtn.dataset.day : "all";

      const { day: today, time: nowTime } = getCurrentDayAndTime();

      // If user selected "All" days, use today's day for next-ferry calculation
      const dayForNext = (day === "all") ? today : day;

      let globalNext = null;
      let globalNextDir = null;

      ["to-male", "to-guraidhoo"].forEach(dir => {
        const section = document.getElementById(`${dir}-section`);
        const container = document.getElementById(`${dir}-cards`);

        if (direction !== "all" && direction !== dir) {
          section.style.display = "none";
          return;
        }
        section.style.display = "block";

        let items = schedule[dir].filter(item => {
          if (operator !== "all" && item.operator !== operator) return false;
          if (!daysInclude(item.days, day)) return false;
          return true;
        });

        items.sort((a, b) => a.time.localeCompare(b.time));

        // Find next ferry for this direction (only if looking at today or all)
        let nextItem = null;
        if (day === "all" || day === today) {
          nextItem = findNextFerry(items, nowTime, today);
          if (nextItem && (!globalNext || nextItem.time < globalNext.time)) {
            globalNext = nextItem;
            globalNextDir = dir;
          }
        }

        if (items.length === 0) {
          container.innerHTML = `
            <div class="empty-state">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <circle cx="12" cy="12" r="10"/>
                <path d="M12 8v4M12 16h.01"/>
              </svg>
              <p>No ferries match your filters</p>
            </div>
          `;
        } else {
          container.innerHTML = items.map(item =>
            createCard(item, nextItem && item.time === nextItem.time && item.operator === nextItem.operator)
          ).join("");
        }
      });

      // Update the next-ferry banner
      const banner = document.getElementById("next-ferry-banner");
      if (banner) {
        if (globalNext) {
          const dirLabel = globalNextDir === "to-male" ? "Guraidhoo → Male" : "Male → Guraidhoo";
          banner.classList.remove("hidden");
          banner.innerHTML = `
            <div>
              <div class="label">Next Ferry · Maldives time</div>
              <div class="details">${globalNext.time} · ${operatorNames[globalNext.operator]} <span>(${dirLabel})</span></div>
            </div>
          `;
        } else {
          banner.classList.add("hidden");
        }
      }
    }

    // Event listeners
    document.getElementById("direction").addEventListener("change", render);
    document.getElementById("operator").addEventListener("change", render);

    document.getElementById("day-btns").addEventListener("click", (e) => {
      if (e.target.classList.contains("day-btn")) {
        document.querySelectorAll(".day-btn").forEach(b => b.classList.remove("active"));
        e.target.classList.add("active");
        render();
      }
    });

    // Initial render
    render();
  </script>
</body>
</html>
