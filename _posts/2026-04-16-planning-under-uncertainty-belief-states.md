---
layout: fullhtml-post
title: "Planning When You Can't See the Whole World"
date: 2026-04-16
categories: ["Planning under Uncertainty"]
tags: ["planning", "pomdp", "belief-states"]
description: "Classical planning assumes you know everything. Real agents almost never do. Belief states give us a principled way to plan under uncertainty — and an extraordinary computational cost in the bargain. Part 1 of the Planning Under Uncertainty series."
_styles: >
  .blog-fullhtml {
  font-family: 'Charter', 'Georgia', serif;
  line-height: 1.75;
  color: #1a1a1a;
  font-size: 18px;
  }
  .blog-fullhtml h1 { font-size: 2em; line-height: 1.2; margin-top: 1.2em; }
  .blog-fullhtml h2 { font-size: 1.5em; margin-top: 2em; color: #222; }
  .blog-fullhtml h3 { font-size: 1.2em; margin-top: 1.5em; }
  .blog-fullhtml .subtitle { font-size: 1.05em; color: #555; font-style: italic; margin-top: -0.5em; }
  .blog-fullhtml hr { border: none; border-top: 1px solid #ddd; margin: 2em 0; }
  .blog-fullhtml code { background: #f4f4f4; padding: 2px 5px; border-radius: 3px; font-size: 0.9em; }
  .blog-fullhtml em { color: #333; }

  .blog-fullhtml .series-nav {
  background: #ecedfa; border-left: 4px solid #5b6abf; border-radius: 0 8px 8px 0;
  padding: 14px 18px; margin: 0 0 24px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; font-size: 0.92em;
  }
  .blog-fullhtml .series-nav strong { color: #3d4a9e; }
  .blog-fullhtml .series-nav .series-nav-links { margin-top: 6px; font-size: 0.85em; color: #555; }
  .blog-fullhtml .series-nav a { color: #3d4a9e; text-decoration: none; border-bottom: 1px solid #5b6abf55; }
  .blog-fullhtml .series-nav a:hover { color: #28327a; }

  .blog-fullhtml .series-footer {
  margin: 3em 0 2em; padding: 20px 22px;
  background: #f5f6fc; border: 1px solid #d4d8ee; border-radius: 10px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .series-footer strong { color: #3d4a9e; }
  .blog-fullhtml .series-footer p { font-size: 0.9em; color: #444; margin: 8px 0 0; line-height: 1.6; }
  .blog-fullhtml .series-footer a { color: #3d4a9e; text-decoration: none; border-bottom: 1px solid #5b6abf55; }

  .blog-fullhtml .vis-container {
  margin: 28px -40px; padding: 24px 26px 16px;
  background: #fff; border: 1px solid #E8E4ED;
  border-radius: 14px; box-shadow: 0 4px 18px rgba(45,32,68,0.06);
  font-family: 'Source Sans 3', -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  @media (max-width: 800px) { .blog-fullhtml .vis-container { margin: 24px 0; } }
  .blog-fullhtml .vis-title { font-family: 'Playfair Display', Georgia, serif; font-size: 1.25rem; color: #2D2044; font-weight: 700; margin: 0; }
  .blog-fullhtml .vis-title::after { content:''; display:block; width:60px; height:3px; background:#C5A55A; margin-top:7px; border-radius:2px; }
  .blog-fullhtml .vis-subtitle { color: #888; font-size: 0.92em; margin-top: 6px; margin-bottom: 18px; font-style: italic; }

  .blog-fullhtml .cmp { display: flex; gap: 14px; align-items: stretch; }
  @media (max-width: 700px) { .blog-fullhtml .cmp { flex-direction: column; } }
  .blog-fullhtml .sd { flex: 1; display: flex; flex-direction: column; border-radius: 10px; overflow: hidden; border: 1.5px solid #E8E4ED; }
  .blog-fullhtml .sd-w { border-color: rgba(230,81,0,.3); }
  .blog-fullhtml .sd-d { border-color: rgba(192,57,43,.3); }
  .blog-fullhtml .sd-d.glow { box-shadow: 0 0 20px 4px rgba(192,57,43,.1); }
  .blog-fullhtml .sh { padding: 9px 14px 5px; display: flex; align-items: center; gap: 8px; }
  .blog-fullhtml .sh h3 { font-family: 'Playfair Display', serif; font-size: 1.05rem; color: #2D2044; font-weight: 700; margin: 0; }
  .blog-fullhtml .bg { font-size: .58rem; font-weight: 700; text-transform: uppercase; letter-spacing: 1.5px; padding: 2px 8px; border-radius: 20px; }
  .blog-fullhtml .bg-w { background: #FFF3E0; color: #E65100; }
  .blog-fullhtml .bg-d { background: #FDECEA; color: #C0392B; }
  .blog-fullhtml .sv { display: flex; align-items: center; justify-content: center; padding: 4px 8px; min-height: 200px; }
  .blog-fullhtml .sv svg { width: 100%; height: auto; max-height: 280px; }
  .blog-fullhtml .ss { padding: 8px 12px 10px; background: #F8F6FA; border-top: 1px solid #E8E4ED; display: flex; gap: 6px; }
  .blog-fullhtml .st { flex: 1; text-align: center; padding: 5px 2px; background: #fff; border-radius: 5px; border: 1px solid #E8E4ED; }
  .blog-fullhtml .st .n { font-family: 'JetBrains Mono', monospace; font-size: 1.05rem; font-weight: 700; }
  .blog-fullhtml .st .l { font-size: .65rem; color: #888; margin-top: 2px; }
  .blog-fullhtml .st.ex { border-color: rgba(192,57,43,.3); background: #FDECEA; }
  .blog-fullhtml .st.ex .n { color: #C0392B; }
  .blog-fullhtml .st.w { border-color: rgba(230,81,0,.3); background: #FFF3E0; }
  .blog-fullhtml .st.w .n { color: #E65100; }

  .blog-fullhtml .vs { display: flex; flex-direction: column; align-items: center; justify-content: center; width: 36px; flex-shrink: 0; }
  @media (max-width: 700px) { .blog-fullhtml .vs { width: auto; flex-direction: row; padding: 4px 0; } }
  .blog-fullhtml .vsb { width: 28px; height: 28px; border-radius: 50%; background: #2D2044; display: flex; align-items: center; justify-content: center; color: #C5A55A; font-family: 'Playfair Display', serif; font-weight: 700; font-size: .7rem; }
  .blog-fullhtml .vsb.po { background: linear-gradient(135deg, #2D2044, #C0392B); color: #fff; }

  .blog-fullhtml .vis-banner { margin-top: 12px; padding: 10px 16px; background: #2D2044; border-radius: 8px; }
  .blog-fullhtml .vis-banner p { font-size: 0.95em; color: rgba(255,255,255,.88); line-height: 1.5; margin: 0; }
  .blog-fullhtml .vis-banner strong { color: #C5A55A; }

  .blog-fullhtml .blog-container { max-width: 760px; margin: 40px auto; padding: 0 20px; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }

  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml h1 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml em { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml .subtitle { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml hr { border-top-color: #2a3045; }
  html[data-theme="dark"] .blog-fullhtml code { background: #2c3237; color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml a:hover { color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml .series-nav { background: #1a1f3a; border-left-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .series-nav strong { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .series-nav-links { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml .series-nav a:hover { color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml .series-footer { background: #15192e; border-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .series-footer strong { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .series-footer p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-footer a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml .vis-container { background: #1e1a30; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .vis-title { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .vis-subtitle { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .sd { border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .sd-w { border-color: rgba(232,160,64,.4); }
  html[data-theme="dark"] .blog-fullhtml .sd-d { border-color: rgba(224,96,96,.5); }
  html[data-theme="dark"] .blog-fullhtml .sh h3 { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .bg-w { background: #2a2010; color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .bg-d { background: #2a1414; color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .ss { background: #15101e; border-top-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .st { background: #1a1530; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .st .l { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .st.ex { background: #2a1414; border-color: #5a3030; }
  html[data-theme="dark"] .blog-fullhtml .st.ex .n { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .st.w { background: #2a2010; border-color: #5a4020; }
  html[data-theme="dark"] .blog-fullhtml .st.w .n { color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .vsb { background: #2a2540; color: #C5A55A; }
  html[data-theme="dark"] .blog-fullhtml .vsb.po { background: linear-gradient(135deg, #2a2540, #2a1414); color: #fff; }
  html[data-theme="dark"] .blog-fullhtml .vis-banner { background: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .vis-banner p { color: rgba(255,255,255,.88); }
  html[data-theme="dark"] .blog-fullhtml .vis-banner strong { color: #C5A55A; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #9888a8; border-top-color: #2a3045; }
---

<article class="blog-container">

<div class="series-nav">
    <strong>Planning Under Uncertainty &middot; Part 1 of 4 &mdash; the setup</strong>
    <div class="series-nav-links">
        <a href="/blog/2026/learning-for-planning-overview/">Overview</a> &middot; Next: <a href="/blog/2026/planning-under-uncertainty-mcts-for-pomdps/">Part 2: MCTS for POMDPs &rarr;</a>
    </div>
</div>

<h1>Planning When You Can't See the Whole World</h1>
<p class="subtitle">Classical planning assumes you know everything. Real agents almost never do. Belief states give us a principled way to plan under uncertainty &mdash; and an extraordinary computational cost in the bargain.</p>

<hr>

<h2>Where the assumption breaks</h2>

<p>The sibling series &mdash; <a href="/blog/2026/learning-for-planning-scaling-problem/">Learning for Planning</a> &mdash; opened with the same warehouse we'll use here: zones A, B, C, D, a robot, a package. There, the robot knew exactly where every package was. The hard part was the search space, not the perception.</p>

<p>Now change one thing. The robot can only see what's <em>in its current zone</em>. It can't see into the other three. If it walks into Zone C and the package is sitting on the shelf, it perceives that. If the package isn't there, it just sees an empty shelf &mdash; and learns one fact about the world, but not where the package actually is.</p>

<p>This is partial observability. The state of the world is what it is &mdash; the package has some real position. But the agent doesn't know it. It only has a probability distribution over possible positions, updated each time it observes something. That distribution is the agent's <em>belief</em>, and it's the only handle on the world the agent can plan against.</p>

<div class="vis-container" id="vis1">
    <h3 class="vis-title">Warehouse Delivery &mdash; now with fog</h3>
    <div class="vis-subtitle">Same 16-zone, 3-package warehouse from the LFP series. The right card adds one change: the robot can only see its current zone.</div>

    <div class="cmp">
        <!-- LEFT: Fully observable, already hard -->
        <div class="sd sd-w">
            <div class="sh"><span class="bg bg-w">Already Hard</span><h3>Fully Observable &middot; 16 Zones &middot; 3 Pkgs</h3></div>
            <div class="sv">
                <svg viewBox="0 0 257 220" preserveAspectRatio="xMidYMid meet">
                    <defs><pattern id="tlf" width="8" height="8" patternUnits="userSpaceOnUse"><rect width="8" height="8" fill="#F0EDF3"/><line x1="0" y1="8" x2="8" y2="8" stroke="#D4CDE0" stroke-width=".1" opacity=".15"/><line x1="8" y1="0" x2="8" y2="8" stroke="#D4CDE0" stroke-width=".1" opacity=".15"/></pattern></defs>
                    <rect x="2" y="2" width="253" height="181" rx="4" fill="none" stroke="#D4CDE0" stroke-width=".6"/>
                    <rect x="4" y="4" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="34" y="30" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">A</text>
                    <rect x="67" y="4" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="97" y="30" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">B</text>
                    <rect x="130" y="4" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="160" y="30" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">C</text>
                    <rect x="193" y="4" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="223" y="30" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">D</text>
                    <rect x="4" y="49" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="34" y="75" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">E</text>
                    <rect x="67" y="49" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="97" y="75" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">F</text>
                    <rect x="130" y="49" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="160" y="75" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">G</text>
                    <rect x="193" y="49" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="223" y="75" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">H</text>
                    <rect x="4" y="94" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="34" y="120" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">I</text>
                    <rect x="67" y="94" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="97" y="120" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">J</text>
                    <rect x="130" y="94" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="160" y="120" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">K</text>
                    <rect x="193" y="94" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="223" y="120" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">L</text>
                    <rect x="4" y="139" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="34" y="165" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">M</text>
                    <rect x="67" y="139" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="97" y="165" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">N</text>
                    <rect x="130" y="139" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="160" y="165" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">O</text>
                    <rect x="193" y="139" width="60" height="42" rx="3" fill="url(#tlf)" stroke="#D4CDE0" stroke-width=".4"/><text x="223" y="165" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">P</text>
                    <g transform="translate(34,18)"><rect x="-4.5" y="-7" width="9" height="5.5" rx="1.5" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/><rect x="-5.2" y="-1.5" width="10.4" height="8" rx="1.1" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/></g>
                    <rect x="215" y="8" width="16" height="11" rx="2" fill="#D68910" stroke="#B7770A" stroke-width=".5"/><text x="223" y="16" text-anchor="middle" fill="#fff" font-size="6" font-weight="700">PKG</text>
                    <rect x="89" y="98" width="16" height="11" rx="2" fill="#D68910" stroke="#B7770A" stroke-width=".5"/><text x="97" y="106" text-anchor="middle" fill="#fff" font-size="6" font-weight="700">PKG</text>
                    <rect x="152" y="143" width="16" height="11" rx="2" fill="#D68910" stroke="#B7770A" stroke-width=".5"/><text x="160" y="151" text-anchor="middle" fill="#fff" font-size="6" font-weight="700">PKG</text>
                    <rect x="26" y="53" width="16" height="11" rx="2" fill="#E3F5EC" stroke="#1E8449" stroke-width="1" stroke-dasharray="3 2" opacity=".5"/>
                    <rect x="215" y="143" width="16" height="11" rx="2" fill="#E3F5EC" stroke="#1E8449" stroke-width="1" stroke-dasharray="3 2" opacity=".5"/>
                    <rect x="152" y="8" width="16" height="11" rx="2" fill="#E3F5EC" stroke="#1E8449" stroke-width="1" stroke-dasharray="3 2" opacity=".5"/>

                    <text x="128" y="200" text-anchor="middle" fill="#E65100" font-size="9" font-weight="700">Search over states</text>
                </svg>
            </div>
            <div class="ss">
                <div class="st w"><div class="n">~1.2M</div><div class="l">Reachable states</div></div>
                <div class="st w"><div class="n">12+</div><div class="l">Plan length</div></div>
                <div class="st w"><div class="n">Hours</div><div class="l">to timeout</div></div>
            </div>
        </div>

        <div class="vs"><div class="vsb po">PO</div></div>

        <!-- RIGHT: Partially observable, intractable -->
        <div class="sd sd-d" id="vis1-po">
            <div class="sh"><span class="bg bg-d">Belief Explosion</span><h3>Partially Observable &middot; same layout</h3></div>
            <div class="sv">
                <svg viewBox="0 0 257 220" preserveAspectRatio="xMidYMid meet">
                    <defs>
                        <pattern id="tlp" width="8" height="8" patternUnits="userSpaceOnUse"><rect width="8" height="8" fill="#F0EDF3"/><line x1="0" y1="8" x2="8" y2="8" stroke="#D4CDE0" stroke-width=".1" opacity=".15"/><line x1="8" y1="0" x2="8" y2="8" stroke="#D4CDE0" stroke-width=".1" opacity=".15"/></pattern>
                        <radialGradient id="fogG" cx="50%" cy="50%" r="55%">
                            <stop offset="0%" stop-color="#2E1A38" stop-opacity=".35"/>
                            <stop offset="100%" stop-color="#2E1A38" stop-opacity=".7"/>
                        </radialGradient>
                    </defs>
                    <rect x="2" y="2" width="253" height="181" rx="4" fill="none" stroke="#D4CDE0" stroke-width=".6"/>
                    <!-- 16 zones (base) -->
                    <rect x="4" y="4" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/><text x="34" y="30" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">A</text>
                    <rect x="67" y="4" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="130" y="4" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="193" y="4" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="4" y="49" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="67" y="49" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="130" y="49" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="193" y="49" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="4" y="94" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="67" y="94" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="130" y="94" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="193" y="94" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="4" y="139" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="67" y="139" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="130" y="139" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>
                    <rect x="193" y="139" width="60" height="42" rx="3" fill="url(#tlp)" stroke="#D4CDE0" stroke-width=".4"/>

                    <!-- Robot in A (visible) -->
                    <g transform="translate(34,22)"><rect x="-4.5" y="-7" width="9" height="5.5" rx="1.5" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/><rect x="-5.2" y="-1.5" width="10.4" height="8" rx="1.1" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/></g>

                    <!-- Fog over zones B-P (everything except A where robot is) -->
                    <rect x="67" y="4" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="130" y="4" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="193" y="4" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="4" y="49" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="67" y="49" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="130" y="49" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="193" y="49" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="4" y="94" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="67" y="94" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="130" y="94" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="193" y="94" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="4" y="139" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="67" y="139" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="130" y="139" width="60" height="42" rx="3" fill="url(#fogG)"/>
                    <rect x="193" y="139" width="60" height="42" rx="3" fill="url(#fogG)"/>

                    <!-- Question marks scattered on fogged zones -->
                    <text x="97" y="32" text-anchor="middle" fill="#E6E0EC" font-size="14" font-weight="700" opacity=".55">?</text>
                    <text x="160" y="77" text-anchor="middle" fill="#E6E0EC" font-size="14" font-weight="700" opacity=".55">?</text>
                    <text x="223" y="122" text-anchor="middle" fill="#E6E0EC" font-size="14" font-weight="700" opacity=".55">?</text>
                    <text x="34" y="167" text-anchor="middle" fill="#E6E0EC" font-size="14" font-weight="700" opacity=".55">?</text>

                    <text x="128" y="200" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700">Search over beliefs</text>
                </svg>
            </div>
            <div class="ss">
                <div class="st ex"><div class="n" id="vis1-belief-counter">0</div><div class="l">Belief states</div></div>
                <div class="st ex"><div class="n">&infin;</div><div class="l">Obs. branching</div></div>
                <div class="st ex"><div class="n">&#x2717;</div><div class="l">Timeout</div></div>
            </div>
        </div>
    </div>

    <div class="vis-banner">
        <p><strong>Same problem configuration.</strong> The fully observable version was already exponential. Adding partial observability multiplies branching by observations, sending the effective state count from ~1.2M to <strong>uncountable</strong>. Belief space is continuous and high-dimensional; exact POMDP solving is PSPACE-hard even before approximation.</p>
    </div>
</div>

<script>
(function(){
    var animated = false;
    var counter = document.getElementById('vis1-belief-counter');
    var poCard = document.getElementById('vis1-po');
    if (!counter || !poCard) return;
    var stages = ['1K', '50K', '2M', '100M', '5B', '800B', '10¹²', '10¹⁵+'];
    function runCounter(){
        if (animated) return; animated = true;
        poCard.classList.add('glow');
        var duration = 2800, stageDuration = duration / stages.length, start = null;
        function tick(ts){
            if (!start) start = ts;
            var elapsed = ts - start;
            var i = Math.min(Math.floor(elapsed / stageDuration), stages.length - 1);
            counter.textContent = stages[i];
            if (i < stages.length - 1) requestAnimationFrame(tick);
        }
        requestAnimationFrame(tick);
    }
    if ('IntersectionObserver' in window) {
        var io = new IntersectionObserver(function(entries){
            entries.forEach(function(e){ if (e.isIntersecting) { runCounter(); io.disconnect(); } });
        }, {threshold: 0.4});
        io.observe(document.getElementById('vis1'));
    } else {
        setTimeout(runCounter, 700);
    }
})();
</script>

<h2>From state to belief, formally</h2>

<p>The right card above isn't just visual flourish &mdash; that counter is real. Each action the robot takes produces an observation (what it sees in its new zone), and each observation could be any of several possibilities, each updating the belief differently. The agent has to plan against all of them. Branching factors compound.</p>

<p>A POMDP &mdash; <em>partially observable Markov decision process</em> &mdash; formalizes this with a seven-tuple <code>&lang;S, A, T, R, Z, O, &gamma;&rang;</code>: states, actions, transition function, reward, observations, observation function, discount. The agent never gets to see <em>s</em>; it only gets observations <em>z</em> drawn from <code>O(s', a, z)</code> after acting.</p>

<p>The agent's belief <code>b(s)</code> &mdash; its probability distribution over the true state &mdash; updates by Bayes' rule:</p>

<blockquote style="background:#F8F6FA;border-left:3px solid #5b6abf;padding:12px 16px;font-family:'JetBrains Mono',monospace;font-size:0.92em;">
b'(s') = &eta; &middot; O(s', a, z) &middot; &Sigma;<sub>s</sub> T(s, a, s') &middot; b(s)
</blockquote>

<p>That is: take the prior belief, push it forward through the transition model, weight by how likely each successor would have produced the observation we got, then normalize. Mechanically simple, computationally brutal. The Bellman equation now ranges over belief states &mdash; an uncountable, infinite-dimensional space &mdash; rather than the finite state space we had in classical planning.</p>

<h2>Two strategies for getting past the wall</h2>

<p>The rest of this series mirrors the LFP series structure, but for the partially observable case. Same two strategies:</p>

<ul>
    <li><strong>Make the problem smaller.</strong> If the agent abstracts away most of the state &mdash; objects, zones, package identities that don't matter for the current decision &mdash; a principled POMDP solver can handle the abstraction. That's <a href="/blog/2026/planning-under-uncertainty-hoo-pomdp/">Part 3, HOO-POMDP</a>.</li>
    <li><strong>Learn to plan in belief space.</strong> Frame the belief as a graph; train a GNN to score actions over that graph; transfer to larger instances. That's <a href="/blog/2026/planning-under-uncertainty-gammazero/">Part 4, GammaZero</a> &mdash; the partially observable cousin of GABAR from the LFP series.</li>
</ul>

<p>Part 2 covers what the community built in between &mdash; online tree search methods like POMCP and DESPOT, which made POMDPs tractable in practice but pushed the difficulty into heuristic ingredients (rollout policies, value estimates). That heuristic gap is exactly what Parts 3 and 4 close, from two different angles.</p>

<p style="font-size: 0.92em; color: #666; font-style: italic; margin-top: 28px;">
If PDDL and classical planning are new to you, the <a href="/blog/category/llms-automated-planning-and-agents/">Planning in the Era of LLMs</a> series covers that background. This series picks up where classical planning ends &mdash; when the assumption of a known state no longer holds.
</p>

<hr>

<div class="series-footer">
    <strong>Where this fits</strong>
    <p>This is the on-ramp. The next post covers POMCP, DESPOT, and the online tree-search family that <a href="/blog/2026/planning-under-uncertainty-hoo-pomdp/">Part 3 (HOO-POMDP)</a> and <a href="/blog/2026/planning-under-uncertainty-gammazero/">Part 4 (GammaZero)</a> both build on or replace.</p>
    <p style="margin-top: 10px; font-size: 0.85em; color: #666;"><a href="/blog/2026/learning-for-planning-overview/">Overview</a> &middot; <a href="/blog/2026/planning-under-uncertainty-mcts-for-pomdps/">Part 2: MCTS for POMDPs &rarr;</a></p>
</div>

</article>
