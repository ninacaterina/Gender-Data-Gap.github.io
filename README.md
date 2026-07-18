<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Gender Data Gap — Awareness Station</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --green: #5db191;
    --green-dark: #3d8a6e;
    --black: #1A1A1A;
    --gray-dark: #555;
    --gray-mid: #999;
    --gray-light: #E8E8E8;
    --gray-bg: #F7F7F7;
    --white: #FFFFFF;
    --radius: 2px;
    --mono: ui-monospace, 'SF Mono', 'Menlo', monospace;
    --sans: 'Avenir Next', 'Avenir', 'Helvetica Neue', Helvetica, Arial, sans-serif;
  }

  html, body {
    min-height: 100%;
    background: var(--white);
    color: var(--black);
    font-family: var(--sans);
    -webkit-font-smoothing: antialiased;
    overflow-x: hidden;
  }

  /* Progress bar */
  #progress-track {
    position: fixed;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: var(--gray-light);
    z-index: 100;
  }
  #progress-fill {
    height: 100%;
    background: var(--green);
    transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
    width: 0%;
  }

  /* Header */
  header {
    padding: 36px 40px 0;
  }
  .logo-sub {
    font-family: var(--mono);
    font-size: 11px;
    letter-spacing: 0.12em;
    color: var(--gray-mid);
    text-transform: uppercase;
  }

  /* Main layout */
  main {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 60px 40px 60px;
  }

  /* Screen */
  .screen {
    display: none;
    width: 100%;
    max-width: 480px;
    animation: fadeUp 0.3s ease both;
  }
  .screen.active { display: block; }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(10px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .screen-label {
    font-family: var(--mono);
    font-size: 10px;
    letter-spacing: 0.22em;
    color: var(--green);
    text-transform: uppercase;
    margin-bottom: 18px;
  }

  .screen h2 {
    font-size: 26px;
    font-weight: 300;
    line-height: 1.25;
    letter-spacing: -0.02em;
    color: var(--black);
    margin-bottom: 6px;
  }

  .screen-desc {
    font-size: 14px;
    color: var(--gray-dark);
    line-height: 1.6;
    margin-bottom: 36px;
    font-weight: 300;
  }

  /* Inputs */
  .field-group { margin-bottom: 24px; }

  label.field-label {
    display: block;
    font-family: var(--mono);
    font-size: 10px;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--gray-mid);
    margin-bottom: 8px;
  }

  input[type="text"],
  input[type="date"] {
    width: 100%;
    padding: 14px 16px;
    font-family: var(--sans);
    font-size: 18px;
    font-weight: 300;
    color: var(--black);
    background: var(--gray-bg);
    border: 1px solid var(--gray-light);
    border-radius: var(--radius);
    outline: none;
    transition: border-color 0.2s;
    -webkit-appearance: none;
  }
  input[type="text"]:focus,
  input[type="date"]:focus {
    border-color: var(--green);
    background: var(--white);
  }
  input::placeholder { color: var(--gray-mid); }

  /* Category grid */
  .cat-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
    margin-bottom: 32px;
  }

  .cat-card {
    padding: 16px 14px;
    background: var(--gray-bg);
    border: 1px solid var(--gray-light);
    border-radius: var(--radius);
    cursor: pointer;
    transition: border-color 0.15s, background 0.15s;
    -webkit-tap-highlight-color: transparent;
    user-select: none;
  }
  .cat-card:active { transform: scale(0.98); }

  .cat-card .cat-num {
    font-family: var(--mono);
    font-size: 9px;
    letter-spacing: 0.15em;
    color: var(--gray-mid);
    display: block;
    margin-bottom: 6px;
  }
  .cat-card .cat-title {
    font-size: 13px;
    font-weight: 400;
    color: var(--black);
    line-height: 1.35;
    display: block;
  }

  .cat-card.selected {
    border-color: var(--green);
    background: var(--white);
  }
  .cat-card.selected .cat-title { color: var(--green); }
  .cat-card.selected .cat-num   { color: var(--green); }

  /* Buttons */
  .btn-primary {
    width: 100%;
    padding: 16px;
    background: var(--green);
    color: var(--white);
    font-family: var(--mono);
    font-size: 11px;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    border: none;
    border-radius: var(--radius);
    cursor: pointer;
    transition: background 0.15s;
    -webkit-tap-highlight-color: transparent;
  }
  .btn-primary:active { background: var(--green-dark); }
  .btn-primary:disabled {
    background: var(--gray-light);
    color: var(--gray-mid);
    cursor: not-allowed;
  }

  .btn-ghost {
    width: 100%;
    padding: 13px;
    background: transparent;
    color: var(--gray-mid);
    font-family: var(--mono);
    font-size: 10px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    border: 1px solid var(--gray-light);
    border-radius: var(--radius);
    cursor: pointer;
    margin-top: 10px;
    transition: border-color 0.15s, color 0.15s;
  }
  .btn-ghost:hover { border-color: var(--gray-dark); color: var(--black); }

  /* Receipt preview */
  .receipt-preview {
    background: var(--white);
    border: 1px solid var(--gray-light);
    border-radius: var(--radius);
    padding: 24px 22px;
    margin-bottom: 24px;
    font-family: var(--mono);
    font-size: 11.5px;
    line-height: 1.85;
    color: var(--black);
  }
  .receipt-preview::before {
    content: '';
    display: block;
    height: 2px;
    background: repeating-linear-gradient(
      90deg, var(--green) 0, var(--green) 6px, transparent 6px, transparent 12px
    );
    margin-bottom: 18px;
  }
  .receipt-line { display: flex; justify-content: space-between; gap: 8px; }
  .receipt-line .rl { color: var(--gray-mid); white-space: nowrap; }
  .receipt-line .rv { text-align: right; }
  .receipt-divider {
    border: none;
    border-top: 1px dashed var(--gray-light);
    margin: 10px 0;
  }
  .receipt-cat-label {
    font-size: 9px;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--green);
    display: block;
    margin-bottom: 8px;
    margin-top: 14px;
  }
  .receipt-fact-text {
    font-size: 11px;
    line-height: 1.75;
    color: var(--black);
  }
  .receipt-footer {
    margin-top: 14px;
    font-size: 9px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--gray-mid);
    text-align: center;
    padding-top: 12px;
    border-top: 1px dashed var(--gray-light);
  }

  /* Print status */
  #print-status {
    font-family: var(--mono);
    font-size: 11px;
    letter-spacing: 0.1em;
    color: var(--gray-mid);
    text-align: center;
    margin-top: 12px;
    min-height: 20px;
  }
  #print-status.error { color: #b30000; }
  #print-status.ok    { color: var(--green-dark); }

  /* Done screen */
  #screen-done .done-mark {
    font-family: var(--mono);
    font-size: 44px;
    color: var(--green);
    display: block;
    margin-bottom: 18px;
  }
  .hairline {
    width: 28px;
    height: 1px;
    background: var(--green);
    margin: 18px 0;
  }

  @media (prefers-reduced-motion: reduce) {
    .screen { animation: none; }
    #progress-fill { transition: none; }
  }
