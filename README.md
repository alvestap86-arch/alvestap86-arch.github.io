<!doctype html><html><head><meta charset=utf8><meta name=viewport content="width=device-width,initial-scale=1,viewport-fit=cover"><style>:root{color-scheme:light;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}html{scroll-padding-top:env(safe-area-inset-top,0px)}body{margin:0;padding:0;font:14px -apple-system,BlinkMacSystemFont,sans-serif;background:#fff;color:#000}img{max-width:100%}[hidden]:not([hidden=until-found i]){display:none!important}</style></head><body>
<meta name="claude:viewport-fit-sides" content="cover">
<title>Calendario Marea</title>
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
  .frame-rim path[fil
