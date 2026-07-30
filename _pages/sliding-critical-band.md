---
layout: splash
permalink: /projects/sliding-critical-band/
title: "Sliding Critical Band in RoPE-based Length Extrapolation"
excerpt: "Why RoPE-based LLMs break past their training length — and what actually predicts it."
---

<style>
#scb-page{
  --scb-ink: #16202b;
  --scb-sub: #5b6773;
  --scb-bg: #fbfbf9;
  --scb-card: #ffffff;
  --scb-line: #e6e6e2;
  --scb-accent: #2f7f93;      /* site info color */
  --scb-accent-dark: #1f5a68;
  --scb-good: #62c462;        /* site success color */
  --scb-harmful: #ee5f5b;     /* site danger color */
  --scb-critical: #f89406;    /* site warning color */
  --scb-benign: #7a8288;      /* site base gray */
  --scb-navy: #0d1321;
  --scb-navy2: #17233a;
  font-family: -apple-system, "San Francisco", "Roboto", "Segoe UI", "Helvetica Neue", Arial, sans-serif;
  color: var(--scb-ink);
  background: var(--scb-bg);
  margin: -1.5px 0 0 0; /* neutralize .page__content top spacing so hero can bleed */
}
:root[data-theme="dark"] #scb-page{
  --scb-ink: #eef1f4;
  --scb-sub: #aab2bb;
  --scb-bg: #10151c;
  --scb-card: #171d26;
  --scb-line: #2a323d;
}
#scb-page *{ box-sizing: border-box; }
#scb-page .scb-wrap{ max-width: 980px; margin: 0 auto; padding: 0 24px; }
#scb-page section{ position: relative; padding: 90px 0; }
#scb-page h2.scb-h2{ font-size: clamp(1.6rem, 3vw, 2.2rem); margin: 0 0 14px; letter-spacing: -0.01em; }
#scb-page h3.scb-h3{ font-size: 1.15rem; margin: 28px 0 10px; color: var(--scb-ink); }
#scb-page p.scb-p{ font-size: 1.05rem; line-height: 1.7; color: var(--scb-sub); margin: 0 0 16px; max-width: 68ch; }
#scb-page .scb-eyebrow{ text-transform: uppercase; letter-spacing: 0.12em; font-size: 0.72rem; font-weight: 700; color: var(--scb-accent); margin-bottom: 10px; }
#scb-page .scb-caption{ font-family: Georgia, "Times New Roman", Times, serif; font-size: 0.85rem; color: var(--scb-sub); line-height: 1.5; margin-top: 10px; }
#scb-page code{ background: rgba(47,127,147,0.1); padding: 1px 6px; border-radius: 4px; font-size: 0.92em; }

/* ---------- reveal-on-scroll ---------- */
#scb-page .reveal{ opacity: 0; transform: translateY(24px); transition: opacity .7s ease, transform .7s ease; }
#scb-page .reveal.is-visible{ opacity: 1; transform: translateY(0); }
@media (prefers-reduced-motion: reduce){
  #scb-page .reveal{ transition: opacity .01s linear; transform: none; }
}

/* ---------- hero ---------- */
#scb-page .scb-hero{
  min-height: 92vh; display: flex; flex-direction: column; align-items: center; justify-content: center;
  text-align: center; background: radial-gradient(1200px 600px at 50% 20%, var(--scb-navy2), var(--scb-navy));
  color: #eef2f6; padding: 60px 24px; overflow: hidden;
}
#scb-page .scb-hero canvas{ position: absolute; inset: 0; width: 100%; height: 100%; opacity: 0.55; }
#scb-page .scb-hero .scb-hero-inner{ position: relative; z-index: 2; max-width: 780px; }
#scb-page .scb-badge{
  display: inline-block; font-size: 0.78rem; letter-spacing: 0.06em; font-weight: 700; text-transform: uppercase;
  color: #bfe3ea; background: rgba(47,127,147,0.22); border: 1px solid rgba(191,227,234,0.35);
  padding: 6px 14px; border-radius: 999px; margin-bottom: 22px;
}
#scb-page .scb-hero h1{ font-size: clamp(2rem, 5vw, 3.2rem); line-height: 1.15; margin: 0 0 16px; letter-spacing: -0.02em; }
#scb-page .scb-hero .scb-hook{ font-size: clamp(1.02rem, 2vw, 1.25rem); color: #cbd6e0; line-height: 1.6; margin: 0 auto 30px; max-width: 62ch; }
#scb-page .scb-authors{ color: #93a6b5; font-size: 0.95rem; margin-bottom: 34px; }
#scb-page .scb-cta-row{ display: flex; gap: 14px; justify-content: center; flex-wrap: wrap; }
#scb-page .scb-btn{
  display: inline-flex; align-items: center; gap: 8px; padding: 12px 22px; border-radius: 10px; font-weight: 600;
  font-size: 0.95rem; text-decoration: none; transition: transform .15s ease, box-shadow .15s ease; cursor: pointer;
  border: 1px solid transparent;
}
#scb-page .scb-btn:hover{ transform: translateY(-2px); }
#scb-page .scb-btn.primary{ background: var(--scb-accent); color: #fff; box-shadow: 0 8px 24px rgba(47,127,147,0.35); }
#scb-page .scb-btn.ghost{ background: rgba(255,255,255,0.06); color: #eef2f6; border-color: rgba(255,255,255,0.25); }
#scb-page .scb-scroll-cue{ position: absolute; bottom: 26px; left: 50%; transform: translateX(-50%); color: #7f93a3; font-size: 0.78rem; letter-spacing: 0.08em; text-transform: uppercase; z-index: 2; }
#scb-page .scb-scroll-cue::after{ content: ""; display: block; width: 1px; height: 26px; background: linear-gradient(#7f93a3, transparent); margin: 8px auto 0; animation: scb-cue 1.8s ease-in-out infinite; }
@keyframes scb-cue{ 0%{ opacity: 0; transform: translateY(-6px);} 50%{ opacity: 1; } 100%{ opacity: 0; transform: translateY(10px);} }

/* ---------- RoPE 101: slider-driven dial row ---------- */
#scb-page .scb-dial-row{ display: flex; gap: 8px; justify-content: space-between; flex-wrap: nowrap; padding: 20px 0 4px; }
#scb-page .scb-dial-col{ display: flex; flex-direction: column; align-items: center; flex: 1 1 0; min-width: 0; }
#scb-page .scb-dial{ width: 50px; height: 50px; border-radius: 50%; border: 2px solid var(--scb-line); position: relative; background: var(--scb-card); }
#scb-page .scb-dial .needle{ position: absolute; left: 50%; top: 50%; width: 2px; height: 21px; background: var(--scb-accent); transform-origin: 50% 100%; margin-left: -1px; margin-top: -21px; border-radius: 2px; }
#scb-page .scb-dial .dot{ position: absolute; left: 50%; top: 50%; width: 5px; height: 5px; margin-left: -2.5px; margin-top: -2.5px; border-radius: 50%; background: var(--scb-accent); }
#scb-page .scb-dial-label{ text-align: center; font-size: 0.64rem; color: var(--scb-sub); margin-top: 8px; line-height: 1.4; font-variant-numeric: tabular-nums; }
#scb-page .scb-dial-label b{ color: var(--scb-ink); display: block; }
#scb-page .scb-m-readout{ font-size: 0.85rem; color: var(--scb-ink); font-weight: 600; margin-bottom: 8px; }
#scb-page .scb-m-readout span{ color: var(--scb-accent); font-variant-numeric: tabular-nums; }
@media (max-width: 480px){
  #scb-page .scb-dial-row{ gap: 4px; }
  #scb-page .scb-dial{ width: 36px; height: 36px; }
  #scb-page .scb-dial .needle{ height: 15px; margin-top: -15px; }
  #scb-page .scb-dial-label{ font-size: 0.56rem; }
}