</style>
</head>
<body>

<div id="progress-track"><div id="progress-fill"></div></div>

<header>
  <span class="logo-sub">Gender Data Gap — Awareness Station</span>
</header>

<main>

  <!-- Step 1: Name + Birthday -->
  <div class="screen active" id="screen-name">
    <p class="screen-label">01 / 02 — Who are you?</p>
    <h2>Let's personalise<br>your receipt.</h2>
    <p class="screen-desc">Tell us your name and birthday — both are optional. Skip either by leaving it blank.</p>

    <div class="field-group">
      <label class="field-label" for="input-name">First name</label>
      <input type="text" id="input-name" placeholder="Your name" autocomplete="off" autocorrect="off" spellcheck="false">
    </div>

    <div class="field-group">
      <label class="field-label" for="input-dob">Date of birth</label>
      <input type="date" id="input-dob" max="">
    </div>

    <button class="btn-primary" onclick="goToStep(2)">Continue</button>
  </div>

  <!-- Step 2: Category selection -->
  <div class="screen" id="screen-category">
    <p class="screen-label">02 / 02 — Choose a topic</p>
    <h2>What do you want<br>to know more about?</h2>
    <p class="screen-desc">Select one area where the gender data gap shows.</p>

    <div class="cat-grid" id="cat-grid">
      <!-- filled by JS -->
    </div>

    <button class="btn-primary" id="btn-cat-next" onclick="goToStep(3)" disabled>See my receipt</button>
    <button class="btn-ghost" onclick="goToStep(1)">← Back</button>
  </div>

  <!-- Step 3: Receipt + print -->
  <div class="screen" id="screen-receipt">
    <p class="screen-label">Your data receipt</p>
    <h2>Ready to print.</h2>
    <p class="screen-desc">Review your receipt below, then connect the printer.</p>

    <div class="receipt-preview" id="receipt-preview"></div>

    <button class="btn-primary" id="btn-print" onclick="printReceipt()">
      Connect printer &amp; print
    </button>
    <button class="btn-ghost" onclick="goToStep(2)">← Back</button>
    <div id="print-status"></div>
  </div>

  <!-- Done -->
  <div class="screen" id="screen-done">
    <span class="done-mark">✓</span>
    <p class="screen-label">Complete</p>
    <h2>Your receipt is printing.</h2>
    <div class="hairline"></div>
    <p class="screen-desc" style="margin-bottom: 36px;">The gender data gap is real. Your awareness is the first step.</p>
    <button class="btn-primary" onclick="resetAll()">Start over</button>
  </div>

