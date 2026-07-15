---
layout: fullhtml-post
title: "Teaching Robots to Tidy Up: Planning Under Uncertainty in Multi-Room Environments"
date: 2026-04-30
categories: ["Planning under Uncertainty", "My Research"]
tags: ["planning", "pomdp", "robotics", "rearrangement"]
description: "How hierarchical planning with object-oriented beliefs enables robots to rearrange objects when they can't see everything at once. Part 3 of the Planning Under Uncertainty series."
_styles: >
  .blog-fullhtml {
  font-family: 'Charter', 'Georgia', serif;
  line-height: 1.75;
  color: #1a1a1a;
  font-size: 18px;
  }
  .blog-fullhtml p { margin: 0 0 1.3em; }
  .blog-fullhtml ul, .blog-fullhtml ol { margin: 0 0 1.3em; }
  .blog-fullhtml h1 { font-size: 2em; line-height: 1.2; margin-top: 1.5em; }
  .blog-fullhtml h2 { font-size: 1.5em; margin-top: 2em; color: #222; }
  .blog-fullhtml h3 { font-size: 1.2em; margin-top: 1.5em; }
  .blog-fullhtml blockquote {
  border-left: 3px solid #2e7d32;
  margin: 1.5em 0;
  padding: 0.5em 1.5em;
  color: #444;
  font-style: italic;
  background: #f0fff0;
  }
  .blog-fullhtml .subtitle {
  font-size: 1.1em;
  color: #555;
  font-style: italic;
  margin-top: -0.5em;
  }
  .blog-fullhtml hr {
  border: none;
  border-top: 1px solid #ddd;
  margin: 2.5em 0;
  }
  .blog-fullhtml code {
  background: #f4f4f4;
  padding: 2px 5px;
  border-radius: 3px;
  font-size: 0.9em;
  }
  .blog-fullhtml .vis-container {
  margin: 2em 0;
  padding: 1.5em;
  background: #fafafa;
  border-radius: 10px;
  border: 1px solid #eee;
  }
  .blog-fullhtml .vis-caption {
  font-size: 0.85em;
  color: #666;
  font-style: italic;
  margin-top: 10px;
  text-align: center;
  }
  .blog-fullhtml .results-block {
  background: #e8f5e9;
  border-left: 4px solid #2e7d32;
  padding: 15px 20px;
  margin: 1.5em 0;
  border-radius: 0 6px 6px 0;
  }
  .blog-fullhtml .results-block strong { color: #1b5e20; }
  .blog-fullhtml .warning-block {
  background: #fff3e0;
  border-left: 4px solid #ff9800;
  padding: 15px 20px;
  margin: 1.5em 0;
  border-radius: 0 6px 6px 0;
  }

  .blog-fullhtml .chart-container {
  margin: 20px 0;
  padding: 20px;
  background: white;
  border-radius: 8px;
  }
  .blog-fullhtml .chart { display: flex; flex-direction: column; gap: 6px; }
  .blog-fullhtml .chart-row { display: flex; align-items: center; gap: 8px; }
  .blog-fullhtml .chart-label { width: 100px; font-size: 0.8em; font-weight: 600; color: #444; text-align: right; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .chart-bar-container { flex: 1; height: 24px; background: #eee; border-radius: 4px; overflow: hidden; }
  .blog-fullhtml .chart-bar { height: 100%; border-radius: 4px; display: flex; align-items: center; justify-content: flex-end; padding-right: 6px; font-size: 0.7em; font-weight: 700; color: white; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .chart-bar.small-text { display: block; padding: 0; line-height: 24px; text-indent: calc(100% + 8px); color: #333; overflow: visible; white-space: nowrap; }
  .blog-fullhtml .chart-title { text-align: center; font-size: 0.9em; font-weight: 700; color: #333; margin-bottom: 12px; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .chart-subtitle { text-align: center; font-size: 0.75em; color: #888; margin-top: -8px; margin-bottom: 12px; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .difficulty-header { font-weight: 700; font-size: 0.8em; color: #555; margin: 10px 0 6px; padding: 3px 8px; background: #f0f0f0; border-radius: 3px; display: inline-block; font-family: -apple-system, sans-serif; }

  .blog-fullhtml .arch-flow { display: flex; gap: 8px; align-items: center; justify-content: center; flex-wrap: wrap; margin: 15px 0; }
  .blog-fullhtml .arch-box {
  text-align: center; padding: 12px 16px; border-radius: 8px;
  min-width: 100px; font-family: -apple-system, sans-serif;
  }
  .blog-fullhtml .arch-box h5 { font-size: 0.75em; color: #555; margin-bottom: 4px; font-weight: 700; }
  .blog-fullhtml .arch-box p { font-size: 0.7em; color: #777; margin: 0; }
  .blog-fullhtml .arch-arrow { font-size: 1.2em; color: #ccc; }

  .blog-fullhtml .compare-table { width: 100%; border-collapse: collapse; font-size: 0.85em; font-family: -apple-system, sans-serif; margin: 15px 0; }
  .blog-fullhtml .compare-table th, .blog-fullhtml .compare-table td { padding: 8px 12px; border: 1px solid #e0e0e0; text-align: center; }
  .blog-fullhtml .compare-table th { background: #f5f5f5; font-weight: 600; }
  .blog-fullhtml .compare-table .yes { color: #2e7d32; font-weight: 600; }
  .blog-fullhtml .compare-table .no { color: #c62828; font-weight: 600; }
  .blog-fullhtml .compare-table .partial { color: #ff9800; font-weight: 600; }

  .blog-fullhtml .belief-demo { display: flex; gap: 20px; align-items: flex-start; justify-content: center; flex-wrap: wrap; margin: 15px 0; }
  .blog-fullhtml .belief-panel { background: white; border-radius: 8px; padding: 12px; border: 1px solid #e0e0e0; min-width: 150px; }
  .blog-fullhtml .belief-panel h5 { font-size: 0.75em; color: #666; margin-bottom: 8px; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .belief-bar { display: flex; align-items: center; gap: 6px; margin: 4px 0; font-size: 0.7em; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .belief-bar-label { width: 40px; text-align: right; font-weight: 600; }
  .blog-fullhtml .belief-bar-track { flex: 1; height: 10px; background: #eee; border-radius: 5px; overflow: hidden; }
  .blog-fullhtml .belief-bar-fill { height: 100%; border-radius: 5px; }

  .blog-fullhtml .video-placeholder {
  width: 100%;
  max-width: 640px;
  margin: 20px auto;
  aspect-ratio: 16/9;
  background: #f0f0f0;
  border-radius: 10px;
  border: 2px dashed #ccc;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  }
  .blog-fullhtml .video-placeholder .play-icon { font-size: 2.5em; color: #bbb; }
  .blog-fullhtml .video-placeholder p { font-size: 0.85em; color: #888; margin: 5px 0; }

  .blog-fullhtml .insight-box {
  background: linear-gradient(135deg, #e3f2fd 0%, #f3e5f5 100%);
  border-radius: 10px;
  padding: 20px;
  margin: 1.5em 0;
  border-left: 5px solid #5b6abf;
  }
  .blog-fullhtml .insight-box h4 { margin: 0 0 10px; color: #333; font-size: 1.1em; }
  .blog-fullhtml .insight-box p { margin: 0; color: #555; }

  .blog-fullhtml .series-nav {
  background: #ecedfa; border-left: 4px solid #5b6abf; border-radius: 0 8px 8px 0;
  padding: 14px 18px; margin: 0 0 32px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; font-size: 0.92em;
  }
  .blog-fullhtml .series-nav strong { color: #3d4a9e; }
  .blog-fullhtml .series-nav .series-nav-links { margin-top: 6px; font-size: 0.85em; color: #555; }
  .blog-fullhtml .series-nav a { color: #3d4a9e; text-decoration: none; border-bottom: 1px solid #5b6abf55; }
  .blog-fullhtml .series-nav a:hover { color: #28327a; border-bottom-color: #28327a; }

  .blog-fullhtml .series-footer {
  margin: 3em 0 2em; padding: 20px 22px;
  background: #f5f6fc; border: 1px solid #d4d8ee; border-radius: 10px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .series-footer strong { color: #3d4a9e; }
  .blog-fullhtml .series-footer p { font-size: 0.9em; color: #444; margin: 8px 0 0; line-height: 1.6; }
  .blog-fullhtml .series-footer a { color: #3d4a9e; text-decoration: none; border-bottom: 1px solid #5b6abf55; }
  .blog-fullhtml .series-footer a:hover { color: #28327a; border-bottom-color: #28327a; }

  .blog-fullhtml .blog-container { max-width: 680px; margin: 40px auto; padding: 0 20px; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }

  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml h1 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml em { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml a:hover { color: #b8c8f0; border-bottom-color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml hr { border-top-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml code { background: #2c3237; color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .subtitle { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml blockquote { background: #142a14; border-left-color: #5cbf5c; color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .vis-container { background: #1a1530; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .vis-caption { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .results-block { background: #142a14; border-left-color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .results-block strong { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .warning-block { background: #2a2010; border-left-color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .chart-container { background: #1a2030; }
  html[data-theme="dark"] .blog-fullhtml .chart-label { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .chart-bar-container { background: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .chart-bar.small-text { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml .chart-title { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml .chart-subtitle { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .difficulty-header { color: #a8b8b8; background: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .arch-box h5 { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .arch-box p { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .arch-arrow { color: #5a6065; }
  html[data-theme="dark"] .blog-fullhtml .compare-table th, html[data-theme="dark"] .blog-fullhtml .compare-table td { border-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .compare-table th { background: #1e2530; }
  html[data-theme="dark"] .blog-fullhtml .compare-table .yes { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .compare-table .no { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .compare-table .partial { color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .belief-panel { background: #1a1530; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .belief-panel h5 { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .belief-bar-track { background: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .video-placeholder { background: #1a1530; border-color: #3a3545; }
  html[data-theme="dark"] .blog-fullhtml .video-placeholder .play-icon { color: #5a5560; }
  html[data-theme="dark"] .blog-fullhtml .video-placeholder p { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .insight-box { background: linear-gradient(135deg, #1a1f3a, #1e1a30); border-left-color: #5b6abf; }
  html[data-theme="dark"] .blog-fullhtml .insight-box h4 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml .insight-box p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav { background: #1a1f3a; border-left-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .series-nav strong { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .series-nav-links { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml .series-nav a:hover { color: #b8c8f0; border-bottom-color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml .series-footer { background: #15192e; border-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .series-footer strong { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .series-footer p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-footer a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml .series-footer a:hover { color: #b8c8f0; border-bottom-color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #6a7888; border-top-color: #2a3545; }
---

<article class="blog-container">

<!-- Series Nav (Planning Under Uncertainty · Part 3 of 4) -->
<div class="series-nav">
    <strong>Planning Under Uncertainty · Part 3 of 4 — the abstraction strategy</strong>
    <div class="series-nav-links">
        ← <a href="/blog/2026/planning-under-uncertainty-mcts-for-pomdps/">Part 2: MCTS for POMDPs</a> · Next: <a href="/blog/2026/planning-under-uncertainty-gammazero/">Part 4: GammaZero →</a> · <em>(Also readable standalone as a paper deep-dive.)</em>
    </div>
</div>

<h1>Teaching Robots to Tidy Up: Planning Under Uncertainty in Multi-Room Environments</h1>

<p class="subtitle">How hierarchical planning with object-oriented beliefs enables robots to rearrange objects when they can't see everything at once.</p>

<p style="background:#FDF6E3; border-left:3px solid #C5A55A; padding:10px 14px; font-size:0.9em; color:#5a4400; margin-top:14px; border-radius:0 6px 6px 0;">
    <strong>Series context &amp; running example.</strong> Within the <em>Planning Under Uncertainty</em> series, HOO-POMDP is the <em>abstraction</em> strategy &mdash; shrink a large POMDP through factorization and hierarchy until a principled planner can handle it; the companion post (<a href="/blog/2026/planning-under-uncertainty-gammazero/" style="color:#7a5a00;">Part 4: GammaZero</a>) takes the complementary <em>learning</em> strategy. The series' running example is warehouse-delivery under partial observability; HOO-POMDP was developed for the <em>MultiRoomR</em> benchmark (described later), the same idea at robotics scale &mdash; object-oriented states, partial observability across rooms instead of zones &mdash; and the reasoning transfers directly back to the warehouse from <a href="/blog/2026/planning-under-uncertainty-belief-states/" style="color:#7a5a00;">Part 1</a>.
</p>

<hr>

<h2>The Challenge: A Messy House, A Limited View</h2>

<p>Imagine asking a robot to tidy up your home. Books need to go to bookshelves, mugs to the kitchen, toys to the bedroom. Simple enough for a human. Catastrophically hard for a robot.</p>

<p>Why? The robot can only see what's directly in front of it. When it starts in the living room, it doesn't know where objects in the bedroom are. It doesn't even know <em>which</em> objects are in the bedroom. It needs to search, remember, plan, and act—all while dealing with the fact that its object detector fails 40% of the time.</p>

<p>This is the <strong>multi-object rearrangement problem under partial observability</strong>. And it gets worse.</p>

<div class="warning-block">
<strong>Real-world complications:</strong>
<ul style="margin: 10px 0 0 20px;">
<li><strong>Blocked paths:</strong> A book on the floor blocks the doorway to the bedroom</li>
<li><strong>Blocked goals:</strong> A bowl sits exactly where the mug needs to go</li>
<li><strong>Swap scenarios:</strong> Object A is where B should be, and B is where A should be</li>
<li><strong>Detector failures:</strong> The robot looks right at an object and doesn't see it</li>
</ul>
</div>

<p>Existing approaches fall into two camps, and both struggle:</p>

<div class="vis-container">
    <div class="tbl-scroll">
<table class="compare-table">
        <tr>
            <th>Approach</th>
            <th>Handles Uncertainty</th>
            <th>Handles Blocked Paths</th>
            <th>Scales to Many Objects</th>
            <th>Optimal Planning</th>
        </tr>
        <tr>
            <td><strong>End-to-End RL</strong></td>
            <td class="partial">Implicit</td>
            <td class="no">No</td>
            <td class="no">No</td>
            <td class="no">No</td>
        </tr>
        <tr>
            <td><strong>Greedy/Heuristic</strong></td>
            <td class="no">No</td>
            <td class="no">No</td>
            <td class="yes">Yes</td>
            <td class="no">No</td>
        </tr>
        <tr>
            <td><strong>HOO-POMDP (Ours)</strong></td>
            <td class="yes">Yes</td>
            <td class="yes">Yes</td>
            <td class="yes">Yes</td>
            <td class="yes">Yes</td>
        </tr>
    </table>
</div>
    <p class="vis-caption">Comparison of approaches to multi-object rearrangement. Only HOO-POMDP handles all challenges in a principled, unified framework.</p>
</div>

<hr>

<h2>The Core Insight: Two Levels of Abstraction</h2>

<p>The key insight behind HOO-POMDP is a separation of concerns:</p>

<blockquote>Don't make the strategic planner worry about how to physically pick up objects. Don't make the low-level controller decide which room to explore next.</blockquote>

<p>This hierarchical decomposition is the "H" in HOO-POMDP:</p>

<div class="vis-container">
    <div class="arch-flow">
        <div class="arch-box" style="background:#e3f2fd; border:2px solid #1565c0;">
            <h5>Perception</h5>
            <p>RGB + Depth<br>&#8594; Object detections</p>
        </div>
        <div class="arch-arrow">&#8594;</div>
        <div class="arch-box" style="background:#fff3e0; border:2px solid #ff9800;">
            <h5>Belief Update</h5>
            <p>Update probabilities<br>per object</p>
        </div>
        <div class="arch-arrow">&#8594;</div>
        <div class="arch-box" style="background:#f3e5f5; border:2px solid #7b1fa2;">
            <h5>Abstraction</h5>
            <p>Continuous &#8594; Discrete<br>Sample locations</p>
        </div>
        <div class="arch-arrow">&#8594;</div>
        <div class="arch-box" style="background:#e8f5e9; border:2px solid #2e7d32;">
            <h5>POMDP Planner</h5>
            <p>Select sub-goal<br>(Move, PickPlace)</p>
        </div>
        <div class="arch-arrow">&#8594;</div>
        <div class="arch-box" style="background:#fce4ec; border:2px solid #c2185b;">
            <h5>Low-Level Policy</h5>
            <p>A* + RL<br>Primitive actions</p>
        </div>
    </div>
    <p class="vis-caption">The HOO-POMDP pipeline: from raw perception to primitive actions, through hierarchical abstraction.</p>
</div>

<h3>Level 1: The Strategic Planner (POMDP)</h3>

<p>At the top level, a POMDP planner reasons about <em>what to do next</em> under uncertainty:</p>

<ul>
<li>Should I search for the missing lamp, or move the mug I've already found?</li>
<li>Should I verify that uncertain plate detection, or trust it and proceed?</li>
<li>The book is blocking the doorway—should I move it first, even though it's not my current target?</li>
</ul>

<p>The planner works with <em>abstract actions</em>: <code>Move(A→B)</code>, <code>Rotate(θ)</code>, <code>PickPlace(Object, Goal)</code>. It doesn't care how these happen—just that they do.</p>

<h3>Level 2: The Low-Level Policies</h3>

<p>Each abstract action maps to a specialized policy:</p>

<ul>
<li><strong>Move/Rotate:</strong> A* pathfinding on the known occupancy map</li>
<li><strong>PickPlace:</strong> Three-stage pipeline—RL to pick, A* to navigate, RL to place</li>
</ul>

<p>These policies handle the continuous control problem: motor commands, collision avoidance, precise manipulation.</p>

<hr>

<h2>Object-Oriented Beliefs: The Scalability Trick</h2>

<p>The "OO" in HOO-POMDP stands for Object-Oriented. This is crucial for scalability.</p>

<p>A naive POMDP would maintain a probability distribution over all possible world states. With 10 objects, each potentially at 100 locations, that's 100<sup>10</sup> states. Intractable.</p>

<p>The object-oriented formulation factors the belief by object:</p>

<div class="insight-box">
<h4>Object Independence Assumption</h4>
<p>The probability of observing object A doesn't depend on where object B is (conditioned on A's state). This lets us maintain <em>separate</em> belief distributions for each object: b = b₁ × b₂ × ... × bₙ</p>
</div>

<p>Now we have n × 100 states instead of 100<sup>n</sup>. Linear instead of exponential.</p>

<div class="vis-container">
    <div class="belief-demo">
        <div class="belief-panel">
            <h5>O1 (Mug) Belief</h5>
            <div class="belief-bar">
                <span class="belief-bar-label">Living</span>
                <div class="belief-bar-track"><div class="belief-bar-fill" style="width:92%; background:#4caf50;"></div></div>
            </div>
            <div class="belief-bar">
                <span class="belief-bar-label">Kitchen</span>
                <div class="belief-bar-track"><div class="belief-bar-fill" style="width:5%; background:#c8e6c9;"></div></div>
            </div>
            <div class="belief-bar">
                <span class="belief-bar-label">Other</span>
                <div class="belief-bar-track"><div class="belief-bar-fill" style="width:3%; background:#e0e0e0;"></div></div>
            </div>
            <p style="font-size:0.65em; color:#2e7d32; margin-top:8px;">High confidence</p>
        </div>
        <div class="belief-panel">
            <h5>O5 (Lamp) Belief</h5>
            <div class="belief-bar">
                <span class="belief-bar-label">Bed A</span>
                <div class="belief-bar-track"><div class="belief-bar-fill" style="width:35%; background:#bdbdbd;"></div></div>
            </div>
            <div class="belief-bar">
                <span class="belief-bar-label">Bed B</span>
                <div class="belief-bar-track"><div class="belief-bar-fill" style="width:40%; background:#bdbdbd;"></div></div>
            </div>
            <div class="belief-bar">
                <span class="belief-bar-label">Other</span>
                <div class="belief-bar-track"><div class="belief-bar-fill" style="width:25%; background:#e0e0e0;"></div></div>
            </div>
            <p style="font-size:0.65em; color:#e53935; margin-top:8px;">Needs exploration</p>
        </div>
        <div class="belief-panel">
            <h5>O2 (Book) Belief</h5>
            <div class="belief-bar">
                <span class="belief-bar-label">Door</span>
                <div class="belief-bar-track"><div class="belief-bar-fill" style="width:88%; background:#ff9800;"></div></div>
            </div>
            <div class="belief-bar">
                <span class="belief-bar-label">Table</span>
                <div class="belief-bar-track"><div class="belief-bar-fill" style="width:8%; background:#ffe0b2;"></div></div>
            </div>
            <div class="belief-bar">
                <span class="belief-bar-label">Other</span>
                <div class="belief-bar-track"><div class="belief-bar-fill" style="width:4%; background:#e0e0e0;"></div></div>
            </div>
            <p style="font-size:0.65em; color:#ff9800; margin-top:8px;">Blocking path!</p>
        </div>
    </div>
    <p class="vis-caption">Object-factored beliefs: each object maintains an independent probability distribution over locations. This enables scalable planning with 10-20 objects.</p>
</div>

<hr>

<h2>The Abstraction Layer: Bridging Continuous and Discrete</h2>

<p>The POMDP planner needs discrete states and actions. But the real world is continuous—objects can be anywhere, not just at grid cells.</p>

<p>The abstraction layer converts the continuous belief state into a discrete representation suitable for planning:</p>

<div class="vis-container">
    <div style="font-family:-apple-system,sans-serif; font-size:0.85em;">
        <div style="background:#f0f0f0; padding:12px; border-radius:6px; margin-bottom:12px;">
            <strong>For each object i, abstract state includes:</strong>
            <ul style="margin:8px 0 0 20px;">
                <li><code>loc_i</code>: Most likely location from belief</li>
                <li><code>pick_i</code>: Location from which robot can pick (sampled from belief)</li>
                <li><code>place_locs</code>: Candidate locations to place (goal + nearby receptacles)</li>
                <li><code>is_held</code>: Is the robot currently holding this object?</li>
                <li><code>at_goal</code>: Is the object at its goal location?</li>
            </ul>
        </div>
        <div style="background:#e8f5e9; padding:12px; border-radius:6px;">
            <strong>Key insight:</strong> By sampling multiple pick/place locations when belief is uncertain, the planner can reason about moving to verify a location OR moving directly to a high-confidence location.
        </div>
    </div>
    <p class="vis-caption">The abstraction layer converts continuous belief probabilities into discrete pick/place options for the POMDP planner.</p>
</div>

<p>This is where blocked paths and goals get handled naturally. If an object is blocking a path:</p>

<ol>
<li>The abstraction layer detects that the robot can't reach certain locations</li>
<li>Those locations are marked as unreachable in the abstract state</li>
<li>The planner is forced to find an alternative—which means moving the blocking object first</li>
</ol>

<hr>

<h2>Results: What Works and What Breaks</h2>

<p>We evaluated HOO-POMDP against multiple baselines across three datasets of increasing difficulty.</p>

<h3>Main Results: HOO-POMDP vs. Baselines</h3>

<div class="vis-container">
    <div class="chart-container">
        <div class="chart-title">Scene Success Rate (All Objects Correctly Placed)</div>
        <div class="chart-subtitle">Higher is better. 100 evaluation episodes per setting.</div>
        <div class="chart">
            <div class="difficulty-header">RoomR (5 objects, 1 room)</div>
            <div class="chart-row"><div class="chart-label">HOO-POMDP</div><div class="chart-bar-container"><div class="chart-bar" style="width:49%;background:#2e7d32;">49%</div></div></div>
            <div class="chart-row"><div class="chart-label">FHC (Heuristic)</div><div class="chart-bar-container"><div class="chart-bar" style="width:38%;background:#81c784;">38%</div></div></div>
            <div class="chart-row"><div class="chart-label">MSS</div><div class="chart-bar-container"><div class="chart-bar" style="width:21%;background:#bbb;">21%</div></div></div>
            <div class="chart-row"><div class="chart-label">VRR (RL)</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:7%;background:#e57373;">7%</div></div></div>

            <div class="difficulty-header" style="margin-top:12px;">ProcThor (5 objects, 2 rooms)</div>
            <div class="chart-row"><div class="chart-label">HOO-POMDP</div><div class="chart-bar-container"><div class="chart-bar" style="width:46%;background:#2e7d32;">46%</div></div></div>
            <div class="chart-row"><div class="chart-label">FHC (Heuristic)</div><div class="chart-bar-container"><div class="chart-bar" style="width:32%;background:#81c784;">32%</div></div></div>
            <div class="chart-row"><div class="chart-label">MSS</div><div class="chart-bar-container"><div class="chart-bar" style="width:14%;background:#bbb;">14%</div></div></div>
            <div class="chart-row"><div class="chart-label">VRR (RL)</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:2%;background:#e57373;">2%</div></div></div>

            <div class="difficulty-header" style="margin-top:12px;">MultiRoomR (10 objects, 3-4 rooms, blocked paths)</div>
            <div class="chart-row"><div class="chart-label">HOO-POMDP</div><div class="chart-bar-container"><div class="chart-bar" style="width:18%;background:#2e7d32;">18%</div></div></div>
            <div class="chart-row"><div class="chart-label">FHC (Heuristic)</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:9%;background:#81c784;">9%</div></div></div>
            <div class="chart-row"><div class="chart-label">MSS</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:0%;background:#bbb;">NC</div></div></div>
            <div class="chart-row"><div class="chart-label">VRR (RL)</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:0%;background:#e57373;">0%</div></div></div>
        </div>
    </div>
    <p class="vis-caption">Scene success: all objects must reach goals. HOO-POMDP consistently outperforms baselines. VRR (pure RL) fails completely at scale. MSS cannot handle blocked paths (NC = Not Computable).</p>
</div>

<div class="vis-container">
    <div class="chart-container">
        <div class="chart-title">Object Success Rate (% of Objects Correctly Placed)</div>
        <div class="chart-subtitle">Higher is better. More forgiving metric—partial success counts.</div>
        <div class="chart">
            <div class="difficulty-header">RoomR (5 objects, 1 room)</div>
            <div class="chart-row"><div class="chart-label">HOO-POMDP</div><div class="chart-bar-container"><div class="chart-bar" style="width:71%;background:#5b6abf;">71%</div></div></div>
            <div class="chart-row"><div class="chart-label">FHC</div><div class="chart-bar-container"><div class="chart-bar" style="width:58%;background:#9fa8da;">58%</div></div></div>
            <div class="chart-row"><div class="chart-label">MSS</div><div class="chart-bar-container"><div class="chart-bar" style="width:44%;background:#bbb;">44%</div></div></div>
            <div class="chart-row"><div class="chart-label">VRR</div><div class="chart-bar-container"><div class="chart-bar" style="width:31%;background:#e0e0e0;">31%</div></div></div>

            <div class="difficulty-header" style="margin-top:12px;">MultiRoomR (15 objects, 3-4 rooms)</div>
            <div class="chart-row"><div class="chart-label">HOO-POMDP</div><div class="chart-bar-container"><div class="chart-bar" style="width:59%;background:#5b6abf;">59%</div></div></div>
            <div class="chart-row"><div class="chart-label">FHC</div><div class="chart-bar-container"><div class="chart-bar" style="width:31%;background:#9fa8da;">31%</div></div></div>
            <div class="chart-row"><div class="chart-label">MSS</div><div class="chart-bar-container"><div class="chart-bar" style="width:11%;background:#bbb;">11%</div></div></div>
            <div class="chart-row"><div class="chart-label">VRR</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:9%;background:#e0e0e0;">9%</div></div></div>

            <div class="difficulty-header" style="margin-top:12px;">MultiRoomR (20 objects, 3-4 rooms, blocked paths)</div>
            <div class="chart-row"><div class="chart-label">HOO-POMDP</div><div class="chart-bar-container"><div class="chart-bar" style="width:36%;background:#5b6abf;">36%</div></div></div>
            <div class="chart-row"><div class="chart-label">FHC</div><div class="chart-bar-container"><div class="chart-bar" style="width:11%;background:#9fa8da;">11%</div></div></div>
            <div class="chart-row"><div class="chart-label">MSS</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:0%;background:#bbb;">NC</div></div></div>
            <div class="chart-row"><div class="chart-label">VRR</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:4%;background:#e0e0e0;">4%</div></div></div>
        </div>
    </div>
    <p class="vis-caption">Object success: partial progress counts. HOO-POMDP maintains 36-71% object success even as complexity increases, while baselines deteriorate rapidly.</p>
</div>

<h3>Ablation: What Matters Most?</h3>

<div class="vis-container">
    <div class="chart-container">
        <div class="chart-title">Ablation Study: Component Importance</div>
        <div class="chart-subtitle">Scene success on MultiRoomR (10 objects, 2 rooms). Removing hierarchy or lookahead is catastrophic.</div>
        <div class="chart">
            <div class="chart-row"><div class="chart-label">Full HOO-POMDP</div><div class="chart-bar-container"><div class="chart-bar" style="width:32%;background:#2e7d32;">32%</div></div></div>
            <div class="chart-row"><div class="chart-label">Perfect Detector</div><div class="chart-bar-container"><div class="chart-bar" style="width:40%;background:#4caf50;">40% (oracle)</div></div></div>
            <div class="chart-row"><div class="chart-label">Perfect Knowledge</div><div class="chart-bar-container"><div class="chart-bar" style="width:41%;background:#4caf50;">41% (oracle)</div></div></div>
            <div class="chart-row"><div class="chart-label" style="color:#c62828;">No Hierarchy</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:5%;background:#ef9a9a;">5%</div></div></div>
            <div class="chart-row"><div class="chart-label" style="color:#c62828;">Depth=1 (Greedy)</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:0%;background:#e57373;">0%</div></div></div>
        </div>
    </div>
    <p class="vis-caption">Ablations reveal what matters: (1) Hierarchy is essential—flat POMDP fails. (2) Lookahead is critical—greedy planning finds no solutions. (3) Performance gap to oracle is small, showing robustness to detector failures.</p>
</div>

<div class="results-block">
<strong>Key finding:</strong> HOO-POMDP achieves 80% of oracle performance (with perfect detection) despite a 50-60% detector success rate. The POMDP belief update gracefully handles perception failures.
</div>

<hr>

<h2>The MultiRoomR Benchmark</h2>

<p>Existing benchmarks don't test the hard cases. RoomR has single rooms with most objects visible. We introduce MultiRoomR:</p>

<div class="vis-container">
    <div class="tbl-scroll">
<table class="compare-table">
        <tr>
            <th>Feature</th>
            <th>RoomR</th>
            <th>ProcThor</th>
            <th>MultiRoomR (Ours)</th>
        </tr>
        <tr>
            <td><strong>Rooms</strong></td>
            <td>1</td>
            <td>2</td>
            <td>2-4</td>
        </tr>
        <tr>
            <td><strong>Objects</strong></td>
            <td>5</td>
            <td>5</td>
            <td>10-20</td>
        </tr>
        <tr>
            <td><strong>Initial Visibility</strong></td>
            <td>~60%</td>
            <td>~40%</td>
            <td>10-30%</td>
        </tr>
        <tr>
            <td><strong>Blocked Paths</strong></td>
            <td class="no">No</td>
            <td class="no">No</td>
            <td class="yes">50% of scenes</td>
        </tr>
        <tr>
            <td><strong>Configurations</strong></td>
            <td>25 × 40</td>
            <td>125 × 80</td>
            <td>400</td>
        </tr>
    </table>
</div>
    <p class="vis-caption">MultiRoomR benchmark: designed to test severe partial observability, large object counts, and complex spatial dependencies.</p>
</div>

<h3>Scaling Performance</h3>

<div class="vis-container">
    <div class="chart-container" style="text-align:center;">
        <div class="chart-title">HOO-POMDP Performance vs. Problem Complexity</div>
        <div class="chart-subtitle">Scene Success Rate across datasets. HOO-POMDP maintains performance close to Oracle even as complexity increases.</div>
        <svg viewBox="0 0 500 220" style="width:100%; max-width:500px; height:auto; margin:0 auto; display:block;">
            <!-- Grid -->
            <line x1="70" y1="30" x2="70" y2="170" stroke="#e0e0e0" stroke-width="1"/>
            <line x1="70" y1="170" x2="470" y2="170" stroke="#333" stroke-width="1.5"/>
            <line x1="70" y1="110" x2="470" y2="110" stroke="#e0e0e0" stroke-width="1" stroke-dasharray="4,4"/>
            <line x1="70" y1="70" x2="470" y2="70" stroke="#e0e0e0" stroke-width="1" stroke-dasharray="4,4"/>

            <!-- Y-axis -->
            <text x="60" y="175" font-size="11" text-anchor="end" fill="#666" font-family="system-ui">0%</text>
            <text x="60" y="115" font-size="11" text-anchor="end" fill="#666" font-family="system-ui">30%</text>
            <text x="60" y="75" font-size="11" text-anchor="end" fill="#666" font-family="system-ui">50%</text>
            <text x="60" y="35" font-size="11" text-anchor="end" fill="#666" font-family="system-ui">70%</text>

            <!-- X-axis labels -->
            <text x="130" y="188" font-size="10" text-anchor="middle" fill="#333" font-family="system-ui">RoomR</text>
            <text x="130" y="200" font-size="8" text-anchor="middle" fill="#888" font-family="system-ui">(5 obj, 1 rm)</text>
            <text x="230" y="188" font-size="10" text-anchor="middle" fill="#333" font-family="system-ui">ProcThor</text>
            <text x="230" y="200" font-size="8" text-anchor="middle" fill="#888" font-family="system-ui">(5 obj, 2 rm)</text>
            <text x="330" y="188" font-size="10" text-anchor="middle" fill="#333" font-family="system-ui">MultiRoomR</text>
            <text x="330" y="200" font-size="8" text-anchor="middle" fill="#888" font-family="system-ui">(10 obj, 2-3 rm)</text>
            <text x="430" y="188" font-size="10" text-anchor="middle" fill="#333" font-family="system-ui">MultiRoomR</text>
            <text x="430" y="200" font-size="8" text-anchor="middle" fill="#888" font-family="system-ui">(15-20 obj)</text>

            <!-- HOO-POMDP line -->
            <polyline points="130,72 230,77 330,98 430,120" fill="none" stroke="#2e7d32" stroke-width="3.5" stroke-linecap="round" stroke-linejoin="round"/>
            <circle cx="130" cy="72" r="6" fill="#2e7d32"/>
            <circle cx="230" cy="77" r="6" fill="#2e7d32"/>
            <circle cx="330" cy="98" r="6" fill="#2e7d32"/>
            <circle cx="430" cy="120" r="6" fill="#2e7d32"/>
            <text x="130" y="62" font-size="10" text-anchor="middle" fill="#1b5e20" font-weight="700" font-family="system-ui">49%</text>
            <text x="230" y="67" font-size="10" text-anchor="middle" fill="#1b5e20" font-weight="700" font-family="system-ui">46%</text>
            <text x="330" y="88" font-size="10" text-anchor="middle" fill="#1b5e20" font-weight="700" font-family="system-ui">32%</text>
            <text x="430" y="110" font-size="10" text-anchor="middle" fill="#1b5e20" font-weight="700" font-family="system-ui">21%</text>

            <!-- Oracle line -->
            <polyline points="130,73 230,82 330,95 430,118" fill="none" stroke="#42a5f5" stroke-width="2.5" stroke-dasharray="8,4" opacity="0.8"/>
            <circle cx="130" cy="73" r="5" fill="#42a5f5" opacity="0.8"/>
            <circle cx="230" cy="82" r="5" fill="#42a5f5" opacity="0.8"/>
            <circle cx="330" cy="95" r="5" fill="#42a5f5" opacity="0.8"/>
            <circle cx="430" cy="118" r="5" fill="#42a5f5" opacity="0.8"/>

            <!-- FHC line (baseline) -->
            <polyline points="130,134 230,143 330,157 430,168" fill="none" stroke="#ff7043" stroke-width="2" opacity="0.7"/>
            <circle cx="130" cy="134" r="4" fill="#ff7043" opacity="0.8"/>
            <circle cx="230" cy="143" r="4" fill="#ff7043" opacity="0.8"/>
            <circle cx="330" cy="157" r="4" fill="#ff7043" opacity="0.8"/>
            <circle cx="430" cy="168" r="4" fill="#ff7043" opacity="0.8"/>

            <!-- Legend -->
            <rect x="330" y="25" width="135" height="55" fill="white" stroke="#e0e0e0" rx="4"/>
            <line x1="340" y1="40" x2="365" y2="40" stroke="#2e7d32" stroke-width="3"/>
            <text x="372" y="44" font-size="10" fill="#333" font-family="system-ui">HOO-POMDP</text>
            <line x1="340" y1="55" x2="365" y2="55" stroke="#42a5f5" stroke-width="2.5" stroke-dasharray="5,3"/>
            <text x="372" y="59" font-size="10" fill="#333" font-family="system-ui">Oracle (PD)</text>
            <line x1="340" y1="70" x2="365" y2="70" stroke="#ff7043" stroke-width="2"/>
            <text x="372" y="74" font-size="10" fill="#333" font-family="system-ui">FHC Baseline</text>
        </svg>
    </div>
    <p class="vis-caption">HOO-POMDP (green) stays close to the Oracle performance (blue dashed) across all complexity levels. Baseline methods (orange) degrade rapidly.</p>
</div>

<h3>Example Scenario: Blocked Paths</h3>

<div class="vis-container">
    <div style="text-align:center;">
        <img src="/assets/img/hoo_pomdp_example.png" alt="HOO-POMDP Example Scenario" style="max-width:100%; border-radius:8px; box-shadow:0 2px 10px rgba(0,0,0,0.1);">
    </div>
    <div style="background:#fff8e1; padding:15px; border-radius:8px; margin-top:15px; border-left:4px solid #ff9800;">
        <strong style="color:#e65100;">Spatial Reasoning Example:</strong>
        <p style="margin:10px 0 0; font-size:0.9em;">
            Objects 1-6 each have colored paths to their goals (dashed squares). <strong>Object 3 (red path)</strong> is sitting on Object 1's goal location.
            The planner must reason: "Move Object 3 first, then Object 2 can clear the corridor, then Object 1 can reach its goal."
        </p>
        <div style="margin-top:12px; display:flex; gap:10px; flex-wrap:wrap; font-size:0.82em; font-family:system-ui;">
            <span style="background:#ffeb3b; padding:4px 10px; border-radius:15px; font-weight:600;">1. Move Obj 3</span>
            <span style="color:#888;">→</span>
            <span style="background:#ce93d8; color:white; padding:4px 10px; border-radius:15px; font-weight:600;">2. Move Obj 2</span>
            <span style="color:#888;">→</span>
            <span style="background:#64b5f6; color:white; padding:4px 10px; border-radius:15px; font-weight:600;">3. Move Obj 1</span>
        </div>
    </div>
    <p class="vis-caption">A real scenario from our benchmark. HOO-POMDP correctly identifies the dependency chain and computes the optimal execution order.</p>
</div>

<hr>

<h2>Lessons for Other Research</h2>

<p>Beyond the specific results, HOO-POMDP demonstrates principles that generalize:</p>

<h3>1. Hierarchy Enables Tractable Abstraction</h3>

<p>POMDP planning is exponentially hard in the action space. But most planning problems have natural hierarchies: strategic decisions (what to do) vs. execution details (how to do it). By separating these, each level becomes tractable.</p>

<p><strong>Applicable when:</strong> Your problem has actions at multiple time scales, or strategic choices that are independent of execution details.</p>

<h3>2. Factor Your Beliefs by Entity</h3>

<p>The object-oriented belief representation—treating each object's location as independent—is a strong assumption but enables dramatic scalability. Similar factorizations work whenever your entities don't directly interact continuously.</p>

<p><strong>Applicable when:</strong> You have multiple similar entities (objects, agents, tasks) whose states are conditionally independent given observations.</p>

<h3>3. Abstraction Bridges Continuous and Discrete</h3>

<p>Real-world problems are continuous. Formal planning methods need discrete states. The abstraction layer in HOO-POMDP shows one way to bridge this: sample from continuous distributions to create discrete action candidates.</p>

<p><strong>Applicable when:</strong> You want to apply discrete planning methods (MCTS, POMDP solvers) to continuous domains.</p>

<h3>4. Robustness to Perception Errors Through Belief Updates</h3>

<p>HOO-POMDP doesn't need a perfect detector. The belief update mechanism naturally handles false negatives (didn't see the object) and false positives (saw it in wrong place). This robustness comes "for free" from the POMDP formulation.</p>

<p><strong>Applicable when:</strong> Your perception system is noisy or unreliable, and you need planning that degrades gracefully.</p>

<hr>

<h2>Limitations and Open Questions</h2>

<ul>
<li><strong>Object independence assumption:</strong> Fails in cluttered environments where objects occlude each other or interact physically. Relaxing this is non-trivial.</li>
<li><strong>Unknown object classes:</strong> HOO-POMDP needs to know which object classes to search for. Handling truly novel objects requires additional machinery.</li>
<li><strong>Low-level policy failures:</strong> Most failures come from the RL pick/place policies, not the high-level planner. Better low-level policies would improve overall performance.</li>
<li><strong>Computation time:</strong> MCTS search adds latency. Real-time applications might need faster approximate methods.</li>
</ul>

<h2>Where HOO-POMDP runs out &mdash; and why the next chapter exists</h2>

<p>Computation time deserves its own treatment because it's where HOO-POMDP's success becomes its own ceiling. At 20 objects, the planner spends nearly half an hour per task. The abstraction layer is doing its job &mdash; the planner only reasons about a handful of object slots at any time, not the full state &mdash; but the inner loop is still POMCP. Every decision still runs MCTS. Every leaf is still evaluated by a random rollout that simulates forward until termination. The rollouts are noisy enough that the planner needs many simulations per decision to get a stable value estimate, and the time per decision grows with both the simulation budget and the depth of the rollouts.</p>

<p>This bottleneck is structural. Better abstractions would help at the margin, but they can't change the fact that POMCP's leaf evaluation is the dominant cost. To break through, the rollout itself has to change &mdash; from "simulate a random trajectory and average the return" to "predict the value directly with a learned function." That swap is exactly what AlphaZero did for board games: replace random rollouts with a neural value head, replace uniform priors with a learned policy head. Applied to POMDPs, with a representation that handles variable-size belief spaces, it becomes GammaZero.</p>

<p>Read HOO-POMDP as the achievable ceiling of principled abstraction with classical search inside. Read the next post as what happens when that inside is replaced.</p>

<div class="vis-container">
    <h3 style="font-family:'Playfair Display',Georgia,serif; font-size:1.2rem; color:#2D2044; font-weight:700; margin:0;">The ceiling, on the foggy warehouse</h3>
    <p style="color:#888; font-size:0.92em; margin-top:6px; margin-bottom:14px; font-style:italic;">Foggy warehouse scaling from 4 to 20 objects. The abstraction layer keeps things tractable in state-space terms; the rollouts make decisions expensive.</p>

    <div style="display:flex; gap:14px; align-items:stretch;">
        <div style="flex:1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#3d4a9e; margin-bottom:6px;">Foggy warehouse · 20 objects</div>
            <svg viewBox="0 0 280 220" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <defs>
                    <radialGradient id="fogP3" cx="50%" cy="50%" r="55%"><stop offset="0%" stop-color="#2E1A38" stop-opacity=".25"/><stop offset="100%" stop-color="#2E1A38" stop-opacity=".55"/></radialGradient>
                </defs>
                <!-- 4x4 grid -->
                <rect x="6" y="6" width="268" height="180" rx="4" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".5"/>
                <g>
                <rect x="8" y="8" width="65" height="44" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="75" y="8" width="65" height="44" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="142" y="8" width="65" height="44" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="209" y="8" width="64" height="44" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="8" y="54" width="65" height="44" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="75" y="54" width="65" height="44" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="142" y="54" width="65" height="44" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="209" y="54" width="64" height="44" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="8" y="100" width="65" height="44" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="75" y="100" width="65" height="44" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="142" y="100" width="65" height="44" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="209" y="100" width="64" height="44" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="8" y="146" width="65" height="38" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="75" y="146" width="65" height="38" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="142" y="146" width="65" height="38" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="209" y="146" width="64" height="38" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                </g>

                <!-- Robot in A -->
                <g transform="translate(40,25)"><rect x="-5" y="-7" width="10" height="6" rx="1.5" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/><rect x="-6" y="-2" width="12" height="9" rx="1.4" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/></g>

                <!-- Fog on all except A -->
                <rect x="75" y="8" width="198" height="44" fill="url(#fogP3)"/>
                <rect x="8" y="54" width="265" height="44" fill="url(#fogP3)"/>
                <rect x="8" y="100" width="265" height="44" fill="url(#fogP3)"/>
                <rect x="8" y="146" width="265" height="38" fill="url(#fogP3)"/>

                <!-- Many packages (some visible, most fogged out) -->
                <text x="135" y="200" text-anchor="middle" fill="#3d4a9e" font-size="10" font-weight="700">20 objects · partially observable</text>
                <text x="135" y="213" text-anchor="middle" fill="#888" font-size="9" font-style="italic">abstraction reduces state space, not decisions</text>
            </svg>
        </div>
        <div style="flex:1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#C0392B; margin-bottom:6px;">Decision time vs object count</div>
            <svg viewBox="0 0 280 220" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <!-- Axes -->
                <line x1="40" y1="180" x2="270" y2="180" stroke="#888" stroke-width="1"/>
                <line x1="40" y1="20" x2="40" y2="180" stroke="#888" stroke-width="1"/>

                <!-- X-axis labels -->
                <text x="40" y="195" text-anchor="middle" fill="#666" font-size="8">4</text>
                <text x="95" y="195" text-anchor="middle" fill="#666" font-size="8">8</text>
                <text x="150" y="195" text-anchor="middle" fill="#666" font-size="8">12</text>
                <text x="205" y="195" text-anchor="middle" fill="#666" font-size="8">16</text>
                <text x="260" y="195" text-anchor="middle" fill="#666" font-size="8">20</text>
                <text x="155" y="210" text-anchor="middle" fill="#666" font-size="9">number of objects</text>

                <!-- Y-axis labels -->
                <text x="35" y="183" text-anchor="end" fill="#666" font-size="8">1s</text>
                <text x="35" y="143" text-anchor="end" fill="#666" font-size="8">1min</text>
                <text x="35" y="103" text-anchor="end" fill="#666" font-size="8">10min</text>
                <text x="35" y="63" text-anchor="end" fill="#666" font-size="8">30min</text>
                <text x="20" y="100" text-anchor="middle" fill="#666" font-size="9" transform="rotate(-90,20,100)">time per task</text>

                <!-- HOO-POMDP curve (steeply rising) -->
                <path d="M40,178 Q70,170 95,160 Q125,140 155,115 Q185,90 220,75 Q250,70 260,70" stroke="#5b6abf" stroke-width="2.5" fill="none"/>
                <circle cx="40" cy="178" r="3" fill="#5b6abf"/>
                <circle cx="95" cy="160" r="3" fill="#5b6abf"/>
                <circle cx="155" cy="115" r="3" fill="#5b6abf"/>
                <circle cx="220" cy="75" r="3" fill="#5b6abf"/>
                <circle cx="260" cy="70" r="4" fill="#5b6abf"/>
                <text x="265" y="62" text-anchor="end" fill="#5b6abf" font-size="9" font-weight="700">HOO-POMDP</text>
                <text x="265" y="73" text-anchor="end" fill="#5b6abf" font-size="8" font-style="italic">~30 min @ 20 obj</text>

                <!-- GammaZero curve (low and flat — projected) -->
                <path d="M40,176 Q90,172 140,168 Q180,164 220,162 Q250,161 260,160" stroke="#1E8449" stroke-width="2.5" fill="none" stroke-dasharray="5 3"/>
                <circle cx="40" cy="176" r="3" fill="#1E8449"/>
                <circle cx="260" cy="160" r="4" fill="#1E8449"/>
                <text x="265" y="156" text-anchor="end" fill="#1E8449" font-size="9" font-weight="700">GammaZero (Part 4)</text>
                <text x="265" y="167" text-anchor="end" fill="#1E8449" font-size="8" font-style="italic">orders of magnitude faster</text>

                <!-- Annotation -->
                <rect x="55" y="28" width="170" height="32" rx="4" fill="#FDECEA" stroke="#C0392B" stroke-width="1"/>
                <text x="140" y="42" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700">Random rollouts dominate</text>
                <text x="140" y="54" text-anchor="middle" fill="#5a2a23" font-size="8.5" font-style="italic">growth as plan depth × particles × leaves</text>
            </svg>
        </div>
    </div>

    <div style="margin-top: 12px; padding: 10px 14px; background: #FDECEA; border-radius: 8px; border-left: 4px solid #C0392B;">
        <p style="font-size: 0.92em; color: #5a2a23; line-height: 1.5; margin: 0;"><strong>The blue curve is real; the green is what comes next.</strong> The HOO-POMDP per-task time scales as plan depth times particles times random rollouts at every leaf. The state-space abstraction reduces one factor; the rollouts dominate the others. GammaZero replaces those rollouts with a single learned forward pass &mdash; the network predicts what the rollout average would have been, so the per-decision cost stops growing with rollout count.</p>
    </div>
</div>

<hr>

<h2>Summary</h2>

<p>HOO-POMDP shows that multi-object rearrangement under partial observability is tractable with the right decomposition:</p>

<ol>
<li><strong>Hierarchical planning</strong> separates strategic decisions from execution</li>
<li><strong>Object-oriented beliefs</strong> enable scalable uncertainty tracking</li>
<li><strong>Abstraction</strong> bridges continuous perception and discrete planning</li>
<li><strong>POMDP formulation</strong> provides principled handling of detector failures</li>
</ol>

<p>The result: a system that handles 20 objects across 4 rooms with blocked paths—scenarios where baselines completely fail.</p>

<hr>

<!-- Series Footer (Planning Under Uncertainty) -->
<div class="series-footer">
    <strong>Where this fits</strong>
    <p>HOO-POMDP is the <em>abstraction</em> strategy in the <em>Planning Under Uncertainty</em> series — shrink the problem until a principled POMDP planner can handle it. The next (and final) post, <a href="/blog/2026/planning-under-uncertainty-gammazero/">GammaZero</a>, takes the complementary <em>learning</em> strategy: keep the problem large, learn a GNN-based policy and value function that guide MCTS through it. Together, the two papers cover the two routes through the partial-observability barrier.</p>
    <p style="margin-top: 10px; font-size: 0.85em; color: #666;">← <a href="/blog/2026/planning-under-uncertainty-mcts-for-pomdps/">Part 2: MCTS for POMDPs</a> · <a href="/blog/2026/planning-under-uncertainty-gammazero/">Part 4: GammaZero (the finale) →</a></p>
</div>

<p><em>Paper: "Hierarchical Object-Oriented POMDP Planning for Object Rearrangement"</em></p>
<p><em>Authors: Rajesh Mangannavar, Alan Fern, Prasad Tadepalli (Oregon State University)</em></p>
<p><em>arXiv: <a href="https://arxiv.org/abs/2412.01348">2412.01348</a></em></p>

<hr>

</article>
