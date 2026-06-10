---
layout: fullhtml-post
title: "How to Use AI Without Losing Your Thinking Power"
date: 2026-05-23
categories: ["Research and Life in the Age of AI"]
tags: ["ai", "thinking", "meta"]
description: "I caught myself outsourcing my ability to reason — not just to code, but to think through everyday life. Here's the framework I built to take it back."
_styles: >
  .blog-fullhtml {
  --ink: #1a1a1a;
  --paper: #FAFAF7;
  --accent: #C04B2D;
  --accent-light: #F3E8E4;
  --green: #2D5F2D;
  --green-light: #E8F0E8;
  --muted: #6B6560;
  --border: #E0DBD5;
  --card-bg: #FFFFFF;
  }

  .blog-fullhtml * { margin: 0; padding: 0; box-sizing: border-box; }

  .blog-fullhtml {
  font-family: 'Source Sans 3', sans-serif;
  color: var(--ink);
  line-height: 1.75;
  font-size: 18px;
  font-weight: 400;
  }

  .blog-fullhtml .hero {
  min-height: 80vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 4rem 2rem;
  position: relative;
  overflow: hidden;
  background: linear-gradient(165deg, #FDFAF6 0%, #F3E8E4 100%);
  }

  .blog-fullhtml .hero-inner {
  max-width: 720px;
  margin: 0 auto;
  position: relative;
  z-index: 1;
  }

  .blog-fullhtml .hero-tag {
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.72rem;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 2rem;
  }

  .blog-fullhtml .hero h1 {
  font-family: 'Playfair Display', serif;
  font-size: clamp(2.2rem, 5.5vw, 3.6rem);
  font-weight: 700;
  line-height: 1.15;
  margin-bottom: 1.5rem;
  }

  .blog-fullhtml .hero h1 em { color: var(--accent); font-style: italic; }

  .blog-fullhtml .hero-subtitle {
  font-size: 1.1rem;
  line-height: 1.7;
  color: var(--muted);
  max-width: 560px;
  font-weight: 300;
  }

  .blog-fullhtml .content { max-width: 720px; margin: 0 auto; padding: 3rem 2rem 6rem; }

  .blog-fullhtml .section-label {
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.68rem;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 0.75rem;
  }

  .blog-fullhtml .story {
  max-width: 640px;
  margin-bottom: 3.5rem;
  }

  .blog-fullhtml .story h2 {
  font-family: 'Playfair Display', serif;
  font-size: 1.6rem;
  font-weight: 700;
  margin-bottom: 1rem;
  }

  .blog-fullhtml .story p {
  margin-bottom: 1rem;
  color: #333;
  font-size: 1.05rem;
  }

  .blog-fullhtml .story p:last-child { margin-bottom: 0; }

  .blog-fullhtml .callout {
  border-left: 3px solid var(--accent);
  padding: 0.75rem 1.25rem;
  margin: 2rem 0;
  font-size: 1.05rem;
  color: var(--muted);
  font-weight: 400;
  max-width: 640px;
  }

  .blog-fullhtml .pullquote {
  font-family: 'Playfair Display', serif;
  font-size: 1.4rem;
  font-style: italic;
  color: var(--accent);
  text-align: center;
  padding: 2.5rem 1.5rem;
  margin: 1rem 0 3.5rem;
  max-width: 640px;
  margin-left: auto;
  margin-right: auto;
  }

  .blog-fullhtml .pq-line {
  display: block;
  width: 50px;
  height: 1px;
  background: var(--border);
  margin: 0 auto 1.25rem;
  }

  .blog-fullhtml .viz { margin-bottom: 3.5rem; }

  .blog-fullhtml .viz-title {
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.68rem;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: var(--muted);
  margin-bottom: 1rem;
  text-align: center;
  }

  .blog-fullhtml .spectrum-wrap {
  padding: 2rem;
  background: var(--card-bg);
  border-radius: 12px;
  border: 1px solid var(--border);
  }

  .blog-fullhtml .spectrum-bar {
  display: flex;
  border-radius: 8px;
  overflow: hidden;
  height: 72px;
  margin-bottom: 0.75rem;
  }

  .blog-fullhtml .sb-zone {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.82rem;
  font-weight: 500;
  text-align: center;
  line-height: 1.3;
  padding: 0.5rem;
  }

  .blog-fullhtml .sb-you { background: var(--green); color: white; flex: 3; }
  .blog-fullhtml .sb-sweet { background: var(--accent); color: white; flex: 2; border-left: 2px solid white; border-right: 2px solid white; }
  .blog-fullhtml .sb-danger { background: #E0DBD5; color: #8C8580; flex: 3; }

  .blog-fullhtml .sb-labels {
  display: flex;
  justify-content: space-between;
  font-size: 0.7rem;
  color: var(--muted);
  font-family: 'JetBrains Mono', monospace;
  }

  .blog-fullhtml .sb-sweet-tag {
  text-align: center;
  font-size: 0.7rem;
  color: var(--accent);
  font-weight: 600;
  margin-top: 0.5rem;
  }

  .blog-fullhtml .compare {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-bottom: 3.5rem;
  }

  @media (max-width: 600px) { .blog-fullhtml .compare { grid-template-columns: 1fr; } }

  .blog-fullhtml .compare-col {
  padding: 1.5rem;
  border-radius: 12px;
  border: 1px solid var(--border);
  }

  .blog-fullhtml .compare-col.bad {
  background: #FDF5F3;
  border-color: #E8C8C0;
  }

  .blog-fullhtml .compare-col.good {
  background: var(--green-light);
  border-color: #B8D4B8;
  }

  .blog-fullhtml .compare-label {
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.65rem;
  letter-spacing: 2px;
  text-transform: uppercase;
  margin-bottom: 1rem;
  }

  .blog-fullhtml .compare-col.bad .compare-label { color: var(--accent); }
  .blog-fullhtml .compare-col.good .compare-label { color: var(--green); }

  .blog-fullhtml .compare-item {
  display: flex;
  align-items: flex-start;
  gap: 0.6rem;
  margin-bottom: 0.75rem;
  font-size: 0.9rem;
  color: #444;
  line-height: 1.5;
  }

  .blog-fullhtml .compare-item:last-child { margin-bottom: 0; }

  .blog-fullhtml .ci-icon {
  flex-shrink: 0;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.7rem;
  margin-top: 2px;
  }

  .blog-fullhtml .bad .ci-icon { background: var(--accent); color: white; }
  .blog-fullhtml .good .ci-icon { background: var(--green); color: white; }

  .blog-fullhtml .costs-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-bottom: 3.5rem;
  }

  @media (max-width: 600px) { .blog-fullhtml .costs-grid { grid-template-columns: 1fr; } }

  .blog-fullhtml .cost-card {
  padding: 1.5rem;
  border-radius: 12px;
  border: 1px solid var(--border);
  background: var(--card-bg);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  }

  .blog-fullhtml .cost-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0,0,0,0.05);
  }

  .blog-fullhtml .cc-num {
  font-family: 'Playfair Display', serif;
  font-size: 2rem;
  font-weight: 700;
  color: var(--accent);
  opacity: 0.25;
  line-height: 1;
  margin-bottom: 0.5rem;
  }

  .blog-fullhtml .cost-card h3 {
  font-size: 1rem;
  font-weight: 600;
  margin-bottom: 0.4rem;
  }

  .blog-fullhtml .cost-card p {
  font-size: 0.88rem;
  color: var(--muted);
  line-height: 1.55;
  margin: 0;
  }

  .blog-fullhtml .flow-wrap {
  padding: 2rem;
  background: var(--card-bg);
  border-radius: 12px;
  border: 1px solid var(--border);
  margin-bottom: 3.5rem;
  }

  .blog-fullhtml .flow-step {
  display: flex;
  align-items: center;
  gap: 0.85rem;
  margin-bottom: 0.35rem;
  }

  .blog-fullhtml .flow-diamond {
  width: 32px; height: 32px;
  background: var(--accent-light);
  border: 2px solid var(--accent);
  transform: rotate(45deg);
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  }

  .blog-fullhtml .flow-diamond span {
  transform: rotate(-45deg);
  font-size: 0.7rem;
  font-weight: 600;
  color: var(--accent);
  }

  .blog-fullhtml .flow-q {
  font-size: 0.95rem;
  font-weight: 500;
  }

  .blog-fullhtml .flow-branch {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-left: 46px;
  margin-bottom: 0.6rem;
  font-size: 0.78rem;
  font-family: 'JetBrains Mono', monospace;
  color: var(--muted);
  }

  .blog-fullhtml .flow-tag {
  padding: 0.25rem 0.65rem;
  border-radius: 4px;
  font-size: 0.78rem;
  font-weight: 500;
  font-family: 'Source Sans 3', sans-serif;
  }

  .blog-fullhtml .ft-think { background: var(--green-light); color: var(--green); }
  .blog-fullhtml .ft-people { background: #E8ECF5; color: #3D5A80; }
  .blog-fullhtml .ft-ai { background: var(--accent-light); color: var(--accent); }

  .blog-fullhtml .flow-line {
  width: 2px;
  height: 18px;
  background: var(--border);
  margin-left: 15px;
  }

  .blog-fullhtml .flow-final {
  display: flex;
  align-items: center;
  gap: 0.85rem;
  margin-top: 0.35rem;
  }

  .blog-fullhtml .flow-final-box {
  background: var(--accent-light);
  border: 2px solid var(--accent);
  border-radius: 8px;
  padding: 0.6rem 1rem;
  font-size: 0.9rem;
  font-weight: 500;
  color: var(--accent);
  }

  .blog-fullhtml .framework {
  padding: 2rem;
  background: var(--card-bg);
  border-radius: 12px;
  border: 2px solid var(--accent);
  margin-bottom: 3.5rem;
  }

  .blog-fullhtml .framework h3 {
  font-family: 'Playfair Display', serif;
  font-size: 1.3rem;
  color: var(--accent);
  margin-bottom: 1.25rem;
  }

  .blog-fullhtml .fw-step {
  display: flex;
  gap: 1rem;
  margin-bottom: 1.1rem;
  padding-bottom: 1.1rem;
  border-bottom: 1px solid var(--border);
  }

  .blog-fullhtml .fw-step:last-child { border-bottom: none; margin-bottom: 0; padding-bottom: 0; }

  .blog-fullhtml .fw-num {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background: var(--accent);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.8rem;
  font-weight: 600;
  flex-shrink: 0;
  margin-top: 2px;
  }

  .blog-fullhtml .fw-text {
  font-size: 0.95rem;
  color: #444;
  line-height: 1.5;
  }

  .blog-fullhtml .fw-text strong {
  color: var(--ink);
  font-weight: 600;
  }

  .blog-fullhtml .article-footer {
  padding-top: 2rem;
  border-top: 1px solid var(--border);
  font-size: 0.85rem;
  color: var(--muted);
  text-align: center;
  max-width: 640px;
  }

  .blog-fullhtml .blog-container { background: var(--paper); }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }
  html[data-theme="dark"] .blog-fullhtml { --ink: #e8e0d8; --paper: #1a1817; --accent: #e8765e; --accent-light: #2a1f1c; --green: #7ac87a; --green-light: #1a2a1a; --muted: #a8a098; --border: #3a342f; --card-bg: #1f1d1c; }
  html[data-theme="dark"] .blog-fullhtml .hero { background: linear-gradient(165deg, #1a1817 0%, #2a1f1c 100%); }
  html[data-theme="dark"] .blog-fullhtml .story p { color: #c8c0b8; }
  html[data-theme="dark"] .blog-fullhtml .compare-col.bad { background: #2a1814; border-color: #5a3530; }
  html[data-theme="dark"] .blog-fullhtml .compare-col.good { background: #1a2a1a; border-color: #2a4a2a; }
  html[data-theme="dark"] .blog-fullhtml .compare-item { color: #b8b0a8; }
  html[data-theme="dark"] .blog-fullhtml .sb-danger { background: #2a2622; color: #6a6560; }
  html[data-theme="dark"] .blog-fullhtml .ft-people { background: #1a1f3a; color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .fw-text { color: #b8b0a8; }
  html[data-theme="dark"] .blog-fullhtml .cost-card:hover { box-shadow: 0 6px 20px rgba(0,0,0,0.25); }
---

<article class="blog-container">

<section class="hero">
  <div class="hero-inner">
    <div class="hero-tag">On Thinking in the Age of AI</div>
    <h1>How to Use AI Without Losing Your <em>Thinking Power</em></h1>
    <p class="hero-subtitle">I caught myself outsourcing my ability to reason — not just to code, but to think through everyday life. Here's the framework I built to take it back.</p>
  </div>
</section>

<div class="content">

  <!-- Story: Origin -->
  <div class="story">
    <div class="section-label">The Origin</div>
    <h2>It Started With Code</h2>
    <p>I spent months letting AI write my code. One day I stared at a problem I should've been able to solve, and my mind was blank. The muscle had atrophied.</p>
    <p>So I stopped. Went back to scratch. Rebuilt the skill. Found a balance: I think and architect, AI handles execution. It works.</p>
    <p>Then I noticed the same pattern creeping into everything else.</p>
  </div>

  <!-- Story: The Creep -->
  <div class="story">
    <h2>The Creep</h2>
    <p>Moving cities — I asked AI what to pack. Renting a car — I asked AI what to look out for. Planning trips — AI. Every new situation, my first instinct was no longer to think. It was to open a chat window.</p>
  </div>

  <div class="callout">The problem isn't using AI for information. It's that my first reaction to any unfamiliar situation is no longer to think — it's to ask.</div>

  <!-- VISUAL: Before/After -->
  <div class="viz">
    <div class="viz-title">What actually changed</div>
    <div class="compare">
      <div class="compare-col bad">
        <div class="compare-label">✕ What I was doing</div>
        <div class="compare-item"><div class="ci-icon">→</div><span>New situation arises</span></div>
        <div class="compare-item"><div class="ci-icon">→</div><span>Immediately open AI chat</span></div>
        <div class="compare-item"><div class="ci-icon">→</div><span>Follow AI's answer exactly</span></div>
        <div class="compare-item"><div class="ci-icon">→</div><span>Move on, learn nothing</span></div>
      </div>
      <div class="compare-col good">
        <div class="compare-label">✓ What works</div>
        <div class="compare-item"><div class="ci-icon">→</div><span>New situation arises</span></div>
        <div class="compare-item"><div class="ci-icon">→</div><span>Sit with it — think 5 mins</span></div>
        <div class="compare-item"><div class="ci-icon">→</div><span>Form a rough plan</span></div>
        <div class="compare-item"><div class="ci-icon">→</div><span>Use AI to catch blind spots</span></div>
      </div>
    </div>
  </div>

  <!-- VISUAL: Spectrum -->
  <div class="viz">
    <div class="viz-title">The AI Usage Spectrum</div>
    <div class="spectrum-wrap">
      <div class="spectrum-bar">
        <div class="sb-zone sb-you">You think<br>it through</div>
        <div class="sb-zone sb-sweet">You think,<br>AI fills gaps</div>
        <div class="sb-zone sb-danger">AI thinks,<br>you consume</div>
      </div>
      <div class="sb-labels">
        <span>← Full independence</span>
        <span>Full dependence →</span>
      </div>
      <div class="sb-sweet-tag">↑ Sweet spot</div>
    </div>
  </div>

  <!-- Pullquote -->
  <div class="pullquote">
    <span class="pq-line"></span>
    Every time you skip your own thinking, you're quietly telling yourself your judgment isn't trustworthy.
  </div>

  <!-- VISUAL: 4 Hidden Costs -->
  <div class="section-label">What's Actually Eroding</div>
  <div style="margin-bottom: 1.5rem;">
    <h2 style="font-family: 'Playfair Display', serif; font-size: 1.6rem; font-weight: 700;">The Hidden Costs</h2>
  </div>

  <div class="costs-grid">
    <div class="cost-card">
      <div class="cc-num">01</div>
      <h3>Confidence Erosion</h3>
      <p>Each skipped reasoning session is a quiet vote against your own competence. Over months, you second-guess yourself even in areas you're good at.</p>
    </div>
    <div class="cost-card">
      <div class="cc-num">02</div>
      <h3>Uncertainty Intolerance</h3>
      <p>"I don't know" is uncomfortable but productive — it's where resourcefulness lives. Instant AI answers kill that discomfort before it can do its work.</p>
    </div>
    <div class="cost-card">
      <div class="cc-num">03</div>
      <h3>Adjacent Knowledge & Network</h3>
      <p>Asking people teaches you unexpected things <em>and</em> builds relationships. That colleague doesn't just answer — they warn you about next week's problem. AI gives you a clean answer and zero human connection.</p>
    </div>
    <div class="cost-card">
      <div class="cc-num">04</div>
      <h3>Creativity Decline</h3>
      <p>Creativity needs deep understanding of the <em>why</em>. Without it, you're limited to solutions AI provides. You can't recombine knowledge you never acquired.</p>
    </div>
  </div>

  <!-- Story bridge — one short para -->
  <div class="story">
    <h2>The Line That Works</h2>
    <p>With coding, the balance was easy — I knew where thinking ended and execution began. For general life, the line is fuzzier. But it exists: <strong>reasoning versus information.</strong> Sitting with a new situation and asking "what could go wrong?" — that's reasoning. Looking up the insurance clause — that's information. Protect the first. Outsource the second.</p>
  </div>

  <!-- VISUAL: Decision Flow -->
  <div class="viz">
    <div class="viz-title">Before You Open the Chat</div>
    <div class="flow-wrap">
      <div class="flow-step">
        <div class="flow-diamond"><span>1</span></div>
        <div class="flow-q">Have I spent 5 minutes thinking about this myself?</div>
      </div>
      <div class="flow-branch">NO → <span class="flow-tag ft-think">Stop. Think first.</span></div>
      <div class="flow-line"></div>

      <div class="flow-step">
        <div class="flow-diamond"><span>2</span></div>
        <div class="flow-q">Am I asking because I'm lazy or because I lack facts?</div>
      </div>
      <div class="flow-branch">LAZY → <span class="flow-tag ft-think">Stop. Think first.</span></div>
      <div class="flow-line"></div>

      <div class="flow-step">
        <div class="flow-diamond"><span>3</span></div>
        <div class="flow-q">Could I ask a person and learn more from the conversation?</div>
      </div>
      <div class="flow-branch">YES → <span class="flow-tag ft-people">Talk to someone.</span></div>
      <div class="flow-line"></div>

      <div class="flow-final">
        <div class="flow-final-box">✓ Now use AI — for facts, blind spots, and edge cases</div>
      </div>
    </div>
  </div>

  <!-- Framework -->
  <div class="framework">
    <h3>The Framework: Think First, Then Check</h3>
    <div class="fw-step">
      <div class="fw-num">1</div>
      <div class="fw-text"><strong>Pause.</strong> Resist the reflex. Sit with the uncertainty for a few minutes.</div>
    </div>
    <div class="fw-step">
      <div class="fw-num">2</div>
      <div class="fw-text"><strong>Reason.</strong> Form a tentative plan. What matters? What could go wrong?</div>
    </div>
    <div class="fw-step">
      <div class="fw-num">3</div>
      <div class="fw-text"><strong>Ask people.</strong> The adjacent knowledge and the relationship are often more valuable than the direct answer.</div>
    </div>
    <div class="fw-step">
      <div class="fw-num">4</div>
      <div class="fw-text"><strong>Then use AI.</strong> For facts, checklists, edge cases. Let it fill gaps in your thinking, not replace it.</div>
    </div>
  </div>

  <!-- Closing — tight -->
  <div class="story">
    <p>Treat AI the way you'd treat a knowledgeable friend. You wouldn't call a friend before you'd even thought about the problem yourself. You'd think, form a plan, then ask: "am I missing anything?"</p>
    <p>That's the balance. It doesn't require domain expertise — just the habit of pausing before you reach for the chat.</p>
  </div>

  <div class="article-footer">
    <p>Written from experience — including the part where I asked AI to help me write this, after thinking about it myself first.</p>
  </div>

</div>

</article>