</main>

<script>
// ── State ──────────────────────────────────────────────────────────────────
const state = { name: '', dob: '', category: null };

// ── Categories & facts ─────────────────────────────────────────────────────
const categories = [
  {
    id: 'training',
    title: 'Training Data\n& Life Phases',
    shortTitle: 'Training Data & Life Phases',
    fact: `Out of 5,261 sport and exercise science studies, women represent only 34% of total participants — and just 6% of studies focus exclusively on female bodies. For women in midlife and beyond, the data gap is even more pronounced and remains largely undocumented.`
  },
  {
    id: 'performance',
    title: 'Performance\nConditions',
    shortTitle: 'Performance Conditions',
    fact: `In exercise thermoregulation research — the science of how the body regulates heat during physical activity — women have represented only about 12–18% of study participants per year. What is considered a "normal" response to heat and exertion is based almost entirely on male data.`
  },
  {
    id: 'nutrition',
    title: 'Sports Nutrition\nData',
    shortTitle: 'Sports Nutrition Data',
    fact: `Only 2.1% of the approximately 4,000 sports nutrition products on the market are specifically formulated for women. Most high-carbohydrate energy gels are sold as "unisex," while female-specific tolerance data — mapped against cycle phase, hormonal status, or life stage — is rarely collected. In endurance sport, gastrointestinal symptoms are reported by up to 37–89% of athletes during long-distance events, yet the female body remains largely excluded from the research that could explain why.`
  },
  {
    id: 'injury',
    title: 'Injury Prevention\nData',
    shortTitle: 'Injury Prevention Data',
    fact: `Women face a two- to threefold higher risk of ACL injuries than men. Yet prevention programmes, diagnostic criteria, and rehabilitation protocols still rely primarily on data gathered from male athletes. The female body is treated as a variation of the male norm — rather than a different system that deserves its own research.`
  },
  {
    id: 'energy',
    title: 'Energy &\nRecovery',
    shortTitle: 'Energy & Recovery',
    fact: `Low energy availability can disrupt the menstrual cycle, suppress estrogen, reduce bone density, and increase the risk of stress fractures and osteoporosis. It can also affect immunity, sleep, coordination, and mental health. Among female endurance runners, rates of the Female Athlete Triad — a combination of low energy, menstrual disruption, and weakened bones — are estimated as high as 75%.`
  },
  {
    id: 'myths',
    title: 'Myths About\nFemale Bodies',
    shortTitle: 'Myths About Female Bodies',
    fact: `In the late 1960s, women in Germany were losing their jobs because it was believed that menstruating women secreted a toxic substance called "menotoxin." They were excluded from photo and X-ray laboratories, butcher shops, and blood donation centres — on the grounds that their blood was destructive. These were not fringe beliefs. They were accepted institutional policy.`
  },
  {
    id: 'rights',
    title: 'Reproductive\nRights',
    shortTitle: 'Reproductive Rights',
    fact: `Around 40% of women worldwide live in countries with restrictive abortion laws. The World Health Organization estimates that 39,000 women and girls die each year from the consequences of unsafe abortions — deaths that are almost entirely preventable. Reproductive rights are not a personal issue. They are a public health crisis.`
  },
  {
    id: 'medicine',
    title: 'Medicine for\nFemale Bodies',
    shortTitle: 'Medicine for Female Bodies',
    fact: `For many years, women were systematically excluded from clinical drug trials. In the United States, this practice was formalised in the early 1970s under the justification that hormonal variability made female bodies methodologically inconvenient. The result: medications were tested on male bodies and then prescribed to bodies that had never been included in the research at all.`
  },
  {
    id: 'economy',
    title: 'Economic\nData',
    shortTitle: 'Economic Data',
    fact: `By 2040, the women's health gap is projected to cause around 75 million years of healthy life lost per year. Closing this gap could boost the global economy by up to one trillion dollars annually and bring an estimated 137 million more women into employment — potentially lifting many out of poverty. The gender data gap is not only a health issue. It is an economic one.`
  },
  {
    id: 'gyn',
    title: 'Gynecological\nDiseases',
    shortTitle: 'Gynecological Diseases',
    fact: `67% of women in Germany report suffering from menstrual pain. Conditions such as endometriosis and PCOS can take an average of ten years to receive a diagnosis — years marked by dismissed symptoms, inconclusive tests, and the quiet suggestion that the pain might not be real. The delay is not a medical mystery. It is a consequence of decades of under-research.`
  }
];

