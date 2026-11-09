---
layout: raw
permalink: /bitw0r1d/
---
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>bitw0r1d - Winters & Charbonneau (2026)</title>
  <meta name="description" content="Run the bitw0r1d model in your browser: a simulation of how cultural systems and search spaces co-evolve, from Winters & Charbonneau (2026).">
  <link rel="canonical" href="https://j-winters.github.io/bitw0r1d/">

  <!-- Link previews (social media, messaging apps) -->
  <meta property="og:type" content="website">
  <meta property="og:url" content="https://j-winters.github.io/bitw0r1d/">
  <meta property="og:title" content="bitw0r1d: A co-evolutionary model of cultural systems and search spaces">
  <meta property="og:description" content="Run the bitw0r1d model in your browser: a simulation of how cultural systems and search spaces co-evolve, from Winters & Charbonneau (2026).">
  <meta name="twitter:card" content="summary">
  <meta name="twitter:title" content="bitw0r1d: A co-evolutionary model of cultural systems and search spaces">
  <meta name="twitter:description" content="Run the bitw0r1d model in your browser: a simulation of how cultural systems and search spaces co-evolve, from Winters & Charbonneau (2026).">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&family=Source+Sans+3:wght@400;600;700&display=swap">
  <style>
    /* Neutral colours. Colours that carry meaning (cultural system, search space,
       and each measure) are set in the COLOURS object in the script below. */
    :root {
      --bg: #ffffff;
      --surface: #f8fafc;
      --border: #e2e8f0;
      --border-strong: #cbd5e1;
      --text: #1e293b;
      --heading: #0f172a;
      --muted: #475569;
      --accent: #0f172a;
    }

    * { box-sizing: border-box; }
    body {
      margin: 0;
      background: var(--bg);
      color: var(--text);
      font-family: 'Source Sans 3', system-ui, -apple-system, 'Segoe UI', sans-serif;
      font-size: 17px;
      line-height: 1.55;
    }
    a { color: var(--heading); text-decoration: underline; text-underline-offset: 2px; }
    a:hover { color: var(--muted); }
    :focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }

    .page { max-width: 1100px; margin: 0 auto; padding: 32px 24px 64px; }
    h1 { margin: 0; font-size: 2.4rem; line-height: 1.1; font-weight: 700; color: var(--heading); }
    .subtitle { margin: 6px 0 14px; font-size: 1.2rem; color: var(--muted); }
    .intro { margin: 0 0 20px; }

    /* Collapsible explanations */
    .info details { border-top: 1px solid var(--border); }
    .info details:last-child { border-bottom: 1px solid var(--border); }
    .info summary { cursor: pointer; list-style: none; padding: 10px 0; font-weight: 600; color: var(--heading); }
    .info summary::-webkit-details-marker { display: none; }
    .info summary::before { content: '\25B8'; display: inline-block; width: 1.2em; color: var(--muted); transition: transform 0.15s ease; }
    .info details[open] > summary::before { transform: rotate(90deg); }
    .info .body { padding: 0 0 6px; }
    .info .body p { margin: 0 0 10px; }
    .eq { white-space: nowrap; font-style: italic; }

    /* Panels */
    .panel { margin-top: 20px; padding: 20px; border: 1px solid var(--border); border-radius: 10px; background: var(--bg); }
    .panel h2 { margin: 0 0 4px; font-size: 1.15rem; color: var(--heading); }
    .note { margin: 0 0 16px; font-size: 0.92rem; color: var(--muted); }

    /* Buttons */
    .buttons { display: flex; flex-wrap: wrap; gap: 10px; margin-bottom: 24px; }
    button {
      display: inline-flex; align-items: center; gap: 8px;
      padding: 9px 16px;
      font: inherit; font-size: 0.95rem; font-weight: 600;
      color: var(--heading); background: var(--bg);
      border: 1px solid var(--border-strong); border-radius: 8px;
      cursor: pointer;
    }
    button:hover:not(:disabled) { background: var(--surface); }
    button.primary { min-width: 7.5em; justify-content: center; color: #ffffff; background: var(--accent); border-color: var(--accent); }
    button.primary:hover:not(:disabled) { background: #334155; border-color: #334155; }
    button:disabled { opacity: 0.45; cursor: not-allowed; }

    /* Controls */
    .columns { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 24px 40px; }
    .field { margin-bottom: 18px; }
    .field label { display: flex; justify-content: space-between; gap: 12px; margin-bottom: 4px; font-size: 0.95rem; font-weight: 600; color: var(--heading); }
    .field .value { font-family: 'JetBrains Mono', ui-monospace, monospace; font-size: 0.88rem; font-weight: 400; color: var(--muted); white-space: nowrap; }
    .help { margin: 2px 0 0; font-size: 0.86rem; color: var(--muted); }
    input[type=range] { width: 100%; margin: 4px 0; }
    input[type=number], select {
      padding: 6px 10px;
      font: inherit; font-size: 0.95rem;
      color: var(--text); background: var(--bg);
      border: 1px solid var(--border-strong); border-radius: 6px;
    }
    input:disabled, select:disabled { opacity: 0.5; cursor: not-allowed; }
    .seed-row { display: flex; gap: 8px; }
    .seed-row input { flex: 1; min-width: 0; }
    .seed-row button { padding: 6px 12px; }

    /* Current state */
    .gen-title .progress { font-size: 0.95rem; font-weight: 400; color: var(--muted); }
    .bits-block { margin: 14px 0 16px; padding-left: 14px; border-left: 4px solid; }
    .bits-label { display: flex; flex-wrap: wrap; justify-content: space-between; gap: 2px 16px; margin-bottom: 6px; font-size: 0.92rem; font-weight: 600; color: var(--muted); }
    .bits-note { font-weight: 400; }
    .bitbox { padding: 10px 12px; background: var(--surface); border: 1px solid var(--border); border-radius: 6px; }
    .bitbox canvas { display: block; }
    .digits { font-family: 'JetBrains Mono', ui-monospace, monospace; font-size: 1.05rem; letter-spacing: 0.08em; line-height: 1.6; word-break: break-all; }
    .digits .zero { color: #64748b; }
    .digits .one { font-weight: 700; }

    .stats { display: grid; grid-template-columns: repeat(auto-fit, minmax(190px, 1fr)); gap: 14px; margin-top: 20px; }
    .stat { padding: 12px 14px; border: 1px solid var(--border); border-top: 3px solid; border-radius: 8px; }
    .stat .k { font-size: 0.9rem; font-weight: 600; color: var(--muted); }
    .stat .v { font-family: 'JetBrains Mono', ui-monospace, monospace; font-size: 1.55rem; font-weight: 700; line-height: 1.3; }
    .stat .s { font-size: 0.85rem; color: var(--muted); }
    .banner { margin-top: 16px; padding: 12px 14px; color: #991b1b; background: #fef2f2; border: 1px solid #fecaca; border-radius: 8px; }

    /* Charts */
    .chart-options {
      display: flex; flex-wrap: wrap; align-items: center; gap: 10px 28px;
      margin: 12px 0 18px; padding: 10px 12px;
      font-size: 0.92rem;
      background: var(--surface); border: 1px solid var(--border); border-radius: 8px;
    }
    .chart-options .opt { display: flex; flex-wrap: wrap; align-items: center; gap: 8px; }
    .chart-options .opt-title { font-weight: 600; color: var(--heading); }
    .chart-options .check { display: inline-flex; align-items: center; gap: 5px; }
    .chart-options input[type=number] { width: 7.5em; padding: 4px 8px; }
    .chart-options select { padding: 4px 8px; }
    .charts { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 28px 32px; }
    .chart h3 { display: flex; flex-wrap: wrap; justify-content: space-between; align-items: baseline; gap: 4px 12px; margin: 0 0 6px; font-size: 1rem; color: var(--heading); }
    .chart-box { min-height: 210px; overflow: hidden; }
    .chart-box svg { display: block; }
    .chart-box svg text { font-family: 'Source Sans 3', system-ui, sans-serif; font-size: 12px; fill: #64748b; }
    .legend { display: inline-flex; flex-wrap: wrap; gap: 4px 14px; font-size: 0.85rem; font-weight: 400; color: var(--muted); }
    .legend i { display: inline-block; width: 14px; height: 3px; margin-right: 6px; vertical-align: middle; border-radius: 2px; }

    /* Footer */
    .footer { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 20px 40px; margin-top: 40px; padding-top: 20px; border-top: 1px solid var(--border); font-size: 0.92rem; color: var(--muted); }
    .footer h2 { margin: 0 0 4px; font-size: 0.95rem; color: var(--heading); }
    .footer p { margin: 0; }

    @media (max-width: 800px) {
      .columns { grid-template-columns: minmax(0, 1fr); }
      .footer { grid-template-columns: minmax(0, 1fr); }
    }
    @media (max-width: 760px) {
      .charts { grid-template-columns: minmax(0, 1fr); }
    }
    @media (max-width: 640px) {
      body { font-size: 16px; }
      .page { padding: 20px 16px 48px; }
      .panel { padding: 16px; }
      h1 { font-size: 2rem; }
    }
  </style>
</head>
<body>
  <div id="root"></div>
  <noscript><p style="padding: 24px; font-family: sans-serif;">This simulation needs JavaScript to run.</p></noscript>

  <!-- Libraries, with versions pinned -->
  <script src="https://cdn.jsdelivr.net/npm/react@18.3.1/umd/react.production.min.js" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/react-dom@18.3.1/umd/react-dom.production.min.js" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/htm@3.1.1/dist/htm.umd.js" crossorigin="anonymous"></script>

  <!-- =======================================================================
       Simulation engine
       -----------------------------------------------------------------------
       This script defines createEngine(). The page copies it into a Web Worker
       so long runs don't freeze the interface. If workers are unavailable, it
       runs on the page itself. All randomness comes from seeded generators, so
       the same seed and settings always give the same run.
       ======================================================================= -->
  <script id="engine-src">
function createEngine(post) {
  'use strict';

  // ---- Seeded random numbers -----------------------------------------------
  // splitmix32 turns a seed into well-mixed starting values for sfc32,
  // a fast 32-bit generator with a long period.
  function splitmix32(a) {
    return function () {
      a = (a + 0x9e3779b9) | 0;
      let t = a ^ (a >>> 16);
      t = Math.imul(t, 0x21f0aaad);
      t = t ^ (t >>> 15);
      t = Math.imul(t, 0x735a2d97);
      return (t ^ (t >>> 15)) >>> 0;
    };
  }

  function sfc32(a, b, c, d) {
    return function () {
      a |= 0; b |= 0; c |= 0; d |= 0;
      const t = (((a + b) | 0) + d) | 0;
      d = (d + 1) | 0;
      a = b ^ (b >>> 9);
      b = (c + (c << 3)) | 0;
      c = (c << 21) | (c >>> 11);
      c = (c + t) | 0;
      return (t >>> 0) / 4294967296;
    };
  }

  // Each use gets its own independent stream:
  // 1 = starting cultural system, 2 = starting search space, 3 = the run itself.
  function makeRng(seed, stream) {
    const mix = splitmix32(Math.imul(seed | 0, 0x9e3779b1) ^ Math.imul(stream, 0x85ebca77));
    const rng = sfc32(mix(), mix(), mix(), mix());
    for (let i = 0; i < 15; i++) rng();
    return rng;
  }

  // ---- Bitstrings ------------------------------------------------------------
  // Bitstrings are stored as arrays of 0s and 1s.

  // Starting strings: draw the probability of a 1, then draw each bit.
  function generateBits(length, rng) {
    const prob = rng();
    const bits = [];
    for (let i = 0; i < length; i++) bits.push(rng() < prob ? 1 : 0);
    return bits;
  }

  // One change, each type with probability 1/3:
  // modification (flip a bit), expansion (insert a bit), simplification (delete a bit).
  function mutate(bits, rng) {
    const n = bits.length;
    if (n === 0) return [rng() < 0.5 ? 1 : 0];
    const out = bits.slice();
    const r = rng();
    if (r < 1 / 3) {
      const pos = Math.floor(rng() * n);
      out[pos] = 1 - out[pos];
    } else if (r < 2 / 3) {
      const pos = Math.floor(rng() * (n + 1));
      out.splice(pos, 0, rng() < 0.5 ? 1 : 0);
    } else if (n > 1) {
      out.splice(Math.floor(rng() * n), 1);
    }
    return out;
  }

  // Levenshtein distance using two rows of the matrix instead of the full matrix.
  let rowA = new Int32Array(256);
  let rowB = new Int32Array(256);
  function levenshtein(s, t) {
    const n = s.length;
    const m = t.length;
    if (n === 0) return m;
    if (m === 0) return n;
    if (rowA.length < m + 1) {
      rowA = new Int32Array(2 * (m + 1));
      rowB = new Int32Array(2 * (m + 1));
    }
    let prev = rowA;
    let cur = rowB;
    for (let j = 0; j <= m; j++) prev[j] = j;
    for (let i = 1; i <= n; i++) {
      cur[0] = i;
      const si = s[i - 1];
      for (let j = 1; j <= m; j++) {
        let v = prev[j - 1] + (si === t[j - 1] ? 0 : 1);
        const del = prev[j] + 1;
        if (del < v) v = del;
        const ins = cur[j - 1] + 1;
        if (ins < v) v = ins;
        cur[j] = v;
      }
      const tmp = prev; prev = cur; cur = tmp;
    }
    return prev[m];
  }

  // Effectiveness: 1 minus the normalised Levenshtein distance.
  function effectiveness(cc, ss) {
    const maxLen = Math.max(cc.length, ss.length);
    return maxLen === 0 ? 1 : 1 - levenshtein(cc, ss) / maxLen;
  }

  // ---- Resources -------------------------------------------------------------
  // Net resources: R = len(search space) * E - len(cultural system) * (1 - E)
  function netResources(lenS, E, lenC) {
    return lenS * E - lenC * (1 - E);
  }

  // Resources available for a generation (same logic as the Python model).
  // If R >= 1, R is available. Otherwise the store pays a deficit
  // (1 if 0 <= R < 1, |R| if R < 0) and one change is allowed.
  // If the store can't pay, the society collapses.
  function drawResources(net, store) {
    if (net >= 1) return { avail: net, store: store, collapsed: false };
    const deficit = net >= 0 ? 1 : -net;
    if (store >= deficit) return { avail: 1, store: store - deficit, collapsed: false };
    return { avail: store, store: 0, collapsed: true };
  }

  // ---- History: one row per completed generation -----------------------------
  const FIELDS = {
    lc: Int32Array, ls: Int32Array,
    E: Float64Array, net: Float64Array, avail: Float64Array, store: Float64Array,
    eta: Float64Array, lambda: Float64Array, p: Float64Array
  };

  function History() {
    this.n = 0;
    this.cap = 1024;
    this.d = {};
    for (const f in FIELDS) this.d[f] = new FIELDS[f](this.cap);
  }
  History.prototype.push = function (row) {
    if (this.n === this.cap) {
      this.cap *= 2;
      for (const f in FIELDS) {
        const grown = new FIELDS[f](this.cap);
        grown.set(this.d[f]);
        this.d[f] = grown;
      }
    }
    for (const f in FIELDS) this.d[f][this.n] = row[f];
    this.n += 1;
  };

  // Rows to send for the charts: evenly spaced rows plus log-spaced rows,
  // so the charts look right on both linear and log generation axes.
  function sampleIndices(n, k) {
    const out = [];
    if (n === 0) return out;
    if (n <= 2 * k) {
      for (let i = 0; i < n; i++) out.push(i);
      return out;
    }
    const lin = [];
    const lg = [];
    const logN = Math.log(n);
    for (let i = 0; i < k; i++) {
      lin.push(Math.round(i * (n - 1) / (k - 1)));
      lg.push(Math.min(n - 1, Math.max(0, Math.round(Math.exp(i * logN / (k - 1))) - 1)));
    }
    let a = 0, b = 0, last = -1;
    while (a < k || b < k) {
      const v = (b >= k || (a < k && lin[a] <= lg[b])) ? lin[a++] : lg[b++];
      if (v !== last) { out.push(v); last = v; }
    }
    return out;
  }

  // ---- State -------------------------------------------------------------------
  const BUDGET_MS = 30;       // longest stretch of work before yielding
  const POST_MS = 50;         // shortest gap between interface updates
  const WORK_CHECK = 250000;  // check the clock after roughly this many Levenshtein cells
  const CHART_POINTS = 400;
  function now() {
    return (typeof performance !== 'undefined' && performance.now) ? performance.now() : Date.now();
  }

  const S = {
    seed: 12345, lc0: 8, ls0: 8, endowment: 100,
    eta: 0.99, lambda: 0.05, p: 0.5, delay: 100, queued: null,
    rng: null, cc: [], ss: [], E: 1, store: 100, initC: '', initS: '',
    gen: 0, running: false, stepOnly: false, collapsed: false, pending: null,
    genStartedAt: 0, lastPost: -Infinity, timer: null,
    maxLen: 0, minNet: Infinity, maxNet: -Infinity, maxStore: 0
  };
  let H = new History();

  function clearTimer() {
    if (S.timer !== null) { clearTimeout(S.timer); S.timer = null; }
  }
  function schedule(ms) {
    clearTimer();
    S.timer = setTimeout(tick, ms);
  }
  function applyParams(m) {
    S.eta = m.eta; S.lambda = m.lambda; S.p = m.p;
  }

  function reset(m) {
    clearTimer();
    S.seed = m.seed | 0;
    S.lc0 = Math.max(1, m.lc0 | 0);
    S.ls0 = Math.max(1, m.ls0 | 0);
    S.endowment = Number(m.endowment);
    S.cc = generateBits(S.lc0, makeRng(S.seed, 1));
    S.ss = generateBits(S.ls0, makeRng(S.seed, 2));
    S.rng = makeRng(S.seed, 3);
    S.initC = S.cc.join('');
    S.initS = S.ss.join('');
    S.E = effectiveness(S.cc, S.ss);
    S.store = S.endowment;
    S.gen = 0;
    S.running = false;
    S.stepOnly = false;
    S.collapsed = false;
    S.pending = null;
    if (S.queued) { applyParams(S.queued); S.queued = null; }
    S.maxLen = 0; S.minNet = Infinity; S.maxNet = -Infinity; S.maxStore = 0;
    H = new History();
  }

  // ---- One generation, done one change at a time -------------------------------
  // A generation can be spread over several ticks, so pausing works even when a
  // single generation takes a long time. Parameter changes are applied only at
  // the start of a generation, so the history records exactly what was used.
  function startGeneration() {
    if (S.queued) { applyParams(S.queued); S.queued = null; }
    const draw = drawResources(netResources(S.ss.length, S.E, S.cc.length), S.store);
    S.store = draw.store;
    if (draw.collapsed) {
      S.collapsed = true;
      S.running = false;
      S.stepOnly = false;
      return false;
    }
    S.pending = { avail: draw.avail, total: draw.avail >= 1 ? Math.round(draw.avail) : 1, done: 0 };
    S.genStartedAt = now();
    return true;
  }

  // One change to either the cultural system (probability p) or the search space.
  // With probability eta (or lambda) only improvements are kept; otherwise the
  // change is kept regardless. The current effectiveness is cached in S.E.
  function oneChange() {
    const rng = S.rng;
    if (rng() < S.p) {
      const cand = mutate(S.cc, rng);
      const candE = effectiveness(cand, S.ss);
      if (rng() < S.eta) {
        if (candE > S.E) { S.cc = cand; S.E = candE; }
      } else {
        S.cc = cand; S.E = candE;
      }
    } else {
      const cand = mutate(S.ss, rng);
      const candE = effectiveness(S.cc, cand);
      if (rng() < S.lambda) {
        if (candE > S.E) { S.ss = cand; S.E = candE; }
      } else {
        S.ss = cand; S.E = candE;
      }
    }
  }

  function finishGeneration() {
    S.gen += 1;
    const net = netResources(S.ss.length, S.E, S.cc.length);
    H.push({
      lc: S.cc.length, ls: S.ss.length, E: S.E, net: net,
      avail: S.pending.avail, store: S.store,
      eta: S.eta, lambda: S.lambda, p: S.p
    });
    S.maxLen = Math.max(S.maxLen, S.cc.length, S.ss.length);
    S.minNet = Math.min(S.minNet, net);
    S.maxNet = Math.max(S.maxNet, net);
    S.maxStore = Math.max(S.maxStore, S.store);
    S.pending = null;
  }

  // Returns 'gen' when a generation completes, 'change' otherwise, 'collapsed' on collapse.
  function advance() {
    if (!S.pending && !startGeneration()) return 'collapsed';
    oneChange();
    S.pending.done += 1;
    if (S.pending.done >= S.pending.total) {
      finishGeneration();
      return 'gen';
    }
    return 'change';
  }

  function tick() {
    S.timer = null;
    if (!S.running) return;
    const t0 = now();
    let work = 0;
    let waitMs = 0;
    while (true) {
      const r = advance();
      if (r === 'collapsed') break;
      work += S.cc.length * S.ss.length;
      if (r === 'gen') {
        if (S.stepOnly) { S.running = false; S.stepOnly = false; break; }
        if (S.delay > 0) { waitMs = Math.max(0, S.delay - (now() - S.genStartedAt)); break; }
        if (now() - t0 > BUDGET_MS) break;
      } else if (work > WORK_CHECK) {
        work = 0;
        if (now() - t0 > BUDGET_MS) break;
      }
    }
    maybePost(!S.running);
    if (S.running) schedule(waitMs);
  }

  // ---- Messages to the page ----------------------------------------------------
  function snapshot() {
    const n = H.n;
    const d = H.d;
    const idx = sampleIndices(n, CHART_POINTS);
    const k = idx.length;
    const series = {
      gen: new Float64Array(k), E: new Float64Array(k), lc: new Float64Array(k),
      ls: new Float64Array(k), net: new Float64Array(k), store: new Float64Array(k)
    };
    for (let j = 0; j < k; j++) {
      const i = idx[j];
      series.gen[j] = i + 1;
      series.E[j] = d.E[i];
      series.lc[j] = d.lc[i];
      series.ls[j] = d.ls[i];
      series.net[j] = d.net[i];
      series.store[j] = d.store[i];
    }
    // What the next generation will get, if one isn't already in progress.
    let next = null;
    if (!S.pending && !S.collapsed) {
      const dr = drawResources(netResources(S.ss.length, S.E, S.cc.length), S.store);
      next = dr.collapsed
        ? { avail: 0, changes: 0, collapses: true }
        : { avail: dr.avail, changes: dr.avail >= 1 ? Math.round(dr.avail) : 1, collapses: false };
    }
    return {
      type: 'snap',
      seed: S.seed, gen: S.gen, endowment: S.endowment,
      running: S.running, stepping: S.stepOnly, depleted: S.collapsed,
      cc: S.cc.join(''), ss: S.ss.join(''), E: S.E, store: S.store,
      pending: S.pending
        ? { avail: S.pending.avail, total: S.pending.total, done: S.pending.done, elapsed: now() - S.genStartedAt }
        : null,
      next: next,
      n: n, series: series,
      maxLen: S.maxLen, minNet: S.minNet, maxNet: S.maxNet, maxStore: S.maxStore
    };
  }

  function maybePost(force) {
    const t = now();
    if (force || t - S.lastPost >= POST_MS) {
      S.lastPost = t;
      post(snapshot());
    }
  }

  // Column order matches the original CSV; net_resources is added at the end.
  // eta, lambda and p_tradeoff are the values used in each generation.
  function buildCsv() {
    const lines = ['seed,generation,culture_complexity,space_complexity,effectiveness,initial_tech,initial_space,eta,lambda,initial_endowment,p_tradeoff,available_resources,resource_store,net_resources'];
    const d = H.d;
    for (let i = 0; i < H.n; i++) {
      lines.push([
        S.seed, i + 1, d.lc[i], d.ls[i], d.E[i].toFixed(6), S.initC, S.initS,
        d.eta[i].toFixed(2), d.lambda[i].toFixed(2), S.endowment, d.p[i].toFixed(2),
        d.avail[i].toFixed(6), d.store[i].toFixed(6), d.net[i].toFixed(6)
      ].join(','));
    }
    return lines.join('\n');
  }

  // ---- Messages from the page --------------------------------------------------
  function handle(m) {
    switch (m.type) {
      case 'init':
        reset(m);
        maybePost(true);
        break;
      case 'params': {
        const delayChanged = m.delay !== S.delay;
        S.delay = m.delay;
        if (S.pending) S.queued = { eta: m.eta, lambda: m.lambda, p: m.p };
        else applyParams(m);
        if (delayChanged && S.running && !S.pending && S.timer !== null) {
          schedule(Math.max(0, S.delay - (now() - S.genStartedAt)));
        }
        break;
      }
      case 'run':
        if (!S.collapsed && !(S.running && !S.stepOnly)) {
          S.running = true;
          S.stepOnly = false;
          schedule(0);
          maybePost(true);
        }
        break;
      case 'pause':
        S.running = false;
        S.stepOnly = false;
        clearTimer();
        maybePost(true);
        break;
      case 'step':
        if (!S.collapsed && !S.running) {
          S.running = true;
          S.stepOnly = true;
          schedule(0);
        }
        break;
      case 'csv':
        post({ type: 'csv', text: buildCsv(), seed: S.seed, gen: S.gen });
        break;
    }
  }

  return { handle: handle };
}
  </script>

  <!-- =======================================================================
       Interface
       ======================================================================= -->
  <script>
(function () {
  'use strict';

  const { useState, useEffect, useLayoutEffect, useRef, useMemo } = React;
  const html = htm.bind(React.createElement);

  // Style objects are passed through css() so the file never contains two
  // opening braces in a row. Jekyll (GitHub Pages) treats those as template
  // code and would break the page.
  const css = function (o) { return o; };

  // ---- Settings ----------------------------------------------------------------
  // Blue and amber are reserved for the cultural system and the search space.
  // Every other measure has its own colour.
  const COLOURS = {
    cult: '#2563eb',      // cultural system: lines, pixel strip, borders
    cultText: '#1d4ed8',  // cultural system: text and digits
    space: '#d97706',     // search space: lines, pixel strip, borders
    spaceText: '#b45309', // search space: text and digits
    eff: '#7c3aed',       // effectiveness
    avail: '#15803d',     // available resources
    store: '#0f766e',     // resource store
    pos: '#16a34a',       // net resources at or above zero
    neg: '#dc2626',       // net resources below zero
    neutral: '#475569',   // tradeoff and speed sliders
    zeroBit: '#cbd5e1',   // 0 bits in the pixel strip
    grid: '#e2e8f0',
    axis: '#94a3b8'
  };
  const SPEEDS = [1, 2, 5, 10, 20, 50, 100, 200, Infinity]; // generations per second
  const DEFAULT_SPEED = 3;                                 // 10 per second
  const STRIP_THRESHOLD = 200;                             // longer strings are drawn as a pixel strip
  const PAPER_URL = 'https://doi.org/10.1098/rstb.2025.0255';
  const REPO_URL = 'https://github.com/j-winters/bitw0r1d';
  const LICENCE_URL = 'https://github.com/j-winters/bitw0r1d/blob/main/LICENSE';
  const EMAIL = 'james.winters@brunel.ac.uk';

  // ---- Connection to the engine ------------------------------------------------
  // Runs the engine in a Web Worker. Falls back to the main thread if the worker
  // can't be created or doesn't start.
  function startEngine(onMessage) {
    let deliver = null;
    let queue = [];
    function flush() {
      const q = queue;
      queue = [];
      q.forEach(function (m) { deliver(m); });
    }
    function useMainThread() {
      if (deliver) return;
      const eng = createEngine(function (m) { setTimeout(function () { onMessage(m); }, 0); });
      deliver = function (m) { eng.handle(m); };
      flush();
    }
    try {
      const src = document.getElementById('engine-src').textContent +
        '\nvar engine = createEngine(function (m) { self.postMessage(m); });' +
        '\nself.onmessage = function (e) { engine.handle(e.data); };' +
        '\nself.postMessage({ type: "ready" });\n';
      const worker = new Worker(URL.createObjectURL(new Blob([src], { type: 'text/javascript' })));
      const fallbackTimer = setTimeout(function () {
        if (!deliver) { worker.terminate(); useMainThread(); }
      }, 3000);
      worker.onmessage = function (e) {
        if (e.data && e.data.type === 'ready') {
          if (deliver) return;
          clearTimeout(fallbackTimer);
          deliver = function (m) { worker.postMessage(m); };
          flush();
        } else {
          onMessage(e.data);
        }
      };
      worker.onerror = function (e) {
        if (!deliver) {
          if (e.preventDefault) e.preventDefault();
          clearTimeout(fallbackTimer);
          worker.terminate();
          useMainThread();
        }
      };
    } catch (err) {
      useMainThread();
    }
    return {
      send: function (m) { if (deliver) deliver(m); else queue.push(m); }
    };
  }

  // ---- Helpers -------------------------------------------------------------------
  function clampInt(text, lo, hi, fallback) {
    const v = parseInt(text, 10);
    return Number.isFinite(v) ? Math.min(hi, Math.max(lo, v)) : fallback;
  }

  function plural(n, word) {
    return n.toLocaleString() + ' ' + word + (n === 1 ? '' : 's');
  }

  // Axis labels: 12000 -> 12k, 0.25 -> 0.25
  function fmt(v) {
    const a = Math.abs(v);
    if (a >= 1e6) return +(v / 1e6).toFixed(1) + 'M';
    if (a >= 1e4) return +(v / 1e3).toFixed(1) + 'k';
    if (Number.isInteger(v)) return String(v);
    return String(+v.toFixed(2));
  }

  function niceStep(span, count, integer) {
    const raw = span / Math.max(1, count);
    const pow = Math.pow(10, Math.floor(Math.log10(raw)));
    const f = raw / pow;
    const step = (f >= 7.5 ? 10 : f >= 3.5 ? 5 : f >= 1.5 ? 2 : 1) * pow;
    return integer ? Math.max(1, step) : step;
  }

  function niceTicks(lo, hi, count, integer) {
    if (!(hi > lo)) return [lo];
    const step = niceStep(hi - lo, count, integer);
    const first = Math.ceil(lo / step - 1e-9);
    const last = Math.floor(hi / step + 1e-9);
    const out = [];
    for (let i = first; i <= last; i++) out.push(+(i * step).toPrecision(12));
    return out;
  }

  function niceDomain(lo, hi, count, integer) {
    if (!(hi > lo)) hi = lo + 1;
    const step = niceStep(hi - lo, count, integer);
    return [Math.floor(lo / step + 1e-9) * step, Math.ceil(hi / step - 1e-9) * step];
  }

  function logTicks(lo, hi) {
    const out = [];
    const a = Math.floor(Math.log10(lo));
    const b = Math.ceil(Math.log10(hi));
    for (let e = a; e <= b; e++) {
      const v = Math.pow(10, e);
      if (v >= lo * (1 - 1e-9) && v <= hi * (1 + 1e-9)) out.push(v);
    }
    if (out.length >= 3) return out;
    for (let e = a; e <= b; e++) {
      [2, 5].forEach(function (mult) {
        const v = mult * Math.pow(10, e);
        if (v > lo && v < hi) out.push(v);
      });
    }
    return out.sort(function (x, y) { return x - y; });
  }

  function makeScale(log, d0, d1, r0, r1) {
    if (log) {
      const l0 = Math.log10(d0);
      const span = (Math.log10(d1) - l0) || 1;
      return function (v) { return r0 + (Math.log10(Math.max(v, d0 * 1e-6)) - l0) / span * (r1 - r0); };
    }
    const span = (d1 - d0) || 1;
    return function (v) { return r0 + (v - d0) / span * (r1 - r0); };
  }

  function buildPath(xs, ys, sx, sy) {
    let d = '';
    for (let i = 0; i < xs.length; i++) {
      d += (i ? 'L' : 'M') + sx(xs[i]).toFixed(1) + ',' + sy(ys[i]).toFixed(1);
    }
    return d;
  }

  function hexToRgb(hex) {
    const n = parseInt(hex.slice(1), 16);
    return [(n >> 16) & 255, (n >> 8) & 255, n & 255];
  }

  function mixRgb(a, b, f) {
    return 'rgb(' + Math.round(a[0] + (b[0] - a[0]) * f) + ',' +
      Math.round(a[1] + (b[1] - a[1]) * f) + ',' +
      Math.round(a[2] + (b[2] - a[2]) * f) + ')';
  }

  // Pixel strip layout: one column per bit, wrapping onto up to 8 rows.
  // Beyond that, each column summarises several bits.
  function stripLayout(n, width) {
    if (!width || !n) return null;
    const MAX_ROWS = 8;
    const GAP = 3;
    const colW = Math.max(1, Math.min(6, Math.floor(width / n)));
    const perRow = Math.max(1, Math.floor(width / colW));
    let bitsPerCol = 1;
    let rows = Math.ceil(n / perRow);
    if (rows > MAX_ROWS) {
      bitsPerCol = Math.ceil(n / (perRow * MAX_ROWS));
      rows = Math.ceil(Math.ceil(n / bitsPerCol) / perRow);
    }
    const rowH = rows === 1 ? 22 : rows <= 3 ? 14 : 10;
    return { colW: colW, perRow: perRow, bitsPerCol: bitsPerCol, rows: rows, rowH: rowH, gap: GAP, height: rows * rowH + (rows - 1) * GAP };
  }

  function downloadText(text, filename) {
    const blob = new Blob([text], { type: 'text/csv;charset=utf-8' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = filename;
    document.body.appendChild(a);
    a.click();
    a.remove();
    setTimeout(function () { URL.revokeObjectURL(url); }, 1000);
  }

  function useElementWidth() {
    const ref = useRef(null);
    const [width, setWidth] = useState(0);
    useEffect(function () {
      const el = ref.current;
      if (!el) return undefined;
      const update = function () { setWidth(Math.floor(el.clientWidth)); };
      update();
      if (typeof ResizeObserver === 'undefined') {
        window.addEventListener('resize', update);
        return function () { window.removeEventListener('resize', update); };
      }
      const ro = new ResizeObserver(update);
      ro.observe(el);
      return function () { ro.disconnect(); };
    }, []);
    return [ref, width];
  }

  // ---- Icons ---------------------------------------------------------------------
  function Icon(props) {
    return html`<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" aria-hidden="true" focusable="false">${props.children}</svg>`;
  }
  const PlayIcon = () => html`<${Icon}><polygon points="6 4 20 12 6 20 6 4" /><//>`;
  const PauseIcon = () => html`<${Icon}><rect x="6" y="4" width="4" height="16" /><rect x="14" y="4" width="4" height="16" /><//>`;
  const StepIcon = () => html`<${Icon}><polygon points="5 4 15 12 5 20 5 4" /><line x1="19" y1="5" x2="19" y2="19" /><//>`;
  const ResetIcon = () => html`<${Icon}><polyline points="1 4 1 10 7 10" /><path d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10" /><//>`;
  const DownloadIcon = () => html`<${Icon}><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4" /><polyline points="7 10 12 15 17 10" /><line x1="12" y1="15" x2="12" y2="3" /><//>`;

  // ---- Explanations ----------------------------------------------------------------
  // Note: htm drops spaces next to line breaks, so each paragraph is on one line.
  const Info = React.memo(function Info() {
    const cs = html`<strong style=${css({ color: COLOURS.cultText })}>cultural systems</strong>`;
    const ss = html`<strong style=${css({ color: COLOURS.spaceText })}>search spaces</strong>`;
    const eq = html`<span className="eq">R = ℓ<sub>S</sub> × E − ℓ<sub>C</sub> × (1 − E)</span>`;
    return html`<div className="info">
      <details>
        <summary>About this simulation</summary>
        <div className="body">
          <p>In bitw0r1d, we model the co-evolutionary relationship between ${cs} (the repertoire of interacting techniques, artifacts, norms and knowledge available to a society) and ${ss} (the collection of needs, problems and goals faced by societies at a given point in time). For our paper, we were interested in how cultural systems and search spaces co-evolved with one another, especially with respect to the emergence of open-ended dynamics.</p>
        </div>
      </details>
      <details>
        <summary>How it works</summary>
        <div className="body">
          <p>Both cultural systems and search spaces are represented as bitstrings that can change via modifications (flipping a randomly selected bit to its opposite value), expansions (inserting a new randomly chosen bit), and simplifications (where a randomly chosen bit is deleted). Changes can either increase or decrease the match between cultural systems and search spaces (measured as <em>effectiveness</em>).</p>
          <p>Critically, societies need to produce resources in order to fund evolutionary changes to both cultural systems and search spaces. There are two ways to increase resources here: (i) by improving the match (or fit) between cultural systems and search spaces, or (ii) by expanding the search space (which serves as an upper-bound on the maximum obtainable resources).</p>
          <p>Each generation, a society produces net resources ${eq}, where ℓ<sub>S</sub> and ℓ<sub>C</sub> are the lengths of the search space and the cultural system, and E is effectiveness. The rounded value of R is the number of changes attempted in that generation. When R falls below 1, the society draws on its resource store and attempts one change.</p>
          <p>Societies start with an initial resource buffer and will collapse if resources run out. The simulation allows you to investigate whether societies can sustain co-evolution and achieve open-ended dynamics.</p>
        </div>
      </details>
      <details>
        <summary>Key parameters</summary>
        <div className="body">
          <p><em style=${css({ color: COLOURS.cultText })}>η (eta)</em> controls how much selection pressure acts on cultural systems (0 = purely random changes, 1 = only accept improvements that increase the fit with the current search space). <em style=${css({ color: COLOURS.spaceText })}>λ (lambda)</em> does the same for search spaces (0 = purely random changes, 1 = only accept improvements that increase the fit with the existing cultural system). The <em>tradeoff (p)</em> determines how resources are split between changing cultural systems versus search spaces. You can also set the initial complexity of cultural systems and search spaces as well as change the random seed before running.</p>
        </div>
      </details>
      <details>
        <summary>Measures</summary>
        <div className="body">
          <p>Once the simulation is running, you will see both bitstrings for cultural systems and search spaces as they change over time. Bitstrings longer than ${STRIP_THRESHOLD} bits are drawn as a pixel strip, with one column per bit (or per group of bits for very long strings).</p>
          <p>You will also see values for <em style=${css({ color: COLOURS.eff })}>Effectiveness</em> (1 minus the normalised edit distance between the cultural system and the search space, ranging from 0 to 1 where 1 is a perfect match), <em>Complexity</em> (the length of the bitstrings, where longer strings denote more complex cultural systems and search spaces), <em>Net resources</em> (R, as defined under “How it works”), <em style=${css({ color: COLOURS.avail })}>Available resources</em> (the resources a society actually has to work with each generation), and the <em style=${css({ color: COLOURS.store })}>Resource store</em> (set at 100 by default; backup resources that are drawn on when net resources fall below 1).</p>
        </div>
      </details>
      <details>
        <summary>Things to try</summary>
        <div className="body">
          <p>Try setting the initial complexity for cultural systems and search spaces to ℓ=2. Then, play around with different parameter values for η and λ as we did in the paper. This will help you develop an intuitive understanding of the model. Importantly, remember to try out the same parameter values with <em>different</em> random seeds. See if you can find parameter values and seeds that lead to open-ended dynamics...</p>
        </div>
      </details>
      <details>
        <summary>Bonus features</summary>
        <div className="body">
          <p>A unique feature of this online version is that you can adjust selection pressures during a run — try starting with high stochasticity to build initial complexity, then gradually increasing selection strength to see if you can stabilise and sustain growth. This real-time manipulation wasn't possible in the Python model, so please feel free to <a href=${'mailto:' + EMAIL}>send an email</a> if you find anything interesting!</p>
          <p>Parameter changes take effect from the next generation, and the CSV records the values used in each generation. A run replays exactly if you use the same seed and settings and make the same changes at the same generations.</p>
        </div>
      </details>
    </div>`;
  });

  // ---- Controls --------------------------------------------------------------------
  function Slider(props) {
    const helpId = props.help ? props.id + '-help' : undefined;
    return html`<div className="field">
      <label htmlFor=${props.id}><span>${props.label}</span><span className="value">${props.display}</span></label>
      <input id=${props.id} type="range" min=${props.min} max=${props.max} step=${props.step} value=${props.value} disabled=${props.disabled} aria-describedby=${helpId} aria-valuetext=${String(props.display)} style=${css({ accentColor: props.accent })} onChange=${e => props.onChange(Number(e.target.value))} />
      ${props.help ? html`<p className="help" id=${helpId}>${props.help}</p>` : null}
    </div>`;
  }

  // ---- Bitstring display -----------------------------------------------------------
  function Digits(props) {
    const bits = props.bits;
    const runs = [];
    let start = 0;
    for (let i = 1; i <= bits.length; i++) {
      if (i === bits.length || bits[i] !== bits[start]) {
        runs.push(bits.slice(start, i));
        start = i;
      }
    }
    return html`<span className="digits">${runs.map((r, i) => r[0] === '1'
      ? html`<span key=${i} className="one" style=${css({ color: props.color })}>${r}</span>`
      : html`<span key=${i} className="zero">${r}</span>`)}</span>`;
  }

  function PixelStrip(props) {
    const { bits, color, width, layout, label } = props;
    const canvasRef = useRef(null);
    const ones = useMemo(function () {
      let c = 0;
      for (let i = 0; i < bits.length; i++) if (bits.charCodeAt(i) === 49) c++;
      return c;
    }, [bits]);
    // Layout effect so the canvas is sized and drawn before the browser paints it.
    useLayoutEffect(function () {
      const cv = canvasRef.current;
      if (!cv || !width || !layout) return;
      const dpr = window.devicePixelRatio || 1;
      cv.width = Math.round(width * dpr);
      cv.height = Math.round(layout.height * dpr);
      cv.style.width = width + 'px';
      cv.style.height = layout.height + 'px';
      const ctx = cv.getContext('2d');
      ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
      ctx.clearRect(0, 0, width, layout.height);
      const one = hexToRgb(color);
      const zero = hexToRgb(COLOURS.zeroBit);
      const oneStyle = mixRgb(zero, one, 1);
      const zeroStyle = mixRgb(zero, one, 0);
      const n = bits.length;
      const per = layout.bitsPerCol;
      for (let col = 0, s = 0; s < n; col++, s += per) {
        const e = Math.min(n, s + per);
        let count = 0;
        for (let i = s; i < e; i++) if (bits.charCodeAt(i) === 49) count++;
        ctx.fillStyle = per === 1 ? (count ? oneStyle : zeroStyle) : mixRgb(zero, one, count / (e - s));
        ctx.fillRect((col % layout.perRow) * layout.colW, Math.floor(col / layout.perRow) * (layout.rowH + layout.gap), layout.colW, layout.rowH);
      }
    }, [bits, width, layout, color]);
    return html`<canvas ref=${canvasRef} role="img" aria-label=${label + ': ' + bits.length.toLocaleString() + ' bits, ' + ones.toLocaleString() + ' of them 1'}></canvas>`;
  }

  function BitDisplay(props) {
    const { label, bits, color, textColor } = props;
    const [boxRef, width] = useElementWidth();
    const n = bits.length;
    const long = n > STRIP_THRESHOLD;
    const layout = useMemo(function () { return long ? stripLayout(n, width) : null; }, [long, n, width]);
    let note = null;
    if (long && layout) {
      note = layout.bitsPerCol === 1
        ? 'Each column is one bit: coloured = 1, grey = 0'
        : 'Each column summarises ' + layout.bitsPerCol + ' bits: darker = more 1s';
    }
    return html`<div className="bits-block" style=${css({ borderLeftColor: color })}>
      <div className="bits-label"><span>${label} (ℓ = ${n.toLocaleString()})</span>${note ? html`<span className="bits-note">${note}</span>` : null}</div>
      <div className="bitbox">
        <div ref=${boxRef}>
          ${long
            ? html`<${PixelStrip} bits=${bits} color=${color} width=${width} layout=${layout} label=${label} />`
            : html`<${Digits} bits=${bits} color=${textColor} />`}
        </div>
      </div>
    </div>`;
  }

  // ---- Current state ---------------------------------------------------------------
  function Stat(props) {
    return html`<div className="stat" style=${css({ borderTopColor: props.color })}>
      <div className="k">${props.label}</div>
      <div className="v" style=${css({ color: props.color })}>${props.value}</div>
      <div className="s">${props.sub}</div>
    </div>`;
  }

  function StatePanel(props) {
    const snap = props.snap;
    const nextGen = (snap.gen + 1).toLocaleString();
    let availValue = '–';
    let availSub = '';
    if (snap.depleted) {
      availValue = '0.00';
      availSub = 'The society has collapsed';
    } else if (snap.pending) {
      availValue = snap.pending.avail.toFixed(2);
      availSub = plural(snap.pending.total, 'change') + ' in generation ' + nextGen;
    } else if (snap.next && snap.next.collapses) {
      availValue = '0.00';
      availSub = 'The store cannot cover generation ' + nextGen;
    } else if (snap.next) {
      availValue = snap.next.avail.toFixed(2);
      availSub = plural(snap.next.changes, 'change') + ' in generation ' + nextGen;
    }
    const slow = snap.pending && snap.pending.elapsed > 500;
    const progress = slow
      ? html`<span className="progress"> · generation ${nextGen} is ${Math.floor(100 * snap.pending.done / snap.pending.total)}% done</span>`
      : null;
    const collapseText = snap.gen === 0 ? 'before the first generation' : 'after generation ' + snap.gen.toLocaleString();
    return html`<section className="panel" aria-label="Current state">
      <h2 className="gen-title">Generation ${snap.gen.toLocaleString()}${progress}</h2>
      <${BitDisplay} label="Cultural system" bits=${snap.cc} color=${COLOURS.cult} textColor=${COLOURS.cultText} />
      <${BitDisplay} label="Search space" bits=${snap.ss} color=${COLOURS.space} textColor=${COLOURS.spaceText} />
      <div className="stats">
        <${Stat} label="Effectiveness (E)" value=${snap.E.toFixed(3)} sub="1 means a perfect match" color=${COLOURS.eff} />
        <${Stat} label="Available resources" value=${availValue} sub=${availSub} color=${COLOURS.avail} />
        <${Stat} label="Resource store" value=${snap.store.toFixed(2)} sub="Drawn on when net resources fall below 1" color=${COLOURS.store} />
      </div>
      ${snap.depleted ? html`<div className="banner" role="alert"><strong>Resources depleted.</strong> The society collapsed ${collapseText}. Reset to start again.</div>` : null}
    </section>`;
  }

  // ---- Charts ------------------------------------------------------------------------
  function LineChart(props) {
    const { title, legend, xDomain, xLog, yDomain, yLog, yInteger, lines, signSplit, empty } = props;
    const [boxRef, width] = useElementWidth();
    const uid = 'c' + React.useId().replace(/[^a-zA-Z0-9]/g, '');
    const height = 210;
    const m = { l: 46, r: 16, t: 10, b: 36 };
    const iw = Math.max(20, width - m.l - m.r);
    const ih = height - m.t - m.b;
    let svg = null;
    if (width > 0) {
      const sx = makeScale(xLog, xDomain[0], xDomain[1], 0, iw);
      const sy = makeScale(yLog, yDomain[0], yDomain[1], ih, 0);
      const xTicks = xLog ? logTicks(xDomain[0], xDomain[1]) : niceTicks(xDomain[0], xDomain[1], Math.max(2, Math.round(iw / 90)), true);
      const yTicks = yLog ? logTicks(yDomain[0], yDomain[1]) : niceTicks(yDomain[0], yDomain[1], 4, yInteger);
      const zeroY = Math.min(ih, Math.max(0, sy(0)));
      const marks = empty ? [] : lines.map(function (l, i) {
        if (l.xs.length === 1) {
          const fill = signSplit ? (l.ys[0] >= 0 ? COLOURS.pos : COLOURS.neg) : l.color;
          return html`<circle key=${i} cx=${sx(l.xs[0])} cy=${sy(l.ys[0])} r="3" fill=${fill} />`;
        }
        const d = buildPath(l.xs, l.ys, sx, sy);
        if (signSplit) {
          return html`<g key=${i}>
            <path d=${d} fill="none" stroke=${COLOURS.pos} strokeWidth="1.75" strokeLinejoin="round" clipPath=${'url(#' + uid + 'u)'} />
            <path d=${d} fill="none" stroke=${COLOURS.neg} strokeWidth="1.75" strokeLinejoin="round" clipPath=${'url(#' + uid + 'd)'} />
          </g>`;
        }
        return html`<path key=${i} d=${d} fill="none" stroke=${l.color} strokeWidth="1.75" strokeLinejoin="round" />`;
      });
      svg = html`<svg width=${width} height=${height} role="img" aria-label=${title + ' over generations'}>
        <defs>
          <clipPath id=${uid + 'p'}><rect x="0" y="-2" width=${iw} height=${ih + 4} /></clipPath>
          <clipPath id=${uid + 'u'}><rect x="0" y="-2" width=${iw} height=${zeroY + 2} /></clipPath>
          <clipPath id=${uid + 'd'}><rect x="0" y=${zeroY} width=${iw} height=${ih - zeroY + 2} /></clipPath>
        </defs>
        <g transform=${'translate(' + m.l + ',' + m.t + ')'}>
          ${yTicks.map(v => html`<g key=${'y' + v}><line x1="0" x2=${iw} y1=${sy(v)} y2=${sy(v)} stroke=${COLOURS.grid} /><text x="-8" y=${sy(v)} textAnchor="end" dominantBaseline="middle">${fmt(v)}</text></g>`)}
          ${xTicks.map(v => html`<text key=${'x' + v} x=${sx(v)} y=${ih + 16} textAnchor="middle">${fmt(v)}</text>`)}
          <line x1="0" x2=${iw} y1=${ih} y2=${ih} stroke=${COLOURS.axis} />
          ${signSplit && !empty ? html`<line x1="0" x2=${iw} y1=${zeroY} y2=${zeroY} stroke=${COLOURS.axis} strokeDasharray="4 3" />` : null}
          <g clipPath=${'url(#' + uid + 'p)'}>${marks}</g>
          <text x=${iw} y=${ih + 31} textAnchor="end">Generation</text>
          ${empty ? html`<text x=${iw / 2} y=${ih / 2} textAnchor="middle" dominantBaseline="middle">No data yet</text>` : null}
        </g>
      </svg>`;
    }
    return html`<div className="chart">
      <h3><span>${title}</span>${legend || null}</h3>
      <div className="chart-box" ref=${boxRef}>${svg}</div>
    </div>`;
  }

  function ChartOptions(props) {
    const { opts, setOpts } = props;
    const set = function (key, value) {
      setOpts(function (o) { const next = Object.assign({}, o); next[key] = value; return next; });
    };
    return html`<div className="chart-options">
      <div className="opt">
        <label htmlFor="xscale" className="opt-title">Generation axis</label>
        <select id="xscale" value=${opts.xLog ? 'log' : 'linear'} onChange=${e => set('xLog', e.target.value === 'log')}>
          <option value="linear">Linear</option>
          <option value="log">Log</option>
        </select>
        <label className="check"><input type="checkbox" checked=${opts.fixX} onChange=${e => set('fixX', e.target.checked)} />Fix maximum at</label>
        <input type="number" min="2" step="100" aria-label="Generation axis maximum" value=${opts.xMaxText} disabled=${!opts.fixX} onChange=${e => set('xMaxText', e.target.value)} />
      </div>
      <div className="opt">
        <label htmlFor="yscale" className="opt-title">Complexity axis</label>
        <select id="yscale" value=${opts.yLog ? 'log' : 'linear'} onChange=${e => set('yLog', e.target.value === 'log')}>
          <option value="linear">Linear</option>
          <option value="log">Log</option>
        </select>
        <label className="check"><input type="checkbox" checked=${opts.fixY} onChange=${e => set('fixY', e.target.checked)} />Fix maximum at</label>
        <input type="number" min="2" step="100" aria-label="Complexity axis maximum" value=${opts.yMaxText} disabled=${!opts.fixY} onChange=${e => set('yMaxText', e.target.value)} />
      </div>
    </div>`;
  }

  function ChartsPanel(props) {
    const { snap, opts, setOpts } = props;
    const s = snap.series;
    const empty = snap.n === 0;
    const xMaxFixed = clampInt(opts.xMaxText, 2, 1e9, 10000);
    const yMaxFixed = clampInt(opts.yMaxText, 2, 1e9, 10000);
    const xDomain = [1, opts.fixX ? xMaxFixed : Math.max(2, snap.gen)];

    const lenMax = empty ? 2 : Math.max(2, snap.maxLen);
    let cDomain;
    if (opts.fixY) cDomain = [opts.yLog ? 1 : 0, yMaxFixed];
    else if (opts.yLog) cDomain = [1, Math.pow(10, Math.ceil(Math.log10(lenMax)))];
    else cDomain = niceDomain(0, lenMax, 4, true);

    const netLo = empty ? -1 : Math.min(0, snap.minNet);
    const netHi = empty ? 1 : Math.max(0, snap.maxNet);
    const netDomain = niceDomain(netLo, netHi > netLo ? netHi : netLo + 1, 4, false);
    const storeDomain = niceDomain(0, Math.max(1, snap.endowment, empty ? 0 : snap.maxStore), 4, false);

    const common = { xDomain: xDomain, xLog: opts.xLog, empty: empty };
    const complexityLegend = html`<span className="legend"><span><i style=${css({ background: COLOURS.cult })}></i>Cultural system</span><span><i style=${css({ background: COLOURS.space })}></i>Search space</span></span>`;
    const netLegend = html`<span className="legend"><span><i style=${css({ background: COLOURS.pos })}></i>Zero or above</span><span><i style=${css({ background: COLOURS.neg })}></i>Below zero</span></span>`;

    return html`<section className="panel" aria-label="Evolution history">
      <h2>Evolution history</h2>
      <${ChartOptions} opts=${opts} setOpts=${setOpts} />
      <div className="charts">
        <${LineChart} ...${common} title="Effectiveness" yDomain=${[0, 1]} yLog=${false} lines=${[{ xs: s.gen, ys: s.E, color: COLOURS.eff }]} />
        <${LineChart} ...${common} title="Complexity" legend=${complexityLegend} yDomain=${cDomain} yLog=${opts.yLog} yInteger=${true} lines=${[{ xs: s.gen, ys: s.ls, color: COLOURS.space }, { xs: s.gen, ys: s.lc, color: COLOURS.cult }]} />
        <${LineChart} ...${common} title="Net resources" legend=${netLegend} yDomain=${netDomain} yLog=${false} signSplit=${true} lines=${[{ xs: s.gen, ys: s.net }]} />
        <${LineChart} ...${common} title="Resource store" yDomain=${storeDomain} yLog=${false} lines=${[{ xs: s.gen, ys: s.store, color: COLOURS.store }]} />
      </div>
    </section>`;
  }

  // ---- Page ------------------------------------------------------------------------------
  function App() {
    const [seedText, setSeedText] = useState('12345');
    const [lc0, setLc0] = useState(8);
    const [ls0, setLs0] = useState(8);
    const [endowment, setEndowment] = useState(100);
    const [eta, setEta] = useState(0.99);
    const [lambda, setLambda] = useState(0.05);
    const [p, setP] = useState(0.5);
    const [speedIdx, setSpeedIdx] = useState(DEFAULT_SPEED);
    const [opts, setOpts] = useState({ xLog: false, fixX: false, xMaxText: '10000', yLog: false, fixY: false, yMaxText: '10000' });
    const [snap, setSnap] = useState(null);
    const engine = useRef(null);
    const seed = clampInt(seedText, 0, 2147483647, 0);

    // Effects run in this order on first load: start engine, send parameters, initialise.
    useEffect(function () {
      engine.current = startEngine(function (msg) {
        if (msg.type === 'snap') setSnap(msg);
        else if (msg.type === 'csv') downloadText(msg.text, 'bitw0r1d_seed' + msg.seed + '_gen' + msg.gen + '.csv');
      });
    }, []);

    useEffect(function () {
      const v = SPEEDS[speedIdx];
      engine.current.send({ type: 'params', eta: eta, lambda: lambda, p: p, delay: v === Infinity ? 0 : 1000 / v });
    }, [eta, lambda, p, speedIdx]);

    // Setup controls are disabled once a run has started, so this only fires before a run.
    useEffect(function () {
      engine.current.send({ type: 'init', seed: seed, lc0: lc0, ls0: ls0, endowment: endowment });
    }, [seed, lc0, ls0, endowment]);

    const send = function (msg) { if (engine.current) engine.current.send(msg); };
    const running = !!(snap && snap.running && !snap.stepping);
    const busy = !!(snap && snap.running);
    const depleted = !!(snap && snap.depleted);
    const started = !!(snap && (snap.gen > 0 || snap.running || snap.pending));
    const gen = snap ? snap.gen : 0;
    const speed = SPEEDS[speedIdx];

    return html`<div className="page">
      <header>
        <h1>bitw<span style=${css({ color: COLOURS.cult })}>0</span>r<span style=${css({ color: COLOURS.space })}>1</span>d</h1>
        <p className="subtitle">A co-evolutionary model of cultural systems and search spaces</p>
        <p className="intro">An online version of the bitw0r1d model from <a href=${PAPER_URL}>Winters & Charbonneau (2026)</a>. It simulates how cultural systems and search spaces co-evolve with each other over time. The dynamics of the model are resource-constrained, i.e., every change costs resources and the dynamics collapse if resources run out. Press Run to start, or open the sections below for more details.</p>
      </header>

      <${Info} />

      <section className="panel" aria-label="Controls">
        <div className="buttons">
          <button className="primary" disabled=${!snap || depleted} onClick=${() => send({ type: running ? 'pause' : 'run' })}>${running ? html`<${PauseIcon} />` : html`<${PlayIcon} />`}${running ? 'Pause' : 'Run'}</button>
          <button disabled=${!snap || busy || depleted} onClick=${() => send({ type: 'step' })}><${StepIcon} />Step</button>
          <button disabled=${!started && !depleted} onClick=${() => send({ type: 'init', seed: seed, lc0: lc0, ls0: ls0, endowment: endowment })}><${ResetIcon} />Reset</button>
          <button disabled=${gen === 0} onClick=${() => send({ type: 'csv' })}><${DownloadIcon} />Download CSV</button>
        </div>

        <div className="columns">
          <div>
            <h2>Initial setup</h2>
            <p className="note">${started ? 'Reset the simulation to change these.' : 'Changes here apply straight away, before the run starts.'}</p>
            <div className="field">
              <label htmlFor="seed"><span>Random seed</span></label>
              <div className="seed-row">
                <input id="seed" type="number" inputMode="numeric" min="0" max="2147483647" step="1" value=${seedText} disabled=${started} aria-describedby="seed-help" onChange=${e => setSeedText(e.target.value)} />
                <button type="button" disabled=${started} onClick=${() => setSeedText(String(Math.floor(Math.random() * 1000000)))}>New seed</button>
              </div>
              <p className="help" id="seed-help">The same seed and settings give the same run.</p>
            </div>
            <${Slider} id="lc0" label="Initial cultural system complexity (ℓ)" value=${lc0} display=${lc0} min="1" max="20" step="1" disabled=${started} accent=${COLOURS.cult} onChange=${setLc0} />
            <${Slider} id="ls0" label="Initial search space complexity (ℓ)" value=${ls0} display=${ls0} min="1" max="20" step="1" disabled=${started} accent=${COLOURS.space} onChange=${setLs0} />
            <${Slider} id="endow" label="Initial endowment" value=${endowment} display=${endowment} min="10" max="500" step="10" disabled=${started} accent=${COLOURS.store} onChange=${setEndowment} help="Starting size of the resource store" />
          </div>

          <div>
            <h2>Parameters</h2>
            <p className="note">These can be changed during a run and take effect from the next generation.</p>
            <${Slider} id="eta" label="η (eta): cultural system selection" value=${eta} display=${eta.toFixed(2)} min="0" max="1" step="0.01" accent=${COLOURS.cult} onChange=${setEta} help="0 = random drift, 1 = strong selection on effectiveness" />
            <${Slider} id="lambda" label="λ (lambda): search space selection" value=${lambda} display=${lambda.toFixed(2)} min="0" max="1" step="0.01" accent=${COLOURS.space} onChange=${setLambda} help="0 = random drift, 1 = strong selection on effectiveness" />
            <${Slider} id="p" label="p (tradeoff)" value=${p} display=${p.toFixed(2)} min="0" max="1" step="0.01" accent=${COLOURS.neutral} onChange=${setP} help="Probability of allocating resources to cultural systems (vs search spaces)" />
            <${Slider} id="speed" label="Speed" value=${speedIdx} display=${speed === Infinity ? 'Maximum' : speed + ' generations/s'} min="0" max=${SPEEDS.length - 1} step="1" accent=${COLOURS.neutral} onChange=${setSpeedIdx} help="Long strings take longer to compute, so runs slow down as complexity grows." />
          </div>
        </div>
      </section>

      ${snap ? html`<${StatePanel} snap=${snap} />` : null}
      ${snap ? html`<${ChartsPanel} snap=${snap} opts=${opts} setOpts=${setOpts} />` : null}

      <footer className="footer">
        <div>
          <h2>How to cite</h2>
          <p>Winters, J., & Charbonneau, M. (2026). Modelling the emergence of open-ended technological evolution. <a href=${PAPER_URL}>10.1098/rstb.2025.0255</a>.</p>
        </div>
        <div>
          <h2>Source code</h2>
          <p>The Python model, the data and the R analysis code are available on <a href=${REPO_URL}>GitHub</a>.</p>
        </div>
        <div>
          <h2>Licence</h2>
          <p>The bitw0r1d code is released under the <a href=${LICENCE_URL}>GNU General Public License v3.0</a>.</p>
        </div>
      </footer>
    </div>`;
  }

  ReactDOM.createRoot(document.getElementById('root')).render(html`<${App} />`);
})();
  </script>
</body>
