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
    height: 100%;
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
    padding: 40px 40px 0;
    display: flex;
    align-items: baseline;
    gap: 12px;
  }
  .logo {
    font-family: var(--mono);
    font-size: 13px;
    letter-spacing: 0.2em;
    color: var(--green);
    font-weight: 400;
  }
  .logo-sub {
    font-family: var(--mono);
    font-size: 11px;
    letter-spacing: 0.1em;
    color: var(--gray-mid);
    text-transform: uppercase;
  }

  /* Step counter */
  .step-counter {
    font-family: var(--mono);
    font-size: 11px;
    letter-spacing: 0.15em;
    color: var(--gray-mid);
    margin-top: 6px;
  }

  /* Main layout */
  main {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 80px 40px 60px;
  }

  /* Screen (each step) */
  .screen {
    display: none;
    width: 100%;
    max-width: 460px;
    animation: fadeUp 0.35s ease both;
  }
  .screen.active { display: block; }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(12px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .screen-label {
    font-family: var(--mono);
    font-size: 11px;
    letter-spacing: 0.2em;
    color: var(--green);
    text-transform: uppercase;
    margin-bottom: 20px;
  }

  .screen h2 {
    font-size: 28px;
    font-weight: 300;
    line-height: 1.2;
    letter-spacing: -0.02em;
    color: var(--black);
    margin-bottom: 8px;
  }

  .screen-desc {
    font-size: 14px;
    color: var(--gray-dark);
    line-height: 1.6;
    margin-bottom: 40px;
    font-weight: 300;
  }

  /* Inputs */
  .field-group { margin-bottom: 28px; }

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

  /* Selection cards */
  .options-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-bottom: 40px;
  }

  .option-card {
    padding: 20px 16px;
    background: var(--gray-bg);
    border: 1px solid var(--gray-light);
    border-radius: var(--radius);
    cursor: pointer;
    transition: border-color 0.15s, background 0.15s;
    text-align: left;
    user-select: none;
    -webkit-tap-highlight-color: transparent;
  }
  .option-card:active { transform: scale(0.98); }

  .option-card .option-icon {
    font-family: var(--mono);
    font-size: 22px;
    display: block;
    margin-bottom: 8px;
    color: var(--gray-dark);
  }
  .option-card .option-title {
    font-size: 15px;
    font-weight: 400;
    color: var(--black);
    display: block;
    margin-bottom: 4px;
  }
  .option-card .option-sub {
    font-family: var(--mono);
    font-size: 10px;
    letter-spacing: 0.1em;
    color: var(--gray-mid);
    display: block;
  }

  .option-card.selected {
    border-color: var(--green);
    background: var(--white);
  }
  .option-card.selected .option-title { color: var(--green); }
  .option-card.selected .option-icon  { color: var(--green); }

  /* Age brackets (step 2b) */
  .age-groups {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 8px;
    margin-bottom: 12px;
  }
  .age-btn {
    padding: 14px 8px;
    background: var(--gray-bg);
    border: 1px solid var(--gray-light);
    border-radius: var(--radius);
    font-family: var(--mono);
    font-size: 12px;
    color: var(--black);
    cursor: pointer;
    text-align: center;
    transition: border-color 0.15s, background 0.15s;
    -webkit-tap-highlight-color: transparent;
  }
  .age-btn.selected {
    border-color: var(--green);
    background: var(--white);
    color: var(--green);
  }

  /* Buttons */
  .btn-primary {
    width: 100%;
    padding: 16px;
    background: var(--green);
    color: var(--white);
    font-family: var(--mono);
    font-size: 12px;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    border: none;
    border-radius: var(--radius);
    cursor: pointer;
    transition: background 0.15s;
    -webkit-tap-highlight-color: transparent;
  }
  .btn-primary:active { background: #3d8a6e; }
  .btn-primary:disabled {
    background: var(--gray-light);
    color: var(--gray-mid);
    cursor: not-allowed;
  }

  .btn-ghost {
    width: 100%;
    padding: 14px;
    background: transparent;
    color: var(--gray-mid);
    font-family: var(--mono);
    font-size: 11px;
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
    padding: 28px 24px;
    margin-bottom: 28px;
    font-family: var(--mono);
    font-size: 12px;
    line-height: 1.8;
    color: var(--black);
    position: relative;
    overflow: hidden;
  }
  .receipt-preview::before {
    content: '';
    display: block;
    height: 2px;
    background: repeating-linear-gradient(
      90deg, var(--green) 0, var(--green) 6px, transparent 6px, transparent 12px
    );
    margin-bottom: 20px;
  }
  .receipt-line { display: flex; justify-content: space-between; gap: 12px; }
  .receipt-line .rl { color: var(--gray-mid); }
  .receipt-line .rv { font-weight: 400; text-align: right; }
  .receipt-divider {
    border: none;
    border-top: 1px dashed var(--gray-light);
    margin: 12px 0;
  }
  .receipt-fact {
    margin-top: 16px;
    padding-top: 16px;
    border-top: 1px dashed var(--gray-light);
    font-size: 10px;
    line-height: 1.7;
    color: var(--gray-dark);
  }
  .receipt-fact-label {
    font-size: 9px;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--green);
    display: block;
    margin-bottom: 6px;
  }
  .receipt-footer {
    margin-top: 16px;
    font-size: 9px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--gray-mid);
    text-align: center;
  }

  /* Print status */
  #print-status {
    font-family: var(--mono);
    font-size: 11px;
    letter-spacing: 0.1em;
    color: var(--gray-mid);
    text-align: center;
    margin-top: 14px;
    min-height: 20px;
  }
  #print-status.error { color: var(--green); }
  #print-status.ok    { color: #2a7a2a; }

  /* Thank you screen */
  #screen-done .done-mark {
    font-family: var(--mono);
    font-size: 48px;
    color: var(--green);
    display: block;
    margin-bottom: 20px;
  }

  /* Divider line */
  .hairline {
    width: 32px;
    height: 1px;
    background: var(--green);
    margin: 20px 0;
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

  <!-- Step 1: Name -->
  <div class="screen active" id="screen-name">
    <p class="screen-label">01 / 03 — Identification</p>
    <h2>What is your name?</h2>
    <p class="screen-desc">Your name will appear on your personalised data receipt.</p>
    <div class="field-group">
      <label class="field-label" for="input-name">First name</label>
      <input type="text" id="input-name" placeholder="Enter your name" autocomplete="off" autocorrect="off" spellcheck="false">
    </div>
    <button class="btn-primary" id="btn-name-next" onclick="goToStep2()" disabled>Continue</button>
  </div>

  <!-- Step 2: Birthday + biological context -->
  <div class="screen" id="screen-bio">
    <p class="screen-label">02 / 03 — Biological context</p>
    <h2>Tell us a bit more.</h2>
    <p class="screen-desc">This helps us generate accurate contextual health data for your receipt.</p>

    <div class="field-group">
      <label class="field-label" for="input-dob">Date of birth</label>
      <input type="date" id="input-dob" max="">
    </div>

    <div class="field-group">
      <label class="field-label">Biological sex</label>
      <div class="options-grid" style="grid-template-columns: 1fr 1fr 1fr; gap: 10px; margin-bottom: 0;">
        <div class="option-card" onclick="selectSex(this, 'female')">
          <span class="option-icon">♀</span>
          <span class="option-title">Female</span>
        </div>
        <div class="option-card" onclick="selectSex(this, 'male')">
          <span class="option-icon">♂</span>
          <span class="option-title">Male</span>
        </div>
        <div class="option-card" onclick="selectSex(this, 'other')">
          <span class="option-icon">⚧</span>
          <span class="option-title">Other</span>
        </div>
      </div>
    </div>

    <div class="field-group">
      <label class="field-label">Age group</label>
      <div class="age-groups">
        <button class="age-btn" onclick="selectAge(this, '13–17')">13–17</button>
        <button class="age-btn" onclick="selectAge(this, '18–29')">18–29</button>
        <button class="age-btn" onclick="selectAge(this, '30–44')">30–44</button>
        <button class="age-btn" onclick="selectAge(this, '45–59')">45–59</button>
        <button class="age-btn" onclick="selectAge(this, '60+')">60+</button>
        <button class="age-btn" onclick="selectAge(this, 'Prefer not')">—</button>
      </div>
    </div>

    <button class="btn-primary" id="btn-bio-next" onclick="goToStep3()" disabled>Continue</button>
    <button class="btn-ghost" onclick="goToStep(1)">← Back</button>
  </div>

  <!-- Step 3: Receipt + print -->
  <div class="screen" id="screen-receipt">
    <p class="screen-label">03 / 03 — Your data receipt</p>
    <h2>Ready to print.</h2>
    <p class="screen-desc">Review your personalised receipt below, then connect the printer.</p>

    <div class="receipt-preview" id="receipt-preview">
      <!-- filled by JS -->
    </div>

    <button class="btn-primary" id="btn-print" onclick="printReceipt()">
      ⬡ Connect USB printer &amp; print
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
    <p class="screen-desc" style="margin-bottom: 40px;">The gender data gap is real. Your awareness is the first step.</p>
    <button class="btn-primary" onclick="resetAll()">Start over</button>
  </div>

</main>

<script>
// ── State ──────────────────────────────────────────────────────────────────
const state = { name: '', dob: '', sex: '', age: '' };

// ── Health data facts keyed by sex + age bracket ───────────────────────────
const facts = {
  female: {
    '13–17': 'Menstrual cycle irregularity is common in the first 2–3 years after menarche. This is physiologically normal, not a disorder.',
    '18–29': 'Women aged 18–29 are statistically the most under-represented group in cardiovascular clinical trials, despite being the highest risk decade for autoimmune disease onset.',
    '30–44': 'Endometriosis affects approximately 1 in 10 people who menstruate. Average time from first symptom to diagnosis: 7–9 years.',
    '45–59': 'Perimenopausal symptoms often go unrecognised in clinical settings. Only 20% of UK medical schools include menopause as a required curriculum topic.',
    '60+':   'Post-menopausal women represent the fastest-growing group for osteoporosis-related fractures, yet bone density screening remains inconsistently offered.',
    'Prefer not': 'The gender data gap in medicine means that drug dosages, symptom profiles, and diagnostic criteria have historically been based on male study populations.'
  },
  male: {
    '13–17': 'Male adolescents are significantly less likely to seek medical help for mental health symptoms — a disparity linked to socialisation norms rather than lower prevalence.',
    '18–29': 'Testicular cancer peaks between 15–35 years. It is among the most treatable cancers when caught early, yet self-examination is rarely discussed in routine care.',
    '30–44': 'Male patients are more likely to present to emergency care in later disease stages due to lower rates of routine preventive screening in this age group.',
    '45–59': 'Cardiovascular disease risk rises significantly from mid-40s in men. Cholesterol and blood pressure are the most actionable early indicators.',
    '60+':   'Prostate-specific antigen (PSA) screening remains contested — it reduces mortality but carries high false-positive rates that lead to unnecessary interventions.',
    'Prefer not': 'Biological sex influences drug metabolism, symptom expression, and disease progression — yet many clinical guidelines remain effectively sex-neutral.'
  },
  other: {
    '13–17': 'Trans and non-binary adolescents face significant gaps in gender-affirming healthcare access, with wait times for specialist referrals averaging 2–4 years in many EU countries.',
    '18–29': 'Gender-diverse individuals report higher rates of delayed or withheld care due to provider inexperience — a structural gap, not a patient behaviour gap.',
    '30–44': 'Hormone therapy in trans individuals affects cardiovascular and bone density markers. Long-term monitoring protocols are still being standardised across clinical settings.',
    '45–59': 'Non-binary and intersex bodies remain almost entirely absent from clinical trial data, meaning evidence-based recommendations are rarely directly applicable.',
    '60+':   'Older trans individuals came of age before legal gender recognition existed in most countries. Their healthcare histories are often fragmented and under-documented.',
    'Prefer not': 'The gender data gap extends beyond male/female binaries. Research on intersex variations, non-binary health needs, and gender-diverse ageing remains in early stages.'
  },
  default: {
    'Prefer not': 'The gender data gap in medicine is a documented structural inequality. Many diagnostic criteria, drug trials, and treatment guidelines were built on predominantly male data.',
    default:      'Across all demographics, the single strongest predictor of health outcome is access — to information, to diagnostic tools, and to providers who recognise your symptoms.'
  }
};

function getFact() {
  const s = state.sex || 'default';
  const a = state.age || 'Prefer not';
  if (facts[s] && facts[s][a]) return facts[s][a];
  if (facts[s] && facts[s]['Prefer not']) return facts[s]['Prefer not'];
  return facts.default[a] || facts.default.default;
}

// ── Progress ───────────────────────────────────────────────────────────────
function setProgress(pct) {
  document.getElementById('progress-fill').style.width = pct + '%';
}

// ── Step navigation ────────────────────────────────────────────────────────
function goToStep(n) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  const screens = ['screen-name', 'screen-bio', 'screen-receipt', 'screen-done'];
  document.getElementById(screens[n - 1]).classList.add('active');
  setProgress([0, 33, 66, 100][n - 1]);
  window.scrollTo(0, 0);
}