// ── Build category grid ────────────────────────────────────────────────────
const grid = document.getElementById('cat-grid');
categories.forEach((cat, i) => {
  const card = document.createElement('div');
  card.className = 'cat-card';
  card.innerHTML = `
    <span class="cat-num">0${i + 1}</span>
    <span class="cat-title">${cat.title.replace('\n', '<br>')}</span>
  `;
  card.addEventListener('click', () => selectCategory(card, cat.id));
  grid.appendChild(card);
});

function selectCategory(el, id) {
  document.querySelectorAll('.cat-card').forEach(c => c.classList.remove('selected'));
  el.classList.add('selected');
  state.category = id;
  document.getElementById('btn-cat-next').disabled = false;
}

// ── Date input max ─────────────────────────────────────────────────────────
document.getElementById('input-dob').max = new Date().toISOString().split('T')[0];

// ── Progress ───────────────────────────────────────────────────────────────
function setProgress(pct) {
  document.getElementById('progress-fill').style.width = pct + '%';
}

// ── Step navigation ────────────────────────────────────────────────────────
const screenIds = ['screen-name', 'screen-category', 'screen-receipt', 'screen-done'];
const progressPcts = [0, 40, 80, 100];

function goToStep(n) {
  if (n === 3) {
    state.name = document.getElementById('input-name').value.trim();
    state.dob  = document.getElementById('input-dob').value;
    buildReceipt();
  }
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(screenIds[n - 1]).classList.add('active');
  setProgress(progressPcts[n - 1]);
  window.scrollTo(0, 0);
}

// ── Helpers ────────────────────────────────────────────────────────────────
function formatDob(val) {
  if (!val) return null;
  const [y, m, d] = val.split('-');
  return `${d}.${m}.${y}`;
}

