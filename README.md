<!doctype html><html><head><meta charset=utf8><meta name=viewport content="width=device-width,initial-scale=1,viewport-fit=cover"><style>:root{color-scheme:light;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}html{scroll-padding-top:env(safe-area-inset-top,0px)}body{margin:0;padding:0;font:14px -apple-system,BlinkMacSystemFont,sans-serif;background:#fff;color:#000}img{max-width:100%}[hidden]:not([hidden=until-found i]){display:none!important}</style></head><body>
<meta name="claude:viewport-fit-sides" content="cover">
<meta name="theme-color" content="#03132f">
<title>Calendario Marea</title>
<link rel="manifest" href="manifest.json">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
<style>
  /*
    Layout concept: the whole screen sits inside a white, sphere-like capsule —
    corners recede, the top and bottom bulge outward like the hull of a buoy.
    Inside it: a sunken skyline low and dark, open water above it, and a bright
    sunlit surface right at the top. Today is the day lit from within; a bottom
    sheet rises like a diver's note surfacing when a day is tapped.
  */
  :root {
    --abyss: #051b40;
    --deep: #0a2d78;
    --ocean: #124a9e;
    --tide: #1269CC;
    --foam: #eef8ff;
    --mist: #8fd9f7;
    --glow: #51EEFC;
    --coral: #ff7a72;
    --frame: #ffffff;
    --skyline: #03132f;
    --ring: rgba(var(--glow-rgb), 0.4);
    --sheet-bg: #0a2558;
    --sheet-line: rgba(var(--foam-rgb), 0.14);
    --font-body: 'Outfit', system-ui, -apple-system, sans-serif;
    --glow-rgb: 81, 238, 252;
    --tide-rgb: 18, 105, 204;
    --foam-rgb: 238, 248, 255;
    --mist-rgb: 169, 198, 228;
    --shade-rgb: 3, 9, 20;
    --ink: #03172b;
    --numeral: #08105a;
    --sat: #bdeeff;
    --surface-light: #86f7ff;
    --moon-lit: #e6ffe9;
    color-scheme: dark;
    /* Grosor del borde blanco en el centro (arriba fino, abajo ancho) y flecha de cada arco.
       El JS los recalcula con los valores reales del dispositivo. */
    --cap-t: calc(14px + env(safe-area-inset-top, 0px));
    --cap-b: calc(64px + env(safe-area-inset-bottom, 0px));
    --s-top: 43px;
    --s-bot: 50px;
  }

  * { box-sizing: border-box; }

  html, body {
    height: 100%;
  }
  html { background: var(--frame); }

  body {
    margin: 0;
    font-family: var(--font-body);
    color: var(--foam);
    background: var(--frame);
    overflow-x: hidden;
    position: relative;
  }

  /* --- el marco: dos arcos circulares independientes ---
     Arriba, un casquete blanco fino con cúpula circular; abajo, otro más ancho con
     curva circular hacia abajo. Los laterales quedan abiertos (el agua llega al borde
     de la pantalla), así que ambos arcos no se tocan nunca.
     El recorte (clip-path) lo calcula el JS con arcos de circunferencia exactos. */
  .sphere-frame {
    position: fixed;
    inset: 0;
    z-index: 1;
    overflow: hidden;
    background: linear-gradient(180deg, var(--ocean) 0%, var(--deep) 46%, var(--abyss) 100%);
  }
  .frame-rim {
    position: fixed;
    inset: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    pointer-events: none;
  }

  .skyline {
    position: absolute;
    left: 0;
    right: 0;
    bottom: var(--cap-b);
    height: 30%;
    display: flex;
    align-items: flex-end;
    gap: 3%;
    padding: 0 4%;
    z-index: 0;
    pointer-events: none;
    opacity: 0.65;
  }
  .skyline .b {
    flex: 1;
    min-width: 0;
    height: 40%;
    background: var(--skyline);
    border-radius: 3px 3px 0 0;
  }

  .water-surface {
    position: absolute;
    top: 0;
    left: -6%;
    width: 112%;
    height: calc((var(--cap-t) + var(--s-top) + 12px) * 2);
    z-index: 0;
    pointer-events: none;
    animation: shimmer 7s ease-in-out infinite;
  }
  @keyframes shimmer {
    0%, 100% { transform: translateX(0); }
    50% { transform: translateX(10px); }
  }

  .caustics {
    position: absolute;
    inset: 0;
    z-index: 0;
    pointer-events: none;
    opacity: 0.4;
    mix-blend-mode: screen;
    background-image:
      repeating-linear-gradient(115deg, rgba(var(--glow-rgb),0.1) 0 2px, transparent 2px 46px),
      repeating-linear-gradient(55deg, rgba(var(--foam-rgb),0.06) 0 1px, transparent 1px 60px);
    animation: causticShift 24s linear infinite;
  }
  @keyframes causticShift {
    0%   { background-position: 0 0, 0 0; }
    100% { background-position: 140px 220px, -180px 150px; }
  }
  .sparkle {
    position: absolute;
    inset: 0;
    z-index: 0;
    pointer-events: none;
    opacity: 0.5;
    background-image:
      radial-gradient(1.5px 1.5px at 12% 22%, rgba(var(--foam-rgb),0.8), transparent),
      radial-gradient(1px 1px at 76% 12%, rgba(var(--glow-rgb),0.9), transparent),
      radial-gradient(1.5px 1.5px at 60% 68%, rgba(var(--foam-rgb),0.6), transparent),
      radial-gradient(1px 1px at 30% 78%, rgba(var(--glow-rgb),0.7), transparent),
      radial-gradient(1.5px 1.5px at 88% 54%, rgba(var(--foam-rgb),0.55), transparent),
      radial-gradient(1px 1px at 20% 48%, rgba(var(--glow-rgb),0.6), transparent);
  }

  .ripples { position: absolute; inset: 0; z-index: 0; pointer-events: none; overflow: hidden; }
  .ripple {
    position: absolute;
    border-radius: 50%;
    border: 1px solid rgba(var(--foam-rgb), 0.08);
  }
  .ripple.r1 { width: 46vh; height: 46vh; top: -14vh; right: -10vh; border-color: rgba(var(--glow-rgb),0.18); }
  .ripple.r2 { width: 30vh; height: 30vh; top: 6vh; right: 4vh; border-color: rgba(var(--glow-rgb),0.13); }
  .ripple.r3 { width: 70vh; height: 70vh; bottom: -30vh; left: -20vh; border-color: rgba(var(--tide-rgb),0.2); }
  .ripple.r4 { width: 18vh; height: 18vh; top: 32vh; left: -6vh; border-color: rgba(var(--glow-rgb),0.12); }
  .ripple.r5 { width: 24vh; height: 24vh; bottom: 10vh; right: -8vh; border-color: rgba(var(--tide-rgb),0.18); }
  .bubble {
    position: absolute;
    bottom: -6vh;
    border-radius: 50%;
    background: radial-gradient(circle at 35% 30%, rgba(var(--foam-rgb),0.5), rgba(var(--foam-rgb),0.03) 70%);
    animation: drift 14s ease-in infinite;
  }
  .bubble.b1 { width: 10px; height: 10px; left: 18%; animation-delay: 0s; }
  .bubble.b2 { width: 6px; height: 6px; left: 72%; animation-delay: 3.5s; }
  .bubble.b3 { width: 14px; height: 14px; left: 45%; animation-delay: 7s; }
  .bubble.b4 { width: 8px; height: 8px; left: 86%; animation-delay: 10.5s; }
  .bubble.b5 { width: 5px; height: 5px; left: 58%; animation-delay: 5s; }
  .bubble.b6 { width: 9px; height: 9px; left: 8%; animation-delay: 12.5s; }
  @keyframes drift {
    0%   { transform: translateY(0) translateX(0); opacity: 0; }
    10%  { opacity: 0.7; }
    90%  { opacity: 0.4; }
    100% { transform: translateY(-112vh) translateX(6vw); opacity: 0; }
  }
  @media (prefers-reduced-motion: reduce) {
    .bubble { animation: none; opacity: 0.25; }
    .caustics { animation: none; }
    .water-surface { animation: none; }
  }

  .app {
    position: relative;
    z-index: 1;
    min-height: 100%;
    display: flex;
    flex-direction: column;
    padding-top: calc(var(--cap-t) + var(--s-top) + 22px);
    padding-bottom: 56px;
    padding-left: max(18px, env(safe-area-inset-left, 0px));
    padding-right: max(18px, env(safe-area-inset-right, 0px));
  }

  .topbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
    margin-bottom: 6px;
  }

  .nav-btn {
    width: 44px;
    height: 44px;
    flex: none;
    border-radius: 50%;
    border: 1px solid rgba(var(--foam-rgb), 0.25);
    background: rgba(var(--foam-rgb), 0.06);
    color: var(--foam);
    font-size: 22px;
    line-height: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: background 0.15s ease, transform 0.15s ease;
  }
  .nav-btn:hover { background: rgba(var(--foam-rgb), 0.14); }
  .nav-btn:active { transform: scale(0.92); }
  .nav-btn:focus-visible { outline: 2px solid var(--glow); outline-offset: 2px; }

  .month-display {
    text-align: center;
    line-height: 1;
    min-width: 0;
  }
  .month-name {
    display: block;
    font-family: var(--font-body);
    font-weight: 800;
    font-size: clamp(28px, 8.5vw, 40px);
    letter-spacing: -0.01em;
    color: var(--foam);
    text-shadow: 0 0 28px rgba(var(--glow-rgb), 0.5);
  }
  .year-name {
    display: block;
    margin-top: 2px;
    font-size: 13px;
    font-weight: 600;
    letter-spacing: 0.3em;
    color: var(--mist);
  }

  .weekdays {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    margin: 18px 0 10px;
  }
  .wd {
    text-align: center;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.08em;
    color: var(--mist);
  }
  .wd.sun { color: var(--coral); }
  .wd.sat { color: var(--glow); }

  .grid {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 4px 2px;
  }

  .day {
    position: relative;
    aspect-ratio: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    border: none;
    background: none;
    color: var(--foam);
    font-family: var(--font-body);
    font-weight: 600;
    font-size: clamp(14px, 4vw, 17px);
    cursor: pointer;
    -webkit-tap-highlight-color: transparent;
  }
  .day .num {
    position: relative;
    z-index: 1;
    width: 72%;
    height: 72%;
    max-width: 42px;
    max-height: 42px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    transition: background 0.2s ease, box-shadow 0.3s ease, transform 0.35s cubic-bezier(0.34, 1.56, 0.64, 1);
  }
  .day:focus-visible .num { outline: 2px solid var(--glow); outline-offset: 2px; }
  .day.pad { visibility: hidden; cursor: default; }
  .day.sun .num { color: #ffb3ae; }
  .day.sat .num { color: var(--sat); }

  .day.has-note::after {
    content: "";
    position: absolute;
    bottom: 10%;
    width: 4px;
    height: 4px;
    border-radius: 50%;
    background: var(--coral);
    z-index: 2;
    animation: dotPop 0.45s cubic-bezier(0.34, 1.56, 0.64, 1) both;
  }

  .day.today .num {
    background: radial-gradient(circle, var(--glow), var(--tide) 72%);
    color: var(--ink);
    font-weight: 800;
    box-shadow: 0 0 0 1px rgba(var(--foam-rgb),0.5), 0 0 22px rgba(var(--glow-rgb),0.7);
  }
  .day.today.has-note::after { background: var(--ink); }

  .day.selected:not(.today) .num {
    box-shadow: inset 0 0 0 2px var(--glow), 0 0 16px rgba(var(--glow-rgb), 0.35);
  }

  /* ===== arcanas: miniatura sobre el día + marca de estado de la quedada ===== */
  .day.has-meet .num {
    background-image: linear-gradient(180deg, rgba(var(--shade-rgb), 0.18), rgba(var(--shade-rgb), 0.58)), var(--meet-img);
    background-size: cover;
    background-position: center;
  }
  .day.has-meet.today .num {
    background-image: linear-gradient(180deg, rgba(3, 23, 43, 0.3), rgba(3, 23, 43, 0.62)), var(--meet-img);
  }
  .meet-badge {
    position: absolute;
    top: 3%;
    right: 10%;
    width: 9px;
    height: 9px;
    border-radius: 50%;
    z-index: 3;
    box-shadow: 0 0 0 1.5px rgba(var(--shade-rgb), 0.6);
    animation: dotPop 0.45s cubic-bezier(0.34, 1.56, 0.64, 1) both;
  }
  .meet-badge.pending { background: var(--glow); box-shadow: 0 0 6px var(--glow), 0 0 0 1.5px rgba(var(--shade-rgb), 0.55); }
  .meet-badge.done { background: #7cffb2; box-shadow: 0 0 6px #7cffb2, 0 0 0 1.5px rgba(var(--shade-rgb), 0.55); }
  .meet-badge.missed { background: #ff7a72; box-shadow: 0 0 6px #ff7a72, 0 0 0 1.5px rgba(var(--shade-rgb), 0.55); }

  .today-btn {
    position: fixed;
    right: max(24px, calc(24px + env(safe-area-inset-right, 0px)));
    bottom: var(--today-b, calc(var(--cap-b) + var(--s-bot) + 12px));
    z-index: 2;
    border: none;
    border-radius: 999px;
    padding: 12px 20px;
    font-family: var(--font-body);
    font-weight: 700;
    font-size: 13px;
    letter-spacing: 0.06em;
    color: var(--ink);
    background: linear-gradient(135deg, var(--glow), var(--tide));
    box-shadow: 0 6px 20px rgba(var(--tide-rgb), 0.5);
    cursor: pointer;
  }
  .today-btn { transition: transform 0.2s ease, box-shadow 0.2s ease; }
  .today-btn:active { transform: scale(0.94); box-shadow: 0 3px 12px rgba(var(--tide-rgb), 0.5); }

  .overlay {
    position: fixed;
    inset: 0;
    background: rgba(var(--shade-rgb), 0.55);
    backdrop-filter: blur(2px);
    z-index: 4;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.35s ease, visibility 0.35s;
  }
  .overlay.open { opacity: 1; visibility: visible; }

  .sheet {
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 5;
    background: var(--sheet-bg);
    border-top: 1px solid rgba(var(--foam-rgb), 0.15);
    border-radius: 22px 22px 0 0;
    box-shadow: 0 -12px 40px rgba(0,0,0,0.5);
    padding: 10px max(18px, env(safe-area-inset-right, 0px)) calc(20px + env(safe-area-inset-bottom, 0px)) max(18px, env(safe-area-inset-left, 0px));
    max-height: 72vh;
    display: flex;
    flex-direction: column;
    transform: translateY(100%);
    transition: transform 0.45s cubic-bezier(0.2, 0.9, 0.25, 1);
  }
  .sheet.open { transform: translateY(0); }

  .sheet-handle {
    width: 36px;
    height: 4px;
    border-radius: 999px;
    background: rgba(var(--foam-rgb),0.25);
    margin: 4px auto 12px;
  }

  .sheet-header {
    display: flex;
    align-items: baseline;
    justify-content: space-between;
    gap: 12px;
    margin-bottom: 4px;
  }
  .sheet-header h2 {
    margin: 0;
    font-family: var(--font-body);
    font-weight: 800;
    font-size: 23px;
    letter-spacing: -0.01em;
  }
  .sheet-header .wd-label {
    display: block;
    font-family: var(--font-body);
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.2em;
    color: var(--mist);
    margin-bottom: 2px;
  }
  .close-btn {
    flex: none;
    width: 34px;
    height: 34px;
    border-radius: 50%;
    border: 1px solid rgba(var(--foam-rgb),0.2);
    background: rgba(var(--foam-rgb),0.05);
    color: var(--foam);
    font-size: 15px;
    cursor: pointer;
  }

  .notes-list {
    list-style: none;
    margin: 14px 0;
    padding: 0;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 8px;
    min-height: 0;
  }
  .notes-empty {
    color: var(--mist);
    font-size: 14px;
    padding: 18px 2px;
    text-align: center;
  }
  .note-item {
    display: flex;
    align-items: center;
    gap: 10px;
    background: rgba(var(--foam-rgb), 0.05);
    border: 1px solid var(--sheet-line);
    border-radius: 14px;
    padding: 10px 12px;
  }
  .note-item .dot {
    flex: none;
    width: 7px;
    height: 7px;
    border-radius: 50%;
    background: var(--coral);
  }
  .note-item span.txt {
    flex: 1;
    min-width: 0;
    font-size: 14.5px;
    word-break: break-word;
  }
  .note-item button {
    flex: none;
    border: none;
    background: none;
    color: var(--mist);
    font-size: 16px;
    cursor: pointer;
    padding: 4px 6px;
  }
  .note-item button:hover { color: var(--coral); }

  .note-form {
    display: flex;
    gap: 8px;
    padding-top: 8px;
    border-top: 1px solid var(--sheet-line);
  }
  .note-form input {
    flex: 1;
    min-width: 0;
    border-radius: 999px;
    border: 1px solid rgba(var(--foam-rgb),0.2);
    background: rgba(var(--shade-rgb), 0.5);
    color: var(--foam);
    padding: 11px 16px;
    font-size: 14.5px;
    font-family: var(--font-body);
  }
  .note-form input::placeholder { color: rgba(var(--mist-rgb), 0.7); }
  .note-form input:focus-visible { outline: 2px solid var(--glow); }
  .note-form button {
    flex: none;
    border: none;
    border-radius: 999px;
    padding: 0 18px;
    font-weight: 700;
    font-size: 13.5px;
    color: var(--ink);
    background: linear-gradient(135deg, var(--glow), var(--tide));
    cursor: pointer;
  }
  .note-form button:disabled { opacity: 0.45; cursor: default; }

  /* ---------- número del mes de fondo + luz de la superficie ---------- */
  .month-num {
    position: absolute;
    z-index: -2;
    left: 0;
    width: 64%;
    top: calc(var(--cap-t) + 28px - 0.17em);
    text-align: center;
    font-family: var(--font-body);
    font-weight: 900;
    font-style: italic;
    font-size: clamp(110px, 46vw, 240px);
    line-height: 1;
    letter-spacing: -0.06em;
    color: var(--numeral);
    pointer-events: none;
    user-select: none;
    -webkit-user-select: none;
  }
  .surface-light {
    position: absolute;
    z-index: -1;
    left: 36%;
    width: 56%;
    aspect-ratio: 320 / 110;
    top: calc(var(--cap-t) + 2px);
    overflow: visible;
    pointer-events: none;
  }

  /* copias del mes saliente durante la transición */
  .app .ghost {
    position: absolute;
    margin: 0;
    pointer-events: none;
    will-change: transform, opacity;
  }

  /* ---------- micro-animaciones al seleccionar un día ---------- */
  .day:active .num { transform: scale(0.86); }
  .day.selected .num { transform: scale(1.08); }
  .day .ping {
    position: absolute;
    left: 50%;
    top: 50%;
    width: min(72%, 42px);
    aspect-ratio: 1;
    border-radius: 50%;
    border: 2px solid var(--glow);
    pointer-events: none;
    transform: translate(-50%, -50%) scale(0.7);
    opacity: 0.85;
    animation: ping 0.6s cubic-bezier(0.2, 0.7, 0.3, 1) forwards;
  }
  @keyframes ping {
    to { transform: translate(-50%, -50%) scale(1.9); opacity: 0; }
  }
  @keyframes dotPop {
    0%   { transform: scale(0); }
    100% { transform: scale(1); }
  }
  .day.pulse .num { animation: todayPulse 0.9s cubic-bezier(0.3, 0.7, 0.3, 1) var(--pd, 0ms) 1 both; }
  @keyframes todayPulse {
    0%   { transform: scale(1); }
    35%  { transform: scale(1.22); box-shadow: 0 0 0 1px rgba(var(--foam-rgb),0.6), 0 0 44px rgba(var(--glow-rgb),0.95); }
    100% { transform: scale(1); }
  }

  /* notas: entrada y salida suaves */
  .notes-empty { animation: fadeUp 0.35s ease both; }
  .note-item.enter {
    animation: noteIn 0.4s cubic-bezier(0.2, 0.9, 0.25, 1) var(--d, 0ms) backwards;
  }
  @keyframes noteIn {
    from { opacity: 0; transform: translateY(10px) scale(0.98); }
    to   { opacity: 1; transform: none; }
  }
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(6px); }
    to   { opacity: 1; transform: none; }
  }
  .note-item.leaving {
    pointer-events: none;
    overflow: hidden;
    animation: noteOut 0.28s cubic-bezier(0.4, 0, 0.6, 1) forwards;
  }
  @keyframes noteOut {
    0%   { opacity: 1; transform: none; max-height: var(--h, 60px); }
    55%  { opacity: 0; transform: translateX(26px); max-height: var(--h, 60px); }
    100% { opacity: 0; transform: translateX(26px); max-height: 0; padding-top: 0; padding-bottom: 0; margin-top: -8px; border-width: 0; }
  }

  @media (prefers-reduced-motion: reduce) {
    .day .ping { display: none; }
    .day.pulse .num, .note-item.enter, .notes-empty { animation: none; }
  }

  /* ===== HORA OSCURA (00:00 - 01:00): azul -> verde, blanco -> negro ===== */
  html.dark-hour {
    --abyss: #010b04;
    --deep: #04230f;
    --ocean: #0b4d21;
    --tide: #17a548;
    --foam: #eaffee;
    --mist: #9fdcae;
    --glow: #6dff93;
    --frame: #000000;
    --skyline: #010a04;
    --sheet-bg: #05190b;
    --glow-rgb: 109, 255, 147;
    --tide-rgb: 23, 165, 72;
    --foam-rgb: 234, 255, 238;
    --mist-rgb: 159, 220, 174;
    --shade-rgb: 0, 10, 3;
    --ink: #021a0a;
    --numeral: #031f0d;
    --sat: #c6ffd4;
    --surface-light: #6dff93;
  }
  .veil {
    position: fixed;
    inset: 0;
    z-index: 50;
    background: #000;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.7s ease;
  }
  .veil.on { opacity: 1; pointer-events: auto; }

  /* partes del SVG que no admiten var() como atributo */
  #waveGrad stop:nth-child(1) { stop-color: var(--foam); }
  #waveGrad stop:nth-child(2), #waveGrad stop:nth-child(3) { stop-color: var(--glow); }
  .frame-rim path[filter] { stroke: rgb(var(--glow-rgb)); }
  .frame-rim path:not([filter]) { stroke: rgb(var(--foam-rgb)); }
  .surface-light path { fill: var(--surface-light); }

  .month-display { position: relative; -webkit-user-select: none; user-select: none; -webkit-touch-callout: none; }
  .dark-tag {
    position: absolute;
    left: 50%;
    top: 100%;
    margin-top: 6px;
    transform: translateX(-50%);
    white-space: nowrap;
    font-family: var(--font-body);
    font-weight: 700;
    font-size: 10.5px;
    letter-spacing: 0.42em;
    text-indent: 0.42em;
    color: var(--glow);
    text-shadow: 0 0 12px rgba(var(--glow-rgb), 0.85);
    opacity: 0;
    visibility: hidden;
  }
  html.dark-hour .dark-tag { opacity: 1; visibility: visible; animation: tagIn 1.2s ease 0.2s both; }
  @keyframes tagIn { from { opacity: 0; letter-spacing: 0.7em; } to { opacity: 1; letter-spacing: 0.42em; } }

  html.dark-hour .surface-light { display: none; }
  html.dark-hour .water-surface { opacity: 0.55; }
  html.dark-hour .skyline { display: none; }

  /* torre (sustituye a los edificios) */
  .tower, .tower-fog { display: none; pointer-events: none; }
  html.dark-hour .tower, html.dark-hour .tower-fog { display: block; }
  .tower-fog {
    position: absolute;
    left: -10%;
    right: -10%;
    bottom: calc(var(--cap-b) - 14px);
    height: 24%;
    z-index: 0;
    background: radial-gradient(ellipse at 50% 100%, rgba(var(--glow-rgb), 0.24), transparent 70%);
  }
  .tower {
    position: absolute;
    z-index: 0;
    left: 50%;
    bottom: var(--cap-b);
    height: min(58%, 640px);
    aspect-ratio: 335 / 548;
    transform: translateX(-63%);
    overflow: visible;
    opacity: 0.94;
    filter: drop-shadow(0 0 10px rgba(var(--glow-rgb), 0.22));
  }
  .tower .t { fill: var(--skyline); stroke: rgba(var(--glow-rgb), 0.2); stroke-width: 0.8; stroke-linejoin: round; }
  .tower .t.s { fill: none; stroke: var(--skyline); }
  .tower .w { fill: rgb(var(--glow-rgb)); }
  .tower .w.f { animation: flick 5s ease-in-out infinite; }
  .tower .orb { fill: rgb(var(--glow-rgb)); animation: orbPulse 6s ease-in-out infinite; }
  .tower .ring { fill: none; stroke: rgba(var(--foam-rgb), 0.35); stroke-width: 1; }
  @keyframes orbPulse { 0%, 100% { opacity: 0.5; } 50% { opacity: 0.95; } }
  @keyframes flick { 0%, 100% { opacity: var(--o, 0.7); } 50% { opacity: 0.12; } }

  /* indicador de fase lunar */
  .moon {
    display: none;
    position: absolute;
    z-index: 2;
    right: calc(max(18px, env(safe-area-inset-right, 0px)) + 58px);
    top: calc(var(--cap-t) + 24px);
    width: 36px;
    height: 36px;
    padding: 0;
    border: none;
    background: none;
    cursor: pointer;
    -webkit-tap-highlight-color: transparent;
    filter: drop-shadow(0 0 7px rgba(var(--glow-rgb), 0.55));
  }
  html.dark-hour .moon { display: block; animation: moonIn 1.2s ease 0.3s both, moonFloat 7s ease-in-out 1.5s infinite; }
  .moon svg { display: block; width: 100%; height: 100%; overflow: visible; }
  .moon .moon-dark { fill: rgba(var(--shade-rgb), 0.8); stroke: rgba(var(--glow-rgb), 0.4); stroke-width: 0.8; }
  .moon .moon-lit { fill: var(--moon-lit); }
  .moon:active { transform: scale(0.92); }
  @keyframes moonIn { from { opacity: 0; transform: translateY(-8px) scale(0.8); } to { opacity: 1; transform: none; } }
  @keyframes moonFloat { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-2.5px); } }

  /* nota especial de la hora oscura (hoja inferior) */
  .dark-note {
    flex: none;
    margin: 0 0 12px;
    padding: 10px 12px 8px;
    border-radius: 16px;
    border: 1px solid rgba(var(--glow-rgb), 0.35);
    background: rgba(var(--glow-rgb), 0.07);
    box-shadow: inset 0 0 24px rgba(var(--glow-rgb), 0.1);
    animation: fadeUp 0.45s ease both;
  }
  .dark-note-label {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 10.5px;
    font-weight: 700;
    letter-spacing: 0.22em;
    color: var(--glow);
    margin-bottom: 6px;
  }
  .dark-note-label::before { content: "\263E"; font-size: 14px; letter-spacing: 0; }
  .dn-status { margin-left: auto; letter-spacing: 0.08em; font-weight: 500; color: var(--mist); opacity: 0; transition: opacity 0.4s ease; }
  .dn-status.show { opacity: 1; }
  .dark-note textarea {
    display: block;
    width: 100%;
    resize: none;
    border: none;
    outline: none;
    background: transparent;
    color: var(--foam);
    font-family: var(--font-body);
    font-size: 14.5px;
    line-height: 1.4;
    padding: 2px 0;
  }
  .dark-note textarea::placeholder { color: rgba(var(--mist-rgb), 0.6); }

  /* marca de nota especial en el día */
  .day.has-dark::before {
    content: "";
    position: absolute;
    top: 7%;
    left: 50%;
    width: 4px;
    height: 4px;
    margin-left: -2px;
    border-radius: 50%;
    background: var(--glow);
    box-shadow: 0 0 7px var(--glow);
    z-index: 2;
    animation: dotPop 0.45s cubic-bezier(0.34, 1.56, 0.64, 1) both;
  }

  .toast {
    position: fixed;
    left: 50%;
    bottom: calc(var(--cap-b) + 78px);
    z-index: 40;
    max-width: 78vw;
    padding: 9px 16px;
    border-radius: 999px;
    font-size: 13px;
    font-weight: 600;
    text-align: center;
    color: var(--foam);
    background: rgba(var(--shade-rgb), 0.78);
    border: 1px solid rgba(var(--foam-rgb), 0.18);
    backdrop-filter: blur(6px);
    -webkit-backdrop-filter: blur(6px);
    opacity: 0;
    transform: translate(-50%, 8px);
    pointer-events: none;
    transition: opacity 0.3s ease, transform 0.3s ease;
  }
  .toast.show { opacity: 1; transform: translate(-50%, 0); }

  @media (prefers-reduced-motion: reduce) {
    .tower .w.f, .tower .orb, html.dark-hour .moon, html.dark-hour .dark-tag { animation: none; }
    .veil { transition: none; }
  }

  /* ===== botón del compendio de arcanas (visible en ambos modos) ===== */
  .arcana-btn {
    position: fixed;
    z-index: 2;
    left: max(24px, calc(24px + env(safe-area-inset-left, 0px)));
    bottom: var(--today-b, calc(var(--cap-b) + var(--s-bot) + 12px));
    width: 44px;
    height: 44px;
    padding: 0;
    border: 1px solid rgba(var(--foam-rgb), 0.3);
    border-radius: 50%;
    background: rgba(var(--foam-rgb), 0.08);
    color: var(--foam);
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    -webkit-tap-highlight-color: transparent;
    box-shadow: 0 6px 20px rgba(var(--tide-rgb), 0.35);
    transition: transform 0.2s ease, box-shadow 0.2s ease;
  }
  .arcana-btn:active { transform: scale(0.94); }
  .arcana-btn svg { width: 20px; height: 20px; display: block; }

  /* ===== hojas secundarias: compendio de arcanas, menú de día, selector de quedada ===== */
  #overlay2 { z-index: 6; }
  #arcanaSheet, #dayMenuSheet, #meetupSheet { z-index: 7; }
  #confirmOverlay { z-index: 8; }

  .arcana-sheet { max-height: 82vh; }
  .arcana-list {
    display: flex;
    flex-direction: column;
    gap: 8px;
    overflow-y: auto;
    margin: 14px 0;
    min-height: 0;
  }
  .arcana-row {
    display: flex;
    align-items: center;
    gap: 12px;
    width: 100%;
    background: rgba(var(--foam-rgb), 0.05);
    border: 1px solid var(--sheet-line);
    border-radius: 14px;
    padding: 8px 12px;
    text-align: left;
    cursor: pointer;
    color: var(--foam);
    font-family: var(--font-body);
  }
  .arcana-row:active { transform: scale(0.98); }
  .arcana-row.active { box-shadow: inset 0 0 0 2px var(--glow); }
  .arcana-thumb {
    flex: none;
    width: 44px;
    height: 44px;
    border-radius: 50%;
    background-size: cover;
    background-position: center;
    box-shadow: 0 0 0 1px rgba(var(--foam-rgb), 0.25);
  }
  .arcana-row-text { flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 2px; }
  .arcana-row-name { font-weight: 700; font-size: 14px; }
  .arcana-row-person { font-size: 12px; color: var(--mist); overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .arcana-row-person.unset { font-style: italic; opacity: 0.65; }
  .arcana-row-level {
    flex: none;
    font-size: 11.5px;
    font-weight: 700;
    color: var(--glow);
    background: rgba(var(--glow-rgb), 0.14);
    border-radius: 999px;
    padding: 4px 9px;
    white-space: nowrap;
  }

  .arcana-detail { display: flex; flex-direction: column; gap: 14px; overflow-y: auto; margin-top: 6px; padding-bottom: 6px; }
  .arcana-detail-img {
    width: 120px;
    height: 120px;
    border-radius: 50%;
    margin: 4px auto 0;
    background-size: cover;
    background-position: center;
    box-shadow: 0 0 0 2px rgba(var(--foam-rgb), 0.3), 0 0 30px rgba(var(--glow-rgb), 0.35);
  }
  .arcana-detail-name { text-align: center; font-weight: 800; font-size: 19px; }
  .arcana-name-label { display: flex; align-items: center; gap: 8px; font-size: 11px; font-weight: 700; letter-spacing: 0.18em; color: var(--mist); }
  #arcanaNameInput {
    width: 100%;
    border-radius: 999px;
    border: 1px solid rgba(var(--foam-rgb), 0.2);
    background: rgba(var(--shade-rgb), 0.5);
    color: var(--foam);
    padding: 11px 16px;
    font-size: 14.5px;
    font-family: var(--font-body);
  }
  #arcanaNameInput:focus-visible { outline: 2px solid var(--glow); }
  .arcana-level-row { display: flex; gap: 5px; justify-content: center; flex-wrap: wrap; }
  .arcana-pip { width: 15px; height: 15px; border-radius: 50%; background: rgba(var(--foam-rgb), 0.12); border: 1px solid rgba(var(--foam-rgb), 0.22); }
  .arcana-pip.filled { background: linear-gradient(135deg, var(--glow), var(--tide)); border-color: transparent; box-shadow: 0 0 8px rgba(var(--glow-rgb), 0.6); }
  .arcana-level-label { text-align: center; font-size: 12.5px; color: var(--mist); }

  .day-menu-options { display: flex; flex-direction: column; gap: 10px; margin: 14px 0 6px; }
  .day-menu-opt {
    display: flex;
    align-items: center;
    gap: 12px;
    width: 100%;
    background: rgba(var(--foam-rgb), 0.06);
    border: 1px solid var(--sheet-line);
    border-radius: 14px;
    padding: 14px 16px;
    color: var(--foam);
    font-family: var(--font-body);
    font-weight: 700;
    font-size: 15px;
    cursor: pointer;
    text-align: left;
  }
  .day-menu-opt:active { transform: scale(0.98); }
  .opt-ic { font-size: 18px; width: 22px; text-align: center; flex: none; }

  .meetup-empty { color: var(--mist); font-size: 13.5px; line-height: 1.5; text-align: center; padding: 10px 4px 16px; }
  .meetup-clear-btn {
    display: block;
    border: none;
    background: none;
    color: var(--coral);
    font-family: var(--font-body);
    font-size: 12.5px;
    font-weight: 700;
    letter-spacing: 0.04em;
    cursor: pointer;
    padding: 2px 0 2px;
    margin-top: 4px;
  }

  /* ===== tarjeta de quedada dentro de la hoja del día ===== */
  .meet-card {
    display: flex;
    align-items: center;
    gap: 12px;
    background: rgba(var(--glow-rgb), 0.08);
    border: 1px solid rgba(var(--glow-rgb), 0.3);
    border-radius: 14px;
    padding: 10px 12px;
    margin-bottom: 12px;
    animation: fadeUp 0.35s ease both;
  }
  .meet-card-thumb { flex: none; width: 40px; height: 40px; border-radius: 50%; background-size: cover; background-position: center; }
  .meet-card-text { flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 1px; }
  .meet-card-name { font-weight: 700; font-size: 13.5px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .meet-card-sub { font-size: 11.5px; color: var(--mist); overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .meet-card-status { font-size: 10px; font-weight: 700; letter-spacing: 0.06em; padding: 3px 8px; border-radius: 999px; flex: none; white-space: nowrap; }
  .meet-card-status.pending { background: rgba(var(--glow-rgb), 0.18); color: var(--glow); }
  .meet-card-status.done { background: rgba(124, 255, 178, 0.18); color: #7cffb2; }
  .meet-card-status.missed { background: rgba(255, 122, 114, 0.18); color: #ff7a72; }
  .meet-card-remove { flex: none; border: none; background: none; color: var(--mist); font-size: 15px; cursor: pointer; padding: 4px 6px; }
  .meet-card-remove:hover { color: var(--coral); }

  /* ===== confirmación de quedada pasada ===== */
  .confirm-card {
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%) scale(0.92);
    z-index: 9;
    width: min(86vw, 360px);
    background: var(--sheet-bg);
    border: 1px solid rgba(var(--foam-rgb), 0.2);
    border-radius: 20px;
    padding: 22px 20px 18px;
    text-align: center;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.25s ease, transform 0.25s ease, visibility 0.25s;
  }
  .confirm-card.open { opacity: 1; visibility: visible; transform: translate(-50%, -50%) scale(1); }
  .confirm-thumb {
    width: 64px;
    height: 64px;
    border-radius: 50%;
    margin: 0 auto 12px;
    background-size: cover;
    background-position: center;
    box-shadow: 0 0 0 2px rgba(var(--foam-rgb), 0.25), 0 0 20px rgba(var(--glow-rgb), 0.45);
  }
  .confirm-card h3 { margin: 0 0 6px; font-size: 17.5px; font-weight: 800; }
  .confirm-card p { margin: 0 0 18px; font-size: 13.5px; color: var(--mist); line-height: 1.45; }
  .confirm-actions { display: flex; gap: 10px; }
  .confirm-btn {
    flex: 1;
    border: none;
    border-radius: 999px;
    padding: 11px 0;
    font-weight: 700;
    font-size: 14px;
    cursor: pointer;
    font-family: var(--font-body);
  }
  .confirm-btn.no { background: rgba(var(--foam-rgb), 0.08); color: var(--foam); border: 1px solid rgba(var(--foam-rgb), 0.22); }
  .confirm-btn.yes { background: linear-gradient(135deg, var(--glow), var(--tide)); color: var(--ink); }
  .confirm-btn:active { transform: scale(0.96); }

  @media (prefers-reduced-motion: reduce) {
    .meet-card { animation: none; }
  }
</style>

<div class="sphere-frame" id="stage">
  <div class="skyline" aria-hidden="true">
    <span class="b" style="height:30%"></span>
    <span class="b" style="height:52%"></span>
    <span class="b" style="height:22%"></span>
    <span class="b" style="height:68%"></span>
    <span class="b" style="height:38%"></span>
    <span class="b" style="height:58%"></span>
    <span class="b" style="height:26%"></span>
    <span class="b" style="height:44%"></span>
    <span class="b" style="height:64%"></span>
    <span class="b" style="height:34%"></span>
  </div>

  <div class="tower-fog" aria-hidden="true"></div>
  <svg class="tower" viewBox="55 30 335 548" aria-hidden="true">
    <polygon class="t" points="246,74 284,74 290,84 296,96 236,96 242,84"/>
    <polygon class="t" points="238,96 296,96 300,112 312,120 314,150 306,176 300,200 214,200 206,176 200,152 212,128 226,114"/>
    <polygon class="t" points="214,200 300,200 318,212 326,244 322,276 308,300 204,300 196,270 200,236 208,214"/>
    <polygon class="t" points="300,304 362,304 364,312 356,314 354,332 344,332 342,316 300,316"/>
    <polygon class="t" points="204,300 322,300 336,320 342,352 338,386 332,420 196,420 188,392 192,350 198,322"/>
    <polygon class="t" points="150,376 160,372 212,360 232,364 236,376 236,394 212,392 160,392 150,388"/>
    <polygon class="t" points="196,420 336,420 350,440 364,462 372,490 374,522 168,522 160,496 164,466 180,440"/>
    <polygon class="t" points="344,398 374,384 378,430 372,452 348,446"/>
    <polygon class="t" points="128,516 150,498 168,484 186,470 190,520 374,520 386,540 372,556 330,566 250,568 170,566 132,552 120,534"/>
    <polygon class="t" points="302,498 352,480 372,520 366,556 322,556 300,540"/>
    <polygon class="t" points="68,474 92,474 92,534 66,534"/>
    <polygon class="t" points="74,458 86,458 88,474 72,474"/>
    <polygon class="t" points="230,548 255,538 280,548 280,576 230,576"/>
    <polygon class="t" points="322,134 346,134 343,158 326,158"/>
    <polygon class="t" points="312,172 342,168 346,196 318,204"/>
    <polygon class="t" points="184,232 204,228 204,270 186,274"/>
    <polygon class="t" points="318,258 346,252 348,282 320,288"/>
    <polygon class="t" points="176,298 198,296 198,330 178,334"/>
    <polygon class="t" points="332,348 356,344 358,380 334,384"/>
    <polygon class="t" points="328,196 354,230 342,238 318,208"/>
    <polygon class="t" points="190,420 176,440 168,462 188,452"/>
    <polygon class="t" points="256,66 262,58 268,58 274,66"/>
    <rect class="t" x="264" y="38" width="2.4" height="38"/>
    <rect class="t" x="256" y="50" width="18" height="1.6"/>
    <rect class="t" x="259" y="58" width="12" height="1.6"/>
    <rect class="t" x="194" y="156" width="22" height="7"/>
    <rect class="t" x="190" y="170" width="26" height="7"/>
    <rect class="t" x="196" y="184" width="24" height="7"/>
    <rect class="t" x="200" y="196" width="22" height="6"/>
    <rect class="t" x="306" y="140" width="30" height="3"/>
    <rect class="t" x="333" y="126" width="2.4" height="8"/>
    <rect class="t" x="196" y="176" width="2" height="36"/>
    <rect class="t" x="203" y="186" width="2" height="30"/>
    <rect class="t" x="355" y="312" width="2" height="30"/>
    <rect class="t" x="348" y="318" width="2" height="22"/>
    <rect class="t" x="154" y="366" width="60" height="3"/>
    <rect class="t" x="62" y="534" width="36" height="6"/>
    <rect class="t" x="226" y="552" width="3" height="24"/>
    <rect class="t" x="282" y="552" width="3" height="24"/>
    <rect class="t" x="205" y="124" width="16" height="12"/>
    <rect class="t" x="186" y="206" width="12" height="8"/>
    <rect class="t" x="330" y="298" width="16" height="8"/>
    <rect class="t" x="190" y="330" width="8" height="26"/>
    <rect class="t" x="342" y="404" width="10" height="6"/>
    <rect class="t" x="205" y="556" width="2" height="20"/>
    <rect class="t" x="211" y="556" width="2" height="20"/>
    <rect class="t" x="217" y="556" width="2" height="20"/>
    <rect class="t" x="223" y="556" width="2" height="20"/>
    <rect class="t" x="288" y="556" width="2" height="20"/>
    <rect class="t" x="294" y="556" width="2" height="20"/>
    <rect class="t" x="300" y="556" width="2" height="20"/>
    <path class="t s" d="M160 392 C190 410 215 405 234 384" stroke-width="7" fill="none"/>
    <path class="t s" d="M247 58 L264 70 M283 58 L266 70 M252 46 L264 62 M278 46 L266 62" stroke-width="1.6" fill="none"/>
    <rect class="w f" x="240" y="125" width="5" height="7" rx=".8" style="--o:0.53;animation-delay:-4.9s"/>
    <rect class="w" x="264" y="125" width="5" height="7" rx=".8" style="opacity:0.62"/>
    <rect class="w" x="252" y="139" width="5" height="7" rx=".8" style="opacity:0.67"/>
    <rect class="w" x="264" y="139" width="5" height="7" rx=".8" style="opacity:0.75"/>
    <rect class="w" x="226" y="250" width="5" height="7" rx=".8" style="opacity:0.79"/>
    <rect class="w f" x="235" y="250" width="5" height="7" rx=".8" style="--o:0.73;animation-delay:-3.8s"/>
    <rect class="w" x="244" y="250" width="5" height="7" rx=".8" style="opacity:0.74"/>
    <rect class="w" x="253" y="250" width="5" height="7" rx=".8" style="opacity:0.56"/>
    <rect class="w" x="226" y="261" width="5" height="7" rx=".8" style="opacity:0.72"/>
    <rect class="w" x="235" y="261" width="5" height="7" rx=".8" style="opacity:0.52"/>
    <rect class="w f" x="244" y="261" width="5" height="7" rx=".8" style="--o:0.48;animation-delay:-5.0s"/>
    <rect class="w" x="253" y="261" width="5" height="7" rx=".8" style="opacity:0.65"/>
    <rect class="w" x="226" y="272" width="5" height="7" rx=".8" style="opacity:0.7"/>
    <rect class="w" x="235" y="272" width="5" height="7" rx=".8" style="opacity:0.74"/>
    <rect class="w" x="244" y="283" width="5" height="7" rx=".8" style="opacity:0.61"/>
    <rect class="w f" x="253" y="283" width="5" height="7" rx=".8" style="--o:0.72;animation-delay:-2.4s"/>
    <rect class="w" x="282" y="222" width="5" height="7" rx=".8" style="opacity:0.49"/>
    <rect class="w" x="291" y="222" width="5" height="7" rx=".8" style="opacity:0.72"/>
    <rect class="w" x="282" y="234" width="5" height="7" rx=".8" style="opacity:0.78"/>
    <rect class="w" x="291" y="234" width="5" height="7" rx=".8" style="opacity:0.75"/>
    <rect class="w f" x="300" y="246" width="5" height="7" rx=".8" style="--o:0.53;animation-delay:-3.7s"/>
    <rect class="w" x="282" y="258" width="5" height="7" rx=".8" style="opacity:0.71"/>
    <rect class="w" x="300" y="258" width="5" height="7" rx=".8" style="opacity:0.78"/>
    <rect class="w" x="212" y="330" width="5" height="7" rx=".8" style="opacity:0.62"/>
    <rect class="w" x="221" y="330" width="5" height="7" rx=".8" style="opacity:0.58"/>
    <rect class="w f" x="239" y="330" width="5" height="7" rx=".8" style="--o:0.79;animation-delay:-3.9s"/>
    <rect class="w" x="221" y="342" width="5" height="7" rx=".8" style="opacity:0.62"/>
    <rect class="w" x="239" y="342" width="5" height="7" rx=".8" style="opacity:0.55"/>
    <rect class="w" x="248" y="342" width="5" height="7" rx=".8" style="opacity:0.75"/>
    <rect class="w" x="248" y="354" width="5" height="7" rx=".8" style="opacity:0.62"/>
    <rect class="w f" x="300" y="340" width="5" height="7" rx=".8" style="--o:0.45;animation-delay:-2.6s"/>
    <rect class="w" x="309" y="340" width="5" height="7" rx=".8" style="opacity:0.77"/>
    <rect class="w" x="282" y="352" width="5" height="7" rx=".8" style="opacity:0.59"/>
    <rect class="w" x="291" y="352" width="5" height="7" rx=".8" style="opacity:0.76"/>
    <rect class="w" x="300" y="352" width="5" height="7" rx=".8" style="opacity:0.54"/>
    <rect class="w f" x="282" y="364" width="5" height="7" rx=".8" style="--o:0.51;animation-delay:-2.5s"/>
    <rect class="w" x="291" y="364" width="5" height="7" rx=".8" style="opacity:0.79"/>
    <rect class="w" x="300" y="364" width="5" height="7" rx=".8" style="opacity:0.63"/>
    <rect class="w" x="309" y="364" width="5" height="7" rx=".8" style="opacity:0.79"/>
    <rect class="w" x="227" y="430" width="5" height="7" rx=".8" style="opacity:0.54"/>
    <rect class="w f" x="249" y="430" width="5" height="7" rx=".8" style="--o:0.67;animation-delay:-2.0s"/>
    <rect class="w" x="260" y="430" width="5" height="7" rx=".8" style="opacity:0.55"/>
    <rect class="w" x="271" y="430" width="5" height="7" rx=".8" style="opacity:0.52"/>
    <rect class="w" x="205" y="444" width="5" height="7" rx=".8" style="opacity:0.46"/>
    <rect class="w" x="216" y="444" width="5" height="7" rx=".8" style="opacity:0.48"/>
    <rect class="w f" x="238" y="444" width="5" height="7" rx=".8" style="--o:0.69;animation-delay:-4.3s"/>
    <rect class="w" x="260" y="444" width="5" height="7" rx=".8" style="opacity:0.78"/>
    <rect class="w" x="271" y="444" width="5" height="7" rx=".8" style="opacity:0.61"/>
    <rect class="w" x="282" y="444" width="5" height="7" rx=".8" style="opacity:0.69"/>
    <rect class="w" x="216" y="458" width="5" height="7" rx=".8" style="opacity:0.74"/>
    <rect class="w f" x="227" y="458" width="5" height="7" rx=".8" style="--o:0.54;animation-delay:-1.9s"/>
    <rect class="w" x="271" y="458" width="5" height="7" rx=".8" style="opacity:0.77"/>
    <circle class="orb" cx="262" cy="165" r="12" style="animation-delay:-0.0s"/>
    <circle class="ring" cx="262" cy="165" r="15"/>
    <circle class="orb" cx="285" cy="208" r="4" style="animation-delay:-0.9s"/>
    <circle class="orb" cx="273" cy="213" r="3" style="animation-delay:-1.8s"/>
    <circle class="orb" cx="248" cy="330" r="10" style="animation-delay:-2.7s"/>
    <circle class="ring" cx="248" cy="330" r="13"/>
    <circle class="orb" cx="288" cy="353" r="9" style="animation-delay:-3.6s"/>
    <circle class="ring" cx="288" cy="353" r="12"/>
    <circle class="orb" cx="344" cy="433" r="12" style="animation-delay:-4.5s"/>
    <circle class="ring" cx="344" cy="433" r="15"/>
    <circle class="orb" cx="358" cy="430" r="7" style="animation-delay:-5.4s"/>
    <circle class="orb" cx="262" cy="488" r="14" style="animation-delay:-6.3s"/>
    <circle class="ring" cx="262" cy="488" r="17"/>
    <circle class="orb" cx="233" cy="512" r="8" style="animation-delay:-7.2s"/>
    <ellipse class="orb" cx="172" cy="505" rx="16" ry="22" style="animation-delay:-2.2s"/>
    <rect class="w" x="252" y="556" width="6" height="18" opacity=".8"/>
  </svg>

  <svg class="water-surface" viewBox="0 0 400 70" preserveAspectRatio="none" aria-hidden="true">
    <defs>
      <linearGradient id="waveGrad" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0" stop-color="#eef8ff" stop-opacity="0.9"/>
        <stop offset="0.5" stop-color="#51EEFC" stop-opacity="0.55"/>
        <stop offset="1" stop-color="#51EEFC" stop-opacity="0"/>
      </linearGradient>
    </defs>
    <path d="M0,35 C50,45 50,25 100,35 C150,45 150,25 200,35 C250,45 250,25 300,35 C350,45 350,25 400,35 L400,0 L0,0 Z" fill="url(#waveGrad)"/>
  </svg>

  <div class="caustics" aria-hidden="true"></div>
  <div class="sparkle" aria-hidden="true"></div>
  <div class="ripples" aria-hidden="true">
    <div class="ripple r1"></div>
    <div class="ripple r2"></div>
    <div class="ripple r3"></div>
    <div class="ripple r4"></div>
    <div class="ripple r5"></div>
    <div class="bubble b1"></div>
    <div class="bubble b2"></div>
    <div class="bubble b3"></div>
    <div class="bubble b4"></div>
    <div class="bubble b5"></div>
    <div class="bubble b6"></div>
  </div>

  <div class="app" id="app">
    <div class="month-num" id="monthNum" aria-hidden="true">10</div>
    <svg class="surface-light" id="surfaceLight" viewBox="0 0 320 110" aria-hidden="true">
      <path d="M96.1,24.0 C96.6,26.0 88.7,28.0 81.5,29.5 C74.4,31.0 61.3,32.8 53.3,32.9 C45.2,33.1 39.1,31.3 33.3,30.2 C27.5,29.1 23.2,27.8 18.5,26.3 C13.8,24.9 2.7,23.0 4.9,21.6 C7.0,20.2 22.8,19.1 31.4,17.9 C40.0,16.6 48.7,14.2 56.5,14.2 C64.4,14.1 72.0,15.6 78.6,17.3 C85.2,18.9 95.6,22.0 96.1,24.0 Z" fill="#86f7ff" fill-opacity="0.95"><animate attributeName="d" dur="8.5s" begin="-3.1s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.33;.66;1" keySplines=".45 0 .55 1;.45 0 .55 1;.45 0 .55 1" values="M96.1,24.0 C96.6,26.0 88.7,28.0 81.5,29.5 C74.4,31.0 61.3,32.8 53.3,32.9 C45.2,33.1 39.1,31.3 33.3,30.2 C27.5,29.1 23.2,27.8 18.5,26.3 C13.8,24.9 2.7,23.0 4.9,21.6 C7.0,20.2 22.8,19.1 31.4,17.9 C40.0,16.6 48.7,14.2 56.5,14.2 C64.4,14.1 72.0,15.6 78.6,17.3 C85.2,18.9 95.6,22.0 96.1,24.0 Z;M89.9,24.0 C90.3,25.6 92.7,26.9 86.7,28.2 C80.6,29.5 63.0,31.4 53.4,31.8 C43.8,32.1 34.6,31.1 29.0,30.3 C23.5,29.6 24.9,28.6 20.2,27.2 C15.5,25.8 -0.3,23.6 0.8,21.7 C1.8,19.7 17.5,16.6 26.4,15.5 C35.4,14.4 44.7,14.7 54.4,15.2 C64.1,15.7 78.7,17.0 84.6,18.5 C90.5,20.0 89.6,22.4 89.9,24.0 Z;M88.0,24.0 C87.7,25.5 88.7,27.3 82.9,28.4 C77.1,29.5 61.7,30.5 53.4,30.8 C45.0,31.1 38.8,30.7 32.8,30.0 C26.8,29.4 22.1,28.5 17.6,27.0 C13.1,25.6 4.9,23.3 5.9,21.3 C6.8,19.2 15.2,15.8 23.4,14.7 C31.5,13.6 44.5,13.9 54.7,14.7 C64.9,15.5 79.1,18.0 84.6,19.5 C90.2,21.1 88.2,22.5 88.0,24.0 Z;M96.1,24.0 C96.6,26.0 88.7,28.0 81.5,29.5 C74.4,31.0 61.3,32.8 53.3,32.9 C45.2,33.1 39.1,31.3 33.3,30.2 C27.5,29.1 23.2,27.8 18.5,26.3 C13.8,24.9 2.7,23.0 4.9,21.6 C7.0,20.2 22.8,19.1 31.4,17.9 C40.0,16.6 48.7,14.2 56.5,14.2 C64.4,14.1 72.0,15.6 78.6,17.3 C85.2,18.9 95.6,22.0 96.1,24.0 Z"/><animateTransform attributeName="transform" type="translate" dur="11.1s" begin="-4.2s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.5;1" keySplines=".45 0 .55 1;.45 0 .55 1" values="0 0;-5 -2;0 0"/></path>
      <path d="M222.4,50.0 C223.6,54.9 215.8,60.9 205.3,63.6 C194.8,66.3 175.1,65.6 159.5,66.2 C143.8,66.7 120.8,68.2 111.2,66.7 C101.6,65.2 107.7,61.0 101.9,57.3 C96.1,53.6 72.1,47.7 76.2,44.3 C80.2,40.9 111.7,40.0 126.2,36.9 C140.7,33.9 151.2,26.5 163.2,26.0 C175.2,25.6 188.3,30.3 198.2,34.3 C208.0,38.3 221.2,45.1 222.4,50.0 Z" fill="#86f7ff" fill-opacity="1"><animate attributeName="d" dur="10s" begin="-3.7s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.33;.66;1" keySplines=".45 0 .55 1;.45 0 .55 1;.45 0 .55 1" values="M222.4,50.0 C223.6,54.9 215.8,60.9 205.3,63.6 C194.8,66.3 175.1,65.6 159.5,66.2 C143.8,66.7 120.8,68.2 111.2,66.7 C101.6,65.2 107.7,61.0 101.9,57.3 C96.1,53.6 72.1,47.7 76.2,44.3 C80.2,40.9 111.7,40.0 126.2,36.9 C140.7,33.9 151.2,26.5 163.2,26.0 C175.2,25.6 188.3,30.3 198.2,34.3 C208.0,38.3 221.2,45.1 222.4,50.0 Z;M206.9,50.0 C209.5,55.3 216.6,62.8 208.9,67.0 C201.1,71.1 176.6,74.8 160.3,74.9 C144.1,75.0 123.8,70.4 111.4,67.3 C98.9,64.3 90.7,60.7 85.6,56.5 C80.5,52.3 74.3,46.5 80.8,42.3 C87.4,38.1 111.4,32.4 124.8,31.3 C138.2,30.1 149.8,34.8 161.2,35.4 C172.6,36.1 185.5,33.0 193.1,35.4 C200.8,37.8 204.3,44.7 206.9,50.0 Z;M215.1,50.0 C211.4,54.2 202.4,58.7 193.8,63.0 C185.2,67.4 177.5,75.6 163.4,76.1 C149.3,76.7 124.8,69.0 109.3,66.1 C93.8,63.3 77.0,63.4 70.2,59.2 C63.5,54.9 61.1,45.5 68.9,40.7 C76.7,35.9 101.9,31.9 116.9,30.6 C131.9,29.2 142.3,31.4 158.9,32.6 C175.4,33.8 206.8,35.1 216.2,38.0 C225.6,40.9 218.8,45.8 215.1,50.0 Z;M222.4,50.0 C223.6,54.9 215.8,60.9 205.3,63.6 C194.8,66.3 175.1,65.6 159.5,66.2 C143.8,66.7 120.8,68.2 111.2,66.7 C101.6,65.2 107.7,61.0 101.9,57.3 C96.1,53.6 72.1,47.7 76.2,44.3 C80.2,40.9 111.7,40.0 126.2,36.9 C140.7,33.9 151.2,26.5 163.2,26.0 C175.2,25.6 188.3,30.3 198.2,34.3 C208.0,38.3 221.2,45.1 222.4,50.0 Z"/><animateTransform attributeName="transform" type="translate" dur="13.0s" begin="-5.0s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.5;1" keySplines=".45 0 .55 1;.45 0 .55 1" values="0 0;5 2;0 0"/></path>
      <path d="M154.7,16.0 C153.4,17.8 140.6,20.2 134.3,22.1 C128.0,23.9 124.7,26.3 117.0,26.9 C109.3,27.6 94.8,27.2 88.2,25.9 C81.6,24.6 78.5,21.5 77.3,19.1 C76.2,16.8 78.4,14.0 81.5,12.0 C84.6,10.1 89.8,8.3 95.8,7.5 C101.8,6.8 109.7,6.9 117.5,7.5 C125.2,8.2 135.8,9.9 142.1,11.3 C148.3,12.7 156.0,14.2 154.7,16.0 Z" fill="#86f7ff" fill-opacity="0.9"><animate attributeName="d" dur="7.5s" begin="-2.8s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.33;.66;1" keySplines=".45 0 .55 1;.45 0 .55 1;.45 0 .55 1" values="M154.7,16.0 C153.4,17.8 140.6,20.2 134.3,22.1 C128.0,23.9 124.7,26.3 117.0,26.9 C109.3,27.6 94.8,27.2 88.2,25.9 C81.6,24.6 78.5,21.5 77.3,19.1 C76.2,16.8 78.4,14.0 81.5,12.0 C84.6,10.1 89.8,8.3 95.8,7.5 C101.8,6.8 109.7,6.9 117.5,7.5 C125.2,8.2 135.8,9.9 142.1,11.3 C148.3,12.7 156.0,14.2 154.7,16.0 Z;M158.6,16.0 C159.2,17.7 152.4,20.5 145.9,21.7 C139.4,22.8 127.5,22.3 119.5,22.9 C111.4,23.5 103.9,25.7 97.6,25.2 C91.3,24.6 84.2,21.5 81.4,19.5 C78.6,17.6 79.8,14.9 80.8,13.3 C81.9,11.7 81.2,10.9 87.8,10.0 C94.3,9.2 111.1,8.1 120.2,8.4 C129.3,8.6 135.9,10.3 142.2,11.6 C148.6,12.8 158.0,14.3 158.6,16.0 Z;M141.9,16.0 C142.4,17.8 152.9,21.3 149.3,22.5 C145.6,23.6 130.0,22.9 119.9,23.0 C109.8,23.2 96.4,24.2 88.6,23.4 C80.9,22.7 75.7,20.3 73.3,18.5 C70.8,16.7 71.4,13.9 73.9,12.4 C76.3,11.0 80.5,11.1 88.0,10.0 C95.5,8.9 109.1,5.4 118.7,5.7 C128.3,6.0 141.9,10.0 145.8,11.8 C149.7,13.5 141.3,14.2 141.9,16.0 Z;M154.7,16.0 C153.4,17.8 140.6,20.2 134.3,22.1 C128.0,23.9 124.7,26.3 117.0,26.9 C109.3,27.6 94.8,27.2 88.2,25.9 C81.6,24.6 78.5,21.5 77.3,19.1 C76.2,16.8 78.4,14.0 81.5,12.0 C84.6,10.1 89.8,8.3 95.8,7.5 C101.8,6.8 109.7,6.9 117.5,7.5 C125.2,8.2 135.8,9.9 142.1,11.3 C148.3,12.7 156.0,14.2 154.7,16.0 Z"/><animateTransform attributeName="transform" type="translate" dur="9.8s" begin="-3.8s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.5;1" keySplines=".45 0 .55 1;.45 0 .55 1" values="0 0;-5 2;0 0"/></path>
      <path d="M268.9,40.0 C267.4,42.4 270.3,47.1 266.5,48.7 C262.6,50.2 253.8,48.4 245.8,49.3 C237.7,50.2 227.8,54.6 218.2,53.8 C208.7,53.0 191.6,47.5 188.6,44.6 C185.6,41.7 196.1,39.5 200.2,36.4 C204.2,33.3 205.3,27.6 212.9,26.0 C220.5,24.3 235.3,25.3 245.7,26.6 C256.2,28.0 271.7,31.8 275.5,34.0 C279.4,36.2 270.4,37.6 268.9,40.0 Z" fill="#86f7ff" fill-opacity="0.95"><animate attributeName="d" dur="9s" begin="-3.3s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.33;.66;1" keySplines=".45 0 .55 1;.45 0 .55 1;.45 0 .55 1" values="M268.9,40.0 C267.4,42.4 270.3,47.1 266.5,48.7 C262.6,50.2 253.8,48.4 245.8,49.3 C237.7,50.2 227.8,54.6 218.2,53.8 C208.7,53.0 191.6,47.5 188.6,44.6 C185.6,41.7 196.1,39.5 200.2,36.4 C204.2,33.3 205.3,27.6 212.9,26.0 C220.5,24.3 235.3,25.3 245.7,26.6 C256.2,28.0 271.7,31.8 275.5,34.0 C279.4,36.2 270.4,37.6 268.9,40.0 Z;M293.5,40.0 C294.9,42.9 282.5,48.0 274.7,50.1 C266.9,52.1 257.5,52.1 246.8,52.1 C236.0,52.0 219.0,51.2 210.3,49.8 C201.6,48.4 198.4,46.5 194.6,43.9 C190.8,41.3 183.2,36.7 187.5,34.2 C191.8,31.8 210.7,30.1 220.2,28.9 C229.8,27.8 237.0,26.7 244.7,27.3 C252.4,28.0 258.2,30.7 266.4,32.9 C274.5,35.0 292.1,37.1 293.5,40.0 Z;M292.4,40.0 C290.8,42.8 269.2,48.6 261.8,50.9 C254.3,53.1 255.1,53.8 247.7,53.4 C240.2,53.1 225.4,50.2 217.1,48.7 C208.8,47.2 201.4,46.7 198.0,44.6 C194.6,42.5 193.2,39.2 196.7,36.0 C200.2,32.9 210.4,26.5 218.9,25.5 C227.4,24.6 238.9,28.9 247.7,30.3 C256.5,31.7 264.4,32.3 271.9,33.9 C279.3,35.5 294.1,37.2 292.4,40.0 Z;M268.9,40.0 C267.4,42.4 270.3,47.1 266.5,48.7 C262.6,50.2 253.8,48.4 245.8,49.3 C237.7,50.2 227.8,54.6 218.2,53.8 C208.7,53.0 191.6,47.5 188.6,44.6 C185.6,41.7 196.1,39.5 200.2,36.4 C204.2,33.3 205.3,27.6 212.9,26.0 C220.5,24.3 235.3,25.3 245.7,26.6 C256.2,28.0 271.7,31.8 275.5,34.0 C279.4,36.2 270.4,37.6 268.9,40.0 Z"/><animateTransform attributeName="transform" type="translate" dur="11.7s" begin="-4.5s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.5;1" keySplines=".45 0 .55 1;.45 0 .55 1" values="0 0;5 -2;0 0"/></path>
      <path d="M311.6,30.0 C312.1,31.6 312.6,33.5 310.0,35.0 C307.4,36.4 301.1,38.2 296.2,38.8 C291.3,39.5 285.7,39.9 280.5,38.9 C275.3,38.0 267.3,35.3 264.9,33.3 C262.5,31.2 263.9,28.3 266.3,26.5 C268.6,24.7 274.0,22.9 279.0,22.3 C284.0,21.8 291.7,22.9 296.3,23.4 C300.9,23.9 304.1,24.2 306.7,25.3 C309.2,26.4 311.0,28.4 311.6,30.0 Z" fill="#86f7ff" fill-opacity="0.9"><animate attributeName="d" dur="7s" begin="-2.6s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.33;.66;1" keySplines=".45 0 .55 1;.45 0 .55 1;.45 0 .55 1" values="M311.6,30.0 C312.1,31.6 312.6,33.5 310.0,35.0 C307.4,36.4 301.1,38.2 296.2,38.8 C291.3,39.5 285.7,39.9 280.5,38.9 C275.3,38.0 267.3,35.3 264.9,33.3 C262.5,31.2 263.9,28.3 266.3,26.5 C268.6,24.7 274.0,22.9 279.0,22.3 C284.0,21.8 291.7,22.9 296.3,23.4 C300.9,23.9 304.1,24.2 306.7,25.3 C309.2,26.4 311.0,28.4 311.6,30.0 Z;M323.4,30.0 C323.8,32.0 314.6,36.0 310.3,37.4 C306.0,38.8 303.3,38.3 297.6,38.7 C291.9,39.0 280.4,40.4 276.0,39.4 C271.6,38.4 271.8,34.7 271.2,32.7 C270.7,30.6 270.9,29.1 272.7,27.2 C274.4,25.4 277.8,22.7 281.6,21.5 C285.5,20.3 291.5,19.2 295.8,19.9 C300.2,20.6 303.2,24.0 307.8,25.7 C312.4,27.4 323.0,28.0 323.4,30.0 Z;M314.3,30.0 C314.9,31.9 318.9,33.4 316.0,35.3 C313.0,37.1 303.0,40.8 296.7,41.2 C290.4,41.5 282.2,38.4 278.1,37.2 C273.9,36.0 273.8,35.7 271.8,33.9 C269.7,32.1 264.7,28.1 265.6,26.3 C266.5,24.6 271.7,24.2 276.9,23.5 C282.2,22.7 291.0,21.9 296.9,22.0 C302.8,22.1 309.5,22.8 312.4,24.2 C315.3,25.5 313.7,28.1 314.3,30.0 Z;M311.6,30.0 C312.1,31.6 312.6,33.5 310.0,35.0 C307.4,36.4 301.1,38.2 296.2,38.8 C291.3,39.5 285.7,39.9 280.5,38.9 C275.3,38.0 267.3,35.3 264.9,33.3 C262.5,31.2 263.9,28.3 266.3,26.5 C268.6,24.7 274.0,22.9 279.0,22.3 C284.0,21.8 291.7,22.9 296.3,23.4 C300.9,23.9 304.1,24.2 306.7,25.3 C309.2,26.4 311.0,28.4 311.6,30.0 Z"/><animateTransform attributeName="transform" type="translate" dur="9.1s" begin="-3.5s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.5;1" keySplines=".45 0 .55 1;.45 0 .55 1" values="0 0;-5 2;0 0"/></path>
      <path d="M249.3,84.0 C250.5,85.5 245.9,87.8 239.5,89.0 C233.1,90.3 221.9,91.6 211.0,91.4 C200.1,91.3 184.2,89.0 174.3,88.1 C164.5,87.2 156.4,87.3 152.0,86.2 C147.5,85.1 142.9,82.4 147.4,81.4 C152.0,80.4 168.3,80.7 179.2,80.3 C190.1,80.0 204.0,79.4 212.9,79.3 C221.8,79.2 226.4,79.0 232.5,79.7 C238.6,80.5 248.2,82.5 249.3,84.0 Z" fill="#86f7ff" fill-opacity="0.8"><animate attributeName="d" dur="11s" begin="-4.1s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.33;.66;1" keySplines=".45 0 .55 1;.45 0 .55 1;.45 0 .55 1" values="M249.3,84.0 C250.5,85.5 245.9,87.8 239.5,89.0 C233.1,90.3 221.9,91.6 211.0,91.4 C200.1,91.3 184.2,89.0 174.3,88.1 C164.5,87.2 156.4,87.3 152.0,86.2 C147.5,85.1 142.9,82.4 147.4,81.4 C152.0,80.4 168.3,80.7 179.2,80.3 C190.1,80.0 204.0,79.4 212.9,79.3 C221.8,79.2 226.4,79.0 232.5,79.7 C238.6,80.5 248.2,82.5 249.3,84.0 Z;M250.1,84.0 C248.7,85.4 245.5,87.7 239.3,88.4 C233.2,89.1 222.1,88.4 213.1,88.3 C204.2,88.2 192.4,88.2 185.7,87.9 C178.9,87.6 175.1,87.6 172.6,86.6 C170.2,85.6 169.1,83.5 171.1,82.0 C173.1,80.5 177.7,78.5 184.6,77.7 C191.5,76.8 201.9,76.5 212.5,76.9 C223.1,77.3 241.8,78.9 248.1,80.1 C254.3,81.3 251.6,82.6 250.1,84.0 Z;M249.5,84.0 C250.1,85.5 250.8,87.4 244.6,88.1 C238.4,88.9 223.7,88.3 212.3,88.5 C201.0,88.7 187.4,89.9 176.5,89.4 C165.6,88.9 151.1,86.7 146.9,85.5 C142.8,84.3 146.5,82.8 151.6,81.9 C156.6,81.1 166.9,80.9 177.3,80.3 C187.7,79.7 203.4,78.5 214.0,78.3 C224.6,78.2 234.9,78.4 240.9,79.3 C246.8,80.3 248.8,82.5 249.5,84.0 Z;M249.3,84.0 C250.5,85.5 245.9,87.8 239.5,89.0 C233.1,90.3 221.9,91.6 211.0,91.4 C200.1,91.3 184.2,89.0 174.3,88.1 C164.5,87.2 156.4,87.3 152.0,86.2 C147.5,85.1 142.9,82.4 147.4,81.4 C152.0,80.4 168.3,80.7 179.2,80.3 C190.1,80.0 204.0,79.4 212.9,79.3 C221.8,79.2 226.4,79.0 232.5,79.7 C238.6,80.5 248.2,82.5 249.3,84.0 Z"/><animateTransform attributeName="transform" type="translate" dur="14.3s" begin="-5.5s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.5;1" keySplines=".45 0 .55 1;.45 0 .55 1" values="0 0;5 2;0 0"/></path>
      <path d="M114.6,72.0 C113.1,73.3 109.7,74.7 106.6,75.7 C103.5,76.6 100.8,77.5 95.8,77.6 C90.7,77.6 80.9,76.7 76.2,76.0 C71.5,75.3 69.6,74.3 67.6,73.3 C65.7,72.4 62.5,71.2 64.8,70.3 C67.0,69.4 75.6,68.5 81.1,68.1 C86.6,67.8 91.9,68.4 97.7,68.3 C103.4,68.3 113.1,67.3 115.9,67.9 C118.7,68.5 116.2,70.7 114.6,72.0 Z" fill="#86f7ff" fill-opacity="0.75"><animate attributeName="d" dur="8s" begin="-3.0s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.33;.66;1" keySplines=".45 0 .55 1;.45 0 .55 1;.45 0 .55 1" values="M114.6,72.0 C113.1,73.3 109.7,74.7 106.6,75.7 C103.5,76.6 100.8,77.5 95.8,77.6 C90.7,77.6 80.9,76.7 76.2,76.0 C71.5,75.3 69.6,74.3 67.6,73.3 C65.7,72.4 62.5,71.2 64.8,70.3 C67.0,69.4 75.6,68.5 81.1,68.1 C86.6,67.8 91.9,68.4 97.7,68.3 C103.4,68.3 113.1,67.3 115.9,67.9 C118.7,68.5 116.2,70.7 114.6,72.0 Z;M120.6,72.0 C119.5,73.2 112.1,75.1 108.3,76.0 C104.5,76.9 103.0,77.2 97.8,77.5 C92.7,77.8 83.3,78.3 77.4,77.6 C71.5,76.9 64.0,74.5 62.3,73.3 C60.6,72.0 64.8,70.9 67.1,70.1 C69.3,69.3 70.9,69.2 75.8,68.4 C80.7,67.7 89.9,65.6 96.4,65.7 C103.0,65.8 110.9,67.9 114.9,68.9 C119.0,70.0 121.7,70.8 120.6,72.0 Z;M110.4,72.0 C109.8,72.9 110.8,74.0 108.5,75.0 C106.2,76.1 101.4,77.8 96.8,78.1 C92.2,78.5 84.8,77.8 80.9,77.1 C77.1,76.4 76.7,74.9 73.6,73.7 C70.5,72.5 61.7,71.1 62.3,70.0 C62.9,69.0 71.4,67.9 77.2,67.4 C83.0,66.8 91.5,66.3 97.3,66.7 C103.1,67.1 109.9,68.7 112.1,69.6 C114.2,70.5 111.0,71.1 110.4,72.0 Z;M114.6,72.0 C113.1,73.3 109.7,74.7 106.6,75.7 C103.5,76.6 100.8,77.5 95.8,77.6 C90.7,77.6 80.9,76.7 76.2,76.0 C71.5,75.3 69.6,74.3 67.6,73.3 C65.7,72.4 62.5,71.2 64.8,70.3 C67.0,69.4 75.6,68.5 81.1,68.1 C86.6,67.8 91.9,68.4 97.7,68.3 C103.4,68.3 113.1,67.3 115.9,67.9 C118.7,68.5 116.2,70.7 114.6,72.0 Z"/><animateTransform attributeName="transform" type="translate" dur="10.4s" begin="-4.0s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.5;1" keySplines=".45 0 .55 1;.45 0 .55 1" values="0 0;-5 -2;0 0"/></path>
      <path d="M218.5,18.0 C218.0,19.5 218.0,21.1 214.2,22.2 C210.3,23.2 202.0,23.9 195.4,24.3 C188.8,24.6 179.0,25.0 174.5,24.2 C170.1,23.5 171.7,21.2 168.8,19.7 C165.9,18.2 155.6,16.9 157.0,15.4 C158.4,13.8 171.3,11.4 177.5,10.4 C183.6,9.5 187.2,9.1 193.8,9.6 C200.4,10.1 213.1,11.9 217.2,13.3 C221.3,14.7 219.0,16.5 218.5,18.0 Z" fill="#86f7ff" fill-opacity="0.85"><animate attributeName="d" dur="6.5s" begin="-2.4s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.33;.66;1" keySplines=".45 0 .55 1;.45 0 .55 1;.45 0 .55 1" values="M218.5,18.0 C218.0,19.5 218.0,21.1 214.2,22.2 C210.3,23.2 202.0,23.9 195.4,24.3 C188.8,24.6 179.0,25.0 174.5,24.2 C170.1,23.5 171.7,21.2 168.8,19.7 C165.9,18.2 155.6,16.9 157.0,15.4 C158.4,13.8 171.3,11.4 177.5,10.4 C183.6,9.5 187.2,9.1 193.8,9.6 C200.4,10.1 213.1,11.9 217.2,13.3 C221.3,14.7 219.0,16.5 218.5,18.0 Z;M222.5,18.0 C221.7,19.6 217.4,22.7 212.7,23.8 C208.1,24.9 200.7,24.7 194.5,24.5 C188.3,24.3 179.9,23.4 175.7,22.7 C171.6,22.0 172.6,21.7 169.4,20.5 C166.2,19.2 155.4,16.8 156.4,15.4 C157.4,13.9 168.8,12.3 175.4,11.5 C181.9,10.7 188.8,9.9 195.8,10.4 C202.7,10.8 212.8,13.0 217.2,14.3 C221.7,15.6 223.2,16.4 222.5,18.0 Z;M219.1,18.0 C220.2,19.5 218.1,21.9 214.4,23.4 C210.6,24.9 202.7,26.7 196.4,26.9 C190.2,27.0 183.5,25.6 177.0,24.5 C170.6,23.5 159.1,22.0 157.8,20.5 C156.4,19.0 165.5,16.8 168.9,15.3 C172.4,13.9 174.1,12.5 178.5,11.6 C183.0,10.7 190.9,9.6 195.8,10.1 C200.7,10.5 204.1,13.1 208.0,14.4 C211.9,15.8 218.1,16.5 219.1,18.0 Z;M218.5,18.0 C218.0,19.5 218.0,21.1 214.2,22.2 C210.3,23.2 202.0,23.9 195.4,24.3 C188.8,24.6 179.0,25.0 174.5,24.2 C170.1,23.5 171.7,21.2 168.8,19.7 C165.9,18.2 155.6,16.9 157.0,15.4 C158.4,13.8 171.3,11.4 177.5,10.4 C183.6,9.5 187.2,9.1 193.8,9.6 C200.4,10.1 213.1,11.9 217.2,13.3 C221.3,14.7 219.0,16.5 218.5,18.0 Z"/><animateTransform attributeName="transform" type="translate" dur="8.5s" begin="-3.2s" repeatCount="indefinite" calcMode="spline" keyTimes="0;.5;1" keySplines=".45 0 .55 1;.45 0 .55 1" values="0 0;5 2;0 0"/></path>
    </svg>

    <button class="moon" id="moonBtn" type="button" aria-label="Fase lunar">
      <svg viewBox="-20 -20 40 40" aria-hidden="true">
        <defs><clipPath id="moonClip"><circle r="17"/></clipPath></defs>
        <circle class="moon-dark" r="17"/>
        <g clip-path="url(#moonClip)"><path class="moon-lit" id="moonLit" d=""/></g>
      </svg>
    </button>

    <header class="topbar">
      <button class="nav-btn" id="prevBtn" aria-label="Mes anterior">‹</button>
      <div class="month-display" id="monthDisplay">
        <span class="month-name" id="monthName">OCTUBRE</span>
        <span class="year-name" id="yearName">2026</span>
        <span class="dark-tag" id="darkTag" aria-hidden="true">HORA OSCURA</span>
      </div>
      <button class="nav-btn" id="nextBtn" aria-label="Mes siguiente">›</button>
    </header>

    <div class="weekdays">
      <span class="wd sun">DOM</span>
      <span class="wd">LUN</span>
      <span class="wd">MAR</span>
      <span class="wd">MIÉ</span>
      <span class="wd">JUE</span>
      <span class="wd">VIE</span>
      <span class="wd sat">SÁB</span>
    </div>

    <div class="grid" id="grid" role="grid" aria-label="Días del mes"></div>
  </div>
</div>

<svg class="frame-rim" id="frameRim" aria-hidden="true">
  <defs>
    <filter id="rimBlur" x="-5%" y="-100%" width="110%" height="300%"><feGaussianBlur stdDeviation="5"/></filter>
  </defs>
  <path data-arc="top" fill="none" stroke="#51EEFC" stroke-opacity="0.45" stroke-width="12" filter="url(#rimBlur)"/>
  <path data-arc="top" fill="none" stroke="#eef8ff" stroke-opacity="0.8" stroke-width="2.5"/>
  <path data-arc="bot" fill="none" stroke="#51EEFC" stroke-opacity="0.2" stroke-width="12" filter="url(#rimBlur)"/>
  <path data-arc="bot" fill="none" stroke="#eef8ff" stroke-opacity="0.4" stroke-width="2"/>
</svg>
<div id="safeProbe" aria-hidden="true" style="position:fixed;top:0;left:0;width:0;height:0;visibility:hidden;padding:env(safe-area-inset-top,0px) env(safe-area-inset-right,0px) env(safe-area-inset-bottom,0px) env(safe-area-inset-left,0px)"></div>

<button class="arcana-btn" id="arcanaBtn" type="button" aria-label="Compendio de arcanas">
  <svg viewBox="0 0 24 24" aria-hidden="true">
    <rect x="3" y="2" width="18" height="20" rx="3" fill="none" stroke="currentColor" stroke-width="1.6"/>
    <circle cx="12" cy="9" r="2.6" fill="currentColor"/>
    <path d="M7 17 Q12 13 17 17" stroke="currentColor" stroke-width="1.6" fill="none" stroke-linecap="round"/>
  </svg>
</button>

<button class="today-btn" id="todayBtn">Hoy</button>

<div class="veil" id="veil" aria-hidden="true"></div>
<div class="toast" id="toast" role="status" aria-live="polite"></div>
<div class="overlay" id="overlay"></div>
<div class="sheet" id="sheet" role="dialog" aria-modal="true" aria-labelledby="sheetDate">
  <div class="sheet-handle"></div>
  <div class="sheet-header">
    <div>
      <span class="wd-label" id="sheetWeekday">MIÉRCOLES</span>
      <h2 id="sheetDate">14 de octubre</h2>
    </div>
    <button class="close-btn" id="closeBtn" aria-label="Cerrar">✕</button>
  </div>
  <div id="meetCardSlot"></div>
  <ul class="notes-list" id="notesList"></ul>
  <section class="dark-note" id="darkNote" hidden>
    <label class="dark-note-label" for="darkNoteInput">NOTA DE LA HORA OSCURA <span class="dn-status" id="darkNoteStatus">guardada</span></label>
    <textarea id="darkNoteInput" rows="3" maxlength="500" placeholder="Solo existe entre las 00:00 y la 01:00…"></textarea>
  </section>
  <form class="note-form" id="noteForm">
    <input type="text" id="noteInput" placeholder="Escribe un evento o nota…" maxlength="140" autocomplete="off">
    <button type="submit" id="addBtn">Añadir</button>
  </form>
</div>

<div class="overlay" id="overlay2"></div>

<div class="sheet arcana-sheet" id="arcanaSheet" role="dialog" aria-modal="true" aria-labelledby="arcanaTitle">
  <div class="sheet-handle"></div>
  <div class="sheet-header">
    <div>
      <span class="wd-label" id="arcanaEyebrow">COMPENDIO</span>
      <h2 id="arcanaTitle">Arcanas</h2>
    </div>
    <div style="display:flex;gap:8px;">
      <button class="close-btn" id="arcanaBackBtn" aria-label="Volver" hidden>‹</button>
      <button class="close-btn" id="arcanaCloseBtn" aria-label="Cerrar">✕</button>
    </div>
  </div>
  <div class="arcana-list" id="arcanaList"></div>
  <div class="arcana-detail" id="arcanaDetail" hidden>
    <div class="arcana-detail-img" id="arcanaDetailImg"></div>
    <div class="arcana-detail-name" id="arcanaDetailName"></div>
    <div>
      <label class="arcana-name-label" for="arcanaNameInput">NOMBRE ASIGNADO <span class="dn-status" id="arcanaNameStatus">guardado</span></label>
      <input type="text" id="arcanaNameInput" placeholder="Nombre de la persona" maxlength="40" autocomplete="off" style="margin-top:8px;">
    </div>
    <div>
      <div class="arcana-level-row" id="arcanaLevelRow"></div>
      <p class="arcana-level-label">Nivel de confidente: <span id="arcanaLevelText">0</span>/10</p>
    </div>
  </div>
</div>

<div class="sheet" id="dayMenuSheet" role="dialog" aria-modal="true" aria-labelledby="dayMenuDate">
  <div class="sheet-handle"></div>
  <div class="sheet-header">
    <div>
      <span class="wd-label" id="dayMenuWeekday">MIÉRCOLES</span>
      <h2 id="dayMenuDate">14 de octubre</h2>
    </div>
    <button class="close-btn" id="dayMenuCloseBtn" aria-label="Cerrar">✕</button>
  </div>
  <div class="day-menu-options">
    <button type="button" class="day-menu-opt" id="optNote">
      <span class="opt-ic">✎</span><span>Dejar nota</span>
    </button>
    <button type="button" class="day-menu-opt" id="optMeet">
      <span class="opt-ic">☽</span><span>Marcar quedada</span>
    </button>
  </div>
</div>

<div class="sheet" id="meetupSheet" role="dialog" aria-modal="true" aria-labelledby="meetupDateLabel">
  <div class="sheet-handle"></div>
  <div class="sheet-header">
    <div>
      <span class="wd-label" id="meetupWeekday">MIÉRCOLES</span>
      <h2 id="meetupDateLabel">Quedada</h2>
    </div>
    <button class="close-btn" id="meetupCloseBtn" aria-label="Cerrar">✕</button>
  </div>
  <button type="button" class="meetup-clear-btn" id="meetupClearBtn" hidden>Quitar quedada de este día</button>
  <div class="arcana-list" id="meetupList"></div>
  <p class="meetup-empty" id="meetupEmptyMsg" hidden>Todavía no hay ninguna arcana con nombre asignado. Abre el Compendio de arcanas para asignar uno.</p>
</div>

<div class="overlay" id="confirmOverlay"></div>
<div class="confirm-card" id="confirmCard" role="alertdialog" aria-modal="true" aria-labelledby="confirmTitle">
  <div class="confirm-thumb" id="confirmThumb"></div>
  <h3 id="confirmTitle">¿Se realizó la quedada?</h3>
  <p>Con <strong id="confirmPerson"></strong> — <span id="confirmArcanaName"></span>, el <span id="confirmDate"></span>.</p>
  <div class="confirm-actions">
    <button type="button" class="confirm-btn no" id="confirmNoBtn">No</button>
    <button type="button" class="confirm-btn yes" id="confirmYesBtn">Sí</button>
  </div>
</div>

<script>
(function () {
  "use strict";

  var MONTHS = ["ENERO","FEBRERO","MARZO","ABRIL","MAYO","JUNIO","JULIO","AGOSTO","SEPTIEMBRE","OCTUBRE","NOVIEMBRE","DICIEMBRE"];
  var WEEKDAYS_LONG = ["DOMINGO","LUNES","MARTES","MIÉRCOLES","JUEVES","VIERNES","SÁBADO"];
  var MONTHS_LONG = ["enero","febrero","marzo","abril","mayo","junio","julio","agosto","septiembre","octubre","noviembre","diciembre"];
  var STORAGE_KEY = "calendario-marea-notes-v1";

  var today = new Date();
  var view = { year: today.getFullYear(), month: today.getMonth() };
  var selectedKey = null;
  var notes = loadNotes();
  var DARK_KEY = "calendario-marea-oscura-v1";
  var darkNotes = loadDarkNotes();
  var darkOn = false;

  var app = document.getElementById("app");
  var monthNum = document.getElementById("monthNum");
  var monthDisplay = document.getElementById("monthDisplay");
  var surfaceLight = document.getElementById("surfaceLight");
  var capBottom = 64;
  var grid = document.getElementById("grid");
  var monthName = document.getElementById("monthName");
  var yearName = document.getElementById("yearName");
  var prevBtn = document.getElementById("prevBtn");
  var nextBtn = document.getElementById("nextBtn");
  var todayBtn = document.getElementById("todayBtn");
  var overlay = document.getElementById("overlay");
  var sheet = document.getElementById("sheet");
  var closeBtn = document.getElementById("closeBtn");
  var sheetDate = document.getElementById("sheetDate");
  var sheetWeekday = document.getElementById("sheetWeekday");
  var notesList = document.getElementById("notesList");
  var noteForm = document.getElementById("noteForm");
  var noteInput = document.getElementById("noteInput");

  function loadNotes() {
    try {
      var raw = window.localStorage.getItem(STORAGE_KEY);
      return raw ? JSON.parse(raw) : {};
    } catch (e) {
      return {};
    }
  }

  function loadDarkNotes() {
    try {
      var raw = window.localStorage.getItem(DARK_KEY);
      return raw ? JSON.parse(raw) : {};
    } catch (e) {
      return {};
    }
  }

  function saveDarkNotes() {
    try {
      window.localStorage.setItem(DARK_KEY, JSON.stringify(darkNotes));
    } catch (e) { /* sin almacenamiento: solo dura la sesión */ }
  }

  function saveNotes() {
    try {
      window.localStorage.setItem(STORAGE_KEY, JSON.stringify(notes));
    } catch (e) {
      /* storage unavailable; state still works for this session */
    }
  }

  /* ---------- arcanas y quedadas (confidentes) ---------- */
  var ARC_KEY = "calendario-marea-arcanas-v1";
  var MEET_KEY = "calendario-marea-quedadas-v1";

  var ARCANA = [
    { id: 1,  roman: "I",     name: "El Mago",               img: "arcana/01.webp" },
    { id: 2,  roman: "II",    name: "La Sacerdotisa",         img: "arcana/02.webp" },
    { id: 3,  roman: "III",   name: "La Emperatriz",          img: "arcana/03.webp" },
    { id: 4,  roman: "IV",    name: "El Emperador",           img: "arcana/04.webp" },
    { id: 5,  roman: "V",     name: "El Hierofante",          img: "arcana/05.webp" },
    { id: 6,  roman: "VI",    name: "Los Enamorados",         img: "arcana/06.webp" },
    { id: 7,  roman: "VII",   name: "El Carro",               img: "arcana/07.webp" },
    { id: 8,  roman: "VIII",  name: "La Justicia",            img: "arcana/08.webp" },
    { id: 9,  roman: "IX",    name: "El Ermitaño",            img: "arcana/09.webp" },
    { id: 10, roman: "X",     name: "La Rueda de la Fortuna", img: "arcana/10.webp" },
    { id: 11, roman: "XI",    name: "La Fuerza",              img: "arcana/11.webp" },
    { id: 12, roman: "XII",   name: "El Colgado",             img: "arcana/12.webp" },
    { id: 13, roman: "XIII",  name: "La Muerte",              img: "arcana/13.webp" },
    { id: 14, roman: "XIV",   name: "La Templanza",           img: "arcana/14.webp" },
    { id: 15, roman: "XV",    name: "El Diablo",              img: "arcana/15.webp" },
    { id: 16, roman: "XVI",   name: "La Torre",               img: "arcana/16.webp" },
    { id: 17, roman: "XVII",  name: "La Estrella",            img: "arcana/17.webp" },
    { id: 18, roman: "XVIII", name: "La Luna",                img: "arcana/18.webp" },
    { id: 19, roman: "XIX",   name: "El Sol",                 img: "arcana/19.webp" },
    { id: 20, roman: "XX",    name: "El Juicio",              img: "arcana/20.webp" }
  ];
  var ARCANA_BY_ID = {};
  ARCANA.forEach(function (a) { ARCANA_BY_ID[a.id] = a; });

  function loadArcanaData() {
    try {
      var raw = window.localStorage.getItem(ARC_KEY);
      return raw ? JSON.parse(raw) : {};
    } catch (e) { return {}; }
  }
  function saveArcanaData() {
    try { window.localStorage.setItem(ARC_KEY, JSON.stringify(arcanaData)); } catch (e) { /* solo dura la sesión */ }
  }
  function loadMeetups() {
    try {
      var raw = window.localStorage.getItem(MEET_KEY);
      return raw ? JSON.parse(raw) : {};
    } catch (e) { return {}; }
  }
  function saveMeetups() {
    try { window.localStorage.setItem(MEET_KEY, JSON.stringify(meetups)); } catch (e) { /* solo dura la sesión */ }
  }
  var arcanaData = loadArcanaData();
  var meetups = loadMeetups();

  function getArcanaData(id) {
    if (!arcanaData[id]) arcanaData[id] = { name: "", level: 0 };
    return arcanaData[id];
  }

  // pinta la miniatura de la arcana y su marca de estado sobre un día
  function updateMeetupVisual(cell) {
    var key = cell.dataset.key;
    var m = meetups[key];
    var num = cell.querySelector(".num");
    cell.classList.toggle("has-meet", !!m);
    var badge = cell.querySelector(".meet-badge");
    if (m) {
      var def = ARCANA_BY_ID[m.arcanaId];
      if (def && num) num.style.setProperty("--meet-img", 'url("' + def.img + '")');
      if (!badge) {
        badge = document.createElement("span");
        badge.className = "meet-badge";
        cell.appendChild(badge);
      }
      badge.className = "meet-badge " + m.status;
    } else {
      if (num) num.style.removeProperty("--meet-img");
      if (badge) badge.remove();
    }
  }

  function pad(n) { return n < 10 ? "0" + n : "" + n; }

  function dateKey(y, m, d) {
    return y + "-" + pad(m + 1) + "-" + pad(d);
  }

  function isToday(y, m, d) {
    return y === today.getFullYear() && m === today.getMonth() && d === today.getDate();
  }

  function renderCalendar() {
    monthName.textContent = MONTHS[view.month];
    yearName.textContent = String(view.year);
    monthNum.textContent = String(view.month + 1);
    grid.innerHTML = "";

    var firstDow = new Date(view.year, view.month, 1).getDay();
    var daysInMonth = new Date(view.year, view.month + 1, 0).getDate();

    for (var i = 0; i < firstDow; i++) {
      var pad0 = document.createElement("div");
      pad0.className = "day pad";
      grid.appendChild(pad0);
    }

    for (var d = 1; d <= daysInMonth; d++) {
      var dow = new Date(view.year, view.month, d).getDay();
      var key = dateKey(view.year, view.month, d);
      var cell = document.createElement("button");
      cell.type = "button";
      cell.className = "day";
      if (dow === 0) cell.classList.add("sun");
      if (dow === 6) cell.classList.add("sat");
      if (isToday(view.year, view.month, d)) cell.classList.add("today");
      if (notes[key] && notes[key].length) cell.classList.add("has-note");
      if (darkOn && darkNotes[key]) cell.classList.add("has-dark");
      if (key === selectedKey) cell.classList.add("selected");
      cell.dataset.key = key;
      cell.dataset.day = String(d);
      cell.setAttribute("aria-label", d + " de " + MONTHS_LONG[view.month]);

      var num = document.createElement("span");
      num.className = "num";
      num.textContent = String(d);
      cell.appendChild(num);
      updateMeetupVisual(cell);

      grid.appendChild(cell);
    }
  }

  function openSheet(key, day) {
    selectedKey = key;
    refreshCells();

    var parts = key.split("-").map(Number);
    var y = parts[0], m = parts[1] - 1;
    var dow = new Date(y, m, day).getDay();

    sheetWeekday.textContent = WEEKDAYS_LONG[dow];
    sheetDate.textContent = day + " de " + MONTHS_LONG[m];

    renderNotes(key, "open");
    renderMeetupCard(key);
    overlay.classList.add("open");
    sheet.classList.add("open");
    syncDarkNote();
    noteInput.value = "";
    setTimeout(function () { noteInput.focus(); }, 200);
  }

  function closeSheet() {
    overlay.classList.remove("open");
    sheet.classList.remove("open");
    flushDarkNote();
    selectedKey = null;
    refreshCells();
    setTimeout(function () { if (!sheet.classList.contains("open")) syncDarkNote(); }, 480);
  }

  function renderNotes(key, mode) {
    notesList.innerHTML = "";
    var items = notes[key] || [];
    if (!items.length) {
      var empty = document.createElement("li");
      empty.className = "notes-empty";
      empty.textContent = "Sin notas todavía. Añade la primera.";
      notesList.appendChild(empty);
      return;
    }
    items.forEach(function (text, idx) {
      var li = document.createElement("li");
      li.className = "note-item";
      if (mode === "open") {
        li.classList.add("enter");
        li.style.setProperty("--d", (140 + idx * 50) + "ms");
      } else if (mode === "add" && idx === items.length - 1) {
        li.classList.add("enter");
      }

      var dot = document.createElement("span");
      dot.className = "dot";
      li.appendChild(dot);

      var span = document.createElement("span");
      span.className = "txt";
      span.textContent = text;
      li.appendChild(span);

      var del = document.createElement("button");
      del.type = "button";
      del.setAttribute("aria-label", "Eliminar nota");
      del.textContent = "✕";
      del.addEventListener("click", function () {
        if (li.classList.contains("leaving")) return;
        var live = notesList.querySelectorAll(".note-item:not(.leaving)");
        var at = Array.prototype.indexOf.call(live, li);
        if (at < 0) return;
        notes[key].splice(at, 1);
        if (!notes[key].length) delete notes[key];
        saveNotes();
        refreshCells();
        li.style.setProperty("--h", li.offsetHeight + "px");
        li.classList.add("leaving");
        setTimeout(function () {
          li.remove();
          if (selectedKey === key && !notes[key]) renderNotes(key);
        }, 290);
      });
      li.appendChild(del);

      notesList.appendChild(li);
    });
  }

  noteForm.addEventListener("submit", function (e) {
    e.preventDefault();
    var text = noteInput.value.trim();
    if (!text || !selectedKey) return;
    if (!notes[selectedKey]) notes[selectedKey] = [];
    notes[selectedKey].push(text);
    saveNotes();
    noteInput.value = "";
    renderNotes(selectedKey, "add");
    refreshCells();
    noteInput.focus();
  });

  /* ---------- animaciones ---------- */
  var reduceMotion = window.matchMedia && window.matchMedia("(prefers-reduced-motion: reduce)");
  function prefersReduced() { return !!(reduceMotion && reduceMotion.matches); }

  if (prefersReduced() && surfaceLight.pauseAnimations) surfaceLight.pauseAnimations();

  // actualiza clases de los días sin reconstruir la cuadrícula
  function refreshCells() {
    var cells = grid.querySelectorAll(".day:not(.pad)");
    for (var i = 0; i < cells.length; i++) {
      var k = cells[i].dataset.key;
      cells[i].classList.toggle("selected", k === selectedKey);
      cells[i].classList.toggle("has-note", !!(notes[k] && notes[k].length));
      cells[i].classList.toggle("has-dark", !!(darkOn && darkNotes[k]));
      updateMeetupVisual(cells[i]);
    }
  }

  // onda que se expande desde el día pulsado
  function ping(cell) {
    var old = cell.querySelector(".ping");
    if (old) old.remove();
    var el = document.createElement("span");
    el.className = "ping";
    el.addEventListener("animationend", function () { el.remove(); });
    cell.appendChild(el);
  }

  function pulseToday(delay) {
    var c = grid.querySelector(".day.today");
    if (!c) return;
    c.classList.remove("pulse");
    void c.offsetWidth;
    c.style.setProperty("--pd", delay + "ms");
    c.classList.add("pulse");
    c.addEventListener("animationend", function onEnd(e) {
      if (e.animationName !== "todayPulse") return;
      c.classList.remove("pulse");
      c.removeEventListener("animationend", onEnd);
    });
  }

  var ghosts = [], anims = [], moving = [], transitionId = 0;

  function clearTransition() {
    anims.forEach(function (a) { try { a.cancel(); } catch (e) {} });
    ghosts.forEach(function (g) { g.remove(); });
    moving.forEach(function (el) { el.style.transformOrigin = ""; });
    anims = []; ghosts = []; moving = [];
  }

  // El mes actual sale hacia un lado girando ligeramente alrededor de un eje situado
  // abajo del todo, en el centro; el nuevo entra desde el lado contrario.
  // dir = +1: mes posterior (sale por la izquierda, entra por la derecha); -1: anterior.
  function transitionMonth(dir, mutate) {
    if (!app.animate) { mutate(); return; }
    clearTransition();
    var id = ++transitionId;
    var reduce = prefersReduced();
    var sets = [monthNum, monthDisplay, grid];
    var appBox = app.getBoundingClientRect();
    var stBox = stage.getBoundingClientRect();
    var px = stBox.left + stBox.width / 2;
    var py = stBox.top + stBox.height - capBottom;

    var snaps = sets.map(function (el) {
      var r = el.getBoundingClientRect();
      var g = el.cloneNode(true);
      g.removeAttribute("id");
      Array.prototype.forEach.call(g.querySelectorAll("[id]"), function (n) { n.removeAttribute("id"); });
      g.classList.add("ghost");
      g.setAttribute("aria-hidden", "true");
      g.setAttribute("inert", "");
      g.style.left = (r.left - appBox.left) + "px";
      g.style.top = (r.top - appBox.top) + "px";
      g.style.width = r.width + "px";
      g.style.height = r.height + "px";
      g.style.transformOrigin = (px - r.left) + "px " + (py - r.top) + "px";
      return { g: g };
    });

    mutate();

    var dx = reduce ? 0 : Math.round(stBox.width * 0.5);
    var ang = reduce ? 0 : 8;
    var dur = reduce ? 260 : 520;
    var ease = "cubic-bezier(0.32, 0.72, 0.24, 1)";

    sets.forEach(function (el, i) {
      var g = snaps[i].g;
      app.appendChild(g);
      ghosts.push(g);
      anims.push(g.animate([
        { transform: "translateX(0) rotate(0deg)", opacity: 1, offset: 0 },
        { opacity: 1, offset: 0.3 },
        { transform: "translateX(" + (-dir * dx) + "px) rotate(" + (-dir * ang) + "deg)", opacity: 0, offset: 1 }
      ], { duration: dur, easing: ease, fill: "forwards" }));

      var r1 = el.getBoundingClientRect();
      el.style.transformOrigin = (px - r1.left) + "px " + (py - r1.top) + "px";
      moving.push(el);
      anims.push(el.animate([
        { transform: "translateX(" + (dir * dx) + "px) rotate(" + (dir * ang) + "deg)", opacity: 0, offset: 0 },
        { opacity: 1, offset: 0.55 },
        { transform: "translateX(0) rotate(0deg)", opacity: 1, offset: 1 }
      ], { duration: dur, delay: reduce ? 0 : 40, easing: ease, fill: "backwards" }));
    });

    setTimeout(function () { if (id === transitionId) clearTransition(); }, dur + 120);
  }

  function goToMonth(year, month) {
    var diff = (year * 12 + month) - (view.year * 12 + view.month);
    if (diff === 0) return false;
    transitionMonth(diff > 0 ? 1 : -1, function () {
      view.year = year;
      view.month = month;
      selectedKey = null;
      renderCalendar();
    });
    return true;
  }

  prevBtn.addEventListener("click", function () {
    var d = new Date(view.year, view.month - 1, 1);
    goToMonth(d.getFullYear(), d.getMonth());
  });

  nextBtn.addEventListener("click", function () {
    var d = new Date(view.year, view.month + 1, 1);
    goToMonth(d.getFullYear(), d.getMonth());
  });

  todayBtn.addEventListener("click", function () {
    var moved = goToMonth(today.getFullYear(), today.getMonth());
    pulseToday(moved ? 380 : 0);
  });

  overlay.addEventListener("click", closeSheet);
  closeBtn.addEventListener("click", closeSheet);

  /* ---------- marco: dos arcos circulares independientes ---------- */
  var stage = document.getElementById("stage");
  var rim = document.getElementById("frameRim");
  var probe = document.getElementById("safeProbe");
  var rimPaths = rim.querySelectorAll("path");
  var rootEl = document.documentElement;

  // radio de la circunferencia que pasa por una cuerda w con flecha (altura del arco) s
  function arcRadius(w, s) { return (w * w / 4 + s * s) / (2 * s); }

  function layoutFrame() {
    var W = stage.clientWidth, H = stage.clientHeight;
    if (!W || !H) return;
    var cs = window.getComputedStyle(probe);
    var safeT = parseFloat(cs.paddingTop) || 0;
    var safeB = parseFloat(cs.paddingBottom) || 0;
    var safeR = parseFloat(cs.paddingRight) || 0;

    var capT = 14 + safeT;                                   // borde superior: fino
    var capB = 64 + safeB;                                   // borde inferior: ancho
    capBottom = capB;
    var sT = Math.max(12, Math.min(Math.round(W * 0.11), Math.round(H * 0.06)));
    var sB = Math.max(14, Math.min(Math.round(W * 0.13), Math.round(H * 0.07)));
    var RT = arcRadius(W, sT), RB = arcRadius(W, sB);
    var yt = capT + sT;                                      // donde la cúpula toca los laterales
    var yb = H - capB - sB;                                  // donde el arco inferior toca los laterales

    var clip = "path('M0," + yt + " A" + RT + "," + RT + " 0 0 1 " + W + "," + yt +
               " L" + W + "," + yb + " A" + RB + "," + RB + " 0 0 1 0," + yb + " Z')";
    stage.style.clipPath = clip; stage.style.webkitClipPath = clip;
    rim.style.clipPath = clip; rim.style.webkitClipPath = clip;
    rim.setAttribute("viewBox", "0 0 " + W + " " + H);

    var dTop = "M0," + yt + " A" + RT + "," + RT + " 0 0 1 " + W + "," + yt;
    var dBot = "M0," + yb + " A" + RB + "," + RB + " 0 0 0 " + W + "," + yb;
    for (var i = 0; i < rimPaths.length; i++) {
      rimPaths[i].setAttribute("d", rimPaths[i].getAttribute("data-arc") === "top" ? dTop : dBot);
    }

    // el botón "Hoy" se apoya justo por encima del arco inferior
    var dd = Math.max(0, W / 2 - (24 + safeR));
    var dropB = RB - Math.sqrt(Math.max(0, RB * RB - dd * dd));

    rootEl.style.setProperty("--cap-t", capT + "px");
    rootEl.style.setProperty("--cap-b", capB + "px");
    rootEl.style.setProperty("--s-top", sT + "px");
    rootEl.style.setProperty("--s-bot", sB + "px");
    rootEl.style.setProperty("--today-b", Math.round(capB + dropB + 12) + "px");
  }

  window.addEventListener("resize", layoutFrame);
  window.addEventListener("orientationchange", layoutFrame);
  if (window.visualViewport) window.visualViewport.addEventListener("resize", layoutFrame);
  layoutFrame();

  /* ---------- HORA OSCURA (00:00 - 01:00) ---------- */
  var veil = document.getElementById("veil");
  var toastEl = document.getElementById("toast");
  var moonBtn = document.getElementById("moonBtn");
  var moonLit = document.getElementById("moonLit");
  var darkNote = document.getElementById("darkNote");
  var darkNoteInput = document.getElementById("darkNoteInput");
  var darkNoteStatus = document.getElementById("darkNoteStatus");
  var darkNoteKey = null, darkTimer = null, statusTimer = null, toastTimer = null;
  var previewDark = null, switching = false;
  var moonInfo = { name: "", illum: 0 };

  function realDark() { return new Date().getHours() === 0; }

  function showToast(msg) {
    toastEl.textContent = msg;
    toastEl.classList.add("show");
    clearTimeout(toastTimer);
    toastTimer = setTimeout(function () { toastEl.classList.remove("show"); }, 2600);
  }

  // fase lunar (aproximación por mes sinódico medio)
  function drawMoon() {
    var jd = Date.now() / 86400000 + 2440587.5;
    var f = (((jd - 2451550.1) / 29.530588853) % 1 + 1) % 1;
    var k = Math.cos(2 * Math.PI * f);
    var illum = (1 - k) / 2;
    var R = 17, rx = Math.abs(k) * R, d;
    if (illum < 0.006) d = "";
    else if (illum > 0.994) d = "M0,-17 A17,17 0 1 1 0,17 A17,17 0 1 1 0,-17 Z";
    else d = "M0,-17 A17,17 0 0 1 0,17 A" + rx.toFixed(2) + ",17 0 0 " + (k > 0 ? 0 : 1) + " 0,-17 Z";
    moonLit.setAttribute("d", d);
    moonLit.setAttribute("transform", f < 0.5 ? "" : "scale(-1 1)");   // creciente: luz a la derecha; menguante: a la izquierda
    var name;
    if (f < 0.03 || f >= 0.97) name = "Luna nueva";
    else if (f < 0.22) name = "Luna creciente";
    else if (f < 0.28) name = "Cuarto creciente";
    else if (f < 0.47) name = "Gibosa creciente";
    else if (f < 0.53) name = "Luna llena";
    else if (f < 0.72) name = "Gibosa menguante";
    else if (f < 0.78) name = "Cuarto menguante";
    else name = "Luna menguante";
    moonInfo = { name: name, illum: Math.round(illum * 100) };
    moonBtn.setAttribute("aria-label", name + ", " + moonInfo.illum + " % iluminada");
  }
  moonBtn.addEventListener("click", function () {
    showToast(moonInfo.name + " · " + moonInfo.illum + " % iluminada");
  });

  // nota especial: una por día, solo existe en pantalla durante la hora oscura
  function flushDarkNote() {
    clearTimeout(darkTimer); darkTimer = null;
    if (!darkNoteKey) return;
    var v = darkNoteInput.value.trim();
    var cur = darkNotes[darkNoteKey] || "";
    if (v === cur) return;
    if (v) darkNotes[darkNoteKey] = v; else delete darkNotes[darkNoteKey];
    saveDarkNotes();
    refreshCells();
    darkNoteStatus.classList.add("show");
    clearTimeout(statusTimer);
    statusTimer = setTimeout(function () { darkNoteStatus.classList.remove("show"); }, 1400);
  }

  function syncDarkNote() {
    if (darkOn && selectedKey && sheet.classList.contains("open")) {
      if (darkNoteKey !== selectedKey) {
        darkNoteKey = selectedKey;
        darkNoteInput.value = darkNotes[selectedKey] || "";
      }
      darkNote.hidden = false;
    } else {
      darkNote.hidden = true;
      darkNoteInput.value = "";      // fuera de la hora oscura el texto no queda en pantalla
      darkNoteKey = null;
      darkNoteStatus.classList.remove("show");
    }
  }

  darkNoteInput.addEventListener("input", function () {
    clearTimeout(darkTimer);
    darkTimer = setTimeout(flushDarkNote, 400);
  });
  window.addEventListener("pagehide", flushDarkNote);
  document.addEventListener("visibilitychange", function () {
    if (document.hidden) flushDarkNote(); else { evaluateMode(false); checkPendingMeetups(); }
  });

  function setDark(on) {
    if (!on) flushDarkNote();
    darkOn = on;
    rootEl.classList.toggle("dark-hour", on);
    if (on) drawMoon();
    refreshCells();
    syncDarkNote();
  }

  // cambio de paleta con un fundido a negro para que sea suave
  function evaluateMode(initial) {
    if (switching) return;
    var want = previewDark !== null ? previewDark : realDark();
    if (want === darkOn) return;
    if (initial || prefersReduced()) { setDark(want); return; }
    switching = true;
    veil.classList.add("on");
    setTimeout(function () {
      setDark(want);
      requestAnimationFrame(function () {
        requestAnimationFrame(function () { veil.classList.remove("on"); });
      });
      setTimeout(function () { switching = false; evaluateMode(false); }, 950);
    }, 700);
  }

  // vista previa: mantén pulsado el nombre del mes ~1 s para alternar
  var holdTimer = null, holdX = 0, holdY = 0;
  function cancelHold() { clearTimeout(holdTimer); holdTimer = null; }
  function togglePreview() {
    holdTimer = null;
    var msg;
    if (previewDark === null) {
      previewDark = !realDark();
      msg = previewDark ? "Vista previa: hora oscura" : "Vista previa: hora normal";
    } else {
      previewDark = null;
      msg = "Vista previa desactivada";
    }
    var willSwitch = (previewDark !== null ? previewDark : realDark()) !== darkOn;
    evaluateMode(false);
    setTimeout(function () { showToast(msg); }, willSwitch && !prefersReduced() ? 1500 : 0);
  }
  monthDisplay.addEventListener("pointerdown", function (e) {
    holdX = e.clientX; holdY = e.clientY;
    cancelHold();
    holdTimer = setTimeout(togglePreview, 900);
  });
  monthDisplay.addEventListener("pointermove", function (e) {
    if (Math.abs(e.clientX - holdX) > 10 || Math.abs(e.clientY - holdY) > 10) cancelHold();
  });
  ["pointerup", "pointercancel", "pointerleave"].forEach(function (ev) {
    monthDisplay.addEventListener(ev, cancelHold);
  });
  monthDisplay.addEventListener("contextmenu", function (e) { e.preventDefault(); });

  /* ---------- panel de arcanas (compendio de confidentes) ---------- */
  var arcanaBtn = document.getElementById("arcanaBtn");
  var overlay2 = document.getElementById("overlay2");
  var arcanaSheet = document.getElementById("arcanaSheet");
  var arcanaEyebrow = document.getElementById("arcanaEyebrow");
  var arcanaTitle = document.getElementById("arcanaTitle");
  var arcanaBackBtn = document.getElementById("arcanaBackBtn");
  var arcanaCloseBtn = document.getElementById("arcanaCloseBtn");
  var arcanaList = document.getElementById("arcanaList");
  var arcanaDetail = document.getElementById("arcanaDetail");
  var arcanaDetailImg = document.getElementById("arcanaDetailImg");
  var arcanaDetailName = document.getElementById("arcanaDetailName");
  var arcanaNameInput = document.getElementById("arcanaNameInput");
  var arcanaNameStatus = document.getElementById("arcanaNameStatus");
  var arcanaLevelRow = document.getElementById("arcanaLevelRow");
  var arcanaLevelText = document.getElementById("arcanaLevelText");
  var currentArcanaId = null;
  var arcanaNameTimer = null;
  var arcanaStatusTimer = null;

  function closeSecondary() {
    flushArcanaName();
    overlay2.classList.remove("open");
    arcanaSheet.classList.remove("open");
    dayMenuSheet.classList.remove("open");
    meetupSheet.classList.remove("open");
  }
  overlay2.addEventListener("click", closeSecondary);

  function renderLevelPips(container, level) {
    container.innerHTML = "";
    for (var i = 0; i < 10; i++) {
      var pip = document.createElement("span");
      pip.className = "arcana-pip" + (i < level ? " filled" : "");
      container.appendChild(pip);
    }
  }

  function buildArcanaRow(a, opts) {
    var ad = getArcanaData(a.id);
    var row = document.createElement("button");
    row.type = "button";
    row.className = "arcana-row" + (opts && opts.active ? " active" : "");
    row.dataset.id = String(a.id);

    var thumb = document.createElement("span");
    thumb.className = "arcana-thumb";
    thumb.style.backgroundImage = 'url("' + a.img + '")';
    row.appendChild(thumb);

    var text = document.createElement("span");
    text.className = "arcana-row-text";
    var nameEl = document.createElement("span");
    nameEl.className = "arcana-row-name";
    nameEl.textContent = a.roman + " · " + a.name;
    var personEl = document.createElement("span");
    personEl.className = "arcana-row-person" + (ad.name ? "" : " unset");
    personEl.textContent = ad.name || "Sin asignar";
    text.appendChild(nameEl);
    text.appendChild(personEl);
    row.appendChild(text);

    var level = document.createElement("span");
    level.className = "arcana-row-level";
    level.textContent = "Nv " + ad.level;
    row.appendChild(level);

    return row;
  }

  function renderArcanaList() {
    arcanaList.innerHTML = "";
    ARCANA.forEach(function (a) {
      var row = buildArcanaRow(a);
      row.addEventListener("click", function () {
        openArcanaDetail(parseInt(this.dataset.id, 10));
      });
      arcanaList.appendChild(row);
    });
  }

  function openArcanaDetail(id) {
    currentArcanaId = id;
    var a = ARCANA_BY_ID[id];
    var ad = getArcanaData(id);
    arcanaDetailImg.style.backgroundImage = 'url("' + a.img + '")';
    arcanaDetailName.textContent = a.roman + " · " + a.name;
    arcanaNameInput.value = ad.name;
    arcanaLevelText.textContent = String(ad.level);
    renderLevelPips(arcanaLevelRow, ad.level);
    arcanaList.hidden = true;
    arcanaDetail.hidden = false;
    arcanaBackBtn.hidden = false;
    arcanaEyebrow.textContent = "FICHA DE CONFIDENTE";
    arcanaTitle.textContent = a.roman;
  }

  function backToArcanaList() {
    flushArcanaName();
    currentArcanaId = null;
    arcanaDetail.hidden = true;
    arcanaList.hidden = false;
    arcanaBackBtn.hidden = true;
    arcanaEyebrow.textContent = "COMPENDIO";
    arcanaTitle.textContent = "Arcanas";
    renderArcanaList();
  }

  function flushArcanaName() {
    clearTimeout(arcanaNameTimer); arcanaNameTimer = null;
    if (!currentArcanaId) return;
    var ad = getArcanaData(currentArcanaId);
    var v = arcanaNameInput.value.trim();
    if (v === ad.name) return;
    ad.name = v;
    saveArcanaData();
    refreshCells();
    arcanaNameStatus.classList.add("show");
    clearTimeout(arcanaStatusTimer);
    arcanaStatusTimer = setTimeout(function () { arcanaNameStatus.classList.remove("show"); }, 1400);
  }
  arcanaNameInput.addEventListener("input", function () {
    clearTimeout(arcanaNameTimer);
    arcanaNameTimer = setTimeout(flushArcanaName, 400);
  });

  arcanaBackBtn.addEventListener("click", backToArcanaList);
  arcanaCloseBtn.addEventListener("click", closeSecondary);

  function openArcanaSheet() {
    closeSheet();
    flushArcanaName();
    currentArcanaId = null;
    arcanaDetail.hidden = true;
    arcanaList.hidden = false;
    arcanaBackBtn.hidden = true;
    arcanaEyebrow.textContent = "COMPENDIO";
    arcanaTitle.textContent = "Arcanas";
    renderArcanaList();
    overlay2.classList.add("open");
    arcanaSheet.classList.add("open");
  }
  arcanaBtn.addEventListener("click", openArcanaSheet);

  /* ---------- menú de día: dejar nota o marcar quedada ---------- */
  var dayMenuSheet = document.getElementById("dayMenuSheet");
  var dayMenuWeekday = document.getElementById("dayMenuWeekday");
  var dayMenuDate = document.getElementById("dayMenuDate");
  var dayMenuCloseBtn = document.getElementById("dayMenuCloseBtn");
  var optNote = document.getElementById("optNote");
  var optMeet = document.getElementById("optMeet");
  var menuKey = null, menuDay = null;

  function openDayMenu(key, day) {
    menuKey = key; menuDay = day;
    var parts = key.split("-").map(Number);
    var dow = new Date(parts[0], parts[1] - 1, day).getDay();
    dayMenuWeekday.textContent = WEEKDAYS_LONG[dow];
    dayMenuDate.textContent = day + " de " + MONTHS_LONG[parts[1] - 1];
    overlay2.classList.add("open");
    dayMenuSheet.classList.add("open");
  }
  dayMenuCloseBtn.addEventListener("click", closeSecondary);
  optNote.addEventListener("click", function () {
    closeSecondary();
    openSheet(menuKey, menuDay);
  });
  optMeet.addEventListener("click", function () {
    closeSecondary();
    openMeetupSelect(menuKey, menuDay);
  });

  /* ---------- selector de arcana para fijar una quedada ---------- */
  var meetupSheet = document.getElementById("meetupSheet");
  var meetupWeekday = document.getElementById("meetupWeekday");
  var meetupDateLabel = document.getElementById("meetupDateLabel");
  var meetupCloseBtn = document.getElementById("meetupCloseBtn");
  var meetupList = document.getElementById("meetupList");
  var meetupEmptyMsg = document.getElementById("meetupEmptyMsg");
  var meetupClearBtn = document.getElementById("meetupClearBtn");

  function openMeetupSelect(key, day) {
    menuKey = key; menuDay = day;
    var parts = key.split("-").map(Number);
    var dow = new Date(parts[0], parts[1] - 1, day).getDay();
    meetupWeekday.textContent = WEEKDAYS_LONG[dow];
    meetupDateLabel.textContent = "Quedada · " + day + " de " + MONTHS_LONG[parts[1] - 1];

    var assigned = ARCANA.filter(function (a) { return getArcanaData(a.id).name; });
    var current = meetups[key];
    meetupList.innerHTML = "";
    meetupEmptyMsg.hidden = assigned.length > 0;
    meetupClearBtn.hidden = !current;

    assigned.forEach(function (a) {
      var row = buildArcanaRow(a, { active: !!(current && current.arcanaId === a.id) });
      row.addEventListener("click", function () {
        assignMeetup(menuKey, parseInt(this.dataset.id, 10));
      });
      meetupList.appendChild(row);
    });

    overlay2.classList.add("open");
    meetupSheet.classList.add("open");
  }
  meetupCloseBtn.addEventListener("click", closeSecondary);
  meetupClearBtn.addEventListener("click", function () {
    removeMeetup(menuKey);
    closeSecondary();
  });

  function assignMeetup(key, arcanaId) {
    meetups[key] = { arcanaId: arcanaId, status: "pending" };
    saveMeetups();
    closeSecondary();
    refreshCells();
    if (selectedKey === key) renderMeetupCard(key);
    checkPendingMeetups();
  }

  function removeMeetup(key) {
    delete meetups[key];
    saveMeetups();
    refreshCells();
    if (selectedKey === key) renderMeetupCard(key);
  }

  /* ---------- tarjeta de quedada dentro de la hoja del día ---------- */
  var meetCardSlot = document.getElementById("meetCardSlot");
  var STATUS_LABEL = { pending: "Pendiente", done: "Realizada", missed: "No realizada" };

  function renderMeetupCard(key) {
    meetCardSlot.innerHTML = "";
    var m = meetups[key];
    if (!m) return;
    var def = ARCANA_BY_ID[m.arcanaId];
    if (!def) return;
    var ad = getArcanaData(m.arcanaId);

    var card = document.createElement("div");
    card.className = "meet-card";

    var thumb = document.createElement("span");
    thumb.className = "meet-card-thumb";
    thumb.style.backgroundImage = 'url("' + def.img + '")';
    card.appendChild(thumb);

    var text = document.createElement("span");
    text.className = "meet-card-text";
    var nameEl = document.createElement("span");
    nameEl.className = "meet-card-name";
    nameEl.textContent = ad.name || "(sin nombre)";
    var subEl = document.createElement("span");
    subEl.className = "meet-card-sub";
    subEl.textContent = "Quedada · " + def.roman + " · " + def.name;
    text.appendChild(nameEl);
    text.appendChild(subEl);
    card.appendChild(text);

    var status = document.createElement("span");
    status.className = "meet-card-status " + m.status;
    status.textContent = STATUS_LABEL[m.status];
    card.appendChild(status);

    var rm = document.createElement("button");
    rm.type = "button";
    rm.className = "meet-card-remove";
    rm.setAttribute("aria-label", "Quitar quedada");
    rm.textContent = "✕";
    rm.addEventListener("click", function () { removeMeetup(key); });
    card.appendChild(rm);

    meetCardSlot.appendChild(card);
  }

  /* ---------- confirmación de quedadas ya pasadas ---------- */
  var confirmOverlay = document.getElementById("confirmOverlay");
  var confirmCard = document.getElementById("confirmCard");
  var confirmThumb = document.getElementById("confirmThumb");
  var confirmPerson = document.getElementById("confirmPerson");
  var confirmArcanaName = document.getElementById("confirmArcanaName");
  var confirmDate = document.getElementById("confirmDate");
  var confirmYesBtn = document.getElementById("confirmYesBtn");
  var confirmNoBtn = document.getElementById("confirmNoBtn");
  var confirmKey = null;

  function showConfirm(key) {
    var m = meetups[key];
    if (!m) return;
    var def = ARCANA_BY_ID[m.arcanaId];
    if (!def) return;
    var ad = getArcanaData(m.arcanaId);
    confirmKey = key;
    var parts = key.split("-").map(Number);
    confirmThumb.style.backgroundImage = 'url("' + def.img + '")';
    confirmPerson.textContent = ad.name || "(sin nombre)";
    confirmArcanaName.textContent = def.roman + " · " + def.name;
    confirmDate.textContent = parts[2] + " de " + MONTHS_LONG[parts[1] - 1];
    closeSheet();
    closeSecondary();
    confirmOverlay.classList.add("open");
    confirmCard.classList.add("open");
  }

  function hideConfirm() {
    confirmOverlay.classList.remove("open");
    confirmCard.classList.remove("open");
    confirmKey = null;
  }

  function resolveConfirm(happened) {
    var key = confirmKey;
    if (!key) return;
    var m = meetups[key];
    if (!m) { hideConfirm(); return; }
    m.status = happened ? "done" : "missed";
    saveMeetups();
    if (happened) {
      var ad = getArcanaData(m.arcanaId);
      ad.level = Math.min(10, ad.level + 1);
      saveArcanaData();
    }
    hideConfirm();
    refreshCells();
    if (selectedKey === key) renderMeetupCard(key);
    setTimeout(checkPendingMeetups, 400);
  }
  confirmYesBtn.addEventListener("click", function () { resolveConfirm(true); });
  confirmNoBtn.addEventListener("click", function () { resolveConfirm(false); });

  function checkPendingMeetups() {
    if (confirmCard.classList.contains("open")) return;
    var todayKey = dateKey(today.getFullYear(), today.getMonth(), today.getDate());
    var keys = Object.keys(meetups).filter(function (k) {
      var m = meetups[k];
      return m && m.status === "pending" && k < todayKey;
    }).sort();
    if (!keys.length) return;
    showConfirm(keys[0]);
  }

  /* ---------- pulsación larga en un día: elegir nota o quedada ---------- */
  var dayHoldTimer = null, dayHoldCell = null, dayHoldX = 0, dayHoldY = 0, dayHoldFired = false;
  grid.addEventListener("contextmenu", function (e) { e.preventDefault(); });
  grid.addEventListener("pointerdown", function (e) {
    var cell = e.target.closest ? e.target.closest(".day:not(.pad)") : null;
    if (!cell) return;
    dayHoldCell = cell; dayHoldX = e.clientX; dayHoldY = e.clientY; dayHoldFired = false;
    clearTimeout(dayHoldTimer);
    dayHoldTimer = setTimeout(function () {
      dayHoldFired = true;
      if (navigator.vibrate) { try { navigator.vibrate(12); } catch (e) {} }
      openDayMenu(cell.dataset.key, parseInt(cell.dataset.day, 10));
    }, 480);
  });
  grid.addEventListener("pointermove", function (e) {
    if (!dayHoldCell) return;
    if (Math.abs(e.clientX - dayHoldX) > 10 || Math.abs(e.clientY - dayHoldY) > 10) clearTimeout(dayHoldTimer);
  });
  ["pointerup", "pointercancel", "pointerleave"].forEach(function (ev) {
    grid.addEventListener(ev, function (e) {
      clearTimeout(dayHoldTimer);
      if (ev === "pointerup" && dayHoldCell && !dayHoldFired) {
        ping(dayHoldCell);
        openSheet(dayHoldCell.dataset.key, parseInt(dayHoldCell.dataset.day, 10));
      }
      dayHoldCell = null; dayHoldFired = false;
    });
  });

  setInterval(function () { evaluateMode(false); checkPendingMeetups(); }, 15000);
  evaluateMode(true);

  renderCalendar();
  checkPendingMeetups();
})();
</script>

<script>
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('./sw.js')
      .then(() => console.log('Service Worker registrado'))
      .catch((err) => console.log('Error en Service Worker', err));
  }
</script>

</body></html>