function goToStep2() {
  state.name = document.getElementById('input-name').value.trim();
  if (!state.name) return;
  goToStep(2);
}

function goToStep3() {
  state.dob = document.getElementById('input-dob').value;
  if (!state.sex) return;
  buildReceipt();
  goToStep(3);
}

// ── Name input validation ──────────────────────────────────────────────────
document.getElementById('input-name').addEventListener('input', function() {
  document.getElementById('btn-name-next').disabled = this.value.trim().length < 1;
});
document.getElementById('input-name').addEventListener('keydown', function(e) {
  if (e.key === 'Enter') goToStep2();
});

// Set max date on DOB to today
document.getElementById('input-dob').max = new Date().toISOString().split('T')[0];

// ── Sex / age selection ────────────────────────────────────────────────────
function selectSex(el, val) {
  document.querySelectorAll('#screen-bio .option-card').forEach(c => c.classList.remove('selected'));
  el.classList.add('selected');
  state.sex = val;
  checkBioStep();
}

function selectAge(el, val) {
  document.querySelectorAll('.age-btn').forEach(b => b.classList.remove('selected'));
  el.classList.add('selected');
  state.age = val;
  checkBioStep();
}

function checkBioStep() {
  document.getElementById('btn-bio-next').disabled = !state.sex;
}