function escHtml(s) {
  return String(s)
    .replace(/&/g,'&amp;').replace(/</g,'&lt;')
    .replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

function getCat() {
  return categories.find(c => c.id === state.category);
}

// ── Receipt preview ────────────────────────────────────────────────────────
function buildReceipt() {
  const cat  = getCat();
  const now  = new Date();
  const date = now.toLocaleDateString('de-DE', { day:'2-digit', month:'2-digit', year:'numeric' });
  const dob  = formatDob(state.dob);

  let rows = '';
  if (state.name) rows += `<div class="receipt-line"><span class="rl">Name</span><span class="rv">${escHtml(state.name)}</span></div>`;
  if (dob)        rows += `<div class="receipt-line"><span class="rl">Birthday</span><span class="rv">${dob}</span></div>`;
  if (rows)       rows += `<hr class="receipt-divider">`;

  document.getElementById('receipt-preview').innerHTML = `
    ${rows}
    <span class="receipt-cat-label">Topic</span>
    <div class="receipt-line"><span class="rv" style="text-align:left">${escHtml(cat.shortTitle)}</span></div>
    <hr class="receipt-divider">
    <span class="receipt-cat-label">Data point</span>
    <div class="receipt-fact-text">${escHtml(cat.fact)}</div>
    <div class="receipt-footer">Gender Data Gap Awareness Station · ${date}</div>
  `;
}

// ── ESC/POS receipt builder ────────────────────────────────────────────────
function buildEscPos() {
  const enc = new TextEncoder();
  const cat = getCat();
  const now = new Date();
  const date = now.toLocaleDateString('de-DE');
  const WIDTH = 32;

  function centre(s) {
    s = String(s).substring(0, WIDTH);
    const pad = Math.floor((WIDTH - s.length) / 2);
    return ' '.repeat(Math.max(0, pad)) + s;
  }

  function row(left, right) {
    left = String(left).substring(0, WIDTH - String(right).length - 1);
    const gap = WIDTH - left.length - String(right).length;
    return left + ' '.repeat(Math.max(1, gap)) + right;
  }

  function wrap(text, width) {
    const words = String(text).split(' ');
    const lines = [];
    let line = '';
    for (const w of words) {
      if ((line + ' ' + w).trim().length > width) {
        if (line) lines.push(line.trim());
        line = w;
      } else {
        line = (line + ' ' + w).trim();
      }
    }
    if (line) lines.push(line.trim());
    return lines;
  }

  const ESC_INIT = [0x1B, 0x40];
  const BOLD_ON  = [0x1B, 0x45, 0x01];
  const BOLD_OFF = [0x1B, 0x45, 0x00];
  const ALIGN_C  = [0x1B, 0x61, 0x01];
  const ALIGN_L  = [0x1B, 0x61, 0x00];
  const CUT      = [0x1D, 0x56, 0x42, 0x00];
  const DIV      = '--------------------------------';

  const chunks = [
    new Uint8Array(ESC_INIT),
    new Uint8Array(ALIGN_C),
    new Uint8Array(BOLD_ON),
    enc.encode('GENDER DATA GAP\n'),
    new Uint8Array(BOLD_OFF),
    enc.encode('Awareness Station\n'),
    enc.encode(DIV + '\n'),
    new Uint8Array(ALIGN_L),
  ];

  if (state.name) chunks.push(enc.encode(row('Name', state.name) + '\n'));
  const dob = formatDob(state.dob);
  if (dob) chunks.push(enc.encode(row('Birthday', dob) + '\n'));
  if (state.name || dob) chunks.push(enc.encode(DIV + '\n'));

  chunks.push(new Uint8Array(BOLD_ON));
  chunks.push(enc.encode('TOPIC\n'));
  chunks.push(new Uint8Array(BOLD_OFF));
  chunks.push(enc.encode(cat.shortTitle + '\n'));
  chunks.push(enc.encode(DIV + '\n'));

  chunks.push(new Uint8Array(BOLD_ON));
  chunks.push(enc.encode('DATA POINT\n'));
  chunks.push(new Uint8Array(BOLD_OFF));

  const factLines = wrap(cat.fact, WIDTH);
  for (const l of factLines) chunks.push(enc.encode(l + '\n'));

  chunks.push(enc.encode(DIV + '\n'));
  chunks.push(new Uint8Array(ALIGN_C));
  chunks.push(enc.encode(date + '\n'));
  chunks.push(enc.encode('\n\n\n'));
  chunks.push(new Uint8Array(CUT));

  const total = chunks.reduce((s, c) => s + c.length, 0);
  const out = new Uint8Array(total);
  let offset = 0;
  for (const c of chunks) { out.set(c, offset); offset += c.length; }
  return out;
}

// ── WebUSB print ───────────────────────────────────────────────────────────
let usbDevice = null;

async function printReceipt() {
  const status = document.getElementById('print-status');
  const btn    = document.getElementById('btn-print');

  if (!navigator.usb) {
    status.className = 'error';
    status.textContent = 'WebUSB not available — open this page in Chrome on your laptop.';
    return;
  }

  btn.disabled = true;
  status.className = '';

  try {
    if (!usbDevice || !usbDevice.opened) {
      status.textContent = 'Select your printer in the popup…';
      usbDevice = await navigator.usb.requestDevice({ filters: [] });
    }

    status.textContent = 'Connecting…';
    if (!usbDevice.opened) await usbDevice.open();
    if (usbDevice.configuration === null) await usbDevice.selectConfiguration(1);
    await usbDevice.claimInterface(0);

    const endpoint = usbDevice.configuration.interfaces[0]
      .alternates[0].endpoints
      .find(e => e.direction === 'out' && e.type === 'bulk');

    if (!endpoint) throw new Error('No bulk-out endpoint found.');

    status.textContent = 'Printing…';
    const data = buildEscPos();
    const chunkSize = 64;
    for (let i = 0; i < data.length; i += chunkSize) {
      await usbDevice.transferOut(endpoint.endpointNumber, data.slice(i, i + chunkSize));
      await new Promise(r => setTimeout(r, 20));
    }

    await usbDevice.releaseInterface(0);
    status.className = 'ok';
    status.textContent = '✓ Sent to printer.';
    setTimeout(() => goToStep(4), 1200);

  } catch (err) {
    btn.disabled = false;
    status.className = 'error';
    if (err.name === 'NotFoundError' || err.name === 'AbortError') {
      status.textContent = 'No printer selected. Try again.';
    } else {
      status.textContent = 'Error: ' + err.message;
    }
  }
}

// ── Reset ──────────────────────────────────────────────────────────────────
function resetAll() {
  state.name = ''; state.dob = ''; state.category = null;
  document.getElementById('input-name').value = '';
  document.getElementById('input-dob').value = '';
  document.getElementById('btn-cat-next').disabled = true;
  document.querySelectorAll('.cat-card').forEach(c => c.classList.remove('selected'));
  document.getElementById('print-status').textContent = '';
  document.getElementById('btn-print').disabled = false;
  goToStep(1);
}
</script>
</body>
</html>