/* ---------- relative-position demo ---------- */
#scb-page .scb-rel-demo{ display: flex; gap: 40px; align-items: center; flex-wrap: wrap; margin-top: 18px; }
#scb-page .scb-rel-demo canvas{ flex: 0 0 auto; background: var(--scb-bg); border-radius: 12px; border: 1px solid var(--scb-line); max-width: 100%; }
#scb-page .scb-rel-controls{ flex: 1 1 320px; min-width: 260px; max-width: 460px; }
#scb-page .scb-rel-controls label{ display: block; font-size: 0.85rem; color: var(--scb-sub); margin-top: 16px; font-weight: 600; }
#scb-page .scb-rel-controls label span{ color: var(--scb-ink); font-variant-numeric: tabular-nums; }

/* ---------- two/one col layouts ---------- */
#scb-page .scb-grid2{ display: grid; grid-template-columns: 1fr 1fr; gap: 46px; align-items: center; }
@media (max-width: 800px){ #scb-page .scb-grid2{ grid-template-columns: 1fr; } }
#scb-page.alt{ background: var(--scb-card); }
#scb-page section.scb-alt{ background: var(--scb-card); border-top: 1px solid var(--scb-line); border-bottom: 1px solid var(--scb-line); }

/* ---------- button groups (ratio / toggle controls) ---------- */
#scb-page .scb-controls{ display: flex; flex-wrap: wrap; gap: 8px; margin: 18px 0 22px; }
#scb-page .scb-chip{
  border: 1.5px solid var(--scb-line); background: var(--scb-card); color: var(--scb-sub); font-weight: 600;
  font-size: 0.85rem; padding: 7px 14px; border-radius: 999px; cursor: pointer; transition: all .15s ease;
}
#scb-page .scb-chip:hover{ border-color: var(--scb-accent); color: var(--scb-accent); }
#scb-page .scb-chip.active{ background: var(--scb-accent); border-color: var(--scb-accent); color: #fff; }
#scb-page .scb-controls-label{ font-size: 0.72rem; text-transform: uppercase; letter-spacing: 0.08em; color: var(--scb-sub); font-weight: 700; align-self: center; margin-right: 4px; }

/* ---------- spectrum widget (sliding band) ---------- */
#scb-page .scb-spectrum-card{ background: var(--scb-card); border: 1px solid var(--scb-line); border-radius: 16px; padding: 28px 26px 22px; box-shadow: 0 10px 30px rgba(16,32,45,0.06); }
#scb-page .scb-spectrum-axis{ position: relative; height: 54px; border-radius: 10px; background: repeating-linear-gradient(90deg, #f1f2ef, #f1f2ef 1px, transparent 1px, transparent calc(100%/24)); overflow: hidden; }
:root[data-theme="dark"] #scb-page .scb-spectrum-axis{ background: repeating-linear-gradient(90deg, #232b36, #232b36 1px, transparent 1px, transparent calc(100%/24)); }
#scb-page .scb-zone{ position: absolute; top: 0; bottom: 0; transition: left 0.7s cubic-bezier(.65,0,.35,1), width 0.7s cubic-bezier(.65,0,.35,1); }
#scb-page .scb-zone.harmful{ background: rgba(238,95,91,0.22); left: 0; }
#scb-page .scb-zone.critical{ background: rgba(248,148,6,0.32); border-left: 2px solid var(--scb-critical); border-right: 2px solid var(--scb-critical); }
#scb-page .scb-zone.benign{ background: rgba(122,130,136,0.14); }
#scb-page .scb-spectrum-ticks{ display: flex; justify-content: space-between; font-size: 0.7rem; color: var(--scb-sub); margin-top: 6px; }
#scb-page .scb-spectrum-legend{ display: flex; gap: 18px; flex-wrap: wrap; margin-top: 18px; font-size: 0.82rem; color: var(--scb-sub); }
#scb-page .scb-spectrum-legend span{ display: inline-flex; align-items: center; gap: 7px; }
#scb-page .scb-swatch{ width: 12px; height: 12px; border-radius: 3px; display: inline-block; }
#scb-page .scb-readout{ display: flex; gap: 28px; flex-wrap: wrap; margin-top: 20px; }
#scb-page .scb-readout .num{ font-size: 1.6rem; font-weight: 700; color: var(--scb-ink); font-variant-numeric: tabular-nums; }
#scb-page .scb-readout .lbl{ font-size: 0.72rem; text-transform: uppercase; letter-spacing: 0.06em; color: var(--scb-sub); }

/* pin section */
#scb-page .scb-pin-stage{ min-height: 100vh; display: flex; flex-direction: column; justify-content: center; }