// ── Receipt builder ────────────────────────────────────────────────────────
function formatDob(val) {
  if (!val) return '—';
  const [y, m, d] = val.split('-');
  return `${d}.${m}.${y}`;
}

function buildReceipt() {
  const now = new Date();
  const ts = now.toLocaleDateString('de-DE', { day:'2-digit', month:'2-digit', year:'numeric' })
    + ' ' + now.toLocaleTimeString('de-DE', { hour:'2-digit', minute:'2-digit' });

  const sexLabel = { female: 'Female', male: 'Male', other: 'Other / prefer not to specify' }[state.sex] || '—';
  const ageLabel = state.age || '—';
  const fact = getFact();

  document.getElementById('receipt-preview').innerHTML = `
    <div class="receipt-line"><span class="rl">Name</span><span class="rv">${escHtml(state.name)}</span></div>
    <div class="receipt-line"><span class="rl">Date of birth</span><span class="rv">${formatDob(state.dob)}</span></div>
    <hr class="receipt-divider">
    <div class="receipt-line"><span class="rl">Biological sex</span><span class="rv">${sexLabel}</span></div>
    <div class="receipt-line"><span class="rl">Age group</span><span class="rv">${ageLabel}</span></div>
    <hr class="receipt-divider">
    <div class="receipt-fact">
      <span class="receipt-fact-label">Data point for you</span>
      ${escHtml(fact)}
    </div>
    <div class="receipt-footer">Awareness Station · ${ts}</div>
  `;
}

