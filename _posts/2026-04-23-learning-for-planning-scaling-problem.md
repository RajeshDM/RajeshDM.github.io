---
layout: fullhtml-post
title: "The Scaling Problem"
date: 2026-04-23
categories: ["Learning for Planning"]
tags: ["planning", "classical-planning", "learning"]
description: "Planning is sound, complete, and optimal. It is also exponentially hard. Learning is how we get past that wall — but to see why it works, we first need to feel the wall. Part 1 of the Learning for Planning series."
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
  background: #e0f5f5; border-left: 4px solid #00A3A1; border-radius: 0 8px 8px 0;
  padding: 14px 18px; margin: 0 0 24px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; font-size: 0.92em;
  }
  .blog-fullhtml .series-nav strong { color: #00807E; }
  .blog-fullhtml .series-nav .series-nav-links { margin-top: 6px; font-size: 0.85em; color: #555; }
  .blog-fullhtml .series-nav a { color: #00807E; text-decoration: none; border-bottom: 1px solid #00A3A155; }
  .blog-fullhtml .series-nav a:hover { color: #005856; border-bottom-color: #005856; }

  .blog-fullhtml .series-footer {
  margin: 3em 0 2em; padding: 20px 22px;
  background: #f0fafa; border: 1px solid #c2e4e3; border-radius: 10px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .series-footer strong { color: #00807E; }
  .blog-fullhtml .series-footer p { font-size: 0.9em; color: #444; margin: 8px 0 0; line-height: 1.6; }
  .blog-fullhtml .series-footer a { color: #00807E; text-decoration: none; border-bottom: 1px solid #00A3A155; }
  .blog-fullhtml .series-footer a:hover { color: #005856; }

  .blog-fullhtml .vis-container {
  margin: 28px -40px; padding: 24px 26px 16px;
  background: #fff; border: 1px solid #E8E4ED;
  border-radius: 14px; box-shadow: 0 4px 18px rgba(45,32,68,0.06);
  font-family: 'Source Sans 3', -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  @media (max-width: 800px) { .blog-fullhtml .vis-container { margin: 24px 0; } }
  .blog-fullhtml .vis-title {
  font-family: 'Playfair Display', 'Charter', Georgia, serif;
  font-size: 1.25rem; color: #2D2044; font-weight: 700; margin: 0;
  }
  .blog-fullhtml .vis-title::after { content:''; display:block; width:60px; height:3px; background:#C5A55A; margin-top:7px; border-radius:2px; }
  .blog-fullhtml .vis-subtitle { color: #888; font-size: 0.92em; margin-top: 6px; margin-bottom: 18px; font-style: italic; }

  .blog-fullhtml .cmp { display: flex; gap: 14px; align-items: stretch; }
  @media (max-width: 700px) { .blog-fullhtml .cmp { flex-direction: column; } }
  .blog-fullhtml .sd { flex: 1; display: flex; flex-direction: column; border-radius: 10px; overflow: hidden; border: 1.5px solid #E8E4ED; }
  .blog-fullhtml .sd-d { border-color: rgba(192,57,43,.25); }
  .blog-fullhtml .sd-d.glow { box-shadow: 0 0 18px 3px rgba(192,57,43,.08); }
  .blog-fullhtml .sh { padding: 9px 14px 5px; display: flex; align-items: center; gap: 8px; }
  .blog-fullhtml .sh h3 { font-family: 'Playfair Display', serif; font-size: 1.05rem; color: #2D2044; font-weight: 700; margin: 0; }
  .blog-fullhtml .bg { font-size: .58rem; font-weight: 700; text-transform: uppercase; letter-spacing: 1.5px; padding: 2px 8px; border-radius: 20px; }
  .blog-fullhtml .bg-ok { background: #E3F5EC; color: #1E8449; }
  .blog-fullhtml .bg-d { background: #FDECEA; color: #C0392B; }
  .blog-fullhtml .sv { display: flex; align-items: center; justify-content: center; padding: 4px 8px; min-height: 200px; }
  .blog-fullhtml .sv svg { width: 100%; height: auto; max-height: 260px; }
  .blog-fullhtml .ss { padding: 8px 12px 10px; background: #F8F6FA; border-top: 1px solid #E8E4ED; display: flex; gap: 6px; }
  .blog-fullhtml .st { flex: 1; text-align: center; padding: 5px 2px; background: #fff; border-radius: 5px; border: 1px solid #E8E4ED; }
  .blog-fullhtml .st .n { font-family: 'JetBrains Mono', monospace; font-size: 1.05rem; font-weight: 700; color: #2D2044; }
  .blog-fullhtml .st .l { font-size: .65rem; color: #888; margin-top: 2px; }
  .blog-fullhtml .st.ex { border-color: rgba(192,57,43,.3); background: #FDECEA; }
  .blog-fullhtml .st.ex .n { color: #C0392B; }

  .blog-fullhtml .vs { display: flex; flex-direction: column; align-items: center; justify-content: center; width: 36px; flex-shrink: 0; }
  @media (max-width: 700px) { .blog-fullhtml .vs { width: auto; flex-direction: row; padding: 4px 0; } }
  .blog-fullhtml .vsb { width: 28px; height: 28px; border-radius: 50%; background: #2D2044; display: flex; align-items: center; justify-content: center; color: #C5A55A; font-family: 'Playfair Display', serif; font-weight: 700; font-size: .72rem; }

  .blog-fullhtml .vis-banner {
  margin-top: 12px; padding: 10px 16px;
  background: #F8F6FA; border-radius: 8px; border-left: 4px solid #C5A55A;
  }
  .blog-fullhtml .vis-banner p { font-size: 0.95em; color: #2D2044; line-height: 1.45; margin: 0; }
  .blog-fullhtml .vis-banner strong { color: #C0392B; }

  .blog-fullhtml .blog-container { max-width: 760px; margin: 40px auto; padding: 0 20px; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }
  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml h1 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml a { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml a:hover { color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .subtitle { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .lead { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml hr { border-top-color: #2a3a3a; }
  html[data-theme="dark"] .blog-fullhtml code { background: #2c3237; color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml em { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav { background: #0f2625; border-left-color: #1e4a48; }
  html[data-theme="dark"] .blog-fullhtml .series-nav strong { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .series-nav-links { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .nav-links { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a { color: #4dd0ce; border-bottom-color: #1e4a48; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a:hover { color: #80e0de; border-bottom-color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .series-footer { background: #142a2a; border-color: #1e4a48; }
  html[data-theme="dark"] .blog-fullhtml .series-footer strong { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .series-footer p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-footer a { color: #4dd0ce; border-bottom-color: #1e4a48; }
  html[data-theme="dark"] .blog-fullhtml .series-footer a:hover { color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .vis-container { background: #1e1a30; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .vis-title { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .vis-subtitle { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .vis-subtitle a { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .sd { border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .sd-d { border-color: rgba(224,96,96,.5); }
  html[data-theme="dark"] .blog-fullhtml .sh h3 { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .bg-ok { background: #1a2a1a; color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .bg-d { background: #2a1414; color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .ss { background: #15101e; border-top-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .st { background: #1a1530; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .st .n { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .st .l { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .st.ex { background: #2a1414; border-color: #5a3030; }
  html[data-theme="dark"] .blog-fullhtml .st.ex .n { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .vsb { background: #2a2540; color: #C5A55A; }
  html[data-theme="dark"] .blog-fullhtml .vis-banner { background: #15101e; }
  html[data-theme="dark"] .blog-fullhtml .vis-banner p { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .vis-banner strong { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #6a7888; border-top-color: #2a3545; }
---

<article class="blog-container">

<div class="series-nav">
    <strong>Learning for Planning · Part 1 of 4 — the problem</strong>
    <div class="series-nav-links">
        <a href="/blog/2026/learning-for-planning-overview/">Overview</a> · Next: <a href="/blog/2026/learning-for-planning-what-to-learn/">Part 2: What to Learn — Objectives Survey →</a>
    </div>
</div>

<h1>The Scaling Problem</h1>
<p class="subtitle">Planning is sound, complete, and optimal. It is also exponentially hard. Learning is how we get past that wall — but to see why it works, we first need to feel the wall.</p>

<hr>

<h2>A simple-looking task</h2>

<p>Consider a tiny warehouse. Four zones — A, B, C, D — arranged in a grid. A robot starts in zone A. A package sits in zone C. We want the package delivered to zone D. The robot can <code>move</code> between zones, <code>pickup</code> a package in the zone it's in, and <code>drop</code> the package wherever it stops.</p>

<p>This is a classical planning problem. Three action types, a handful of objects, four locations. A planner like A* with a good heuristic — or even a domain-independent system like Fast-Downward — solves it in milliseconds. The plan has three steps: <code>move(A→C)</code>, <code>pickup(pkg, C)</code>, <code>move(C→D)</code>. Done.</p>

<p>Now scale it up. Same actions, same rules. Just more zones and more packages.</p>

<div class="vis-container" id="vis1">
    <h3 class="vis-title">Warehouse Delivery — small vs. large</h3>
    <div class="vis-subtitle">Same domain, same actions. Only the number of zones and packages changes.</div>

    <div class="cmp">
        <!-- LEFT: Tractable -->
        <div class="sd">
            <div class="sh"><span class="bg bg-ok">Tractable</span><h3>4 Zones · 1 Package</h3></div>
            <div class="sv">
                <svg viewBox="0 0 260 210" preserveAspectRatio="xMidYMid meet">
                    <defs><pattern id="t1a" width="13" height="13" patternUnits="userSpaceOnUse"><rect width="13" height="13" fill="#F0EDF3"/><line x1="0" y1="13" x2="13" y2="13" stroke="#D4CDE0" stroke-width=".15" opacity=".25"/><line x1="13" y1="0" x2="13" y2="13" stroke="#D4CDE0" stroke-width=".15" opacity=".25"/></pattern></defs>
                    <rect x="10" y="4" width="240" height="96" rx="5" fill="none" stroke="#D4CDE0" stroke-width=".8"/>
                    <rect x="12" y="6" width="116" height="44" rx="3" fill="url(#t1a)" stroke="#D4CDE0" stroke-width=".3"/><text x="70" y="36" text-anchor="middle" fill="#B0A8C0" font-size="13" font-weight="600">Zone A</text>
                    <rect x="132" y="6" width="116" height="44" rx="3" fill="url(#t1a)" stroke="#D4CDE0" stroke-width=".3"/><text x="190" y="36" text-anchor="middle" fill="#B0A8C0" font-size="13" font-weight="600">Zone B</text>
                    <rect x="12" y="54" width="116" height="44" rx="3" fill="url(#t1a)" stroke="#D4CDE0" stroke-width=".3"/><text x="70" y="84" text-anchor="middle" fill="#B0A8C0" font-size="13" font-weight="600">Zone C</text>
                    <rect x="132" y="54" width="116" height="44" rx="3" fill="url(#t1a)" stroke="#D4CDE0" stroke-width=".3"/><text x="190" y="84" text-anchor="middle" fill="#B0A8C0" font-size="13" font-weight="600">Zone D</text>
                    <g transform="translate(70,24)"><line x1="0" y1="-11.8" x2="0" y2="-9.1" stroke="#4A3360" stroke-width="0.7"/><circle cx="0" cy="-12.7" r="1.1" fill="#4A3360"/><rect x="-5.5" y="-9.1" width="10.9" height="6.4" rx="1.8" fill="#7B5E99" stroke="#4A3360" stroke-width=".6"/><circle cx="-1.8" cy="-5.9" r="1.1" fill="#E6E0EC"/><circle cx="1.8" cy="-5.9" r="1.1" fill="#E6E0EC"/><rect x="-6.4" y="-2.7" width="12.7" height="10" rx="1.4" fill="#7B5E99" stroke="#4A3360" stroke-width=".6"/></g>
                    <rect x="62" y="61" width="16" height="12" rx="2" fill="#D68910" stroke="#B7770A" stroke-width=".6"/><text x="70" y="70" text-anchor="middle" fill="#fff" font-size="6" font-weight="700">PKG</text>
                    <rect x="182" y="61" width="16" height="12" rx="2" fill="#E3F5EC" stroke="#1E8449" stroke-width="1" stroke-dasharray="3 2" opacity=".55"/><text x="190" y="69" text-anchor="middle" fill="#1E8449" font-size="5" font-weight="700" opacity=".7">GOAL</text>

                    <g transform="translate(130,112)">
                        <line x1="0" y1="6" x2="-48" y2="30" stroke="#C5A55A" stroke-width="2.5" opacity=".5"/>
                        <line x1="0" y1="6" x2="0" y2="30" stroke="#D4CDE0" stroke-width="1.2"/>
                        <line x1="0" y1="6" x2="48" y2="30" stroke="#D4CDE0" stroke-width="1.2"/>
                        <line x1="-48" y1="36" x2="-65" y2="56" stroke="#D4CDE0" stroke-width="1"/>
                        <line x1="-48" y1="36" x2="-31" y2="56" stroke="#C5A55A" stroke-width="2.5" opacity=".5"/>
                        <line x1="-31" y1="62" x2="-31" y2="80" stroke="#C5A55A" stroke-width="2.5" opacity=".5"/>
                        <circle cx="0" cy="4" r="9" fill="#fff" stroke="#2D2044" stroke-width="1.2"/><text x="0" y="7.5" text-anchor="middle" fill="#2D2044" font-size="7.5" font-weight="700">s&#x2080;</text>
                        <circle cx="-48" cy="33" r="7.5" fill="#fff" stroke="#2D2044" stroke-width="1"/>
                        <circle cx="0" cy="33" r="7.5" fill="#fff" stroke="#D4CDE0" stroke-width=".8" opacity=".4"/>
                        <circle cx="48" cy="33" r="7.5" fill="#fff" stroke="#D4CDE0" stroke-width=".8" opacity=".4"/>
                        <circle cx="-65" cy="59" r="6.5" fill="#fff" stroke="#D4CDE0" stroke-width=".7" opacity=".3"/>
                        <circle cx="-31" cy="59" r="6.5" fill="#fff" stroke="#2D2044" stroke-width=".8"/>
                        <circle cx="-31" cy="83" r="7.5" fill="#E3F5EC" stroke="#1E8449" stroke-width="1.3"/><text x="-31" y="86" text-anchor="middle" fill="#1E8449" font-size="6.5" font-weight="700">GOAL</text>
                        <text x="0" y="48" text-anchor="middle" fill="#D4CDE0" font-size="11">&#x22EF;</text>
                        <text x="48" y="48" text-anchor="middle" fill="#D4CDE0" font-size="11">&#x22EF;</text>
                    </g>
                </svg>
            </div>
            <div class="ss">
                <div class="st"><div class="n">~20</div><div class="l">Reachable states</div></div>
                <div class="st"><div class="n">3</div><div class="l">Plan length</div></div>
                <div class="st"><div class="n" style="color:#1E8449">&#x2713;</div><div class="l">Milliseconds</div></div>
            </div>
        </div>

        <div class="vs"><div class="vsb">vs</div></div>

        <!-- RIGHT: Intractable -->
        <div class="sd sd-d" id="vis1-bad">
            <div class="sh"><span class="bg bg-d">Intractable</span><h3>16 Zones · 3 Packages</h3></div>
            <div class="sv">
                <svg viewBox="0 0 257 250" preserveAspectRatio="xMidYMid meet">
                    <defs><pattern id="t2a" width="8" height="8" patternUnits="userSpaceOnUse"><rect width="8" height="8" fill="#F0EDF3"/><line x1="0" y1="8" x2="8" y2="8" stroke="#D4CDE0" stroke-width=".1" opacity=".15"/><line x1="8" y1="0" x2="8" y2="8" stroke="#D4CDE0" stroke-width=".1" opacity=".15"/></pattern></defs>
                    <rect x="2" y="2" width="253" height="181" rx="4" fill="none" stroke="#D4CDE0" stroke-width=".6"/>
                    <rect x="4" y="4" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="34" y="30" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">A</text>
                    <rect x="67" y="4" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="97" y="30" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">B</text>
                    <rect x="130" y="4" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="160" y="30" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">C</text>
                    <rect x="193" y="4" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="223" y="30" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">D</text>
                    <rect x="4" y="49" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="34" y="75" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">E</text>
                    <rect x="67" y="49" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="97" y="75" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">F</text>
                    <rect x="130" y="49" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="160" y="75" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">G</text>
                    <rect x="193" y="49" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="223" y="75" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">H</text>
                    <rect x="4" y="94" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="34" y="120" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">I</text>
                    <rect x="67" y="94" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="97" y="120" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">J</text>
                    <rect x="130" y="94" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="160" y="120" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">K</text>
                    <rect x="193" y="94" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="223" y="120" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">L</text>
                    <rect x="4" y="139" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="34" y="165" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">M</text>
                    <rect x="67" y="139" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="97" y="165" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">N</text>
                    <rect x="130" y="139" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="160" y="165" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">O</text>
                    <rect x="193" y="139" width="60" height="42" rx="3" fill="url(#t2a)" stroke="#D4CDE0" stroke-width=".4"/><text x="223" y="165" text-anchor="middle" fill="#C0A8CC" font-size="11" font-weight="600">P</text>
                    <g transform="translate(34,18)"><line x1="0" y1="-9" x2="0" y2="-7" stroke="#4A3360" stroke-width="0.6"/><circle cx="0" cy="-10" r="0.9" fill="#4A3360"/><rect x="-4.5" y="-7" width="9" height="5.5" rx="1.5" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/><rect x="-5.2" y="-1.5" width="10.4" height="8" rx="1.1" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/></g>
                    <rect x="215" y="8" width="16" height="11" rx="2" fill="#D68910" stroke="#B7770A" stroke-width=".5"/><text x="223" y="16" text-anchor="middle" fill="#fff" font-size="6" font-weight="700">PKG</text>
                    <rect x="89" y="98" width="16" height="11" rx="2" fill="#D68910" stroke="#B7770A" stroke-width=".5"/><text x="97" y="106" text-anchor="middle" fill="#fff" font-size="6" font-weight="700">PKG</text>
                    <rect x="152" y="143" width="16" height="11" rx="2" fill="#D68910" stroke="#B7770A" stroke-width=".5"/><text x="160" y="151" text-anchor="middle" fill="#fff" font-size="6" font-weight="700">PKG</text>
                    <rect x="26" y="53" width="16" height="11" rx="2" fill="#E3F5EC" stroke="#1E8449" stroke-width="1" stroke-dasharray="3 2" opacity=".5"/>
                    <rect x="215" y="143" width="16" height="11" rx="2" fill="#E3F5EC" stroke="#1E8449" stroke-width="1" stroke-dasharray="3 2" opacity=".5"/>
                    <rect x="152" y="8" width="16" height="11" rx="2" fill="#E3F5EC" stroke="#1E8449" stroke-width="1" stroke-dasharray="3 2" opacity=".5"/>

                    <g transform="translate(128,196)">
                        <circle cx="0" cy="0" r="5" fill="#fff" stroke="#C0392B" stroke-width="1"/>
                        <g opacity=".45"><line x1="0" y1="5" x2="-95" y2="22" stroke="#C0392B" stroke-width=".5"/><line x1="0" y1="5" x2="-70" y2="22" stroke="#C0392B" stroke-width=".5"/><line x1="0" y1="5" x2="-45" y2="22" stroke="#C0392B" stroke-width=".5"/><line x1="0" y1="5" x2="-20" y2="22" stroke="#C0392B" stroke-width=".5"/><line x1="0" y1="5" x2="0" y2="22" stroke="#C0392B" stroke-width=".5"/><line x1="0" y1="5" x2="20" y2="22" stroke="#C0392B" stroke-width=".5"/><line x1="0" y1="5" x2="45" y2="22" stroke="#C0392B" stroke-width=".5"/><line x1="0" y1="5" x2="70" y2="22" stroke="#C0392B" stroke-width=".5"/><line x1="0" y1="5" x2="95" y2="22" stroke="#C0392B" stroke-width=".5"/></g>
                        <g opacity=".3"><circle cx="-95" cy="24" r="2.5" fill="#FDECEA" stroke="#C0392B" stroke-width=".4"/><circle cx="-70" cy="24" r="2.5" fill="#FDECEA" stroke="#C0392B" stroke-width=".4"/><circle cx="-45" cy="24" r="2.5" fill="#FDECEA" stroke="#C0392B" stroke-width=".4"/><circle cx="-20" cy="24" r="2.5" fill="#FDECEA" stroke="#C0392B" stroke-width=".4"/><circle cx="0" cy="24" r="2.5" fill="#FDECEA" stroke="#C0392B" stroke-width=".4"/><circle cx="20" cy="24" r="2.5" fill="#FDECEA" stroke="#C0392B" stroke-width=".4"/><circle cx="45" cy="24" r="2.5" fill="#FDECEA" stroke="#C0392B" stroke-width=".4"/><circle cx="70" cy="24" r="2.5" fill="#FDECEA" stroke="#C0392B" stroke-width=".4"/><circle cx="95" cy="24" r="2.5" fill="#FDECEA" stroke="#C0392B" stroke-width=".4"/></g>
                        <text x="0" y="42" text-anchor="middle" fill="#C0392B" font-size="9" opacity=".25">&#x22EF; &#x22EF; &#x22EF; &#x22EF; &#x22EF;</text>
                    </g>
                </svg>
            </div>
            <div class="ss">
                <div class="st ex"><div class="n" id="vis1-counter">0</div><div class="l">Reachable states</div></div>
                <div class="st ex"><div class="n">12+</div><div class="l">Plan length</div></div>
                <div class="st ex"><div class="n">&#x2717;</div><div class="l">Hours / Timeout</div></div>
            </div>
        </div>
    </div>

    <div class="vis-banner">
        <p>Adding zones and objects doesn't make the problem <em>linearly</em> harder &mdash; it makes it <strong>exponentially</strong> harder. The state space grows as O(zones<sup>objects</sup>), and the search tree explodes with plan length.</p>
    </div>
</div>

<script>
(function(){
    var animated = false;
    var counter = document.getElementById('vis1-counter');
    var badCard = document.getElementById('vis1-bad');
    if (!counter || !badCard) return;
    function runCounter(){
        if (animated) return; animated = true;
        badCard.classList.add('glow');
        var target = 1200000, duration = 2600, start = null;
        function tick(ts){
            if (!start) start = ts;
            var p = Math.min((ts - start) / duration, 1);
            var eased = 1 - Math.pow(1 - p, 3);
            var c = Math.round(target * eased);
            if (c < 1000) counter.textContent = c.toLocaleString();
            else if (c < 1e6) counter.textContent = (c/1000).toFixed(0) + 'K';
            else counter.textContent = (c/1e6).toFixed(1) + 'M';
            if (p >= 1) counter.textContent = '1.2M+';
            if (p < 1) requestAnimationFrame(tick);
        }
        requestAnimationFrame(tick);
    }
    if ('IntersectionObserver' in window) {
        var io = new IntersectionObserver(function(entries){
            entries.forEach(function(e){ if (e.isIntersecting) { runCounter(); io.disconnect(); } });
        }, {threshold: 0.4});
        io.observe(document.getElementById('vis1'));
    } else {
        setTimeout(runCounter, 600);
    }
})();
</script>

<h2>The exponential wall</h2>

<p>That counter on the right is not artistic license. With 16 zones and 3 packages, accounting for which package is where and whether the robot is carrying one of them, the reachable state space is on the order of a million configurations. The search tree the planner has to explore &mdash; branching at roughly 18 actions per state, plan length 12+ &mdash; is many orders of magnitude larger.</p>

<p>Classical planning is known to be PSPACE-complete. In practical terms: the algorithm doesn't care whether you used a smart heuristic or a domain expert hand-tuned the search order. The worst-case scaling is exponential in the size of the problem description. Tiny instances solve in milliseconds. Modestly larger instances take seconds. Realistic instances time out.</p>

<p>This is not a quirk. It's the central reason classical planning, despite being a beautifully principled framework, struggles to deliver on tasks people actually want &mdash; household robotics, multi-step manipulation, logistics at scale.</p>

<h2>What if we could just <em>learn</em> what to do?</h2>

<p>Here is the seductive premise of learning for planning: the small instances are easy. A classical planner can solve them &mdash; millions of them, if we want. So <em>collect</em> those solutions. Train a model on them. Then deploy the model on the large instances the planner can't touch.</p>

<p>If it works, we get the best of both worlds: soundness from the planner that generated the data, and scalability from the neural network that learned from it.</p>

<p>The premise has been pursued for years. It mostly hasn't worked. The standard recipe &mdash; train a value function on solved instances, then use it to guide search at test time &mdash; generalizes poorly when the test instances are bigger than the training instances. Values learned for "small warehouses" don't carry over to "large warehouses" in any useful way, because the input representation itself depends on the size of the problem. A 1000-zone warehouse needs a different network than a 4-zone one. There is no obvious way to share weights.</p>

<p>The rest of this series surveys what has been tried, what has worked, and what still hasn't. To structure the survey, two design axes show up in every paper:</p>

<h2>The two axes that organize the field</h2>

<p>Every neural method for classical planning has to make two distinct choices. The literature is best understood as a 2&times;3 grid spanned by these two axes, and each cell has been explored.</p>

<ul>
    <li><strong>Axis 1 &mdash; What to learn (the prediction target).</strong> Three families exist: learn a <em>heuristic</em> for search guidance (ASNets, STRIPS-HGN, GOOSE); learn a <em>value function</em> for greedy policies (GPL); or learn a <em>ranking</em> &mdash; either over states (RankSVM, GBFS-rank) or directly over actions (GRAPL, GABAR). Each family makes a different bet about what a neural network can usefully predict and inherits a different failure mode at scale.</li>
    <li><strong>Axis 2 &mdash; How to represent the input (the encoding).</strong> Graph neural networks are the dominant choice because they handle variable-sized inputs naturally. But within "GNN," the design space is wide: lifted graphs vs grounded, action-as-node vs action-implicit, hypergraph vs ordinary graph, with vs without a global aggregation node. The encoding choice determines whether a single trained network can read instances of vastly different sizes.</li>
</ul>

<p>The two axes are <em>orthogonal</em>. A paper makes one choice on each. GPL is GNN+value-function. ASNets is alternating-layers+heuristic. STRIPS-HGN is hypergraph+heuristic. GRAPL is GNN+action-ranking with independent parameter decoding. GABAR is GNN+action-ranking with sequential parameter decoding. Same axes, different cells.</p>

<p>The rest of the series follows this structure:</p>

<ul>
    <li><strong>Part 2</strong> takes the first axis seriously and surveys the three learning-objective families with the papers that defined each.</li>
    <li><strong>Part 3</strong> takes the second axis seriously and surveys the graph-representation choices, with concrete contrasts between encodings.</li>
    <li><strong>Part 4</strong> reads GABAR &mdash; one specific cell in the design space &mdash; with a full understanding of why each choice was the right one given the literature.</li>
    <li><strong>Part 5</strong> closes the series and bridges to the partially-observable cousin.</li>
</ul>

<p>The payoff at Part 4: GABAR trains on warehouses with 4 zones and 1 package, and solves warehouses with hundreds of zones and dozens of packages without retraining. The path to that result runs through both axes.</p>

<p>For now, the only thing to internalize is the visualization above. The left card is what classical planning is good at. The right card is what classical planning will never be good at. Everything that follows is about closing that gap.</p>

<hr>

<div class="series-footer">
    <strong>Where this fits</strong>
    <p>This is the problem statement and the framing for the whole series. The two surveys (Parts 2 and 3) walk the learning-objective axis and the representation axis with the literature. Part 4 is GABAR. The sibling series, <a href="/blog/category/planning-under-uncertainty/">Planning Under Uncertainty</a>, picks up the same warehouse but with the robot's view fogged out.</p>
    <p style="margin-top: 10px; font-size: 0.85em; color: #666;"><a href="/blog/2026/learning-for-planning-overview/">Overview</a> &middot; <a href="/blog/2026/learning-for-planning-what-to-learn/">Part 2: What to Learn &mdash; Objectives Survey &rarr;</a></p>
</div>

</article>
