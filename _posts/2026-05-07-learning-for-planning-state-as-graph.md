---
layout: fullhtml-post
title: "Your Planning State Is a Graph"
date: 2026-05-07
categories: ["Learning for Planning"]
tags: ["planning", "gnn", "representation"]
description: "Why fixed-size vectors can't represent a planning problem, and what to do instead. Part 3 of the Learning for Planning series."
_styles: >
  .blog-fullhtml {
  font-family: 'Charter', 'Georgia', serif;
  line-height: 1.75;
  color: #1a1a1a;
  font-size: 18px;
  }
  .blog-fullhtml p { margin: 0 0 1.3em; }
  .blog-fullhtml ul, .blog-fullhtml ol { margin: 0 0 1.3em; }
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
  .blog-fullhtml .series-nav a:hover { color: #005856; }

  .blog-fullhtml .series-footer {
  margin: 3em 0 2em; padding: 20px 22px;
  background: #f0fafa; border: 1px solid #c2e4e3; border-radius: 10px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .series-footer strong { color: #00807E; }
  .blog-fullhtml .series-footer p { font-size: 0.9em; color: #444; margin: 8px 0 0; line-height: 1.6; }
  .blog-fullhtml .series-footer a { color: #00807E; text-decoration: none; border-bottom: 1px solid #00A3A155; }

  .blog-fullhtml .vis-container {
  margin: 28px -40px; padding: 22px 24px 16px;
  background: #fff; border: 1px solid #E8E4ED;
  border-radius: 14px; box-shadow: 0 4px 18px rgba(45,32,68,0.06);
  font-family: 'Source Sans 3', -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  @media (max-width: 800px) { .blog-fullhtml .vis-container { margin: 24px 0; } }
  .blog-fullhtml .vis-title { font-family: 'Playfair Display', Georgia, serif; font-size: 1.25rem; color: #2D2044; font-weight: 700; margin: 0; }
  .blog-fullhtml .vis-title::after { content:''; display:block; width:60px; height:3px; background:#C5A55A; margin-top:7px; border-radius:2px; }
  .blog-fullhtml .vis-subtitle { color: #888; font-size: 0.92em; margin-top: 6px; margin-bottom: 14px; font-style: italic; }

  .blog-fullhtml .stage-row { display: flex; align-items: center; gap: 12px; flex-wrap: wrap; margin-bottom: 12px; }
  .blog-fullhtml .stage-pill {
  font-family: 'JetBrains Mono', monospace; font-size: 0.78rem; font-weight: 700;
  padding: 5px 12px; border-radius: 18px;
  background: #F8F6FA; color: #6B5B7B; border: 1px solid #E8E4ED; cursor: pointer;
  transition: all 0.2s;
  }
  .blog-fullhtml .stage-pill.active { background: #2D2044; color: #C5A55A; border-color: #2D2044; }
  .blog-fullhtml .stage-pill.done { background: #E8F4F0; color: #1E8449; border-color: rgba(30,132,73,.3); }
  .blog-fullhtml .stage-pill:hover:not(.active) { background: #ECE7F2; }

  .blog-fullhtml .stage-desc {
  font-size: 0.92em; color: #2D2044; line-height: 1.5;
  padding: 10px 14px; background: #F8F6FA; border-left: 3px solid #C5A55A;
  border-radius: 0 6px 6px 0; margin-bottom: 14px; min-height: 44px;
  }
  .blog-fullhtml .stage-desc strong { color: #C5A55A; }

  .blog-fullhtml .graph-frame {
  background: #FDFCFE; border: 1px solid #D4CDE0; border-radius: 8px;
  overflow: hidden; min-height: 280px;
  }
  .blog-fullhtml .graph-frame svg { width: 100%; height: auto; max-height: 380px; display: block; }

  .blog-fullhtml .legend { display: flex; gap: 14px; flex-wrap: wrap; padding-top: 10px; font-size: 0.78em; color: #6B5B7B; }
  .blog-fullhtml .legend-item { display: flex; align-items: center; gap: 6px; }
  .blog-fullhtml .legend-swatch { width: 14px; height: 10px; border-radius: 2px; }
  .blog-fullhtml .legend-swatch.obj { background: rgba(176,142,42,.13); border: 1.2px solid #B08E2A; }
  .blog-fullhtml .legend-swatch.pred { background: rgba(192,57,43,.13); border: 1.2px solid #C0392B; }
  .blog-fullhtml .legend-swatch.goal { background: rgba(30,132,73,.13); border: 1.2px solid #1E8449; border-style: dashed; }
  .blog-fullhtml .legend-swatch.act { background: rgba(36,113,163,.13); border: 1.2px solid #2471A3; }

  .blog-fullhtml .controls { display: flex; gap: 8px; justify-content: center; padding-top: 12px; }
  .blog-fullhtml .controls button {
  font-family: 'Source Sans 3', sans-serif; padding: 6px 16px; font-size: 0.82rem; font-weight: 600;
  background: #E5DFE8; color: #2E1A38; border: 1px solid #C0A8CC; border-radius: 5px; cursor: pointer;
  }
  .blog-fullhtml .controls button:hover:not(:disabled) { background: #D4CDE0; }
  .blog-fullhtml .controls button:disabled { opacity: 0.35; cursor: default; }

  .blog-fullhtml .paper-tag { display: inline-block; font-family: 'JetBrains Mono', monospace; font-size: 0.78em; background: #2D2044; color: #C5A55A; padding: 1px 8px; border-radius: 3px; font-weight: 600; margin-right: 4px; }
  .blog-fullhtml .compare-table { width: 100%; border-collapse: collapse; font-size: 0.88em; margin: 12px 0; font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; }
  .blog-fullhtml .compare-table th { background: #F8F6FA; padding: 8px 10px; text-align: left; color: #2D2044; font-weight: 700; border-bottom: 2px solid #C5A55A; }
  .blog-fullhtml .compare-table td { padding: 7px 10px; vertical-align: top; border-bottom: 1px solid #eee; color: #333; }
  .blog-fullhtml .compare-table .yes { color: #1E8449; font-weight: 700; }
  .blog-fullhtml .compare-table .no { color: #C0392B; font-weight: 700; }

  .blog-fullhtml .refs { font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; }
  .blog-fullhtml .refs ol { padding-left: 1.3em; margin: 0; }
  .blog-fullhtml .refs li { font-size: 0.85em; color: #444; line-height: 1.55; margin-bottom: 8px; }
  .blog-fullhtml .refs li em { color: #444; }

  .blog-fullhtml .blog-container { max-width: 680px; margin: 40px auto; padding: 0 20px; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }

  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml .refs li { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .refs li em { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml h1 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml em { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml a { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml a:hover { color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .subtitle { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml hr { border-top-color: #2a3a3a; }
  html[data-theme="dark"] .blog-fullhtml code { background: #2c3237; color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .series-nav { background: #0f2625; border-left-color: #1e4a48; color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav strong { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .series-nav-links { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a { color: #4dd0ce; border-bottom-color: rgba(77,208,206,0.33); }
  html[data-theme="dark"] .blog-fullhtml .series-nav a:hover { color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .series-footer { background: #142a2a; border-color: #1e4a48; }
  html[data-theme="dark"] .blog-fullhtml .series-footer strong { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .series-footer p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-footer a { color: #4dd0ce; border-bottom-color: rgba(77,208,206,0.33); }
  html[data-theme="dark"] .blog-fullhtml .vis-container { background: #1e1a30; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .vis-title { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .vis-subtitle { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .vis-subtitle a { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .stage-pill { background: #15101e; color: #9888a8; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .stage-pill.active { background: #2a2410; color: #d4b870; border-color: #8e7e2c; }
  html[data-theme="dark"] .blog-fullhtml .stage-pill.done { background: #142a14; color: #5cbf5c; border-color: #1a6b1a; }
  html[data-theme="dark"] .blog-fullhtml .stage-pill:hover:not(.active) { background: #1a1530; }
  html[data-theme="dark"] .blog-fullhtml .stage-desc { background: #15101e; color: #c5aae8; border-left-color: #8e7e2c; }
  html[data-theme="dark"] .blog-fullhtml .stage-desc strong { color: #d4b870; }
  html[data-theme="dark"] .blog-fullhtml .graph-frame { background: #1a1530; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .legend { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .legend-swatch.obj { background: rgba(212,184,112,0.13); border-color: #8e7e2c; }
  html[data-theme="dark"] .blog-fullhtml .legend-swatch.pred { background: rgba(224,96,96,0.13); border-color: #5a3030; }
  html[data-theme="dark"] .blog-fullhtml .legend-swatch.goal { background: rgba(92,191,92,0.13); border-color: #1a6b1a; }
  html[data-theme="dark"] .blog-fullhtml .legend-swatch.act { background: rgba(106,175,230,0.13); border-color: #4682b4; }
  html[data-theme="dark"] .blog-fullhtml .controls button { background: #1e142e; color: #c5aae8; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .controls button:hover:not(:disabled) { background: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .paper-tag { background: #2a2410; color: #d4b870; }
  html[data-theme="dark"] .blog-fullhtml .compare-table th { background: #15101e; color: #c5aae8; border-bottom-color: #8e7e2c; }
  html[data-theme="dark"] .blog-fullhtml .compare-table td { color: #c9c9ca; border-bottom-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .compare-table .yes { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .compare-table .no { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #9888a8; border-top-color: #2a2540; }
---

<article class="blog-container">

<div class="series-nav">
    <strong>Learning for Planning &middot; Part 3 of 4 &mdash; the representation</strong>
    <div class="series-nav-links">
        &larr; <a href="/blog/2026/learning-for-planning-what-to-learn/">Part 2: What to Learn</a> &middot; Next: <a href="/blog/2026/learning-for-planning-gabar/">Part 4: GABAR &rarr;</a>
    </div>
</div>

<h1>Your Planning State Is a Graph</h1>
<p class="subtitle">Why fixed-size vectors can't represent a planning problem, and what to do instead.</p>

<hr>

<p>Part 2 ended with a claim: if you score actions instead of states, you can sidestep the "states have different sizes in different problems" issue &mdash; <em>as long as your network can read variable-size inputs.</em></p>

<p>This post is about the input. The whole trick of GABAR &mdash; and a recent wave of papers in learning-for-planning &mdash; is that a planning state is naturally a graph, and graph neural networks naturally process variable-size graphs with the same weights. The hard work is making the conversion principled, so that a 4-zone warehouse and a 4000-zone warehouse become graphs the same network can read.</p>

<h2>Why a vector won't do</h2>

<p>The temptation is always to flatten things. The state of a warehouse with <em>k</em> zones and <em>m</em> packages is "just" the position of the robot, the positions of all packages, and which (if any) the robot is holding. That's a vector of length <em>m</em>+2 if you allow integer-valued zone IDs.</p>

<p>Two problems with this. First, the length of the vector depends on <em>m</em> &mdash; the network trained on 1-package warehouses cannot read a 3-package warehouse's input. Second, even if you padded to a fixed maximum, the network has to learn that "the integer 3 in position 5" means "package 5 is in zone 3" &mdash; an entirely arbitrary indexing convention. There is nothing about the integers <em>per se</em> that tells the network zones C and D are physically adjacent.</p>

<p>The graph view fixes both. A planning state is <em>relational</em>: it's a set of objects with properties and binary relationships among them. That structure transfers across instance sizes intact &mdash; only the cardinality of the object set changes.</p>

<h2>Building the graph, one piece at a time</h2>

<p>The construction below is the one used in GABAR (the next post's subject). Each stage adds one type of node or edge. But first &mdash; here is the task itself. Press <strong>Watch Task</strong> to see the robot carry out the delivery; then, below, watch that same state turn into its graph form.</p>

<div class="vis-container" id="gw-demo" style="font-family:'Source Sans 3',-apple-system,'Helvetica Neue',Arial,sans-serif;">
    <h3 class="vis-title">Watch the task play out &mdash; warehouse delivery</h3>
    <div class="vis-subtitle">Fully observable: the robot knows where the package is. Robot in Zone A, package in Zone C, deliver to Zone D.</div>

    <div style="display:flex; gap:14px; align-items:stretch; flex-wrap:wrap;">
        <div style="flex:1; min-width:230px; display:flex; flex-direction:column;">
            <div style="font-size:.6rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#6B5B7B; margin-bottom:5px; text-align:center;">Start state</div>
            <div style="background:#FDFCFE; border:1px solid #D4CDE0; border-radius:9px; overflow:hidden;">
                <svg id="gw-startSvg" viewBox="0 0 280 205" preserveAspectRatio="xMidYMid meet" style="width:100%; height:auto; display:block;">
                    <g id="gw-startBg"></g><g id="gw-robotG" style="transition:transform 0.85s ease-in-out"></g>
                </svg>
            </div>
        </div>
        <div style="flex:1; min-width:230px; display:flex; flex-direction:column;">
            <div style="font-size:.6rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#1E8449; margin-bottom:5px; text-align:center;">Goal state</div>
            <div style="background:#FDFCFE; border:1px solid #D4CDE0; border-radius:9px; overflow:hidden;">
                <svg id="gw-goalSvg" viewBox="0 0 280 205" preserveAspectRatio="xMidYMid meet" style="width:100%; height:auto; display:block;"></svg>
            </div>
        </div>
    </div>

    <div id="gw-info" style="margin-top:12px; padding:9px 12px; background:#e0f5f5; border-radius:6px; text-align:center; font-size:0.9rem; font-weight:600; color:#00807E;">Press &ldquo;Watch Task&rdquo; to run the episode</div>

    <div style="display:flex; gap:10px; justify-content:center; align-items:center; padding-top:12px;">
        <button id="gw-demoB" style="font-family:inherit; padding:7px 18px; font-size:0.85rem; font-weight:700; background:#00807E; color:#fff; border:none; border-radius:6px; cursor:pointer;">&#9654; Watch Task</button>
        <button id="gw-resetB" style="font-family:inherit; padding:7px 16px; font-size:0.85rem; font-weight:600; background:#E5DFE8; color:#2E1A38; border:1px solid #C0A8CC; border-radius:6px; cursor:pointer;">Reset</button>
    </div>
    <p style="font-size:0.85em; color:#666; font-style:italic; margin-top:10px; text-align:center;">The four-step plan &mdash; <code style="font-style:normal;">move(A,C)</code>, <code style="font-style:normal;">pickup(Pkg,C)</code>, <code style="font-style:normal;">move(C,D)</code>, <code style="font-style:normal;">drop(Pkg,D)</code> &mdash; is exactly what the graph below is built to help a network produce.</p>
</div>

<script>
(function(){
    var co={muted:'#6B5B7B',dim:'#C0A8CC',robot:'#7B5E99',rD:'#4A3360',pkg:'#D68910',pkD:'#B7770A',goal:'#1E8449',gD:'#E3F5EC',floor:'#F0EDF3',border:'#D4CDE0',shelf:'#8B7FA0',shB:'#6B5B7B'};
    var CW=120,CH=80,GA=6,WW=280,GW=CW*2+GA,GH=CH*2+GA,GX=(WW-GW)/2,GY=6;
    var ShW=30,ShH=3,PkW=24,PkH=16;
    var cls=[{id:'a',r:0,c:0},{id:'b',r:0,c:1},{id:'c',r:1,c:0},{id:'d',r:1,c:1}].map(function(c){var x=GX+c.c*(CW+GA),y=GY+c.r*(CH+GA);return{id:c.id,x:x,y:y,cx:x+CW/2,cy:y+CH/2,ri:x+CW,bo:y+CH};});
    var CM={};cls.forEach(function(c){CM[c.id]=c;});
    function sfp(c){return{x:c.cx-ShW/2,y:c.y+12};}
    function pkp(c){var s=sfp(c);return{x:c.cx-PkW/2,y:s.y+ShH+2};}
    function cc(id){return{x:CM[id].cx,y:CM[id].cy+6};}
    var PP=pkp(CM.c),GP=pkp(CM.d);

    function drawBase(){
        var s='<defs><pattern id="gwl" width="13" height="13" patternUnits="userSpaceOnUse"><rect width="13" height="13" fill="'+co.floor+'"/><line x1="0" y1="13" x2="13" y2="13" stroke="'+co.border+'" stroke-width=".2" opacity=".2"/><line x1="13" y1="0" x2="13" y2="13" stroke="'+co.border+'" stroke-width=".2" opacity=".2"/></pattern></defs>';
        s+='<rect x="'+(GX-2)+'" y="'+(GY-2)+'" width="'+(GW+4)+'" height="'+(GH+4)+'" rx="4" fill="none" stroke="'+co.border+'" stroke-width=".8"/>';
        cls.forEach(function(c){var hs=c.id==='c'||c.id==='d';s+='<rect x="'+c.x+'" y="'+c.y+'" width="'+CW+'" height="'+CH+'" rx="2" fill="url(#gwl)" stroke="'+co.border+'" stroke-width=".4"/>';s+='<text x="'+c.cx+'" y="'+(c.bo-4)+'" text-anchor="middle" fill="'+co.muted+'" font-size="13" font-weight="600">Zone '+'ABCD'['abcd'.indexOf(c.id)]+'</text>';if(hs){var sp=sfp(c);s+='<g opacity=".45"><rect x="'+sp.x+'" y="'+sp.y+'" width="'+ShW+'" height="'+ShH+'" rx="1" fill="'+co.shelf+'" stroke="'+co.shB+'" stroke-width=".3"/></g>';}});
        [{f:'a',d:'h'},{f:'a',d:'v'},{f:'b',d:'v'},{f:'c',d:'h'}].forEach(function(j){var a=CM[j.f];if(j.d==='h')s+='<line x1="'+(a.ri+GA/2)+'" y1="'+(a.y+3)+'" x2="'+(a.ri+GA/2)+'" y2="'+(a.bo-3)+'" stroke="'+co.dim+'" stroke-width=".4" stroke-dasharray="4 3" opacity=".25"/>';else s+='<line x1="'+(a.x+3)+'" y1="'+(a.bo+GA/2)+'" x2="'+(a.ri-3)+'" y2="'+(a.bo+GA/2)+'" stroke="'+co.dim+'" stroke-width=".4" stroke-dasharray="4 3" opacity=".25"/>';});
        return s;
    }

    var demoActive=false,demoTimers=[],demoState=null;
    function clearT(){demoTimers.forEach(function(t){clearTimeout(t);});demoTimers=[];}
    function reset(){clearT();demoActive=false;demoState=null;ren();}
    function runDemo(){
        if(demoActive)return;demoActive=true;
        var pA=cc('a'),pC=cc('c'),pD=cc('d');
        var init={rx:pA.x,ry:pA.y,facing:1,held:false,placed:false,phase:'start',done:false};
        demoState=init;
        var steps=[[0,init],[300,{phase:'moving'}],[400,{rx:pC.x,ry:pC.y}],[1400,{phase:'reaching',facing:0}],[1800,{held:true,phase:'grabbing'}],[2100,{phase:'holding',facing:1}],[2400,{rx:pD.x,ry:pD.y,phase:'moving'}],[3300,{phase:'placing',facing:0}],[3700,{held:false,placed:true,phase:'placed'}],[4000,{done:true,phase:'done',facing:1}],[5600,null]];
        demoTimers=steps.map(function(st){return setTimeout(function(){if(!st[1]){demoActive=false;demoState=null;ren();return;}demoState=Object.assign({},demoState||init,st[1]);ren();},st[0]);});
    }

    function ren(){
        var da=demoActive,ds=demoState;
        var robX=da?ds.rx:cc('a').x,robY=da?ds.ry:cc('a').y,facing=da?(ds.facing||1):1,held=da&&ds.held,placed=da&&ds.placed;
        var showPkgC=!held&&!placed,showPkgD=da&&placed&&!held;
        var s=drawBase();
        if(showPkgC)s+='<rect x="'+PP.x+'" y="'+PP.y+'" width="'+PkW+'" height="'+PkH+'" rx="3" fill="'+co.pkg+'" stroke="'+co.pkD+'" stroke-width=".7"/><text x="'+(PP.x+PkW/2)+'" y="'+(PP.y+PkH/2+4)+'" text-anchor="middle" fill="#fff" font-size="8" font-weight="700">PKG</text>';
        if(showPkgD)s+='<rect x="'+GP.x+'" y="'+GP.y+'" width="'+PkW+'" height="'+PkH+'" rx="3" fill="'+co.pkg+'" stroke="'+co.pkD+'" stroke-width=".7"/><text x="'+(GP.x+PkW/2)+'" y="'+(GP.y+PkH/2+4)+'" text-anchor="middle" fill="#fff" font-size="8" font-weight="700">PKG</text>';
        if(!showPkgD)s+='<rect x="'+GP.x+'" y="'+GP.y+'" width="'+PkW+'" height="'+PkH+'" rx="3" fill="'+co.gD+'" stroke="'+co.goal+'" stroke-width="1.5" stroke-dasharray="4 3" opacity=".6"/><text x="'+(GP.x+PkW/2)+'" y="'+(GP.y+PkH/2+3)+'" text-anchor="middle" fill="'+co.goal+'" font-size="7" font-weight="700" opacity=".7">GOAL</text>';
        if(da&&ds&&ds.done)s+='<circle cx="'+CM.d.cx+'" cy="'+(GP.y+PkH+9)+'" r="6" fill="'+co.goal+'" opacity=".9"/><polyline points="'+(CM.d.cx-2.5)+','+(GP.y+PkH+9)+' '+(CM.d.cx-.3)+','+(GP.y+PkH+11.5)+' '+(CM.d.cx+3)+','+(GP.y+PkH+6)+'" stroke="#fff" stroke-width="1.4" fill="none" stroke-linecap="round"/>';
        s+='<text x="'+(WW/2)+'" y="200" text-anchor="middle" fill="'+co.dim+'" font-size="11" font-weight="600">Robot in A, package on C</text>';
        document.getElementById('gw-startBg').innerHTML=s;
        var rg=document.getElementById('gw-robotG');rg.style.transform='translate('+robX+'px,'+robY+'px)';
        var r='<line x1="0" y1="-13" x2="0" y2="-10" stroke="'+co.rD+'" stroke-width="1"/><circle cx="0" cy="-14" r="1.3" fill="'+co.rD+'"/><rect x="-7" y="-10" width="14" height="8" rx="2.8" fill="'+co.robot+'" stroke="'+co.rD+'" stroke-width=".5"/><circle cx="'+(facing>=0?-1.7:-3.3)+'" cy="-6.5" r="1.4" fill="#E6E0EC"/><circle cx="'+(facing>=0?3.3:1.7)+'" cy="-6.5" r="1.4" fill="#E6E0EC"/><rect x="-8" y="-2" width="16" height="12" rx="2.2" fill="'+co.robot+'" stroke="'+co.rD+'" stroke-width=".5"/><rect x="-10" y="-1" width="2" height="7" rx="1" fill="'+co.rD+'" opacity=".7"/><rect x="8" y="-1" width="2" height="7" rx="1" fill="'+co.rD+'" opacity=".7"/><rect x="-4.5" y="10" width="2.2" height="4.5" rx=".8" fill="'+co.rD+'" opacity=".6"/><rect x="2.3" y="10" width="2.2" height="4.5" rx=".8" fill="'+co.rD+'" opacity=".6"/>';
        if(held)r+='<rect x="'+(facing>=0?10:-10-PkW+5)+'" y="'+(-PkH/2)+'" width="'+(PkW-4)+'" height="'+(PkH-3)+'" rx="2" fill="'+co.pkg+'" stroke="'+co.pkD+'" stroke-width=".4"/><text x="'+(facing>=0?10+(PkW-4)/2:-10-(PkW-4)/2+5)+'" y="2" text-anchor="middle" fill="#fff" font-size="7" font-weight="700">PKG</text>';
        rg.innerHTML=r;
        var gs=drawBase();
        gs+='<rect x="'+GP.x+'" y="'+GP.y+'" width="'+PkW+'" height="'+PkH+'" rx="3" fill="'+co.pkg+'" stroke="'+co.pkD+'" stroke-width=".7"/><text x="'+(GP.x+PkW/2)+'" y="'+(GP.y+PkH/2+4)+'" text-anchor="middle" fill="#fff" font-size="8" font-weight="700">PKG</text>';
        gs+='<rect x="'+(CM.d.cx-28)+'" y="'+(GP.y+PkH+3)+'" width="56" height="15" rx="4" fill="'+co.gD+'" stroke="'+co.goal+'" stroke-width=".6"/><text x="'+CM.d.cx+'" y="'+(GP.y+PkH+13)+'" text-anchor="middle" fill="'+co.goal+'" font-size="9" font-weight="700">&#x2713; DELIVERED</text>';
        var rp2=cc('a');gs+='<g transform="translate('+rp2.x+','+rp2.y+')" opacity=".22"><line x1="0" y1="-13" x2="0" y2="-10" stroke="'+co.robot+'" stroke-width=".8"/><circle cx="0" cy="-14" r="1.1" fill="'+co.robot+'"/><rect x="-6" y="-10" width="12" height="7" rx="2.5" fill="'+co.robot+'"/><circle cx="-1.7" cy="-6.5" r="1.2" fill="#E6E0EC"/><circle cx="1.7" cy="-6.5" r="1.2" fill="#E6E0EC"/><rect x="-7" y="-2" width="14" height="11" rx="2" fill="'+co.robot+'"/></g>';
        gs+='<text x="'+(WW/2)+'" y="200" text-anchor="middle" fill="'+co.dim+'" font-size="11" font-weight="600">Package delivered to Zone D</text>';
        document.getElementById('gw-goalSvg').innerHTML=gs;
        var info=document.getElementById('gw-info');
        if(da){var ph=ds?ds.phase:'',msg=ds&&ds.done?'✓ Delivered!':ph==='moving'?'Moving…':ph==='reaching'?'Reaching for the package…':ph==='grabbing'?'Picking up…':ph==='holding'?'Holding the package…':ph==='placing'?'Placing…':ph==='placed'?'Placed':'Starting…';info.textContent=msg;}
        else info.textContent='Press “Watch Task” to run the episode';
        var b=document.getElementById('gw-demoB');b.disabled=da;b.style.opacity=da?'.5':'1';b.style.cursor=da?'default':'pointer';
    }
    document.getElementById('gw-demoB').onclick=runDemo;
    document.getElementById('gw-resetB').onclick=reset;
    ren();
})();
</script>

<div class="vis-container" id="vis-graph">
    <h3 class="vis-title">State &rarr; Graph &mdash; warehouse delivery</h3>
    <div class="vis-subtitle">Robot in Zone A, package on Zone C, goal: deliver to Zone D.</div>

    <div class="stage-row" id="vis-stages">
        <div class="stage-pill active" data-stage="0">0 &middot; Problem</div>
        <div class="stage-pill" data-stage="1">1 &middot; Objects</div>
        <div class="stage-pill" data-stage="2">2 &middot; Predicates</div>
        <div class="stage-pill" data-stage="3">3 &middot; Goal Preds</div>
        <div class="stage-pill" data-stage="4">4 &middot; Action Schemas</div>
    </div>

    <div class="stage-desc" id="vis-desc">A robot in Zone A must deliver a package from Zone C to Zone D. Click through to see how this state becomes a graph the GNN can read.</div>

    <div class="graph-frame">
        <svg id="vis-graph-svg" viewBox="0 0 700 320" preserveAspectRatio="xMidYMid meet"></svg>
    </div>

    <div class="controls">
        <button id="vis-prev" disabled>&larr; Prev</button>
        <button id="vis-next">Next &rarr;</button>
        <button id="vis-reset">Reset</button>
    </div>

    <div class="legend">
        <div class="legend-item"><div class="legend-swatch obj"></div>Object</div>
        <div class="legend-item"><div class="legend-swatch pred"></div>Predicate</div>
        <div class="legend-item"><div class="legend-swatch goal"></div>Goal Pred.</div>
        <div class="legend-item"><div class="legend-swatch act"></div>Action Schema</div>
    </div>
</div>

<script>
(function(){
    var co = {action:'#2471A3', aD:'#E8F0F8', object:'#B08E2A', oD:'#FDF6E3', pred:'#C0392B', pD:'#FDECEA', goal:'#1E8449', gD:'#E3F5EC', text:'#2E1A38', muted:'#6B5B7B', dim:'#C0A8CC'};
    var tC = {object:co.object, pred:co.pred, goal:co.goal, action:co.action};
    var tD = {object:co.oD, pred:co.pD, goal:co.gD, action:co.aD};

    var STAGES = [
        {t:'Stage 0 &middot; Problem', s:'A robot in Zone A must deliver a package from Zone C to Zone D. Click through to see how this state becomes a graph the GNN can read.'},
        {t:'Stage 1 &middot; Objects', s:'<strong>Every entity becomes an object node.</strong> The robot, the package, and the four zones are all object nodes. The network treats them all the same way at this stage &mdash; they are just typed identifiers.'},
        {t:'Stage 2 &middot; Predicates', s:'<strong>Current-state facts become predicate nodes,</strong> each connected to the objects it mentions. <code>at(Bot, A)</code>, <code>handempty</code>, and <code>on(Pkg, C)</code> say what is true right now.'},
        {t:'Stage 3 &middot; Goal Predicates', s:'<strong>The goal becomes a special node too</strong> &mdash; <code>on(Pkg, D)</code>, drawn with a dashed border. It is connected to the same object nodes, so the network can compare "where the package is" with "where it should be."'},
        {t:'Stage 4 &middot; Action Schemas', s:'<strong>Ungrounded action types are added as schema nodes.</strong> <code>move</code> connects to whatever can move (the robot) and to locations; <code>pickup</code> connects to the robot and to objects that can be picked up. The same schemas appear in every warehouse instance &mdash; only the bindings change.'}
    ];

    var NODES = [
        {id:'robot', x:80, y:170, l:'Robot', t:'object', s:1},
        {id:'rm_a',  x:185, y:170, l:'Zone A', t:'object', s:1},
        {id:'rm_b',  x:290, y:170, l:'Zone B', t:'object', s:1},
        {id:'rm_c',  x:395, y:170, l:'Zone C', t:'object', s:1},
        {id:'pkg',   x:500, y:170, l:'Pkg', t:'object', s:1},
        {id:'rm_d',  x:610, y:170, l:'Zone D', t:'object', s:1},
        {id:'at_bot',  x:120, y:265, l:'at(Bot,A)', t:'pred', s:2},
        {id:'hand_e',  x:270, y:265, l:'handempty', t:'pred', s:2},
        {id:'on_pkg',  x:445, y:265, l:'on(Pkg,C)', t:'pred', s:2},
        {id:'goal_pkg', x:615, y:265, l:'on(Pkg,D)', t:'goal', s:3},
        {id:'move',  x:185, y:60, l:'move', t:'action', s:4},
        {id:'pick',  x:395, y:60, l:'pickup', t:'action', s:4}
    ];

    var EDGES = [
        {f:'at_bot', t:'robot', s:2, ty:'pred'},
        {f:'at_bot', t:'rm_a', s:2, ty:'pred'},
        {f:'hand_e', t:'robot', s:2, ty:'pred'},
        {f:'on_pkg', t:'pkg', s:2, ty:'pred'},
        {f:'on_pkg', t:'rm_c', s:2, ty:'pred'},
        {f:'goal_pkg', t:'pkg', s:3, ty:'goal'},
        {f:'goal_pkg', t:'rm_d', s:3, ty:'goal'},
        {f:'move', t:'robot', s:4, ty:'action'},
        {f:'move', t:'rm_a', s:4, ty:'action'},
        {f:'move', t:'rm_b', s:4, ty:'action'},
        {f:'pick', t:'robot', s:4, ty:'action'},
        {f:'pick', t:'pkg', s:4, ty:'action'}
    ];

    var NM = {}; NODES.forEach(function(n){ NM[n.id] = n; });
    var maxStage = 4, stage = 0;

    function render(){
        document.getElementById('vis-desc').innerHTML = STAGES[stage].s;
        document.querySelectorAll('#vis-stages .stage-pill').forEach(function(el){
            var s = parseInt(el.getAttribute('data-stage'));
            el.classList.remove('active','done');
            if (s === stage) el.classList.add('active');
            else if (s < stage) el.classList.add('done');
        });
        document.getElementById('vis-prev').disabled = (stage === 0);
        document.getElementById('vis-next').disabled = (stage === maxStage);

        var g = '';
        if (stage >= 4) g += '<text x="14" y="48" fill="' + co.action + '" font-size="13" font-weight="700" opacity=".4">Action Schemas</text>';
        if (stage >= 1) g += '<text x="14" y="174" fill="' + co.object + '" font-size="13" font-weight="700" opacity=".4">Objects</text>';
        if (stage >= 2) g += '<text x="14" y="269" fill="' + co.pred + '" font-size="13" font-weight="700" opacity=".4">Predicates</text>';

        EDGES.forEach(function(e){
            if (e.s > stage) return;
            var f = NM[e.f], t = NM[e.t];
            g += '<line x1="' + f.x + '" y1="' + f.y + '" x2="' + t.x + '" y2="' + t.y + '" stroke="' + tC[e.ty] + '" stroke-width="1.4" stroke-opacity=".25" stroke-dasharray="' + (e.ty === 'goal' ? '6 4' : 'none') + '"/>';
        });

        NODES.forEach(function(n){
            if (n.s > stage) return;
            var c = tC[n.t], bg = tD[n.t], isObj = n.t === 'object', isGoal = n.t === 'goal';
            var w = isObj ? 0 : Math.max(n.l.length * 8 + 24, isGoal ? 110 : 90);
            if (isObj) {
                g += '<circle cx="' + n.x + '" cy="' + n.y + '" r="26" fill="' + bg + '" stroke="' + c + '" stroke-width="1.5"/>';
            } else {
                g += '<rect x="' + (n.x - w/2) + '" y="' + (n.y - 17) + '" width="' + w + '" height="34" rx="7" fill="' + bg + '" stroke="' + c + '" stroke-width="1.5" stroke-dasharray="' + (isGoal ? '5 3' : 'none') + '"/>';
            }
            if (isGoal) g += '<text x="' + n.x + '" y="' + (n.y - 4) + '" text-anchor="middle" fill="' + c + '" font-size="9" opacity=".65">GOAL</text>';
            g += '<text x="' + n.x + '" y="' + (isGoal ? n.y + 9 : n.y + 4.5) + '" text-anchor="middle" fill="' + c + '" font-size="13" font-weight="600">' + n.l + '</text>';
        });

        if (stage === 0) {
            g += '<rect x="220" y="120" width="260" height="80" rx="10" fill="' + co.oD + '" stroke="' + co.muted + '" stroke-width="1.5" stroke-dasharray="6 4" opacity=".6"/>';
            g += '<text x="350" y="155" text-anchor="middle" fill="' + co.muted + '" font-size="14" font-style="italic">A planning state.</text>';
            g += '<text x="350" y="180" text-anchor="middle" fill="' + co.muted + '" font-size="11">Click <strong>Next &rarr;</strong> to build its graph.</text>';
        }

        document.getElementById('vis-graph-svg').innerHTML = g;
    }

    document.getElementById('vis-next').addEventListener('click', function(){ if (stage < maxStage) { stage++; render(); } });
    document.getElementById('vis-prev').addEventListener('click', function(){ if (stage > 0) { stage--; render(); } });
    document.getElementById('vis-reset').addEventListener('click', function(){ stage = 0; render(); });
    document.querySelectorAll('#vis-stages .stage-pill').forEach(function(el){
        el.addEventListener('click', function(){ stage = parseInt(el.getAttribute('data-stage')); render(); });
    });

    render();
})();
</script>

<h2>What you just saw, conceptually</h2>

<p>The graph has four kinds of nodes, by design:</p>

<ul>
    <li><strong>Objects</strong> &mdash; the things in the problem (robot, packages, zones).</li>
    <li><strong>Predicates</strong> &mdash; the facts that hold right now, each connected to the objects it concerns.</li>
    <li><strong>Goal predicates</strong> &mdash; the facts that should hold at the end, drawn separately.</li>
    <li><strong>Action schemas</strong> &mdash; ungrounded actions (<code>move</code>, <code>pickup</code>, <code>drop</code>), each connected to the objects that fit their parameter types.</li>
</ul>

<p>Crucially, the <em>structure</em> of this graph is determined by the PDDL domain, not by the instance. A 4-zone warehouse and a 4000-zone warehouse have the same node types, the same edge types, the same action schemas. The graphs differ only in size &mdash; the 4000-zone version has more <code>Zone</code> object nodes, more <code>At</code> predicates, more <code>Move</code> schemas grounded over more parameters.</p>

<p>A graph neural network processes this. Briefly: each node has a learned embedding, and on each "round" of message passing, every node updates its embedding by aggregating its neighbors'. After a few rounds, the action-schema nodes have absorbed enough context to rank themselves &mdash; which action looks most useful in this state? That ranking is the policy.</p>

<p>The number of rounds is fixed; the depth of the network is fixed. What scales with the problem is the <em>width</em> of each round &mdash; how many nodes get updated &mdash; which is handled by the graph framework, not by adding parameters to the model. <em>The same weights work on any size warehouse.</em></p>

<p>The animation above shows GABAR's specific graph encoding, but it's only one of many possible constructions. The rest of this post is a survey of the choices the field has explored, organized along three concrete sub-decisions every graph-based L4P method has to make.</p>

<hr>

<h2>Sub-decision 1: lifted vs grounded</h2>

<p>The first design choice is how much instantiation to put into the graph. There are two extremes.</p>

<p>A <strong>lifted</strong> graph encodes objects and the relations among them, but not the full set of instantiated atoms. For a warehouse with 50 packages and 100 zones, the lifted graph has nodes for each object (the robot, each package, each zone) and edges encoding their relationships, but does <em>not</em> have a separate node for every ground atom like <code>at(robot, zone-37)</code>. The action schemas appear as well, but ungrounded — the schema for <code>move</code>, not all the move(?from, ?to) instantiations.</p>

<p>A <strong>grounded</strong> graph instantiates the propositional structure: every ground atom that holds in the current state (or could hold) gets its own node. Every ground action (every instantiation of every schema) also gets its own node. The graph is much larger — for the same warehouse it might have thousands of ground-atom nodes and hundreds of ground-action nodes — but the structure is much closer to what the planner actually reasons about.</p>

<p>The lifted-vs-grounded debate has been open for years. Lifted graphs are smaller and faster to construct; grounded graphs are more expressive. <span class="paper-tag">Chen, Thiébaux, Trevizan 2024</span> in GOOSE tested both head-to-head and found grounded graphs outperform lifted ones on benchmark IPC domains when paired with standard message-passing networks. The follow-up survey by <span class="paper-tag">Chen, Hao, Thiébaux, Trevizan 2024</span> formalizes this: grounded encodings are strictly more expressive in a precise sense, though they incur higher per-instance computational cost.</p>

<p>The expressivity result connects to a deeper limitation. <span class="paper-tag">Barceló, Kostylev, Monet, Pérez, Reutter, Silva 2020</span> proved that standard message-passing GNNs are expressively bounded by the C<sub>2</sub> fragment of first-order logic (graded modal logic). This means there are planning problems where the relationships needed to pick the right action <em>cannot be expressed</em> by vanilla GNNs over a lifted graph; the message-passing limit is the bottleneck, not the data. Going grounded gives the graph richer structure that partially side-steps this. Going beyond GNNs to architectures with more expressive aggregations is the other escape route (<span class="paper-tag">Ståhlberg, Bonet, Geffner 2024</span>).</p>

<p>GABAR sits at the grounded end of this spectrum, with one notable variant: it grounds <em>predicates</em> (instantiated atoms get nodes) but keeps <em>action schemas</em> ungrounded. The action schema nodes connect to the objects that could serve as parameters, with edge features encoding which parameter slot. This hybrid keeps the action-side graph small while the predicate-side gets the expressivity benefit of grounding.</p>

<div class="vis-container">
    <h3 class="vis-title">Lifted vs grounded encoding &mdash; same warehouse, two graphs</h3>
    <div class="vis-subtitle">Same 4-zone state: robot in A, package on C, goal to deliver to D. Lifted reads "objects and their schemas." Grounded reads "every fact that holds, instantiated."</div>

    <div style="display:flex; gap:14px; align-items:stretch;">
        <div style="flex:1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#6B5B7B; margin-bottom:6px;">Lifted graph &middot; 6 nodes</div>
            <svg viewBox="0 0 280 220" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <circle cx="50" cy="50" r="22" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.4"/>
                <text x="50" y="54" text-anchor="middle" fill="#B08E2A" font-size="11" font-weight="600">Robot</text>
                <circle cx="140" cy="40" r="22" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.4"/>
                <text x="140" y="44" text-anchor="middle" fill="#B08E2A" font-size="11" font-weight="600">Zone A</text>
                <circle cx="230" cy="50" r="22" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.4"/>
                <text x="230" y="54" text-anchor="middle" fill="#B08E2A" font-size="11" font-weight="600">Zone B</text>
                <circle cx="50" cy="160" r="22" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.4"/>
                <text x="50" y="164" text-anchor="middle" fill="#B08E2A" font-size="11" font-weight="600">Zone C</text>
                <circle cx="140" cy="170" r="22" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.4"/>
                <text x="140" y="174" text-anchor="middle" fill="#B08E2A" font-size="11" font-weight="600">Zone D</text>
                <circle cx="230" cy="160" r="22" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.4"/>
                <text x="230" y="164" text-anchor="middle" fill="#B08E2A" font-size="11" font-weight="600">Pkg</text>

                <!-- A few abstract edges (object relations) -->
                <line x1="50" y1="72" x2="50" y2="138" stroke="#C0A8CC" stroke-width="1" stroke-dasharray="3 2" opacity=".5"/>
                <line x1="140" y1="62" x2="50" y2="138" stroke="#C0A8CC" stroke-width="1" stroke-dasharray="3 2" opacity=".5"/>
                <line x1="140" y1="62" x2="230" y2="138" stroke="#C0A8CC" stroke-width="1" stroke-dasharray="3 2" opacity=".5"/>

                <text x="140" y="208" text-anchor="middle" fill="#888" font-size="9" font-style="italic">objects + abstract relations</text>
            </svg>
            <div style="font-size:.78em; color:#888; margin-top:6px; text-align:center;"><strong>4 zones × 1 pkg:</strong> 6 nodes</div>
            <div style="font-size:.78em; color:#888; text-align:center;"><strong>16 zones × 3 pkg:</strong> 20 nodes</div>
        </div>
        <div style="flex:1.3; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#2471A3; margin-bottom:6px;">Grounded graph &middot; 9 nodes (this state) + more potentials</div>
            <svg viewBox="0 0 360 220" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <!-- Objects (smaller) -->
                <circle cx="40" cy="40" r="16" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.2"/>
                <text x="40" y="43" text-anchor="middle" fill="#B08E2A" font-size="9" font-weight="600">R</text>
                <circle cx="110" cy="40" r="16" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.2"/>
                <text x="110" y="43" text-anchor="middle" fill="#B08E2A" font-size="9" font-weight="600">A</text>
                <circle cx="180" cy="40" r="16" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.2"/>
                <text x="180" y="43" text-anchor="middle" fill="#B08E2A" font-size="9" font-weight="600">B</text>
                <circle cx="250" cy="40" r="16" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.2"/>
                <text x="250" y="43" text-anchor="middle" fill="#B08E2A" font-size="9" font-weight="600">C</text>
                <circle cx="320" cy="40" r="16" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.2"/>
                <text x="320" y="43" text-anchor="middle" fill="#B08E2A" font-size="9" font-weight="600">D</text>

                <!-- Ground predicate nodes (current state) -->
                <rect x="20" y="110" width="78" height="22" rx="4" fill="#FDECEA" stroke="#C0392B" stroke-width="1.2"/>
                <text x="59" y="124" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="600">at(R, A)</text>
                <rect x="108" y="110" width="88" height="22" rx="4" fill="#FDECEA" stroke="#C0392B" stroke-width="1.2"/>
                <text x="152" y="124" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="600">handempty</text>
                <rect x="206" y="110" width="78" height="22" rx="4" fill="#FDECEA" stroke="#C0392B" stroke-width="1.2"/>
                <text x="245" y="124" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="600">on(P, C)</text>
                <rect x="294" y="110" width="60" height="22" rx="4" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.2"/>
                <text x="324" y="124" text-anchor="middle" fill="#B08E2A" font-size="9" font-weight="600">P</text>

                <!-- Goal ground -->
                <rect x="120" y="155" width="100" height="22" rx="4" fill="#E3F5EC" stroke="#1E8449" stroke-width="1.2" stroke-dasharray="3 2"/>
                <text x="170" y="169" text-anchor="middle" fill="#1E8449" font-size="9" font-weight="700">G: on(P, D)</text>

                <!-- Edges (some) -->
                <line x1="40" y1="56" x2="40" y2="110" stroke="#C0392B" stroke-width=".8" stroke-opacity=".4"/>
                <line x1="78" y1="110" x2="110" y2="56" stroke="#C0392B" stroke-width=".8" stroke-opacity=".4"/>
                <line x1="152" y1="110" x2="40" y2="56" stroke="#C0392B" stroke-width=".8" stroke-opacity=".4"/>
                <line x1="245" y1="110" x2="250" y2="56" stroke="#C0392B" stroke-width=".8" stroke-opacity=".4"/>
                <line x1="245" y1="110" x2="324" y2="110" stroke="#C0392B" stroke-width=".8" stroke-opacity=".4"/>
                <line x1="120" y1="160" x2="324" y2="120" stroke="#1E8449" stroke-width=".8" stroke-opacity=".4" stroke-dasharray="3 2"/>
                <line x1="220" y1="160" x2="320" y2="56" stroke="#1E8449" stroke-width=".8" stroke-opacity=".4" stroke-dasharray="3 2"/>

                <text x="180" y="205" text-anchor="middle" fill="#888" font-size="9" font-style="italic">objects + every ground fact that holds, instantiated</text>
            </svg>
            <div style="font-size:.78em; color:#888; margin-top:6px; text-align:center;"><strong>4 zones × 1 pkg:</strong> ~10 nodes</div>
            <div style="font-size:.78em; color:#888; text-align:center;"><strong>16 zones × 3 pkg:</strong> ~80+ nodes</div>
        </div>
    </div>

    <div style="margin-top: 12px; padding: 10px 14px; background: #F8F6FA; border-radius: 8px; border-left: 4px solid #C5A55A;">
        <p style="font-size: 0.92em; color: #2D2044; line-height: 1.5; margin: 0;">The grounded graph carries explicit "<code>on(P, C)</code> is true" structure that the lifted graph would have to infer. <span class="paper-tag">Chen, Thiébaux, Trevizan 2024</span> show this extra structure beats lifted-only encodings on benchmark domains. The cost is graph size &mdash; at 16 zones × 3 packages, grounding adds 80+ predicate nodes &mdash; but GNN compute scales gracefully with node count.</p>
    </div>
</div>

<hr>

<h2>Sub-decision 2: how to represent actions</h2>

<p>The single most consequential design choice in the field — and the central technical contribution of GABAR — is whether actions appear in the graph at all, and if so, how.</p>

<p>Three patterns exist in the literature:</p>

<h3>Pattern 1: actions are implicit (no action nodes)</h3>

<p>Most graph-based L4P methods represent only objects and predicates as nodes. The model must <em>infer action-relevant information from the predicate and object nodes</em>, essentially reverse-engineering each action schema's preconditions from the current state. <span class="paper-tag">Ståhlberg, Bonet, Geffner 2022a</span> (GPL) and <span class="paper-tag">Karia, Srivastava 2021</span> (GRAPL, in its base form) both fall here.</p>

<p>The cost: the network has to learn the preconditions implicitly, which requires more training data and more model capacity. For domains with many action schemas (Logistics, IPC composite domains), the implicit approach scales poorly because each schema's precondition pattern needs to be re-discovered.</p>

<h3>Pattern 2: actions woven in via alternating layers</h3>

<p>ASNets (<span class="paper-tag">Toyer, Thiébaux, Trevizan, Xie 2020</span>) takes a structurally distinct approach: alternate action layers and proposition layers, with weight sharing across all groundings of the same schema. An action layer has one unit per ground action; a proposition layer has one unit per ground atom. Each action unit attends to its precondition propositions; each proposition unit attends to the actions that affect it. After several alternations, action units have absorbed information from a fixed-depth neighborhood.</p>

<p>This is more inductive-bias-rich than the implicit pattern — actions are first-class — but the fixed alternation depth caps the network's receptive field. Long-horizon dependencies (effects rippling through many propositions) exceed what a small stack can model. ASNets does well on local domains but struggles on long-chain reasoning.</p>

<h3>Pattern 3: actions as graph nodes (explicit)</h3>

<p>GABAR represents action schemas as first-class nodes in the graph, connected by edges to the objects that could serve as their parameters. The edges encode both the parameter position (which role the object plays) and the predicate satisfaction (which of the object's properties make the action applicable).</p>

<p>The advantage: <em>the model directly reads the relationships between actions and the objects they manipulate</em>, rather than inferring them. This is particularly powerful when paired with the action-ranking objective from Part 2 — the action schema nodes' final embeddings serve as the scoring vectors for each candidate action.</p>

<p>The cost: more nodes per graph, more edges, larger memory footprint. In practice this is dominated by the gain in learning efficiency.</p>

<h3>Pattern 4: hypergraphs</h3>

<p>STRIPS-HGN (<span class="paper-tag">Shen, Trevizan, Thiébaux 2020</span>) is a separate point in this space: actions are <em>hyperedges</em> connecting multiple proposition nodes (one per precondition or effect), and message passing operates over the hypergraph. This naturally captures the multi-precondition structure of STRIPS actions without the alternation depth limit of ASNets.</p>

<p>Hypergraph networks are more expressive than ordinary GNNs for certain planning patterns, but the architectural machinery is heavier and less well-supported by standard tooling. STRIPS-HGN's empirical results are strong on small instances but it has not scaled to the largest IPC problem sizes that grounded GNN approaches like GOOSE have hit.</p>

<div class="vis-container">
    <h3 class="vis-title">Four ways to put actions in the warehouse graph</h3>
    <div class="vis-subtitle">Same state. Four different encodings. The model has to read the same action precondition structure in each, but the structure is exposed differently.</div>

    <div style="display:grid; grid-template-columns: 1fr 1fr; gap:12px;">
        <!-- Pattern 1: Implicit -->
        <div style="border:1.5px solid #D4CDE0; border-radius:9px; padding:10px; background:#fff;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#6B5B7B;">Pattern 1 · Implicit (GPL)</div>
            <div style="font-family:'Playfair Display',Georgia,serif; font-size:.92rem; color:#2D2044; font-weight:700; margin-top:2px;">No action nodes</div>
            <svg viewBox="0 0 240 130" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto; margin-top:4px;">
                <circle cx="40" cy="30" r="14" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                <text x="40" y="33" text-anchor="middle" fill="#B08E2A" font-size="8" font-weight="600">Robot</text>
                <circle cx="100" cy="30" r="14" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                <text x="100" y="33" text-anchor="middle" fill="#B08E2A" font-size="8" font-weight="600">A</text>
                <circle cx="160" cy="30" r="14" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                <text x="160" y="33" text-anchor="middle" fill="#B08E2A" font-size="8" font-weight="600">C</text>
                <circle cx="220" cy="30" r="14" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                <text x="220" y="33" text-anchor="middle" fill="#B08E2A" font-size="8" font-weight="600">Pkg</text>
                <rect x="50" y="65" width="60" height="18" rx="3" fill="#FDECEA" stroke="#C0392B" stroke-width="1"/>
                <text x="80" y="77" text-anchor="middle" fill="#C0392B" font-size="8" font-weight="600">at(R,A)</text>
                <rect x="135" y="65" width="60" height="18" rx="3" fill="#FDECEA" stroke="#C0392B" stroke-width="1"/>
                <text x="165" y="77" text-anchor="middle" fill="#C0392B" font-size="8" font-weight="600">on(P,C)</text>
                <line x1="40" y1="44" x2="55" y2="65" stroke="#C0392B" stroke-width=".8" stroke-opacity=".4"/>
                <line x1="100" y1="44" x2="100" y2="65" stroke="#C0392B" stroke-width=".8" stroke-opacity=".4"/>
                <line x1="160" y1="44" x2="160" y2="65" stroke="#C0392B" stroke-width=".8" stroke-opacity=".4"/>
                <line x1="220" y1="44" x2="190" y2="65" stroke="#C0392B" stroke-width=".8" stroke-opacity=".4"/>
                <text x="120" y="115" text-anchor="middle" fill="#C0392B" font-size="9" font-style="italic" font-weight="600">Model must infer precondition</text>
                <text x="120" y="125" text-anchor="middle" fill="#C0392B" font-size="9" font-style="italic" font-weight="600">structure from these alone</text>
            </svg>
        </div>

        <!-- Pattern 2: Alternating (ASNets) -->
        <div style="border:1.5px solid #D4CDE0; border-radius:9px; padding:10px; background:#fff;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#6B5B7B;">Pattern 2 · Alternating layers (ASNets)</div>
            <div style="font-family:'Playfair Display',Georgia,serif; font-size:.92rem; color:#2D2044; font-weight:700; margin-top:2px;">Action ↔ proposition layers</div>
            <svg viewBox="0 0 240 130" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto; margin-top:4px;">
                <!-- Stacked layers -->
                <rect x="20" y="10" width="200" height="18" rx="4" fill="#EAF2FA" stroke="#2471A3" stroke-width="1"/>
                <text x="120" y="22" text-anchor="middle" fill="#2471A3" font-size="9" font-weight="700">Action layer 1 (one unit / ground action)</text>
                <rect x="20" y="32" width="200" height="18" rx="4" fill="#FDECEA" stroke="#C0392B" stroke-width="1"/>
                <text x="120" y="44" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700">Proposition layer 1</text>
                <rect x="20" y="54" width="200" height="18" rx="4" fill="#EAF2FA" stroke="#2471A3" stroke-width="1"/>
                <text x="120" y="66" text-anchor="middle" fill="#2471A3" font-size="9" font-weight="700">Action layer 2</text>
                <rect x="20" y="76" width="200" height="18" rx="4" fill="#FDECEA" stroke="#C0392B" stroke-width="1"/>
                <text x="120" y="88" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700">Proposition layer 2</text>
                <line x1="120" y1="28" x2="120" y2="32" stroke="#6B5B7B" stroke-width="1"/>
                <line x1="120" y1="50" x2="120" y2="54" stroke="#6B5B7B" stroke-width="1"/>
                <line x1="120" y1="72" x2="120" y2="76" stroke="#6B5B7B" stroke-width="1"/>
                <text x="120" y="113" text-anchor="middle" fill="#2471A3" font-size="9" font-style="italic" font-weight="600">Weight sharing across schemas</text>
                <text x="120" y="125" text-anchor="middle" fill="#2471A3" font-size="9" font-style="italic" font-weight="600">but fixed receptive field</text>
            </svg>
        </div>

        <!-- Pattern 3: Explicit action nodes (GABAR) -->
        <div style="border:1.5px solid rgba(36,113,163,.5); border-radius:9px; padding:10px; background:#fafcff;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#2471A3;">Pattern 3 · Explicit action nodes (GABAR)</div>
            <div style="font-family:'Playfair Display',Georgia,serif; font-size:.92rem; color:#2D2044; font-weight:700; margin-top:2px;">Action schemas as first-class nodes</div>
            <svg viewBox="0 0 240 130" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto; margin-top:4px;">
                <rect x="40" y="6" width="50" height="18" rx="4" fill="#EAF2FA" stroke="#2471A3" stroke-width="1.4"/>
                <text x="65" y="18" text-anchor="middle" fill="#2471A3" font-size="9" font-weight="700">move</text>
                <rect x="100" y="6" width="50" height="18" rx="4" fill="#EAF2FA" stroke="#2471A3" stroke-width="1.4"/>
                <text x="125" y="18" text-anchor="middle" fill="#2471A3" font-size="9" font-weight="700">pickup</text>
                <rect x="160" y="6" width="50" height="18" rx="4" fill="#EAF2FA" stroke="#2471A3" stroke-width="1.4"/>
                <text x="185" y="18" text-anchor="middle" fill="#2471A3" font-size="9" font-weight="700">drop</text>

                <circle cx="40" cy="55" r="12" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                <text x="40" y="58" text-anchor="middle" fill="#B08E2A" font-size="8" font-weight="600">R</text>
                <circle cx="90" cy="55" r="12" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                <text x="90" y="58" text-anchor="middle" fill="#B08E2A" font-size="8" font-weight="600">A</text>
                <circle cx="140" cy="55" r="12" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                <text x="140" y="58" text-anchor="middle" fill="#B08E2A" font-size="8" font-weight="600">C</text>
                <circle cx="190" cy="55" r="12" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                <text x="190" y="58" text-anchor="middle" fill="#B08E2A" font-size="8" font-weight="600">P</text>

                <line x1="65" y1="24" x2="40" y2="43" stroke="#2471A3" stroke-width="1" stroke-opacity=".5"/>
                <line x1="65" y1="24" x2="90" y2="43" stroke="#2471A3" stroke-width="1" stroke-opacity=".5"/>
                <line x1="65" y1="24" x2="140" y2="43" stroke="#2471A3" stroke-width="1" stroke-opacity=".5"/>
                <line x1="125" y1="24" x2="40" y2="43" stroke="#2471A3" stroke-width="1" stroke-opacity=".5"/>
                <line x1="125" y1="24" x2="190" y2="43" stroke="#2471A3" stroke-width="1" stroke-opacity=".5"/>

                <rect x="60" y="85" width="60" height="14" rx="3" fill="#FDECEA" stroke="#C0392B" stroke-width=".8"/>
                <text x="90" y="95" text-anchor="middle" fill="#C0392B" font-size="7.5" font-weight="600">at(R,A)</text>
                <rect x="130" y="85" width="60" height="14" rx="3" fill="#FDECEA" stroke="#C0392B" stroke-width=".8"/>
                <text x="160" y="95" text-anchor="middle" fill="#C0392B" font-size="7.5" font-weight="600">on(P,C)</text>

                <text x="120" y="120" text-anchor="middle" fill="#2471A3" font-size="9" font-style="italic" font-weight="600">Network reads precondition structure directly</text>
            </svg>
        </div>

        <!-- Pattern 4: Hypergraph -->
        <div style="border:1.5px solid #D4CDE0; border-radius:9px; padding:10px; background:#fff;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#6B5B7B;">Pattern 4 · Hypergraph (STRIPS-HGN)</div>
            <div style="font-family:'Playfair Display',Georgia,serif; font-size:.92rem; color:#2D2044; font-weight:700; margin-top:2px;">Action as hyperedge</div>
            <svg viewBox="0 0 240 130" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto; margin-top:4px;">
                <!-- Propositions -->
                <rect x="20" y="22" width="50" height="18" rx="3" fill="#FDECEA" stroke="#C0392B" stroke-width="1"/>
                <text x="45" y="34" text-anchor="middle" fill="#C0392B" font-size="8" font-weight="600">at(R,A)</text>
                <rect x="90" y="22" width="60" height="18" rx="3" fill="#FDECEA" stroke="#C0392B" stroke-width="1"/>
                <text x="120" y="34" text-anchor="middle" fill="#C0392B" font-size="8" font-weight="600">handempty</text>
                <rect x="170" y="22" width="50" height="18" rx="3" fill="#FDECEA" stroke="#C0392B" stroke-width="1"/>
                <text x="195" y="34" text-anchor="middle" fill="#C0392B" font-size="8" font-weight="600">on(P,C)</text>

                <!-- Hyperedge as enclosing shape -->
                <ellipse cx="120" cy="70" rx="105" ry="22" fill="none" stroke="#2471A3" stroke-width="1.4" stroke-dasharray="5 3"/>
                <text x="120" y="73" text-anchor="middle" fill="#2471A3" font-size="9" font-weight="700">pickup(?p, ?z)</text>
                <line x1="45" y1="40" x2="60" y2="55" stroke="#2471A3" stroke-width=".8" stroke-opacity=".5"/>
                <line x1="120" y1="40" x2="120" y2="50" stroke="#2471A3" stroke-width=".8" stroke-opacity=".5"/>
                <line x1="195" y1="40" x2="180" y2="55" stroke="#2471A3" stroke-width=".8" stroke-opacity=".5"/>
                <text x="120" y="115" text-anchor="middle" fill="#2471A3" font-size="9" font-style="italic" font-weight="600">One hyperedge connects all 3 preconditions</text>
                <text x="120" y="125" text-anchor="middle" fill="#2471A3" font-size="9" font-style="italic" font-weight="600">richer than pairwise edges</text>
            </svg>
        </div>
    </div>

    <div style="margin-top: 12px; padding: 10px 14px; background: #FDF6E3; border-radius: 8px; border-left: 4px solid #C5A55A;">
        <p style="font-size: 0.92em; color: #5a4400; line-height: 1.5; margin: 0;"><strong>Pattern 3 is GABAR's contribution.</strong> Each pickup, drop, and move schema is its own graph node. When the model ranks actions, it reads the schema-node embeddings directly &mdash; no need to re-infer "this action needs the robot in the package's zone." Pattern 1 leaves that inference to the network. Pattern 2 weaves it through alternation. Pattern 4 captures it via hyperedge richness but pays in architectural complexity.</p>
    </div>
</div>

<h2>Sub-decision 3: how to construct grounded actions from the graph</h2>

<p>Even once action information is in the graph, there is a remaining question: how does the model produce a <em>fully grounded</em> action — schema plus all its parameters — at execution time? The output is structured: an action like <code>transport(package-7, vehicle-3, city-2)</code> has multiple slots that must be filled coherently.</p>

<p>Two approaches exist.</p>

<h3>Independent parameter selection</h3>

<p>GRAPL (<span class="paper-tag">Karia, Srivastava 2021</span>) decomposes a multi-parameter action into independent decisions: "pick the best package," "pick the best source zone," "pick the best destination" — three separate scorings whose results are concatenated to form the grounded action. Each parameter is selected without conditioning on the others.</p>

<p>This is computationally simple but loses parameter dependencies. In the warehouse, the correct source zone for a transport action depends on which package was selected (it has to be the zone that package is actually in). Under independent decoding, the model has no way to express this: the package and source-zone decisions happen in parallel and can be inconsistent.</p>

<h3>Sequential conditional decoding</h3>

<p>GABAR's GRU-based decoder fixes this. The decoder picks the action schema first, then iterates over parameter positions, conditioning each choice on what came before. After picking the package, the GRU state encodes "given this package," and the next parameter choice respects that condition. The output is a fully grounded action where the parameters are mutually consistent.</p>

<p>This is the GABAR-specific contribution that ranking methods before it lacked. Without it, the action-ranking framing from Part 2 stops working on domains with parameter coupling — exactly the kinds of domains where size generalization matters most.</p>

<div class="vis-container">
    <h3 class="vis-title">Parameter decoding on a 3-package warehouse</h3>
    <div class="vis-subtitle">Pick action: <code>transport(?pkg, ?source, ?dest)</code>. Three packages scattered across zones. Independent vs sequential decoding produce different (and differently valid) outputs.</div>

    <div style="display:flex; gap:14px; align-items:stretch;">
        <div style="flex:1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#C0392B; margin-bottom:6px;">Independent decoding (GRAPL-style)</div>
            <svg viewBox="0 0 280 260" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <rect x="10" y="10" width="80" height="50" rx="6" fill="#FDECEA" stroke="#C0392B" stroke-width="1.4"/>
                <text x="50" y="28" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700">decoder 1</text>
                <text x="50" y="40" text-anchor="middle" fill="#888" font-size="7.5">pick ?pkg</text>
                <text x="50" y="52" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700" font-family="JetBrains Mono,monospace">→ P3</text>

                <rect x="100" y="10" width="80" height="50" rx="6" fill="#FDECEA" stroke="#C0392B" stroke-width="1.4"/>
                <text x="140" y="28" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700">decoder 2</text>
                <text x="140" y="40" text-anchor="middle" fill="#888" font-size="7.5">pick ?source</text>
                <text x="140" y="52" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700" font-family="JetBrains Mono,monospace">→ Zone B</text>

                <rect x="190" y="10" width="80" height="50" rx="6" fill="#FDECEA" stroke="#C0392B" stroke-width="1.4"/>
                <text x="230" y="28" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700">decoder 3</text>
                <text x="230" y="40" text-anchor="middle" fill="#888" font-size="7.5">pick ?dest</text>
                <text x="230" y="52" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700" font-family="JetBrains Mono,monospace">→ Zone D</text>

                <text x="140" y="78" text-anchor="middle" fill="#888" font-size="8" font-style="italic">All three pick in parallel · no shared context</text>

                <rect x="40" y="92" width="200" height="34" rx="4" fill="#FFF" stroke="#C0392B" stroke-width="1.5"/>
                <text x="140" y="107" text-anchor="middle" fill="#2D2044" font-size="10" font-weight="700" font-family="JetBrains Mono,monospace">transport(P3, B, D)</text>
                <text x="140" y="120" text-anchor="middle" fill="#C0392B" font-size="8.5" font-weight="600">✗ INVALID</text>

                <rect x="20" y="140" width="240" height="110" rx="6" fill="#FDECEA" stroke="#C0392B" stroke-width="1"/>
                <text x="140" y="158" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700">Why it broke:</text>
                <text x="140" y="174" text-anchor="middle" fill="#5a2a23" font-size="8.5">P3 is actually in Zone O</text>
                <text x="140" y="187" text-anchor="middle" fill="#5a2a23" font-size="8.5">(not Zone B that decoder 2 picked)</text>
                <line x1="40" y1="200" x2="240" y2="200" stroke="#C0392B" stroke-width=".5" stroke-opacity=".4"/>
                <text x="140" y="215" text-anchor="middle" fill="#5a2a23" font-size="8.5" font-style="italic">Decoders couldn't share state.</text>
                <text x="140" y="227" text-anchor="middle" fill="#5a2a23" font-size="8.5" font-style="italic">No mechanism for "given P3 is the package,</text>
                <text x="140" y="239" text-anchor="middle" fill="#5a2a23" font-size="8.5" font-style="italic">which zone contains P3?"</text>
            </svg>
        </div>
        <div style="flex:1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#1E8449; margin-bottom:6px;">Sequential decoding (GABAR GRU)</div>
            <svg viewBox="0 0 280 260" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <defs>
                    <marker id="arrSeqR" markerWidth="6" markerHeight="6" refX="5" refY="3" orient="auto"><path d="M0,0 L5,3 L0,6" fill="#1E8449"/></marker>
                </defs>
                <rect x="10" y="10" width="80" height="50" rx="6" fill="#E8F4F0" stroke="#1E8449" stroke-width="1.4"/>
                <text x="50" y="22" text-anchor="middle" fill="#1E8449" font-size="9" font-weight="700">step 1</text>
                <text x="50" y="34" text-anchor="middle" fill="#888" font-size="7.5">pick ?pkg</text>
                <text x="50" y="52" text-anchor="middle" fill="#1E8449" font-size="9" font-weight="700" font-family="JetBrains Mono,monospace">→ P3</text>

                <path d="M90,35 L100,35" stroke="#1E8449" stroke-width="1.5" fill="none" marker-end="url(#arrSeqR)"/>

                <rect x="100" y="10" width="80" height="50" rx="6" fill="#E8F4F0" stroke="#1E8449" stroke-width="1.4"/>
                <text x="140" y="22" text-anchor="middle" fill="#1E8449" font-size="9" font-weight="700">step 2</text>
                <text x="140" y="32" text-anchor="middle" fill="#888" font-size="6.5">pick ?source</text>
                <text x="140" y="42" text-anchor="middle" fill="#5a4400" font-size="6.5" font-style="italic">GRU: P3</text>
                <text x="140" y="54" text-anchor="middle" fill="#1E8449" font-size="9" font-weight="700" font-family="JetBrains Mono,monospace">→ Zone O</text>

                <path d="M180,35 L190,35" stroke="#1E8449" stroke-width="1.5" fill="none" marker-end="url(#arrSeqR)"/>

                <rect x="190" y="10" width="80" height="50" rx="6" fill="#E8F4F0" stroke="#1E8449" stroke-width="1.4"/>
                <text x="230" y="22" text-anchor="middle" fill="#1E8449" font-size="9" font-weight="700">step 3</text>
                <text x="230" y="32" text-anchor="middle" fill="#888" font-size="6.5">pick ?dest</text>
                <text x="230" y="42" text-anchor="middle" fill="#5a4400" font-size="6.5" font-style="italic">GRU: P3, O</text>
                <text x="230" y="54" text-anchor="middle" fill="#1E8449" font-size="9" font-weight="700" font-family="JetBrains Mono,monospace">→ Zone D</text>

                <text x="140" y="78" text-anchor="middle" fill="#888" font-size="8" font-style="italic">Each step conditions on all earlier picks</text>

                <rect x="40" y="92" width="200" height="34" rx="4" fill="#FFF" stroke="#1E8449" stroke-width="1.5"/>
                <text x="140" y="107" text-anchor="middle" fill="#2D2044" font-size="10" font-weight="700" font-family="JetBrains Mono,monospace">transport(P3, O, D)</text>
                <text x="140" y="120" text-anchor="middle" fill="#1E8449" font-size="8.5" font-weight="600">✓ VALID</text>

                <rect x="20" y="140" width="240" height="110" rx="6" fill="#E8F4F0" stroke="#1E8449" stroke-width="1"/>
                <text x="140" y="158" text-anchor="middle" fill="#1E8449" font-size="9" font-weight="700">Why it works:</text>
                <text x="140" y="174" text-anchor="middle" fill="#1c5a2e" font-size="8.5">After picking P3, the GRU hidden state</text>
                <text x="140" y="187" text-anchor="middle" fill="#1c5a2e" font-size="8.5">encodes "package = P3"</text>
                <line x1="40" y1="200" x2="240" y2="200" stroke="#1E8449" stroke-width=".5" stroke-opacity=".4"/>
                <text x="140" y="215" text-anchor="middle" fill="#1c5a2e" font-size="8.5" font-style="italic">Source-zone decoder reads that state,</text>
                <text x="140" y="227" text-anchor="middle" fill="#1c5a2e" font-size="8.5" font-style="italic">ranks only zones where P3 actually is</text>
                <text x="140" y="239" text-anchor="middle" fill="#1c5a2e" font-size="8.5" font-style="italic">(Zone O wins; B never even considered)</text>
            </svg>
        </div>
    </div>

    <div style="margin-top: 12px; padding: 10px 14px; background: #FDF6E3; border-radius: 8px; border-left: 4px solid #C5A55A;">
        <p style="font-size: 0.92em; color: #5a4400; line-height: 1.5; margin: 0;"><strong>The single-package warehouse from Part 1 is too easy to expose this bug.</strong> It only matters once multiple objects of the same type couple across action parameters &mdash; which is true for almost every interesting IPC domain (Logistics, Blocks, Rovers). GRAPL gets the right answer when actions have single parameters; it fails on coupled-parameter actions because the design choice was independence. GABAR's sequential decoder fixes this with one architectural change.</p>
    </div>
</div>

<hr>

<h2>Representations at a glance</h2>

<div class="vis-container">
    <h3 class="vis-title">Graph representation choices across the literature</h3>
    <div class="vis-subtitle">Same three sub-decisions, different choices. GABAR is the cell at the intersection.</div>

    <div class="tbl-scroll">
<table class="compare-table">
        <thead>
            <tr>
                <th>Method</th>
                <th>Lifted vs grounded</th>
                <th>Action representation</th>
                <th>Parameter construction</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>ASNets (Toyer et al. 2020)</td>
                <td>Grounded</td>
                <td>Alternating layers (woven)</td>
                <td>Per-ground-action output</td>
            </tr>
            <tr>
                <td>STRIPS-HGN (Shen et al. 2020)</td>
                <td>Grounded (hypergraph)</td>
                <td>Action as hyperedge</td>
                <td>N/A (heuristic only)</td>
            </tr>
            <tr>
                <td>GPL (Ståhlberg et al. 2022a)</td>
                <td>Lifted-ish</td>
                <td>Implicit (no action nodes)</td>
                <td>N/A (value function)</td>
            </tr>
            <tr>
                <td>GRAPL (Karia &amp; Srivastava 2021)</td>
                <td>Canonical abstraction</td>
                <td>Implicit (object groups)</td>
                <td class="no">Independent</td>
            </tr>
            <tr>
                <td>GOOSE (Chen et al. 2024)</td>
                <td>Both compared</td>
                <td>Implicit (no action nodes)</td>
                <td>N/A (heuristic)</td>
            </tr>
            <tr style="background: #FDF6E3;">
                <td><strong>GABAR (ours)</strong></td>
                <td>Predicates grounded, schemas ungrounded</td>
                <td class="yes">Action as graph node (explicit)</td>
                <td class="yes">Sequential (GRU decoder)</td>
            </tr>
        </tbody>
    </table>
</div>
</div>

<p>Looking down the columns: GABAR's cell is a genuinely new combination. No prior method combined explicit action-schema nodes with sequential conditional decoding. ASNets has actions explicit (via alternation) but does not condition parameter selection. GRAPL is closer in spirit (also action-ranking) but uses canonical abstraction with independent decoding. STRIPS-HGN has actions explicit (via hyperedges) but is a heuristic, not a policy.</p>

<p>The grid is sparse for reasons beyond just "no one had time": each cell requires a specific combination of architectural choices that don't fall out of any single design philosophy. GABAR's cell only makes sense once you've committed to (i) action ranking as the objective (Part 2's Family 3), (ii) action-centric graphs as the encoding (this post), and (iii) sequential decoding as the way to handle parameter coupling. Three choices, all justified by specific limitations of prior work.</p>

<h2>Beyond GNNs: transformers and other alternatives</h2>

<p>One more thread worth flagging: not everyone uses GNNs. <span class="paper-tag">Müller, Sánchez, Hoffmann, Wolf, Gros 2024</span> recently benchmarked standard off-the-shelf GNNs against transformers for general policy learning, finding the trade-offs subtle: transformers handle larger context well but lose the size-generalization properties GNNs get from message passing's permutation invariance. The field has not converged on a winner.</p>

<p>For now, GNNs remain the dominant choice for L4P because the relational structure of planning problems is exactly what they were designed for. Transformers may eventually catch up, particularly as architectures evolve to incorporate relational inductive biases, but as of 2025 the strongest L4P systems all use GNNs.</p>

<h2>References</h2>

<div class="refs">
<ol>
    <li>Chen, D. Z., Thi&eacute;baux, S., &amp; Trevizan, F. (2024). <em>Learning Domain-Independent Heuristics for Grounded and Lifted Planning</em> (GOOSE). AAAI 2024.</li>
    <li>Chen, D. Z., Hao, M., Thi&eacute;baux, S., &amp; Trevizan, F. (2024). On the expressiveness of grounded vs lifted graph encodings for learning to plan.</li>
    <li>Barcel&oacute;, P., Kostylev, E., Monet, M., P&eacute;rez, J., Reutter, J., &amp; Silva, J. P. (2020). <em>The Logical Expressiveness of Graph Neural Networks.</em> ICLR 2020.</li>
    <li>St&aring;hlberg, S., Bonet, B., &amp; Geffner, H. (2024). <em>Learning General Policies for Classical Planning Domains: Getting Beyond C&#8322;.</em></li>
    <li>Toyer, S., Thi&eacute;baux, S., Trevizan, F., &amp; Xie, F. (2020). <em>ASNets: Deep Learning for Generalised Planning.</em> Journal of Artificial Intelligence Research, 68.</li>
    <li>St&aring;hlberg, S., Bonet, B., &amp; Geffner, H. (2022). <em>Learning Generalized Policies Without Supervision Using GNNs</em> (GPL). KR 2022.</li>
    <li>Karia, R., &amp; Srivastava, S. (2021). <em>GRAPL: Generalized Relational Action Policy Learning.</em></li>
    <li>Shen, W., Trevizan, F., &amp; Thi&eacute;baux, S. (2020). <em>Learning Domain-Independent Planning Heuristics with Hypergraph Networks</em> (STRIPS-HGN). ICAPS 2020.</li>
    <li>M&uuml;ller, F., S&aacute;nchez, P., Hoffmann, J., Wolf, V., &amp; Gros, T. P. (2024). Comparing off-the-shelf GNNs and transformers for generalized policy learning.</li>
    <li><em>Graph Neural Network Based Action Ranking for Planning</em> (GABAR). NeurIPS 2025.</li>
</ol>
</div>

<hr>

<div class="series-footer">
    <strong>Where this fits</strong>
    <p>You now have both axes covered: <em>what to learn</em> (Part 2's three families) and <em>how to represent the input</em> (this post's three sub-decisions). Part 4 reads GABAR &mdash; one specific cell in the design space &mdash; with a full understanding of why each choice was the right one given the literature.</p>
    <p style="margin-top: 10px; font-size: 0.85em; color: #666;">&larr; <a href="/blog/2026/learning-for-planning-what-to-learn/">Part 2: What to Learn</a> &middot; <a href="/blog/2026/learning-for-planning-gabar/">Part 4: GABAR &rarr;</a></p>
</div>

</article>