/* ---------- stat tiles ---------- */
#scb-page .scb-tiles{ display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; margin-top: 22px; }
@media (max-width: 700px){ #scb-page .scb-tiles{ grid-template-columns: 1fr; } }
#scb-page .scb-tile{ background: var(--scb-card); border: 1px solid var(--scb-line); border-radius: 14px; padding: 18px 20px; }
#scb-page .scb-tile .tile-lbl{ font-size: 0.72rem; text-transform: uppercase; letter-spacing: 0.06em; color: var(--scb-sub); font-weight: 700; }
#scb-page .scb-tile .tile-val{ font-size: 1.5rem; font-weight: 700; margin-top: 6px; font-variant-numeric: tabular-nums; }
#scb-page .scb-tile .tile-val.good{ color: var(--scb-good); }
#scb-page .scb-tile .tile-val.bad{ color: var(--scb-harmful); }

/* ---------- figure 5 compare ---------- */
#scb-page .scb-compare{ position: relative; border-radius: 14px; overflow: hidden; border: 1px solid var(--scb-line); background: #fff; }
#scb-page .scb-compare img{ display: block; width: 100%; height: auto; }
#scb-page .scb-compare .after{ position: absolute; inset: 0; clip-path: inset(0 50% 0 0); transition: clip-path .06s linear; }
#scb-page .scb-compare-slider{ width: 100%; margin-top: 14px; accent-color: var(--scb-accent); }
#scb-page .scb-compare-tag{ position: absolute; top: 10px; font-size: 0.7rem; font-weight: 700; text-transform: uppercase; letter-spacing: 0.05em; padding: 4px 10px; border-radius: 999px; z-index: 3; }
#scb-page .scb-compare-tag.left{ left: 10px; background: rgba(238,95,91,0.9); color: #fff; }
#scb-page .scb-compare-tag.right{ right: 10px; background: rgba(98,196,98,0.9); color: #fff; }

/* ---------- table4 mini table ---------- */
#scb-page table.scb-table{ width: 100%; border-collapse: collapse; font-size: 0.88rem; margin-top: 10px; }
#scb-page table.scb-table th, #scb-page table.scb-table td{ padding: 8px 10px; border-bottom: 1px solid var(--scb-line); text-align: right; }
#scb-page table.scb-table th:first-child, #scb-page table.scb-table td:first-child{ text-align: left; }
#scb-page table.scb-table th{ font-size: 0.72rem; text-transform: uppercase; letter-spacing: 0.04em; color: var(--scb-sub); }
#scb-page table.scb-table td.best{ color: var(--scb-good); font-weight: 700; }
#scb-page details.scb-details summary{ cursor: pointer; font-weight: 600; color: var(--scb-accent); margin-top: 16px; }

/* ---------- footer / CTA ---------- */
#scb-page .scb-final{ background: var(--scb-navy); color: #dfe6ec; text-align: center; }
#scb-page .scb-final h2.scb-h2{ color: #fff; }
#scb-page .scb-final p.scb-p{ color: #aebccb; margin: 0 auto 28px; }
#scb-page .scb-bibtex{ text-align: left; background: rgba(255,255,255,0.06); border: 1px solid rgba(255,255,255,0.15); border-radius: 12px; padding: 16px 20px; font-family: Monaco, Consolas, monospace; font-size: 0.78rem; color: #cfe3e8; overflow-x: auto; white-space: pre; margin: 24px auto; max-width: 640px; }
#scb-page .scb-back{ display: inline-block; margin-top: 10px; color: #8fb3bd; font-size: 0.85rem; text-decoration: none; }
#scb-page .scb-back:hover{ text-decoration: underline; }
</style>

<div id="scb-page">

<!-- ============ HERO ============ -->
<section class="scb-hero">
  <canvas id="scb-hero-canvas"></canvas>
  <div class="scb-hero-inner">
    <span class="scb-badge">DeLTa Workshop &middot; ICLR 2026</span>
    <h1>Sliding Critical Band</h1>
    <p class="scb-hook">A hidden band of RoPE frequencies decides whether a language model collapses past its training length &mdash; and that band isn't fixed. It <em>slides</em>.</p>
    <div class="scb-authors">Zifei Bai &amp; Zhiwei Xu</div>
    <div class="scb-cta-row">
      <a class="scb-btn primary" href="https://openreview.net/forum?id=y2IeQSTxmc" target="_blank" rel="noopener">Read the Paper</a>
      <a class="scb-btn ghost" href="https://github.com/zifei-bai/Sliding-Critical-Band-in-RoPE-based-Length-Extrapolation" target="_blank" rel="noopener">View Code</a>
    </div>
  </div>
  <div class="scb-scroll-cue">Scroll</div>
</section>

<!-- ============ 1. THE PROBLEM ============ -->
<section>
  <div class="scb-wrap">
    <div class="scb-grid2">
      <div class="reveal">
        <div class="scb-eyebrow">The Problem</div>
        <h2 class="scb-h2">LLMs are great at their training length. Then they fall apart.</h2>
        <p class="scb-p">Rotary Positional Encoding (RoPE) tells a transformer where a token sits by <strong>rotating</strong> its query/key vector. Split the vector into pairs \((x_1,x_2), (x_3,x_4), \ldots\) and spin the \(i\)-th pair by angle \(m\theta_i\), where \(m\) is the token's position and \(\theta_i = \theta^{-2i/d}\) shrinks fast as \(i\) grows.</p>
        <p class="scb-p">Drag the slider below: low-<code>i</code> pairs (left) whirl around many times as \(m\) increases; high-<code>i</code> pairs (right) barely twitch. Push \(m\) past anything seen in training and some of these rotations land on angles the model has never encountered &mdash; performance falls off a cliff.</p>
      </div>
      <div class="reveal">
        <div class="scb-spectrum-card">
          <div class="scb-m-readout">Token position m = <span id="scb-rope-m-val">12</span></div>
          <input type="range" min="0" max="100000" value="12" step="0.5" class="scb-compare-slider" id="scb-rope-m-slider" aria-label="Token position m">
          <div class="scb-dial-row" id="scb-dial-row"></div>
        </div>
        <p class="scb-caption" style="text-align:center;">Each dial is one real rotary subspace \( i \in \{0,16,32,48,64,95\} \), rotated by the actual \( m\theta_i \) &mdash; not a decorative loop.</p>
      </div>
    </div>

    <div class="reveal" style="margin-top:56px;">
      <div class="scb-spectrum-card">
        <h3 class="scb-h3" style="margin-top:0;">Why rotating encodes relative position</h3>
        <p class="scb-p" style="font-size:0.95rem; max-width:none;">Two copies of the same token at two different positions rotate to different absolute angles &mdash; but the <strong>angle between them</strong> depends only on the distance apart, \( (m_B - m_A)\theta \). That's the whole trick behind RoPE.</p>
        <div class="scb-rel-demo">
          <canvas id="scb-rel-canvas" width="280" height="280"></canvas>
          <div class="scb-rel-controls">
            <label>Token A position m<sub>A</sub> = <span id="scb-rel-ma-val">0</span>
              <input type="range" min="0" max="40" value="0" step="0.5" class="scb-compare-slider" id="scb-rel-ma-slider">
            </label>
            <label>Token B position m<sub>B</sub> = <span id="scb-rel-mb-val">18</span>
              <input type="range" min="0" max="40" value="18" step="0.5" class="scb-compare-slider" id="scb-rel-mb-slider">
            </label>
            <div class="scb-readout" style="margin-top:16px;">
              <div><div class="num" id="scb-rel-delta" style="font-size:1.2rem;">&mdash;</div><div class="lbl">&Delta;angle = (m<sub>B</sub> &minus; m<sub>A</sub>) &middot; &theta;</div></div>
            </div>
            <p class="scb-caption">Try moving <strong>both</strong> sliders by the same amount &mdash; the angle between A and B never changes.</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ============ 2. OLD THEORY VS TWIST ============ -->
<section class="scb-alt">
  <div class="scb-wrap">
    <div class="reveal">
      <div class="scb-eyebrow">The Prevailing Theory</div>
      <h2 class="scb-h2">"Just fix the low frequencies," they said.</h2>
      <p class="scb-p">The standard explanation says failure comes from <strong>low-frequency</strong> dimensions rotating past a fixed <strong>critical dimension</strong> \( d_{\text{extra}} \) into out-of-distribution angles they never saw in training:</p>
      <p class="scb-p" style="text-align:center;">\[ d_{\text{extra}} = \left\lceil \frac{d_{\max}}{2}\,\log_{\theta}\!\left(\frac{L_{\text{train}}}{2\pi}\right) \right\rceil \]</p>
      <p class="scb-p">For a 100-digit copying task, that threshold lands at <code>d_extra = 37</code>. If the theory were right, the model should only need help below that point &mdash; and everything above it should be free to roam.</p>
      <p class="scb-p">We tested that directly by interpolating only a chosen range of dimensions and sweeping where that range starts. The result didn't match the theory.</p>
    </div>

    <div class="reveal" style="margin-top:36px;">
      <div class="scb-spectrum-card">
        <div class="scb-controls" id="scb-fig1-controls"></div>
        <div id="scb-fig1-plot" style="height:340px;"></div>
        <p class="scb-caption">Each marker is a real measurement from the paper: the empirically-best cutoff <code>d_upper</code> (green) and the perplexity exactly at the theoretical threshold <code>d_extra = 37</code> (grey), for the 100-digit task. Notice <code>d_upper</code> sits <strong>before</strong> the dashed line &mdash; the textbook boundary is already too permissive at mild extrapolation ratios.</p>
      </div>
    </div>
  </div>
</section>

<!-- ============ 3. FORMALIZING THE BAND ============ -->
<section>
  <div class="scb-wrap">
    <div class="reveal">
      <div class="scb-eyebrow">Reframing It</div>
      <h2 class="scb-h2">Three zones, not one boundary</h2>
      <p class="scb-p">Sweeping the interpolation range from both directions reveals two boundaries, not one: an <strong>upper bound</strong> where the optimal cutoff sits, and a <strong>lower bound</strong> past which more interpolation stops helping. Together they carve the spectrum into three functional regions:</p>
    </div>
    <div class="reveal" style="margin-top:10px;">
      <div class="scb-spectrum-card">
        <div class="scb-spectrum-axis" id="scb-explainer-axis">
          <div class="scb-zone harmful" id="ex-harmful"></div>
          <div class="scb-zone critical" id="ex-critical"></div>
          <div class="scb-zone benign" id="ex-benign"></div>
        </div>
        <div class="scb-spectrum-ticks"><span>0</span><span>24</span><span>48</span><span>72</span><span>95</span></div>
        <div class="scb-spectrum-legend">
          <span><span class="scb-swatch" style="background:var(--scb-harmful)"></span><strong>Harmful [0, 20)</strong>&nbsp;&mdash; carries local structure; interpolating it confuses nearby tokens</span>
          <span><span class="scb-swatch" style="background:var(--scb-critical)"></span><strong>Critical Band [20, 68]</strong>&nbsp;&mdash; must be interpolated to survive extrapolation</span>
          <span><span class="scb-swatch" style="background:var(--scb-benign)"></span><strong>Benign (68, 95]</strong>&nbsp;&mdash; interpolating it is basically a no-op</span>
        </div>
        <p class="scb-caption">Shown for the 100-digit task at extrapolation ratio <code>s = 1.2</code>, matching the paper's empirical boundaries.</p>
      </div>
    </div>
  </div>
</section>

<!-- ============ 4. THE SLIDING BAND (HERO WIDGET) ============ -->
<section class="scb-alt scb-pin-stage" id="scb-slide-section">
  <div class="scb-wrap">
    <div class="reveal">
      <div class="scb-eyebrow">The Core Finding</div>
      <h2 class="scb-h2">The band doesn't sit still &mdash; it slides.</h2>
      <p class="scb-p">As the extrapolation ratio \( s = L' / L_{\text{train}} \) grows, both boundaries migrate toward the low-frequency end of the spectrum. The "right" dimensions to interpolate at \( s = 1.2 \) are the wrong ones at \( s = 8 \).</p>
    </div>
    <div class="reveal" style="margin-top:18px;">
      <div class="scb-spectrum-card">
        <div class="scb-controls">
          <span class="scb-controls-label">Task length</span>
          <div id="scb-slide-length-controls"></div>
        </div>
        <div class="scb-controls">
          <span class="scb-controls-label">Extrapolation ratio s</span>
          <div id="scb-slide-ratio-controls"></div>
        </div>
        <div class="scb-spectrum-axis" id="scb-slide-axis">
          <div class="scb-zone harmful" id="sl-harmful"></div>
          <div class="scb-zone critical" id="sl-critical"></div>
          <div class="scb-zone benign" id="sl-benign"></div>
        </div>
        <div class="scb-spectrum-ticks" id="scb-slide-ticks"></div>
        <div class="scb-readout">
          <div><div class="num" id="sl-upper">&mdash;</div><div class="lbl">d upper</div></div>
          <div><div class="num" id="sl-lower">&mdash;</div><div class="lbl">d lower</div></div>
          <div><div class="num" id="sl-width">&mdash;</div><div class="lbl">band width</div></div>
        </div>
        <p class="scb-caption">Values are the real upper/lower bounds reported in the paper (Table 2) for 50/100/500-digit string-copying tasks across six extrapolation ratios.</p>
      </div>
    </div>
  </div>
</section>

<!-- ============ 5. BANDWIDTH EXHAUSTION ============ -->
<section>
  <div class="scb-wrap">
    <div class="reveal">
      <div class="scb-eyebrow">Why Bigger RoPE-Base Helps</div>
      <h2 class="scb-h2">Spectrum Bandwidth Exhaustion</h2>
      <p class="scb-p">Push \( s \) high enough and the lower bound eventually runs past the last available dimension &mdash; the spectrum simply runs out of room, and the model collapses outright. Raising the RoPE base \( \theta \) lowers the frequency floor, which <strong>buys the band more room to slide</strong> before that happens.</p>
    </div>
    <div class="reveal" style="margin-top:18px;">
      <div class="scb-spectrum-card">
        <div class="scb-controls">
          <span class="scb-controls-label">RoPE base &theta;</span>
          <div id="scb-theta-controls"></div>
        </div>
        <div class="scb-controls">
          <span class="scb-controls-label">Extrapolation ratio s</span>
          <div id="scb-theta-ratio-controls"></div>
        </div>
        <div class="scb-spectrum-axis" id="scb-theta-axis">
          <div class="scb-zone harmful" id="th-harmful"></div>
          <div class="scb-zone critical" id="th-critical"></div>
          <div class="scb-zone benign" id="th-benign"></div>
        </div>
        <div class="scb-spectrum-ticks"><span>0</span><span>24</span><span>48</span><span>72</span><span>95</span></div>
        <div class="scb-tiles">
          <div class="scb-tile"><div class="tile-lbl">PPL, s=2</div><div class="tile-val" id="tile-s2">&mdash;</div></div>
          <div class="scb-tile"><div class="tile-lbl">PPL, s=4</div><div class="tile-val" id="tile-s4">&mdash;</div></div>
          <div class="scb-tile"><div class="tile-lbl">PPL, s=8</div><div class="tile-val" id="tile-s8">&mdash;</div></div>
        </div>
        <p class="scb-caption">500-digit task. Switch &theta; from <code>1e4</code> to <code>1e5</code> and watch the band pull back inside the spectrum, with perplexity dropping accordingly (e.g. at <code>s = 8</code>: 3.20 &rarr; 2.00).</p>
      </div>
    </div>
  </div>
</section>

<!-- ============ 6. ATTENTION RECOVERY ============ -->
<section class="scb-alt">
  <div class="scb-wrap">
    <div class="reveal">
      <div class="scb-eyebrow">What It Looks Like Inside the Model</div>
      <h2 class="scb-h2">The slash pattern comes back</h2>
      <p class="scb-p">Long-context attention heads often develop a clean diagonal "slash" &mdash; attending to tokens a fixed relative distance back. Under extrapolation that slash blurs and breaks apart. Interpolating exactly the Critical Band restores it.</p>
    </div>
    <div class="reveal" style="margin-top:18px;">
      <div class="scb-compare" id="scb-fig5-compare">
        <span class="scb-compare-tag left">Original RoPE</span>
        <span class="scb-compare-tag right">Critical Band interpolated</span>
        <img src="/images/scb/fig5-original-row.png" alt="Attention patterns under original RoPE, four heads, extrapolation ratio 1.5">
        <div class="after"><img src="/images/scb/fig5-interpolated-row.png" alt="Attention patterns after Critical Band interpolation, same four heads"></div>
      </div>
      <input type="range" min="0" max="100" value="50" class="scb-compare-slider" id="scb-fig5-slider" aria-label="Drag to compare original vs interpolated attention patterns">
      <p class="scb-caption">Four attention heads (L0H0, L0H1, L1H0, L3H1) on the 100-digit task at <code>s = 1.5</code>. Drag the slider &mdash; the interpolated row (right) keeps its diagonal slash sharp all the way to the sequence end; the original (left) fades and fragments past the training length.</p>
    </div>
  </div>
</section>

<!-- ============ 7. REAL WORLD VALIDATION ============ -->
<section>
  <div class="scb-wrap">
    <div class="reveal">
      <div class="scb-eyebrow">Beyond Toy Models</div>
      <h2 class="scb-h2">Same sliding dynamics on LLaMA-7B + C4</h2>
      <p class="scb-p">The synthetic string-copying task is a controlled lens, so we re-ran the same boundary search on real LLaMA-7B weights over the C4 English corpus.</p>
    </div>
    <div class="reveal" style="margin-top:18px;">
      <div class="scb-spectrum-card">
        <div class="scb-controls">
          <span class="scb-controls-label">Extrapolation ratio s</span>
          <div id="scb-llama-controls"></div>
        </div>
        <div class="scb-spectrum-axis" id="scb-llama-axis">
          <div class="scb-zone harmful" id="ll-harmful"></div>
          <div class="scb-zone critical" id="ll-critical"></div>
          <div class="scb-zone benign" id="ll-benign"></div>
        </div>
        <div class="scb-spectrum-ticks"><span>0</span><span>16</span><span>32</span><span>48</span><span>63</span></div>
        <p class="scb-caption">LLaMA-7B head dimension is 128 (64 rotary subspaces). Band positions for <code>s = 1.125, 1.25, 2.0</code> are exact values from the paper; <code>s = 1.75</code> is a visual interpolation between the two neighboring measured ratios (not separately reported).</p>
      </div>
    </div>

    <div class="reveal" style="margin-top:34px;">
      <h3 class="scb-h3">Zero-shot perplexity at extended context (C4)</h3>
      <p class="scb-p">Sliding Critical Band (SCB) needs <em>no fine-tuning</em> to match or beat Position Interpolation and YaRN &mdash; it's already the model's intrinsic mechanism, just correctly targeted.</p>
      <div id="scb-table4-plot" style="height:340px;"></div>
      <details class="scb-details">
        <summary>Show full results table (all ratios, ZS &amp; fine-tuned)</summary>
        <div id="scb-table4-full"></div>
      </details>
    </div>
  </div>
</section>

<!-- ============ 8. CTA ============ -->
<section class="scb-final">
  <div class="scb-wrap">
    <div class="reveal">
      <h2 class="scb-h2">Not a fixed boundary. A moving target.</h2>
      <p class="scb-p">Sliding Critical Band reframes RoPE extrapolation failure as a dynamic, trackable phenomenon &mdash; and Spectrum Bandwidth Exhaustion explains the hard physical limit underneath it. Both point toward position encodings that track their own frequency boundaries in real time.</p>
      <div class="scb-cta-row">
        <a class="scb-btn primary" href="https://openreview.net/forum?id=y2IeQSTxmc" target="_blank" rel="noopener">Read the Paper</a>
        <a class="scb-btn ghost" href="https://github.com/zifei-bai/Sliding-Critical-Band-in-RoPE-based-Length-Extrapolation" target="_blank" rel="noopener">View Code</a>
      </div>
      <div class="scb-bibtex">@inproceedings{bai2026sliding,
  title     = {Sliding Critical Band in RoPE-based Length Extrapolation},
  author    = {Bai, Zifei and Xu, Zhiwei},
  booktitle = {DeLTa Workshop @ ICLR 2026},
  year      = {2026},
  url       = {https://openreview.net/forum?id=y2IeQSTxmc}
}</div>
      <a class="scb-back" href="/">&larr; Back to zifei-bai.github.io</a>
    </div>
  </div>
</section>

</div>

<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/ScrollTrigger.min.js"></script>
<script>
(function(){
  "use strict";
  var reduceMotion = window.matchMedia && window.matchMedia("(prefers-reduced-motion: reduce)").matches;

  /* ---------------- generic scroll reveal ---------------- */
  var revealEls = document.querySelectorAll("#scb-page .reveal");
  if ("IntersectionObserver" in window){
    var io = new IntersectionObserver(function(entries){
      entries.forEach(function(e){ if (e.isIntersecting){ e.target.classList.add("is-visible"); io.unobserve(e.target); } });
    }, { threshold: 0.15 });
    revealEls.forEach(function(el){ io.observe(el); });
  } else {
    revealEls.forEach(function(el){ el.classList.add("is-visible"); });
  }

  /* ---------------- hero canvas: rotating RoPE dials ---------------- */
  (function heroCanvas(){
    var canvas = document.getElementById("scb-hero-canvas");
    if (!canvas) return;
    var ctx = canvas.getContext("2d");
    var w, h, dpr = Math.min(window.devicePixelRatio || 1, 2);
    var hero = canvas.parentElement;
    function resize(){
      w = hero.clientWidth; h = hero.clientHeight;
      canvas.width = w * dpr; canvas.height = h * dpr;
      canvas.style.width = w + "px"; canvas.style.height = h + "px";
      ctx.setTransform(dpr,0,0,dpr,0,0);
    }
    resize();
    window.addEventListener("resize", resize);

    var N = 48;
    var points = [];
    for (var i = 0; i < N; i++){
      points.push({
        x: Math.random(), y: Math.random(),
        freq: Math.pow(10000, -2*(i % 12)/12) * (0.6 + Math.random()*0.8),
        angle: Math.random() * Math.PI * 2,
        r: 10 + Math.random() * 16
      });
    }
    var last = performance.now();
    function frame(t){
      var dt = (t - last) / 1000; last = t;
      ctx.clearRect(0,0,w,h);
      points.forEach(function(p){
        if (!reduceMotion) p.angle += dt * (0.15 + p.freq * 40);
        var cx = p.x * w, cy = p.y * h;
        var x2 = cx + Math.cos(p.angle) * p.r;
        var y2 = cy + Math.sin(p.angle) * p.r;
        ctx.strokeStyle = "rgba(143,196,205,0.55)";
        ctx.lineWidth = 1.4;
        ctx.beginPath(); ctx.moveTo(cx,cy); ctx.lineTo(x2,y2); ctx.stroke();
        ctx.fillStyle = "rgba(143,196,205,0.85)";
        ctx.beginPath(); ctx.arc(x2,y2,1.8,0,Math.PI*2); ctx.fill();
      });
      if (!reduceMotion) requestAnimationFrame(frame); else requestAnimationFrame(function(){});
    }
    requestAnimationFrame(frame);
  })();

  /* ---------------- RoPE 101 (section 1): real math, slider-driven ---------------- */
  var DMAX_HALF = 96;
  function thetaOf(i){ return Math.pow(10000, -2*i/DMAX_HALF); }
  var ROPE_DIMS = [0, 16, 32, 48, 64, 95];

  (function ropeDemo(){
    var row = document.getElementById("scb-dial-row");
    var slider = document.getElementById("scb-rope-m-slider");
    var mVal = document.getElementById("scb-rope-m-val");
    if (!row || !slider) return;

    var needles = ROPE_DIMS.map(function(i){
      var col = document.createElement("div"); col.className = "scb-dial-col";
      var dial = document.createElement("div"); dial.className = "scb-dial";
      var needle = document.createElement("div"); needle.className = "needle";
      var dot = document.createElement("div"); dot.className = "dot";
      dial.appendChild(needle); dial.appendChild(dot);
      col.appendChild(dial);
      var label = document.createElement("div"); label.className = "scb-dial-label";
      label.innerHTML = "<b>i=" + i + "</b>&theta;&approx;" + thetaOf(i).toExponential(1);
      col.appendChild(label);
      row.appendChild(col);
      return { needle: needle, theta: thetaOf(i) };
    });

    function render(m){
      mVal.textContent = m.toFixed(0);
      needles.forEach(function(n){
        n.needle.style.transform = "rotate(" + ((m * n.theta) * 180/Math.PI) + "deg)";
      });
    }
    slider.addEventListener("input", function(){ render(parseFloat(slider.value)); });
    render(parseFloat(slider.value));

    /* auto-sweep m from 0 -> 100 once, the first time this section scrolls into view */
    var section = row.closest("section");
    if (section && !reduceMotion && "IntersectionObserver" in window){
      var played = false;
      var obs = new IntersectionObserver(function(entries){
        entries.forEach(function(e){
          if (e.isIntersecting && !played){
            played = true;
            var start = null, dur = 3200;
            function step(t){
              if (start === null) start = t;
              var p = Math.min(1, (t - start) / dur);
              var m = p * 100;
              slider.value = m; render(m);
              if (p < 1) requestAnimationFrame(step);
            }
            requestAnimationFrame(step);
          }
        });
      }, { threshold: 0.5 });
      obs.observe(section);
    }
  })();

  (function relDemo(){
    var canvas = document.getElementById("scb-rel-canvas");
    var sliderA = document.getElementById("scb-rel-ma-slider");
    var sliderB = document.getElementById("scb-rel-mb-slider");
    var valA = document.getElementById("scb-rel-ma-val");
    var valB = document.getElementById("scb-rel-mb-val");
    var deltaEl = document.getElementById("scb-rel-delta");
    if (!canvas || !sliderA || !sliderB) return;
    var ctx = canvas.getContext("2d");
    var W = canvas.width, H = canvas.height, cx = W/2, cy = H/2, R = Math.min(W,H)/2 - 26;
    var thetaRel = thetaOf(16); // ~0.046 rad/step: a full sweep is visible across the 0-40 slider range

    function drawArrow(angle, color, label){
      var x2 = cx + Math.cos(angle) * R, y2 = cy - Math.sin(angle) * R;
      ctx.strokeStyle = color; ctx.fillStyle = color; ctx.lineWidth = 3;
      ctx.beginPath(); ctx.moveTo(cx, cy); ctx.lineTo(x2, y2); ctx.stroke();
      var ah = 8, a2 = Math.atan2(cy - y2, x2 - cx);
      ctx.beginPath();
      ctx.moveTo(x2, y2);
      ctx.lineTo(x2 - ah*Math.cos(a2-0.4), y2 + ah*Math.sin(a2-0.4));
      ctx.lineTo(x2 - ah*Math.cos(a2+0.4), y2 + ah*Math.sin(a2+0.4));
      ctx.closePath(); ctx.fill();
      ctx.font = "12px -apple-system, sans-serif";
      ctx.fillText(label, x2 + Math.cos(angle)*14, y2 - Math.sin(angle)*14 + 4);
    }

    function draw(){
      var mA = parseFloat(sliderA.value), mB = parseFloat(sliderB.value);
      valA.textContent = mA.toFixed(0); valB.textContent = mB.toFixed(0);
      var angA = mA * thetaRel, angB = mB * thetaRel, delta = angB - angA;
      deltaEl.textContent = delta.toFixed(3) + " rad (" + (delta*180/Math.PI).toFixed(1) + "°)";

      ctx.clearRect(0, 0, W, H);
      ctx.strokeStyle = "rgba(122,130,136,0.25)"; ctx.lineWidth = 1;
      ctx.beginPath(); ctx.moveTo(0, cy); ctx.lineTo(W, cy); ctx.moveTo(cx, 0); ctx.lineTo(cx, H); ctx.stroke();

      ctx.strokeStyle = "rgba(248,148,6,0.85)"; ctx.lineWidth = 2;
      ctx.beginPath(); ctx.arc(cx, cy, 30, -angA, -angB, angB < angA); ctx.stroke();

      drawArrow(angA, "#2f7f93", "A");
      drawArrow(angB, "#ee5f5b", "B");
    }
    sliderA.addEventListener("input", draw);
    sliderB.addEventListener("input", draw);
    draw();
  })();

  /* ================================================================
     Reusable "spectrum widget": three colored zones over a fixed
     dimension axis, driven by [d_upper, d_lower] band data.
     ================================================================ */
  function paintZones(ids, dUpper, dLower, dmaxHalf){
    var pct = function(v){ return (v/dmaxHalf*100).toFixed(2) + "%"; };
    var harmful = document.getElementById(ids.harmful);
    var critical = document.getElementById(ids.critical);
    var benign = document.getElementById(ids.benign);
    harmful.style.left = "0%"; harmful.style.width = pct(dUpper);
    critical.style.left = pct(dUpper); critical.style.width = pct(dLower - dUpper);
    benign.style.left = pct(dLower); benign.style.width = pct(dmaxHalf - dLower);
  }

  function makeChips(container, items, activeIndex, onSelect){
    container.innerHTML = "";
    items.forEach(function(label, idx){
      var chip = document.createElement("button");
      chip.type = "button";
      chip.className = "scb-chip" + (idx === activeIndex ? " active" : "");
      chip.textContent = label;
      chip.addEventListener("click", function(){
        Array.prototype.forEach.call(container.children, function(c){ c.classList.remove("active"); });
        chip.classList.add("active");
        onSelect(idx);
      });
      container.appendChild(chip);
    });
  }

  /* ---- explainer static diagram (section 3), fixed at 100-digit s=1.2 ---- */
  paintZones({harmful:"ex-harmful", critical:"ex-critical", benign:"ex-benign"}, 20, 68, 96);

  /* ---- Table 2: sliding band by task length & ratio ---- */
  var RATIOS = [1.1, 1.2, 1.5, 2, 4, 8];
  var BAND_BY_LENGTH = {
    50:  { 1.1:[8,48],  1.2:[0,37],  1.5:[20,63], 2:[25,68], 4:[33,87], 8:[30,87] },
    100: { 1.1:[16,73], 1.2:[20,68], 1.5:[19,64], 2:[24,71], 4:[31,84], 8:[31,95] },
    500: { 1.1:[0,89],  1.2:[23,85], 1.5:[27,94], 2:[32,93], 4:[35,95], 8:[35,95] }
  };
  var slideState = { length: 100, ratio: 1.2 };
  function renderSlide(){
    var band = BAND_BY_LENGTH[slideState.length][slideState.ratio];
    paintZones({harmful:"sl-harmful", critical:"sl-critical", benign:"sl-benign"}, band[0], band[1], 96);
    document.getElementById("sl-upper").textContent = band[0];
    document.getElementById("sl-lower").textContent = band[1];
    document.getElementById("sl-width").textContent = (band[1]-band[0]);
  }
  makeChips(document.getElementById("scb-slide-length-controls"), ["50-digit","100-digit","500-digit"], 1, function(i){
    slideState.length = [50,100,500][i]; renderSlide();
  });
  makeChips(document.getElementById("scb-slide-ratio-controls"), RATIOS.map(function(r){return "s="+r;}), 1, function(i){
    slideState.ratio = RATIOS[i]; renderSlide();
  });
  (function ticks(){
    var t = document.getElementById("scb-slide-ticks");
    [0,24,48,72,95].forEach(function(v){ var s=document.createElement("span"); s.textContent=v; t.appendChild(s); });
  })();
  renderSlide();

  /* auto-advance through ratios once, first time the section is scrolled into view */
  (function autoplay(){
    var section = document.getElementById("scb-slide-section");
    if (!section || reduceMotion || !("IntersectionObserver" in window)) return;
    var played = false;
    var obs = new IntersectionObserver(function(entries){
      entries.forEach(function(e){
        if (e.isIntersecting && !played){
          played = true;
          var i = 0;
          var chips = document.querySelectorAll("#scb-slide-ratio-controls .scb-chip");
          var timer = setInterval(function(){
            if (i >= RATIOS.length){ clearInterval(timer); return; }
            chips[i].click();
            i++;
          }, 750);
        }
      });
    }, { threshold: 0.6 });
    obs.observe(section);
  })();

  /* ---- Table 3 / Figure 4: theta comparison, 500-digit ---- */
  var BAND_BY_THETA = {
    "1e4": { 1.1:[0,89],  1.2:[23,85], 1.5:[27,94], 2:[32,93], 4:[35,95], 8:[35,95] },
    "1e5": { 1.1:[18,73], 1.2:[27,85], 1.5:[28,82], 2:[24,87], 4:[26,95], 8:[29,93] }
  };
  var PPL_BY_THETA = {
    "1e4": { 2:"1.0013", 4:"1.0389", 8:"3.2016" },
    "1e5": { 2:"1.0002", 4:"1.0178", 8:"1.9984" }
  };
  var thetaState = { theta: "1e4", ratio: 8 };
  function renderTheta(){
    var band = BAND_BY_THETA[thetaState.theta][thetaState.ratio];
    paintZones({harmful:"th-harmful", critical:"th-critical", benign:"th-benign"}, band[0], band[1], 96);
    ["2","4","8"].forEach(function(s){
      var el = document.getElementById("tile-s"+s);
      if (!el) return;
      el.textContent = PPL_BY_THETA[thetaState.theta][s] || "&mdash;";
      el.className = "tile-val " + (thetaState.theta === "1e5" ? "good" : "bad");
    });
  }
  makeChips(document.getElementById("scb-theta-controls"), ["θ = 10⁴","θ = 10⁵"], 0, function(i){
    thetaState.theta = i === 0 ? "1e4" : "1e5"; renderTheta();
  });
  makeChips(document.getElementById("scb-theta-ratio-controls"), RATIOS.map(function(r){return "s="+r;}), 5, function(i){
    thetaState.ratio = RATIOS[i]; renderTheta();
  });
  renderTheta();

  /* ---- Figure 5 before/after slider ---- */
  (function fig5(){
    var slider = document.getElementById("scb-fig5-slider");
    var after = document.querySelector("#scb-fig5-compare .after");
    if (!slider || !after) return;
    function update(){
      var v = slider.value;
      after.style.clipPath = "inset(0 " + (100-v) + "% 0 0)";
    }
    slider.addEventListener("input", update);
    update();
  })();

  /* ---- LLaMA-7B / C4 band widget ---- */
  var LLAMA_RATIOS = [1.125, 1.25, 1.75, 2.0];
  var LLAMA_BAND = { 1.125:[30,58], 1.25:[33,60], 1.75:[34,62], 2:[35,63] }; // 1.75 visually interpolated, see caption
  var llamaState = { ratio: 1.125 };
  function renderLlama(){
    var band = LLAMA_BAND[llamaState.ratio];
    paintZones({harmful:"ll-harmful", critical:"ll-critical", benign:"ll-benign"}, band[0], band[1], 64);
  }
  makeChips(document.getElementById("scb-llama-controls"), LLAMA_RATIOS.map(function(r){return "s="+r;}), 0, function(i){
    llamaState.ratio = LLAMA_RATIOS[i]; renderLlama();
  });
  renderLlama();

  /* ---------------- Plotly: Figure 1 U-curve twist ---------------- */
  var FIG1 = {
    1.1: { dUpper: 16, minPpl: 1.0000, ppl37: 1.0006 },
    1.2: { dUpper: 20, minPpl: 1.0001, ppl37: 1.0032 },
    1.5: { dUpper: 19, minPpl: 1.0004, ppl37: 1.0055 },
    2:   { dUpper: 24, minPpl: 1.0006, ppl37: 1.0093 },
    4:   { dUpper: 31, minPpl: 1.0717, ppl37: 1.0979 },
    8:   { dUpper: 31, minPpl: 13.0095, ppl37: 15.1535 }
  };
  var fig1State = { ratio: 1.2 };
  function renderFig1(){
    if (!window.Plotly) return;
    var d = FIG1[fig1State.ratio];
    var xs = [d.dUpper, 37];
    var ys = [d.minPpl, d.ppl37];
    var traces = [
      { x: xs, y: ys, mode: "lines+markers", line: {color: "#2f7f93", width: 3, dash: "dot"},
        marker: {size: 10, color: ["#62c462", "#7a8288"]}, name: "PPL (empirical)" }
    ];
    var layout = {
      margin: {l: 55, r: 20, t: 10, b: 40},
      xaxis: { title: "Interpolation cutoff, dimension d", range: [0, 50], zeroline: false, gridcolor: "#eee" },
      yaxis: { title: "Perplexity", type: "log", gridcolor: "#eee" },
      shapes: [
        { type: "rect", xref: "x", yref: "paper", x0: 0, x1: d.dUpper, y0: 0, y1: 1, fillcolor: "rgba(238,95,91,0.10)", line: {width:0} },
        { type: "line", xref: "x", yref: "paper", x0: 37, x1: 37, y0: 0, y1: 1, line: {color: "#7a8288", width: 1.5, dash: "dash"} }
      ],
      annotations: [
        { x: d.dUpper, y: Math.log10(d.minPpl), text: "d_upper = " + d.dUpper, showarrow: true, arrowhead: 0, ax: 0, ay: -34, font: {color:"#62c462", size: 11} },
        { x: 37, y: 1, xref: "x", yref: "paper", yanchor: "bottom", text: "theoretical d_extra = 37", showarrow: false, font: {color:"#7a8288", size: 11} }
      ],
      font: { family: "-apple-system, sans-serif", size: 12 },
      showlegend: false,
      paper_bgcolor: "rgba(0,0,0,0)", plot_bgcolor: "rgba(0,0,0,0)"
    };
    Plotly.newPlot("scb-fig1-plot", traces, layout, {displayModeBar: false, responsive: true});
  }
  makeChips(document.getElementById("scb-fig1-controls"), RATIOS.map(function(r){return "s="+r;}), 1, function(i){
    fig1State.ratio = RATIOS[i]; renderFig1();
  });

  /* ---------------- Plotly: Table 4 results ---------------- */
  var TABLE4 = {
    1.125: { Vanilla: 6.7351, PI: 6.4362, YaRN: 6.4319, SCB: 6.4306 },
    1.25:  { Vanilla: 10.5324, PI: 6.6323, YaRN: 6.6147, SCB: 6.6110 },
    1.75:  { Vanilla: 59.4537, PI: 5.9608, YaRN: 5.7068, SCB: 5.6999 },
    2:     { Vanilla: 109.7982, PI: 6.0521, YaRN: 5.6790, SCB: 5.6637 }
  };
  var TABLE4_FULL = [
    ["1.125","Vanilla","6.0490","6.0533","6.7351","6.4862"],
    ["1.125","PI","6.0473","6.0381","6.4362","6.4271"],
    ["1.125","YaRN","6.0434","6.0385","6.4319","6.4245"],
    ["1.125","SCB (ours)","6.0394","6.0374","6.4306","6.4241"],
    ["1.25","Vanilla","6.0490","6.0928","10.5324","7.2228"],
    ["1.25","PI","6.0596","6.0388","6.6323","6.6041"],
    ["1.25","YaRN","6.0559","6.0395","6.6147","6.6010"],
    ["1.25","SCB (ours)","6.0434","6.0374","6.6110","6.5996"],
    ["1.75","Vanilla","6.0490","6.0906","59.4537","14.0116"],
    ["1.75","PI","6.2922","6.0502","5.9608","5.6688"],
    ["1.75","YaRN","6.0991","6.0407","5.7068","5.6422"],
    ["1.75","SCB (ours)","6.0886","6.0547","5.6999","5.6448"],
    ["2.0","Vanilla","6.0490","6.1238","109.7982","19.1655"],
    ["2.0","PI","6.4359","6.6526","6.0521","6.2305"],
    ["2.0","YaRN","6.1227","6.6136","5.6790","6.1631"],
    ["2.0","SCB (ours)","6.1212","6.6611","5.6637","6.1643"]
  ];
  function renderTable4(){
    if (!window.Plotly) return;
    var order = ["Vanilla","PI","YaRN","SCB"];
    var colors = { Vanilla: "#7a8288", PI: "#f89406", YaRN: "#2f7f93", SCB: "#62c462" };
    var data = TABLE4[1.75]; // most illustrative gap; user can read full table below
    var trace = {
      x: order, y: order.map(function(m){ return data[m]; }),
      type: "bar", marker: {color: order.map(function(m){ return colors[m]; })},
      text: order.map(function(m){ return data[m].toFixed(2); }), textposition: "outside"
    };
    var layout = {
      margin: {l: 55, r: 20, t: 10, b: 40},
      yaxis: { title: "Zero-shot PPL at extended context", type: "log", gridcolor: "#eee" },
      xaxis: { gridcolor: "#eee" },
      font: { family: "-apple-system, sans-serif", size: 12 },
      paper_bgcolor: "rgba(0,0,0,0)", plot_bgcolor: "rgba(0,0,0,0)",
      title: { text: "Ratio s = 1.75 &mdash; zero-shot, extended context", font: {size: 13, color: "#5b6773"} }
    };
    Plotly.newPlot("scb-table4-plot", [trace], layout, {displayModeBar: false, responsive: true});

    var host = document.getElementById("scb-table4-full");
    var html = '<table class="scb-table"><thead><tr><th>Ratio</th><th>Method</th><th>2k ZS</th><th>2k FT</th><th>Ext ZS</th><th>Ext FT</th></tr></thead><tbody>';
    TABLE4_FULL.forEach(function(row){
      html += "<tr><td>" + row[0] + "</td><td>" + row[1] + "</td><td>" + row[2] + "</td><td>" + row[3] + "</td><td>" + row[4] + "</td><td>" + row[5] + "</td></tr>";
    });
    html += "</tbody></table>";
    host.innerHTML = html;
  }

  /* ---------------- init plots once Plotly (deferred) is ready ---------------- */
  function waitForPlotly(cb){
    if (window.Plotly) return cb();
    var tries = 0;
    var iv = setInterval(function(){
      tries++;
      if (window.Plotly){ clearInterval(iv); cb(); }
      else if (tries > 100){ clearInterval(iv); }
    }, 100);
  }
  waitForPlotly(function(){ renderFig1(); renderTable4(); });

  /* ---------------- GSAP pin for the sliding-band hero widget ---------------- */
  if (window.gsap && window.ScrollTrigger && !reduceMotion){
    gsap.registerPlugin(ScrollTrigger);
    ScrollTrigger.create({
      trigger: "#scb-slide-section",
      start: "top top",
      end: "+=60%",
      pin: true,
      pinSpacing: true
    });
  }
})();
</script>