function escHtml(s) {
  return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

// ── ESC/POS receipt bytes ──────────────────────────────────────────────────
function buildEscPos() {
  const enc = new TextEncoder();
  const lines = [];

  const esc = (bytes) => bytes;
  const text = (s) => enc.encode(s);
  const nl = () => enc.encode('\n');

  // Helper: pad to 32 chars (58mm printer = 32 chars wide at 12cpi)
  function centre(s, width = 32) {
    s = String(s).substring(0, width);
    const pad = Math.max(0, Math.floor((width - s.length) / 2));
    return ' '.repeat(pad) + s;
  }
  function row(left, right, width = 32) {
    left = String(left).substring(0, width - right.length - 1);
    const gap = width - left.length - right.length;
    return left + ' '.repeat(Math.max(1, gap)) + right;
  }

  const ESC_INIT    = [0x1B, 0x40];
  const BOLD_ON     = [0x1B, 0x45, 0x01];
  const BOLD_OFF    = [0x1B, 0x45, 0x00];
  const ALIGN_C     = [0x1B, 0x61, 0x01];
  const ALIGN_L     = [0x1B, 0x61, 0x00];
  const CUT         = [0x1D, 0x56, 0x42, 0x00];

  const now = new Date();
  const ts = now.toLocaleDateString('de-DE') + ' ' + now.toLocaleTimeString('de-DE', {hour:'2-digit',minute:'2-digit'});
  const sexLabel = { female: 'Female', male: 'Male', other: 'Other' }[state.sex] || '-';

  const chunks = [
    new Uint8Array(ESC_INIT),
    new Uint8Array(ALIGN_C),
    new Uint8Array(BOLD_ON),
    text('GENDER DATA GAP\n'),
    new Uint8Array(BOLD_OFF),
    text('Awareness Station\n'),
    text('--------------------------------\n'),
    new Uint8Array(ALIGN_L),
    text(row('Name', state.name) + '\n'),
    text(row('Date of birth', formatDob(state.dob)) + '\n'),
    text('--------------------------------\n'),
    text(row('Biological sex', sexLabel) + '\n'),
    text(row('Age group', state.age || '-') + '\n'),
    text('--------------------------------\n'),
    new Uint8Array(BOLD_ON),
    text('DATA POINT FOR YOU\n'),
    new Uint8Array(BOLD_OFF),
  ];

  // Word-wrap the fact to 32 chars
  const fact = getFact();
  const words = fact.split(' ');
  let line = '';
  for (const w of words) {
    if ((line + ' ' + w).trim().length > 32) {
      chunks.push(text(line.trim() + '\n'));
      line = w;
    } else {
      line = (line + ' ' + w).trim();
    }
  }
  if (line) chunks.push(text(line + '\n'));

  chunks.push(
    text('--------------------------------\n'),
    new Uint8Array(ALIGN_C),
    text(ts + '\n'),
    text('ninacaterina.github.io/Gender-Data-Gap\n'),
    text('\n\n\n'),
    new Uint8Array(CUT)
  );

  const total = chunks.reduce((s, c) => s + c.length, 0);
  const out = new Uint8Array(total);
  let offset = 0;
  for (const c of chunks) { out.set(c, offset); offset += c.length; }
  return out;
}

// ── WebUSB print (AnJet58 / generic ESC/POS USB) ───────────────────────────
let usbDevice = null;

async function printReceipt() {
  const status = document.getElementById('print-status');
  const btn    = document.getElementById('btn-print');

  if (!navigator.usb) {
    status.className = 'error';
    status.textContent = 'WebUSB not available — open this page in Chrome or Edge on your laptop.';
    return;
  }

  btn.disabled = true;
  status.className = '';

  try {
    // Ask user to pick the printer if not already paired
    if (!usbDevice || !usbDevice.opened) {
      status.textContent = 'Select your AnJet58 printer in the popup…';
      usbDevice = await navigator.usb.requestDevice({
        filters: [] // show all USB devices — select the AnJet58 from the list
      });
    }

    status.textContent = 'Connecting…';
    if (!usbDevice.opened) await usbDevice.open();

    // Select configuration and claim interface
    if (usbDevice.configuration === null) await usbDevice.selectConfiguration(1);
    await usbDevice.claimInterface(0);

    // Find the bulk-out endpoint (where we send print data)
    const iface = usbDevice.configuration.interfaces[0];
    const altIface = iface.alternates[0];
    const endpoint = altIface.endpoints.find(e => e.direction === 'out' && e.type === 'bulk');

    if (!endpoint) throw new Error('No bulk-out endpoint found on this printer.');

    status.textContent = 'Printing…';
    const data = buildEscPos();

    // Send in chunks
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
      status.textContent = 'No printer selected. Try again and pick the AnJet58 from the list.';
    } else if (err.message.includes('Access denied') || err.name === 'SecurityError') {
      status.textContent = 'USB access denied — make sure you\'re on HTTPS or localhost.';
    } else {
      status.textContent = 'Error: ' + err.message;
    }
  }
}

// ── Reset ──────────────────────────────────────────────────────────────────
function resetAll() {
  state.name = ''; state.dob = ''; state.sex = ''; state.age = '';
  document.getElementById('input-name').value = '';
  document.getElementById('input-dob').value = '';
  document.getElementById('btn-name-next').disabled = true;
  document.getElementById('btn-bio-next').disabled = true;
  document.querySelectorAll('.option-card, .age-btn').forEach(el => el.classList.remove('selected'));
  document.getElementById('print-status').textContent = '';
  document.getElementById('btn-print').disabled = false;
  goToStep(1);
}
</script>
</body>
</html>
