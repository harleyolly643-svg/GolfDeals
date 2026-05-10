<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="Find cheap golf clubs online. AI-powered scanner searches eBay, 2nd Swing, GlobalGolf and more for the best deals.">
<title>GolfDeals — Find Cheap Golf Clubs Online</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;1,300&display=swap" rel="stylesheet">
<style>
  /* ─── TOKENS ─────────────────────────────────────── */
  :root {
    --green:       #1a6b2f;
    --green-mid:   #2d9c4a;
    --green-light: #4dc46a;
    --gold:        #c9a84c;
    --gold-light:  #e8c96a;
    --cream:       #f5f0e8;
    --dark:        #0b160d;
    --dark-mid:    #111d14;
    --surface:     rgba(255,255,255,0.035);
    --border:      rgba(255,255,255,0.08);
    --border-gold: rgba(201,168,76,0.25);
    --text:        #ddeee0;
    --muted:       #6a9672;
    --error:       #e07070;
  }

  /* ─── RESET ──────────────────────────────────────── */
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  html { scroll-behavior: smooth; }

  /* ─── BASE ───────────────────────────────────────── */
  body {
    background: var(--dark);
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
    cursor: default;
  }

  /* animated fairway-stripe background */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    z-index: 0;
    background:
      radial-gradient(ellipse 80% 60% at 15% 10%,  rgba(26,107,47,.28) 0%, transparent 70%),
      radial-gradient(ellipse 60% 50% at 85% 85%,  rgba(201,168,76,.10) 0%, transparent 65%),
      repeating-linear-gradient(160deg,
        transparent 0px, transparent 80px,
        rgba(255,255,255,.012) 80px, rgba(255,255,255,.012) 81px);
    pointer-events: none;
  }

  /* grain overlay */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    z-index: 0;
    opacity: .04;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
    background-size: 180px 180px;
    pointer-events: none;
  }

  .wrap {
    position: relative;
    z-index: 1;
    max-width: 940px;
    margin: 0 auto;
    padding: 0 24px;
  }

  /* ─── HEADER ─────────────────────────────────────── */
  header {
    padding: 56px 0 28px;
    text-align: center;
  }

  .logo-flag {
    font-size: 52px;
    display: block;
    margin-bottom: 10px;
    filter: drop-shadow(0 0 20px rgba(201,168,76,.55));
    animation: flagWave .6s ease both;
  }

  @keyframes flagWave {
    from { transform: scale(.7) rotate(-8deg); opacity: 0; }
    to   { transform: scale(1)  rotate(0deg);  opacity: 1; }
  }

  h1 {
    font-family: 'Bebas Neue', sans-serif;
    font-size: clamp(64px, 12vw, 110px);
    letter-spacing: 6px;
    line-height: .88;
    background: linear-gradient(140deg, var(--gold-light) 0%, var(--gold) 45%, var(--green-light) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    animation: riseIn .7s .1s ease both;
  }

  .sub {
    margin-top: 12px;
    font-size: 12px;
    letter-spacing: 4px;
    text-transform: uppercase;
    color: var(--muted);
    font-weight: 300;
    animation: riseIn .7s .2s ease both;
  }

  @keyframes riseIn {
    from { opacity: 0; transform: translateY(14px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  /* ─── SEARCH PANEL ───────────────────────────────── */
  .panel {
    background: var(--surface);
    border: 1px solid var(--border-gold);
    border-radius: 20px;
    padding: 32px 36px;
    margin: 32px 0 20px;
    backdrop-filter: blur(14px);
    animation: riseIn .7s .3s ease both;
  }

  .panel-label {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 18px;
    letter-spacing: 2.5px;
    color: var(--gold);
    margin-bottom: 16px;
  }

  /* chips */
  .chips {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 22px;
  }

  .chip {
    background: rgba(26,107,47,.22);
    border: 1px solid rgba(45,156,74,.35);
    border-radius: 24px;
    padding: 5px 15px;
    font-size: 13px;
    color: var(--text);
    cursor: pointer;
    transition: background .18s, border-color .18s, transform .15s;
    user-select: none;
  }

  .chip:hover { background: rgba(26,107,47,.45); transform: translateY(-1px); }
  .chip.on    { background: var(--green-mid); border-color: var(--green-light); color: #fff; }

  /* input row */
  .input-row {
    display: flex;
    gap: 12px;
  }

  .s-input {
    flex: 1;
    background: rgba(0,0,0,.35);
    border: 1px solid var(--border-gold);
    border-radius: 12px;
    padding: 14px 20px;
    font-family: 'DM Sans', sans-serif;
    font-size: 15px;
    color: var(--text);
    outline: none;
    transition: border-color .2s, box-shadow .2s;
  }

  .s-input::placeholder { color: var(--muted); }

  .s-input:focus {
    border-color: var(--gold);
    box-shadow: 0 0 0 3px rgba(201,168,76,.12);
  }

  .s-btn {
    background: linear-gradient(135deg, var(--gold) 0%, var(--gold-light) 100%);
    color: var(--dark);
    border: none;
    border-radius: 12px;
    padding: 14px 30px;
    font-family: 'Bebas Neue', sans-serif;
    font-size: 19px;
    letter-spacing: 1.5px;
    cursor: pointer;
    transition: transform .18s, box-shadow .18s, opacity .18s;
    white-space: nowrap;
    flex-shrink: 0;
  }

  .s-btn:hover:not(:disabled) {
    transform: translateY(-2px);
    box-shadow: 0 10px 28px rgba(201,168,76,.35);
  }

  .s-btn:disabled { opacity: .55; cursor: not-allowed; transform: none; }

  /* filters */
  .filters {
    display: flex;
    gap: 10px;
    margin-top: 14px;
    flex-wrap: wrap;
  }

  .f-select {
    background: rgba(0,0,0,.3);
    border: 1px solid var(--border);
    border-radius: 9px;
    padding: 8px 14px;
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-size: 13px;
    outline: none;
    cursor: pointer;
    transition: border-color .2s;
  }

  .f-select:focus { border-color: var(--gold); }
  .f-select option { background: #111d14; }

  /* ─── DISCLAIMER ─────────────────────────────────── */
  .disclaimer {
    background: rgba(201,168,76,.07);
    border: 1px solid var(--border-gold);
    border-radius: 11px;
    padding: 13px 18px;
    font-size: 12px;
    color: var(--muted);
    line-height: 1.7;
    margin-bottom: 24px;
    animation: riseIn .7s .4s ease both;
  }

  /* ─── ERROR ──────────────────────────────────────── */
  .err {
    background: rgba(180,40,40,.14);
    border: 1px solid rgba(220,60,60,.3);
    border-radius: 11px;
    padding: 16px 20px;
    color: var(--error);
    font-size: 14px;
    line-height: 1.6;
    display: none;
    margin-bottom: 20px;
  }
  .err.show { display: block; }

  /* ─── PAYWALL OVERLAY ───────────────────────────────── */
  #paywall {
    position: fixed;
    inset: 0;
    z-index: 999;
    background: rgba(11,22,13,.92);
    backdrop-filter: blur(18px);
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 24px;
    animation: riseIn .5s ease both;
  }

  #paywall.hidden { display: none; }

  .pw-box {
    background: linear-gradient(160deg, rgba(26,107,47,.18) 0%, rgba(11,22,13,.95) 100%);
    border: 1px solid var(--border-gold);
    border-radius: 24px;
    padding: 48px 44px;
    max-width: 480px;
    width: 100%;
    text-align: center;
    box-shadow: 0 32px 80px rgba(0,0,0,.6);
  }

  .pw-icon {
    font-size: 52px;
    display: block;
    margin-bottom: 16px;
    filter: drop-shadow(0 0 16px rgba(201,168,76,.5));
  }

  .pw-title {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 42px;
    letter-spacing: 4px;
    background: linear-gradient(135deg, var(--gold-light), var(--gold));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    line-height: 1;
    margin-bottom: 10px;
  }

  .pw-sub {
    font-size: 13px;
    color: var(--muted);
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-bottom: 28px;
  }

  .pw-perks {
    list-style: none;
    margin-bottom: 32px;
    text-align: left;
    display: inline-block;
  }

  .pw-perks li {
    font-size: 14px;
    color: var(--text);
    padding: 5px 0;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .pw-perks li::before {
    content: '✓';
    color: var(--green-light);
    font-weight: 700;
    font-size: 15px;
    flex-shrink: 0;
  }

  .pw-price {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 52px;
    letter-spacing: 2px;
    color: #fff;
    line-height: 1;
  }

  .pw-price span {
    font-family: 'DM Sans', sans-serif;
    font-size: 16px;
    color: var(--muted);
    font-weight: 300;
    letter-spacing: 1px;
  }

  .pw-cta {
    display: block;
    width: 100%;
    margin-top: 20px;
    background: linear-gradient(135deg, var(--gold) 0%, var(--gold-light) 100%);
    color: var(--dark);
    text-decoration: none;
    border: none;
    border-radius: 13px;
    padding: 16px 28px;
    font-family: 'Bebas Neue', sans-serif;
    font-size: 22px;
    letter-spacing: 2px;
    cursor: pointer;
    transition: transform .18s, box-shadow .18s;
  }

  .pw-cta:hover {
    transform: translateY(-2px);
    box-shadow: 0 12px 32px rgba(201,168,76,.4);
  }

  .pw-divider {
    display: flex;
    align-items: center;
    gap: 12px;
    margin: 20px 0;
    color: var(--muted);
    font-size: 12px;
    letter-spacing: 1px;
  }

  .pw-divider::before, .pw-divider::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  .pw-owner-row {
    display: flex;
    gap: 8px;
  }

  .pw-code-input {
    flex: 1;
    background: rgba(0,0,0,.4);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 11px 16px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    color: var(--text);
    outline: none;
    transition: border-color .2s;
    letter-spacing: 2px;
  }

  .pw-code-input::placeholder { color: var(--muted); letter-spacing: 1px; }
  .pw-code-input:focus { border-color: var(--green-light); }

  .pw-code-btn {
    background: var(--green);
    border: 1px solid var(--green-mid);
    border-radius: 10px;
    padding: 11px 18px;
    color: #fff;
    font-family: 'DM Sans', sans-serif;
    font-size: 13px;
    font-weight: 500;
    cursor: pointer;
    transition: background .18s;
    white-space: nowrap;
  }

  .pw-code-btn:hover { background: var(--green-mid); }

  .pw-code-err {
    font-size: 12px;
    color: var(--error);
    margin-top: 8px;
    min-height: 16px;
  }

  .pw-cancel {
    font-size: 11px;
    color: var(--muted);
    margin-top: 18px;
    letter-spacing: .5px;
  }

  @media (max-width: 520px) {
    .pw-box { padding: 36px 24px; }
    .pw-title { font-size: 34px; }
  }

  /* ─── LOADER ─────────────────────────────────────── */
  .loader {
    text-align: center;
    padding: 64px 20px;
    display: none;
  }
  .loader.show { display: block; }

  .ball {
    width: 52px;
    height: 52px;
    margin: 0 auto 22px;
    border-radius: 50%;
    background: radial-gradient(circle at 36% 33%, #fff 0%, #ccc 100%);
    box-shadow: inset -4px -4px 8px rgba(0,0,0,.2), 0 0 0 2px rgba(255,255,255,.08);
    animation: ballBounce .75s ease-in-out infinite alternate;
    position: relative;
    overflow: hidden;
  }

  .ball::after {
    content: '';
    position: absolute;
    inset: 5px;
    border-radius: 50%;
    background: repeating-conic-gradient(rgba(0,0,0,.07) 0deg 18deg, transparent 18deg 36deg);
  }

  @keyframes ballBounce {
    from { transform: translateY(0)   scaleX(1)    scaleY(1); }
    to   { transform: translateY(-22px) scaleX(.96) scaleY(1.04); }
  }

  .loader-label {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 20px;
    letter-spacing: 3px;
    color: var(--gold);
    margin-bottom: 16px;
  }

  .loader-steps { display: flex; flex-direction: column; gap: 7px; align-items: center; }

  .l-step {
    font-size: 13px;
    color: var(--muted);
    opacity: 0;
    animation: fadeUp .5s forwards;
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(6px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  /* ─── RESULTS ────────────────────────────────────── */
  .results-hdr {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
    flex-wrap: wrap;
    gap: 8px;
  }

  .results-count {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 22px;
    letter-spacing: 2px;
    color: var(--gold);
  }

  .live-badge {
    display: flex;
    align-items: center;
    gap: 6px;
    font-size: 11px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: var(--muted);
  }

  .live-dot {
    width: 7px; height: 7px;
    border-radius: 50%;
    background: var(--green-light);
    animation: pulse 1.4s ease-in-out infinite;
  }

  @keyframes pulse {
    0%,100% { opacity: 1; transform: scale(1); }
    50%      { opacity: .4; transform: scale(.7); }
  }

  /* deal card */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 26px 28px;
    margin-bottom: 16px;
    transition: border-color .2s, background .2s, transform .2s, box-shadow .2s;
    opacity: 0;
    transform: translateY(18px);
    animation: riseIn .45s ease forwards;
  }

  .card:hover {
    border-color: var(--border-gold);
    background: rgba(255,255,255,.055);
    transform: translateY(-3px);
    box-shadow: 0 16px 40px rgba(0,0,0,.35);
  }

  .card:nth-child(1) { animation-delay: .04s; }
  .card:nth-child(2) { animation-delay: .09s; }
  .card:nth-child(3) { animation-delay: .14s; }
  .card:nth-child(4) { animation-delay: .19s; }
  .card:nth-child(5) { animation-delay: .24s; }
  .card:nth-child(6) { animation-delay: .29s; }
  .card:nth-child(7) { animation-delay: .34s; }
  .card:nth-child(8) { animation-delay: .39s; }

  .card-top {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    gap: 16px;
    flex-wrap: wrap;
  }

  .card-name {
    font-size: 17px;
    font-weight: 600;
    color: #fff;
    line-height: 1.35;
    flex: 1;
    min-width: 200px;
  }

  .price-block { text-align: right; flex-shrink: 0; }

  .price-now {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 30px;
    letter-spacing: 1px;
    color: var(--gold-light);
    line-height: 1;
  }

  .price-now.unknown {
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    font-weight: 400;
    color: var(--muted);
  }

  .price-was {
    font-size: 12px;
    color: var(--muted);
    text-decoration: line-through;
    margin-top: 2px;
  }

  .tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-top: 12px;
    align-items: center;
  }

  .tag {
    font-size: 11px;
    letter-spacing: 1px;
    text-transform: uppercase;
    padding: 3px 11px;
    border-radius: 20px;
    font-weight: 500;
  }

  .tag-src  { background: rgba(26,107,47,.28);  border: 1px solid rgba(77,196,106,.3); color: #7de09a; }
  .tag-cond { background: rgba(201,168,76,.14); border: 1px solid rgba(201,168,76,.3); color: var(--gold-light); }
  .tag-type { background: rgba(255,255,255,.06); border: 1px solid var(--border);      color: var(--muted); }

  .card-desc {
    margin-top: 13px;
    font-size: 14px;
    color: rgba(255,255,255,.55);
    line-height: 1.65;
    font-style: italic;
  }

  .card-link {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    margin-top: 16px;
    background: linear-gradient(130deg, var(--green) 0%, var(--green-mid) 100%);
    color: #fff;
    text-decoration: none;
    padding: 11px 22px;
    border-radius: 9px;
    font-size: 13px;
    font-weight: 500;
    letter-spacing: .3px;
    transition: transform .18s, box-shadow .18s;
  }

  .card-link:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 22px rgba(45,156,74,.32);
  }

  /* ─── EMPTY ──────────────────────────────────────── */
  .empty {
    text-align: center;
    padding: 64px 20px;
    color: var(--muted);
  }

  .empty-icon { font-size: 52px; margin-bottom: 18px; }
  .empty h3   { font-family: 'Bebas Neue', sans-serif; font-size: 24px; letter-spacing: 2px; color: var(--gold); margin-bottom: 8px; }
  .empty p    { font-size: 14px; line-height: 1.7; }

  /* ─── FOOTER ─────────────────────────────────────── */
  footer {
    text-align: center;
    padding: 44px 0 32px;
    font-size: 11px;
    letter-spacing: 2px;
    color: rgba(255,255,255,.18);
    text-transform: uppercase;
  }

  footer a { color: rgba(255,255,255,.28); text-decoration: none; }
  footer a:hover { color: var(--gold); }

  /* ─── RESPONSIVE ─────────────────────────────────── */
  @media (max-width: 620px) {
    .panel         { padding: 24px 20px; }
    .input-row     { flex-direction: column; }
    .card-top      { flex-direction: column; }
    .price-block   { text-align: left; }
  }
</style>
</head>
<body>

<!-- ═══════════════════════════════════════════════
     PAYWALL OVERLAY
     To unlock as owner: type your secret code below
════════════════════════════════════════════════ -->
<div id="paywall">
  <div class="pw-box">
    <span class="pw-icon">⛳</span>
    <div class="pw-title">GOLF DEALS</div>
    <p class="pw-sub">AI-Powered Deal Scanner</p>

    <ul class="pw-perks">
      <li>Live AI search across 8+ golf retailers</li>
      <li>Find used, clearance &amp; refurbished clubs</li>
      <li>Filter by brand, type &amp; condition</li>
      <li>New deals every search — always fresh</li>
    </ul>

    <div class="pw-price">$2.99 <span>/ month</span></div>

    <a id="stripeBtn" class="pw-cta" href="YOUR_STRIPE_LINK_HERE" target="_blank">
      SUBSCRIBE &amp; GET ACCESS
    </a>

    <div class="pw-divider">already subscribed / owner?</div>

    <div class="pw-owner-row">
      <input
        id="codeInput"
        class="pw-code-input"
        type="password"
        placeholder="Enter access code"
        autocomplete="off"
      />
      <button class="pw-code-btn" onclick="checkCode()">UNLOCK</button>
    </div>
    <div class="pw-code-err" id="codeErr"></div>

    <p class="pw-cancel">Cancel anytime &nbsp;·&nbsp; Secure payment via Stripe</p>
  </div>
</div>

<div class="wrap">

  <!-- HEADER -->
  <header>
    <span class="logo-flag">⛳</span>
    <h1>GOLF<br>DEALS</h1>
    <p class="sub">AI-powered golf club deal scanner</p>
  </header>

  <!-- SEARCH PANEL -->
  <div class="panel">
    <p class="panel-label">⚡ Quick Keywords</p>

    <div class="chips" id="chips">
      <span class="chip" data-kw="used driver cheap">Used Driver</span>
      <span class="chip" data-kw="cheap irons set">Cheap Irons</span>
      <span class="chip" data-kw="discount wedge">Discount Wedge</span>
      <span class="chip" data-kw="titleist used clubs">Titleist Used</span>
      <span class="chip" data-kw="callaway clearance sale">Callaway Clearance</span>
      <span class="chip" data-kw="taylormade deals">TaylorMade Deals</span>
      <span class="chip" data-kw="ping iron set cheap">Ping Irons</span>
      <span class="chip" data-kw="beginner golf clubs complete set">Beginner Set</span>
      <span class="chip" data-kw="refurbished putter">Refurbished Putter</span>
      <span class="chip" data-kw="golf club sale">General Sale</span>
      <span class="chip" data-kw="cleveland wedge discount">Cleveland Wedge</span>
      <span class="chip" data-kw="cobra fairway wood cheap">Cobra Fairway</span>
    </div>

    <div class="input-row">
      <input
        id="query"
        class="s-input"
        type="text"
        placeholder="e.g.  used Titleist irons under $200 …"
        autocomplete="off"
      />
      <button id="searchBtn" class="s-btn" onclick="runSearch()">FIND DEALS</button>
    </div>

    <div class="filters">
      <select id="fCond" class="f-select">
        <option value="">Any Condition</option>
        <option value="used">Used / Pre-owned</option>
        <option value="new">New</option>
        <option value="refurbished">Refurbished</option>
        <option value="clearance">Clearance</option>
      </select>
      <select id="fType" class="f-select">
        <option value="">Any Club Type</option>
        <option value="driver">Driver</option>
        <option value="irons">Irons</option>
        <option value="wedge">Wedge</option>
        <option value="putter">Putter</option>
        <option value="hybrid">Hybrid</option>
        <option value="fairway wood">Fairway Wood</option>
        <option value="complete set">Complete Set</option>
      </select>
      <select id="fBrand" class="f-select">
        <option value="">Any Brand</option>
        <option value="Titleist">Titleist</option>
        <option value="Callaway">Callaway</option>
        <option value="TaylorMade">TaylorMade</option>
        <option value="Ping">Ping</option>
        <option value="Cleveland">Cleveland</option>
        <option value="Cobra">Cobra</option>
        <option value="Mizuno">Mizuno</option>
        <option value="Wilson">Wilson</option>
      </select>
    </div>
  </div>

  <!-- DISCLAIMER -->
  <div class="disclaimer">
    🔍 This tool uses Claude AI + live web search to find golf club deals across eBay, 2nd Swing, GlobalGolf, Rock Bottom Golf, Callaway Pre-Owned, and more.
    Always verify prices and availability directly on the seller's site before purchasing. Prices change frequently.
  </div>

  <!-- ERROR -->
  <div class="err" id="errBox"></div>

  <!-- LOADER -->
  <div class="loader" id="loader">
    <div class="ball"></div>
    <div class="loader-label">Scanning the web…</div>
    <div class="loader-steps" id="loaderSteps"></div>
  </div>

  <!-- RESULTS -->
  <div id="results"></div>

</div><!-- /wrap -->

<footer>
  GolfDeals &nbsp;·&nbsp; Powered by <a href="https://anthropic.com" target="_blank">Claude AI</a>
  &nbsp;·&nbsp; Prices subject to change &nbsp;·&nbsp; Not affiliated with any retailer
</footer>

<script>
/* ─────────────────────────────────────────────────────
   ⚙️  CONFIGURATION
   1. Paste your Anthropic API key (console.anthropic.com)
   2. Paste your Stripe payment link (dashboard.stripe.com)
   3. Set your secret owner code (anything you want)
──────────────────────────────────────────────────────── */
const API_KEY      = "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";       // ← Anthropic key
const STRIPE_LINK  = "YOUR_STRIPE_LINK_HERE";   // ← Stripe payment link
const OWNER_CODE   = "GOLF2024";                // ← Your free-access code (change this!)
const SUB_CODE     = "GOLF2024";                // ← Give this to paying subscribers too

/* ─────────────────────────────────────────────────────
   PAYWALL
──────────────────────────────────────────────────────── */
(function initPaywall() {
  // Update Stripe button href dynamically
  document.getElementById('stripeBtn').href = STRIPE_LINK;

  // Check if already unlocked this session
  if (sessionStorage.getItem('gd_access') === '1') {
    document.getElementById('paywall').classList.add('hidden');
    return;
  }

  // Enter key on code input
  document.getElementById('codeInput').addEventListener('keydown', e => {
    if (e.key === 'Enter') checkCode();
  });
})();

function checkCode() {
  const val = document.getElementById('codeInput').value.trim();
  const errEl = document.getElementById('codeErr');

  if (val === OWNER_CODE || val === SUB_CODE) {
    sessionStorage.setItem('gd_access', '1');
    document.getElementById('paywall').classList.add('hidden');
    errEl.textContent = '';
  } else {
    errEl.textContent = '✗ Incorrect code. Check your email or subscribe below.';
    document.getElementById('codeInput').value = '';
    document.getElementById('codeInput').focus();
  }
}

/* ─────────────────────────────────────────────────────
   CHIPS
──────────────────────────────────────────────────────── */
const active = new Set();

document.querySelectorAll('.chip').forEach(c => {
  c.addEventListener('click', () => {
    const kw = c.dataset.kw;
    if (active.has(kw)) { active.delete(kw); c.classList.remove('on'); }
    else                { active.add(kw);    c.classList.add('on');    }
    document.getElementById('query').value = [...active].join(', ');
  });
});

document.getElementById('query').addEventListener('keydown', e => {
  if (e.key === 'Enter') runSearch();
});

/* ─────────────────────────────────────────────────────
   SEARCH
──────────────────────────────────────────────────────── */
async function runSearch() {
  const raw   = document.getElementById('query').value.trim();
  const cond  = document.getElementById('fCond').value;
  const type  = document.getElementById('fType').value;
  const brand = document.getElementById('fBrand').value;

  if (!raw) { showErr('Please enter a keyword or tap a quick keyword above.'); return; }

  if (API_KEY === 'YOUR_API_KEY_HERE') {
    showErr('⚠️ No API key set. Open index.html and replace YOUR_API_KEY_HERE with your Anthropic API key from console.anthropic.com');
    return;
  }

  hideErr();
  setLoading(true);
  document.getElementById('results').innerHTML = '';

  const steps = [
    '🔎 Searching eBay, 2nd Swing, GlobalGolf…',
    '🏪 Checking Rock Bottom Golf & Callaway Pre-Owned…',
    '💰 Comparing prices & spotting savings…',
    '✅ Filtering the best deals for you…'
  ];

  const stepsEl = document.getElementById('loaderSteps');
  stepsEl.innerHTML = '';
  steps.forEach((s, i) => {
    const p = document.createElement('p');
    p.className = 'l-step';
    p.textContent = s;
    p.style.animationDelay = `${i * .85}s`;
    stepsEl.appendChild(p);
  });

  try {
    const prompt = buildPrompt(raw, cond, type, brand);
    const json   = await callClaude(prompt);
    const deals  = parseDeals(json);
    setLoading(false);
    renderDeals(deals, raw);
  } catch (err) {
    setLoading(false);
    showErr('Search failed: ' + err.message + '. Please try again in a moment.');
  }
}

/* ─────────────────────────────────────────────────────
   PROMPT
──────────────────────────────────────────────────────── */
function buildPrompt(raw, cond, type, brand) {
  const extras = [cond, type, brand].filter(Boolean).join(', ');
  return `You are a golf equipment deal-finding assistant. Use web search to find REAL, current cheap golf club listings matching:

Keywords: "${raw}"${extras ? `\nFilters: ${extras}` : ''}

Search these sites specifically:
- ebay.com (used/auction listings)
- 2ndswing.com (certified pre-owned)
- globalgolf.com (used & closeout)
- rockbottomgolf.com (clearance new)
- callawaygolfpreowned.com
- golfgalaxy.com (sale section)
- amazon.com (golf clubs deals)
- pgasuperstore.com (clearance)

Return ONLY a raw JSON array — no markdown, no backticks, no explanation. Each element:
{
  "name": "Full product name including brand, model, specs",
  "price": "$XX" (or "Check Site" if unavailable),
  "originalPrice": "$XX" or null,
  "condition": "Used" | "New" | "Refurbished" | "Clearance",
  "source": "Site name",
  "url": "Direct URL to the listing or category page",
  "type": "Driver" | "Irons" | "Wedge" | "Putter" | "Hybrid" | "Fairway Wood" | "Complete Set" | "Other",
  "description": "1-2 sentences on why this is a good deal or notable feature",
  "dealRating": "🔥 Hot Deal" | "💰 Good Value" | "✅ Fair Price"
}

Return 5–8 results. Prioritise genuine discounts and pre-owned value. Mix brands where possible.`;
}

/* ─────────────────────────────────────────────────────
   API CALL
──────────────────────────────────────────────────────── */
async function callClaude(prompt) {
  const res = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': API_KEY,
      'anthropic-version': '2023-06-01',
      'anthropic-dangerous-direct-browser-access': 'true'
    },
    body: JSON.stringify({
      model: 'claude-sonnet-4-5',
      max_tokens: 1500,
      tools: [{ type: 'web_search_20250305', name: 'web_search' }],
      messages: [{ role: 'user', content: prompt }]
    })
  });

  if (!res.ok) {
    const e = await res.json().catch(() => ({}));
    throw new Error(e.error?.message || `HTTP ${res.status}`);
  }

  const data = await res.json();
  return data.content.filter(b => b.type === 'text').map(b => b.text).join('\n');
}

/* ─────────────────────────────────────────────────────
   PARSE
──────────────────────────────────────────────────────── */
function parseDeals(text) {
  try {
    const clean = text.replace(/```json|```/g, '').trim();
    const m = clean.match(/\[[\s\S]*\]/);
    if (m) return JSON.parse(m[0]);
  } catch (_) {}
  return [];
}

/* ─────────────────────────────────────────────────────
   RENDER
──────────────────────────────────────────────────────── */
function renderDeals(deals, query) {
  const el = document.getElementById('results');

  if (!deals.length) {
    el.innerHTML = `
      <div class="empty">
        <div class="empty-icon">🏌️</div>
        <h3>No Deals Found</h3>
        <p>No results for "<strong>${esc(query)}</strong>".<br>
        Try different keywords, remove filters, or check back later.</p>
      </div>`;
    return;
  }

  const hdr = document.createElement('div');
  hdr.className = 'results-hdr';
  hdr.innerHTML = `
    <span class="results-count">⛳ ${deals.length} DEAL${deals.length !== 1 ? 'S' : ''} FOUND</span>
    <span class="live-badge"><span class="live-dot"></span> Live web search</span>
  `;
  el.appendChild(hdr);

  deals.forEach(d => {
    const card = document.createElement('div');
    card.className = 'card';

    const priceHtml = d.price && d.price !== 'Check Site'
      ? `<div class="price-now">${esc(d.price)}</div>`
      : `<div class="price-now unknown">See Site</div>`;

    const wasHtml = d.originalPrice
      ? `<div class="price-was">${esc(d.originalPrice)}</div>`
      : '';

    card.innerHTML = `
      <div class="card-top">
        <div class="card-name">${d.dealRating ? esc(d.dealRating) + ' ' : ''}${esc(d.name || 'Golf Club Deal')}</div>
        <div class="price-block">${priceHtml}${wasHtml}</div>
      </div>
      <div class="tags">
        ${d.source    ? `<span class="tag tag-src">🏪 ${esc(d.source)}</span>`    : ''}
        ${d.condition ? `<span class="tag tag-cond">${esc(d.condition)}</span>`   : ''}
        ${d.type      ? `<span class="tag tag-type">${esc(d.type)}</span>`        : ''}
      </div>
      ${d.description ? `<p class="card-desc">${esc(d.description)}</p>` : ''}
      ${d.url ? `<a class="card-link" href="${esc(d.url)}" target="_blank" rel="noopener noreferrer">View Deal →</a>` : ''}
    `;
    el.appendChild(card);
  });
}

/* ─────────────────────────────────────────────────────
   HELPERS
──────────────────────────────────────────────────────── */
function esc(s) {
  return String(s)
    .replace(/&/g,'&amp;').replace(/</g,'&lt;')
    .replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

function setLoading(on) {
  document.getElementById('loader').classList.toggle('show', on);
  const btn = document.getElementById('searchBtn');
  btn.disabled  = on;
  btn.textContent = on ? 'SCANNING…' : 'FIND DEALS';
}

function showErr(msg) {
  const b = document.getElementById('errBox');
  b.textContent = msg;
  b.classList.add('show');
}

function hideErr() {
  document.getElementById('errBox').classList.remove('show');
}
</script>
</body>
</html>
