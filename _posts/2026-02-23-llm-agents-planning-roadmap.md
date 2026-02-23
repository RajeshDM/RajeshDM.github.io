---
layout: fullhtml-post
title: "Making LLM Agents Actually Plan: A Roadmap"
date: 2026-02-23
categories: ["LLMs Automated Planning and Agents"]
tags: ["planning", "llm"]
description: "Why your LLM agent fails at multi-step tasks, and a 7-part guide to fixing it. Part 1 of the Planning in the Era of LLMs series."
_styles: >
  .blog-fullhtml *, .blog-fullhtml *::before, .blog-fullhtml *::after { box-sizing: border-box; margin: 0; padding: 0; }
  .blog-fullhtml {
      font-family: 'Georgia', 'Times New Roman', serif;
      line-height: 1.8;
      color: #1a1a2e;
  }
  .blog-fullhtml .hero {
      background: linear-gradient(135deg, #0d1b2a 0%, #1b2838 40%, #2a4066 100%);
      color: #f0f0f0;
      padding: 80px 20px 60px;
      text-align: center;
  }
  .blog-fullhtml .hero .series-label {
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.85rem;
      text-transform: uppercase;
      letter-spacing: 3px;
      color: #7eb8da;
      margin-bottom: 16px;
  }
  .blog-fullhtml .hero h1 {
      font-size: 2.6rem;
      font-weight: 700;
      line-height: 1.25;
      max-width: 820px;
      margin: 0 auto 20px;
      color: #f0f0f0;
      border: none;
      padding: 0;
  }
  .blog-fullhtml .hero .subtitle {
      font-size: 1.15rem;
      color: #b0c4de;
      max-width: 640px;
      margin: 0 auto 28px;
      font-style: italic;
  }
  .blog-fullhtml .blog-container {
      max-width: 780px;
      margin: 0 auto;
      padding: 48px 24px 80px;
  }
  .blog-fullhtml h2 {
      font-size: 1.85rem;
      color: #0d1b2a;
      margin: 56px 0 20px;
      padding-bottom: 8px;
      border-bottom: 3px solid #2a4066;
  }
  .blog-fullhtml h3 {
      font-size: 1.35rem;
      color: #1b2838;
      margin: 40px 0 14px;
  }
  .blog-fullhtml p { margin-bottom: 18px; font-size: 1.05rem; }
  .blog-fullhtml strong { color: #0d1b2a; }
  .blog-fullhtml a { color: #2a6496; text-decoration: none; border-bottom: 1px solid #2a649644; }
  .blog-fullhtml a:hover { color: #1a4060; border-bottom-color: #1a4060; }
  .blog-fullhtml .lead { font-size: 1.2rem; color: #333; line-height: 1.9; margin-bottom: 28px; }
  .blog-fullhtml ul, .blog-fullhtml ol { margin: 0 0 20px 28px; font-size: 1.05rem; }
  .blog-fullhtml li { margin-bottom: 6px; }
  .blog-fullhtml .series-nav {
      background: #f0f4f8;
      border: 1px solid #d0d8ef;
      border-radius: 8px;
      padding: 20px 24px;
      margin-bottom: 32px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.92rem;
      color: #444;
  }
  .blog-fullhtml .series-nav strong { color: #2a4066; font-size: 1rem; }
  .blog-fullhtml .series-nav .nav-desc { margin: 8px 0; color: #555; line-height: 1.6; }
  .blog-fullhtml .series-nav .nav-links {
      margin-top: 10px;
      font-size: 0.88rem;
      color: #2a4066;
      font-weight: 600;
  }
  .blog-fullhtml .callout {
      border-left: 4px solid #2a4066;
      background: #f0f4f8;
      padding: 20px 24px;
      margin: 28px 0;
      border-radius: 0 6px 6px 0;
  }
  .blog-fullhtml .callout.insight {
      border-left-color: #228b22;
      background: #f0faf0;
  }
  .blog-fullhtml .callout.warning {
      border-left-color: #b22222;
      background: #fdf2f2;
  }
  .blog-fullhtml .callout.question {
      border-left-color: #d4740e;
      background: #fef9f0;
  }
  .blog-fullhtml .callout-label {
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-weight: 700;
      font-size: 0.8rem;
      text-transform: uppercase;
      letter-spacing: 1.5px;
      margin-bottom: 8px;
  }
  .blog-fullhtml .callout.insight .callout-label { color: #228b22; }
  .blog-fullhtml .callout.warning .callout-label { color: #b22222; }
  .blog-fullhtml .callout.question .callout-label { color: #d4740e; }
  .blog-fullhtml .callout p:last-child { margin-bottom: 0; }
  .blog-fullhtml .agentic-sidebar {
      background: linear-gradient(135deg, #f5f0ff, #ede4ff);
      border: 1px solid #d4c5f0;
      border-radius: 10px;
      padding: 24px;
      margin: 36px 0;
  }
  .blog-fullhtml .agentic-sidebar .sidebar-title {
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-weight: 700;
      font-size: 1rem;
      color: #6b3fa0;
      margin-bottom: 12px;
  }
  .blog-fullhtml .agentic-sidebar p { font-size: 0.95rem; color: #3a2a5a; }
  .blog-fullhtml pre {
      background: #1e1e2e;
      color: #cdd6f4;
      padding: 20px 24px;
      border-radius: 8px;
      overflow-x: auto;
      font-family: 'Fira Code', 'Consolas', 'Monaco', monospace;
      font-size: 0.9rem;
      line-height: 1.6;
      margin: 24px 0;
  }
  .blog-fullhtml code {
      font-family: 'Fira Code', 'Consolas', 'Monaco', monospace;
      font-size: 0.88em;
  }
  .blog-fullhtml p code, .blog-fullhtml li code {
      background: #e8edf2;
      padding: 2px 6px;
      border-radius: 3px;
      color: #2a4066;
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
  .blog-fullhtml .interactive-container {
      border: 1px solid #d0d8ef;
      border-radius: 10px;
      padding: 24px;
      margin: 36px 0;
      background: #fff;
  }
  .blog-fullhtml .auto-demo-btn {
      background: #2a9d8f;
      color: white;
      border: none;
      padding: 10px 24px;
      border-radius: 6px;
      cursor: pointer;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.9em;
      font-weight: 600;
      margin-top: 12px;
  }
  .blog-fullhtml .auto-demo-btn:hover { background: #238577; }
  .blog-fullhtml .next-post {
      margin: 56px 0 0; padding: 28px;
      background: linear-gradient(135deg, #1b2838, #2a4066);
      border-radius: 10px; color: #e0e8f0;
  }
  .blog-fullhtml .next-post h3 { color: #7eb8da; margin-top: 0; font-size: 1.15rem; }
  .blog-fullhtml .next-post p { font-size: 0.95rem; color: #b0c4de; }
  .blog-fullhtml .references { margin-top: 48px; padding-top: 24px; border-top: 2px solid #dde; }
  .blog-fullhtml .references h2 { border-bottom: none; font-size: 1.4rem; margin-top: 0; }
  .blog-fullhtml .references ol { font-size: 0.9rem; color: #444; line-height: 1.7; }
  .blog-fullhtml .references li { margin-bottom: 8px; }
  .blog-fullhtml .blog-footer {
      text-align: center; padding: 32px 20px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.82rem; color: #888; border-top: 1px solid #eee;
  }
  .blog-fullhtml .math { font-family: 'Cambria Math', 'Georgia', serif; font-style: italic; color: #2a4066; }
  .blog-fullhtml .paradigm-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin: 24px 0; }
  .blog-fullhtml .paradigm-box { padding: 16px; border-radius: 8px; border: 2px solid; min-width: 0; }
  .blog-fullhtml .paradigm-box.p1 { background: #eff5ff; border-color: #4682b4; }
  .blog-fullhtml .paradigm-box.p2 { background: #fff7ed; border-color: #d4740e; }
  .blog-fullhtml .paradigm-box h4 { font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.95rem; margin-bottom: 8px; }
  .blog-fullhtml .paradigm-box.p1 h4 { color: #2a5080; }
  .blog-fullhtml .paradigm-box.p2 h4 { color: #b35c00; }
  .blog-fullhtml .paradigm-box ul { margin-left: 16px; font-size: 0.85rem; }
  .blog-fullhtml .paradigm-box .tag {
      display: inline-block; font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.72rem; font-weight: 700; text-transform: uppercase;
      letter-spacing: 1px; padding: 3px 10px; border-radius: 4px; margin-bottom: 12px;
  }
  .blog-fullhtml .paradigm-box.p1 .tag { background: #4682b4; color: #fff; }
  .blog-fullhtml .paradigm-box.p2 .tag { background: #d4740e; color: #fff; }
  .blog-fullhtml .paradigm-box .flow-row { justify-content: flex-start; }
  .blog-fullhtml .paradigm-box .flow-node { min-width: 0; padding: 5px 8px; font-size: 0.68rem; border-radius: 5px; }
  .blog-fullhtml .paradigm-box .flow-arrow { padding: 0 5px; font-size: 0.85rem; }
  .blog-fullhtml .flow-row {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 0;
      flex-wrap: nowrap;
  }
  .blog-fullhtml .flow-node {
      padding: 14px 24px;
      border-radius: 8px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.92rem;
      font-weight: 600;
      color: #fff;
      text-align: center;
      min-width: 140px;
      line-height: 1.4;
  }
  .blog-fullhtml .flow-node.formal-rules { background: #1b2838; }
  .blog-fullhtml .flow-node.llm-assists { background: #6b3fa0; }
  .blog-fullhtml .flow-node.verified-plan { background: #228b22; }
  .blog-fullhtml .flow-node.nl-input { background: #2a9d8f; }
  .blog-fullhtml .flow-node.llm-generates { background: #6b3fa0; }
  .blog-fullhtml .flow-arrow {
      font-size: 1.6rem;
      color: #555;
      padding: 0 14px;
      font-weight: 700;
      flex-shrink: 0;
  }
  .blog-fullhtml .timeline-track {
      display: flex;
      align-items: flex-start;
      justify-content: space-between;
      position: relative;
      padding: 20px 0 0;
      margin: 10px 0;
  }
  .blog-fullhtml .timeline-track::before {
      content: '';
      position: absolute;
      top: 38px;
      left: 5%;
      right: 5%;
      height: 4px;
      background: linear-gradient(90deg, #b22222, #d4740e, #2a9d8f, #228b22);
      border-radius: 2px;
  }
  .blog-fullhtml .timeline-node {
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 22%;
      position: relative;
      z-index: 1;
  }
  .blog-fullhtml .timeline-dot {
      width: 20px;
      height: 20px;
      border-radius: 50%;
      border: 3px solid #fff;
      box-shadow: 0 0 0 2px currentColor;
      margin-bottom: 12px;
  }
  .blog-fullhtml .timeline-node:nth-child(1) .timeline-dot { background: #b22222; color: #b22222; }
  .blog-fullhtml .timeline-node:nth-child(2) .timeline-dot { background: #d4740e; color: #d4740e; }
  .blog-fullhtml .timeline-node:nth-child(3) .timeline-dot { background: #2a9d8f; color: #2a9d8f; }
  .blog-fullhtml .timeline-node:nth-child(4) .timeline-dot { background: #228b22; color: #228b22; }
  .blog-fullhtml .timeline-year {
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-weight: 700;
      font-size: 0.85rem;
      color: #1b2838;
      margin-bottom: 4px;
  }
  .blog-fullhtml .timeline-question {
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.78rem;
      color: #555;
      text-align: center;
      font-style: italic;
      margin-bottom: 4px;
  }
  .blog-fullhtml .timeline-answer {
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.75rem;
      color: #777;
      text-align: center;
  }
  .blog-fullhtml .timeline-post-ref {
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.7rem;
      color: #2a4066;
      font-weight: 600;
      margin-top: 4px;
  }
  .blog-fullhtml .roadmap-list {
      list-style: none;
      margin: 0;
      padding: 0;
  }
  .blog-fullhtml .roadmap-list li {
      padding: 14px 18px;
      margin-bottom: 8px;
      border-radius: 8px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.95rem;
      line-height: 1.6;
      border-left: 4px solid;
      transition: transform 0.15s;
  }
  .blog-fullhtml .roadmap-list li:hover { transform: translateX(4px); }
  .blog-fullhtml .roadmap-list .post-num {
      font-weight: 700;
      font-size: 0.8rem;
      text-transform: uppercase;
      letter-spacing: 1px;
      display: block;
      margin-bottom: 2px;
  }
  .blog-fullhtml .roadmap-list .post-title {
      font-weight: 700;
      font-size: 1rem;
  }
  .blog-fullhtml .roadmap-list li:nth-child(1) { background: #f0f4f8; border-color: #2a4066; }
  .blog-fullhtml .roadmap-list li:nth-child(1) .post-num { color: #2a4066; }
  .blog-fullhtml .roadmap-list li:nth-child(2) { background: #f0f4f8; border-color: #2a4066; }
  .blog-fullhtml .roadmap-list li:nth-child(2) .post-num { color: #2a4066; }
  .blog-fullhtml .roadmap-list li:nth-child(3) { background: #fdf2f2; border-color: #b22222; }
  .blog-fullhtml .roadmap-list li:nth-child(3) .post-num { color: #b22222; }
  .blog-fullhtml .roadmap-list li:nth-child(4) { background: #f0faf0; border-color: #228b22; }
  .blog-fullhtml .roadmap-list li:nth-child(4) .post-num { color: #228b22; }
  .blog-fullhtml .roadmap-list li:nth-child(5) { background: #fff7ed; border-color: #d4740e; }
  .blog-fullhtml .roadmap-list li:nth-child(5) .post-num { color: #d4740e; }
  .blog-fullhtml .roadmap-list li:nth-child(6) { background: #f5f0ff; border-color: #6b3fa0; }
  .blog-fullhtml .roadmap-list li:nth-child(6) .post-num { color: #6b3fa0; }
  .blog-fullhtml .warehouse-scene {
      position: relative;
      width: 100%;
      height: 420px;
      background: linear-gradient(180deg, #e8ecf1 0%, #d5dbe3 100%);
      border-radius: 10px;
      overflow: hidden;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      border: 1px solid #bcc5d3;
      transition: box-shadow 0.3s ease;
  }
  .blog-fullhtml .wh-floor {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      height: 260px;
      background:
          repeating-linear-gradient(
              90deg,
              #c8cdd4 0px, #c8cdd4 1px,
              transparent 1px, transparent 80px
          ),
          repeating-linear-gradient(
              0deg,
              #c8cdd4 0px, #c8cdd4 1px,
              transparent 1px, transparent 80px
          ),
          #dfe3e8;
  }
  .blog-fullhtml .wh-ceiling {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      height: 25px;
      background: #3a4a5c;
  }
  .blog-fullhtml .wh-light {
      position: absolute;
      top: 25px;
      width: 40px;
      height: 8px;
      background: #ffe8a0;
      border-radius: 0 0 4px 4px;
      box-shadow: 0 4px 20px 8px rgba(255, 232, 160, 0.25);
  }
  .blog-fullhtml .wh-shelf {
      position: absolute;
      bottom: 100px;
      width: 110px;
      height: 160px;
      background: linear-gradient(180deg, #6a7585 0%, #555f6e 100%);
      border-radius: 3px 3px 0 0;
      display: flex;
      flex-direction: column;
      justify-content: space-around;
      align-items: center;
      padding: 8px 6px;
      box-shadow: 2px 2px 8px rgba(0,0,0,0.2);
  }
  .blog-fullhtml .wh-shelf-label {
      position: absolute;
      top: -22px;
      left: 50%;
      transform: translateX(-50%);
      font-size: 0.7rem;
      font-weight: 700;
      color: #444;
      background: #f0f2f5;
      padding: 2px 8px;
      border-radius: 3px;
      white-space: nowrap;
  }
  .blog-fullhtml .wh-shelf-row {
      width: 94%;
      height: 3px;
      background: #8a95a5;
      border-radius: 1px;
  }
  .blog-fullhtml .wh-package {
      width: 32px;
      height: 28px;
      border-radius: 3px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.6rem;
      font-weight: 700;
      color: #fff;
      position: absolute;
      transition: all 0.6s ease;
      box-shadow: 1px 1px 3px rgba(0,0,0,0.3);
  }
  .blog-fullhtml .pkg-1 { background: #e07030; }
  .blog-fullhtml .pkg-2 { background: #3080c0; }
  .blog-fullhtml .pkg-3 { background: #d04050; }
  .blog-fullhtml .pkg-4 { background: #30a060; }
  .blog-fullhtml .pkg-5 { background: #8050c0; }
  .blog-fullhtml .wh-dock {
      position: absolute;
      right: 20px;
      bottom: 20px;
      width: 130px;
      height: 80px;
      background: repeating-linear-gradient(
          -45deg,
          #f5c542 0px, #f5c542 10px,
          #1b2838 10px, #1b2838 20px
      );
      border-radius: 6px;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 2px 2px 8px rgba(0,0,0,0.2);
  }
  .blog-fullhtml .wh-dock-inner {
      background: #1b2838;
      color: #f5c542;
      font-size: 0.72rem;
      font-weight: 800;
      text-transform: uppercase;
      letter-spacing: 1.5px;
      padding: 6px 14px;
      border-radius: 4px;
  }
  .blog-fullhtml .wh-robot {
      position: absolute;
      width: 50px;
      height: 60px;
      transition: all 0.6s ease;
      z-index: 10;
  }
  .blog-fullhtml .wh-robot-body {
      width: 50px;
      height: 40px;
      background: linear-gradient(180deg, #6b3fa0, #5a2e8f);
      border-radius: 8px 8px 4px 4px;
      display: flex;
      align-items: center;
      justify-content: center;
      position: relative;
  }
  .blog-fullhtml .wh-robot-eyes {
      display: flex;
      gap: 8px;
  }
  .blog-fullhtml .wh-robot-eye {
      width: 10px;
      height: 10px;
      background: #7eb8da;
      border-radius: 50%;
      box-shadow: 0 0 4px #7eb8da;
  }
  .blog-fullhtml .wh-robot-wheels {
      display: flex;
      justify-content: space-between;
      padding: 2px 5px 0;
  }
  .blog-fullhtml .wh-robot-wheel {
      width: 14px;
      height: 14px;
      background: #333;
      border-radius: 50%;
      border: 2px solid #555;
  }
  .blog-fullhtml .wh-robot-arm {
      position: absolute;
      top: 8px;
      right: -12px;
      width: 14px;
      height: 6px;
      background: #888;
      border-radius: 2px;
      transition: all 0.3s;
  }
  .blog-fullhtml .wh-robot-arm.carrying {
      height: 10px;
      background: #aaa;
  }
  .blog-fullhtml .wh-robot-label {
      position: absolute;
      bottom: -18px;
      left: 50%;
      transform: translateX(-50%);
      font-size: 0.65rem;
      font-weight: 700;
      color: #6b3fa0;
      white-space: nowrap;
  }
  .blog-fullhtml .wh-status {
      position: absolute;
      top: 35px;
      left: 12px;
      right: 12px;
      background: rgba(27,40,56,0.88);
      color: #e0e8f0;
      padding: 8px 14px;
      border-radius: 6px;
      font-size: 0.78rem;
      font-family: 'Fira Code', 'Consolas', monospace;
      z-index: 20;
      display: flex;
      justify-content: space-between;
  }
  .blog-fullhtml .wh-status .status-action { color: #7eb8da; }
  .blog-fullhtml .wh-status .status-state { color: #a6e3a1; }
  .blog-fullhtml .wh-status .status-error { color: #f38ba8; }
  .blog-fullhtml .wh-controls {
      display: flex;
      gap: 8px;
      align-items: center;
      flex-wrap: wrap;
      margin-top: 14px;
  }
  .blog-fullhtml .wh-btn {
      padding: 7px 16px;
      border-radius: 5px;
      border: none;
      cursor: pointer;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.82rem;
      font-weight: 600;
      transition: background 0.15s;
  }
  .blog-fullhtml .wh-btn.primary { background: #6b3fa0; color: #fff; }
  .blog-fullhtml .wh-btn.primary:hover { background: #5a2e8f; }
  .blog-fullhtml .wh-btn.secondary { background: #dde; color: #444; }
  .blog-fullhtml .wh-btn.secondary:hover { background: #ccd; }
  .blog-fullhtml .wh-btn:disabled { opacity: 0.4; cursor: not-allowed; }
  .blog-fullhtml .comparison-wrapper {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
      margin-top: 16px;
  }
  .blog-fullhtml .comp-panel {
      border-radius: 8px;
      padding: 18px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.85rem;
  }
  .blog-fullhtml .comp-panel.naive {
      background: #fdf2f2;
      border: 2px solid #e8a0a0;
  }
  .blog-fullhtml .comp-panel.planned {
      background: #f0faf0;
      border: 2px solid #a0d8a0;
  }
  .blog-fullhtml .comp-panel h4 {
      font-size: 0.95rem;
      margin-bottom: 12px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .comp-panel.naive h4 { color: #b22222; }
  .blog-fullhtml .comp-panel.planned h4 { color: #228b22; }
  .blog-fullhtml .comp-step {
      padding: 6px 10px;
      border-radius: 5px;
      margin-bottom: 5px;
      font-size: 0.82rem;
      line-height: 1.45;
      opacity: 0.3;
      transition: opacity 0.35s;
      background: #f0f0f0;
  }
  .blog-fullhtml .comp-step.active { opacity: 1; }
  .blog-fullhtml .comp-step.ok { background: #d4edda; border-left: 3px solid #228b22; }
  .blog-fullhtml .comp-step.fail { background: #f8d7da; border-left: 3px solid #b22222; }
  .blog-fullhtml .comp-step.neutral { background: #e8edf2; border-left: 3px solid #6c757d; }
  .blog-fullhtml .comp-result {
      margin-top: 10px;
      padding: 12px;
      border-radius: 6px;
      font-size: 0.82rem;
      line-height: 1.6;
      visibility: hidden;
  }
  .blog-fullhtml .comp-result.errors {
      background: #fff0f0;
      border: 1px solid #f0b8b8;
      color: #8b1a1a;
  }
  .blog-fullhtml .comp-result.success {
      background: #f0fff0;
      border: 1px solid #b5e8b5;
      color: #1a6b1a;
  }
  .blog-fullhtml .comp-result strong { display: block; margin-bottom: 4px; }
  @media (max-width: 700px) {
      .blog-fullhtml .hero h1 { font-size: 1.8rem; }
      .blog-fullhtml .blog-container { padding: 32px 16px 60px; }
      .blog-fullhtml .paradigm-grid { grid-template-columns: 1fr; }
      .blog-fullhtml .comparison-wrapper { grid-template-columns: 1fr; }
      .blog-fullhtml .timeline-track { flex-wrap: wrap; gap: 16px; justify-content: center; }
      .blog-fullhtml .timeline-track::before { display: none; }
      .blog-fullhtml .timeline-node { width: 45%; }
      .blog-fullhtml .flow-row { flex-wrap: wrap; gap: 8px; }
      .blog-fullhtml .flow-node { min-width: 110px; font-size: 0.82rem; padding: 10px 14px; }
      .blog-fullhtml .flow-arrow { padding: 0 6px; font-size: 1.2rem; }
      .blog-fullhtml .warehouse-scene { height: 350px; }
  }
  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #e0e8f0; border-bottom-color: #4a7ab5; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml a { color: #6aafe6; border-bottom-color: rgba(106,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml a:hover { color: #8ec5f0; border-bottom-color: #8ec5f0; }
  html[data-theme="dark"] .blog-fullhtml .lead { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav { background: #1e2a3a; border-color: #2a3a50; color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav strong { color: #7eb8da; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .nav-desc { color: #9aa8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .nav-links { color: #7eb8da; }
  html[data-theme="dark"] .blog-fullhtml .vis-container { background: #1e2530; border-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .vis-caption { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .callout { background: #1a2535; }
  html[data-theme="dark"] .blog-fullhtml .callout.insight { background: #1a2a1a; }
  html[data-theme="dark"] .blog-fullhtml .callout.warning { background: #2a1a1a; }
  html[data-theme="dark"] .blog-fullhtml .callout.question { background: #2a2418; }
  html[data-theme="dark"] .blog-fullhtml .callout.insight .callout-label { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .callout.warning .callout-label { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .callout.question .callout-label { color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .agentic-sidebar { background: linear-gradient(135deg, #1e1a2e, #251e3a); border-color: #3a2e55; }
  html[data-theme="dark"] .blog-fullhtml .agentic-sidebar .sidebar-title { color: #b088e0; }
  html[data-theme="dark"] .blog-fullhtml .agentic-sidebar p { color: #b0a8c8; }
  html[data-theme="dark"] .blog-fullhtml p code, html[data-theme="dark"] .blog-fullhtml li code { background: #2c3237; color: #7eb8da; }
  html[data-theme="dark"] .blog-fullhtml .interactive-container { background: #1c1c1d; border-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .paradigm-box.p1 { background: #141e2e; border-color: #4682b4; }
  html[data-theme="dark"] .blog-fullhtml .paradigm-box.p2 { background: #2a1e10; border-color: #d4740e; }
  html[data-theme="dark"] .blog-fullhtml .paradigm-box.p1 h4 { color: #6aafe6; }
  html[data-theme="dark"] .blog-fullhtml .paradigm-box.p2 h4 { color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .paradigm-box ul { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .flow-arrow { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .timeline-year { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml .timeline-question { color: #9aa8b8; }
  html[data-theme="dark"] .blog-fullhtml .timeline-answer { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .timeline-post-ref { color: #6aafe6; }
  html[data-theme="dark"] .blog-fullhtml .timeline-dot { border-color: #1c1c1d; }
  html[data-theme="dark"] .blog-fullhtml .roadmap-list li:nth-child(1),
  html[data-theme="dark"] .blog-fullhtml .roadmap-list li:nth-child(2) { background: #141e2e; }
  html[data-theme="dark"] .blog-fullhtml .roadmap-list li:nth-child(3) { background: #2a1414; }
  html[data-theme="dark"] .blog-fullhtml .roadmap-list li:nth-child(4) { background: #142a14; }
  html[data-theme="dark"] .blog-fullhtml .roadmap-list li:nth-child(5) { background: #2a2010; }
  html[data-theme="dark"] .blog-fullhtml .roadmap-list li:nth-child(6) { background: #1e142e; }
  html[data-theme="dark"] .blog-fullhtml .roadmap-list .post-title { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml .roadmap-list li { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .warehouse-scene { background: linear-gradient(180deg, #1a2030 0%, #141a24 100%); border-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .wh-floor { background: repeating-linear-gradient(90deg, #2a3040 0px, #2a3040 1px, transparent 1px, transparent 80px), repeating-linear-gradient(0deg, #2a3040 0px, #2a3040 1px, transparent 1px, transparent 80px), #1a2030; }
  html[data-theme="dark"] .blog-fullhtml .wh-ceiling { background: #0d1520; }
  html[data-theme="dark"] .blog-fullhtml .wh-shelf { background: linear-gradient(180deg, #3a4555 0%, #2a3545 100%); }
  html[data-theme="dark"] .blog-fullhtml .wh-shelf-label { color: #c0c8d0; background: #1a2030; }
  html[data-theme="dark"] .blog-fullhtml .wh-shelf-row { background: #4a5565; }
  html[data-theme="dark"] .blog-fullhtml .comp-panel.naive { background: #2a1818; border-color: #5a3030; }
  html[data-theme="dark"] .blog-fullhtml .comp-panel.planned { background: #182a18; border-color: #305a30; }
  html[data-theme="dark"] .blog-fullhtml .comp-panel.naive h4 { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .comp-panel.planned h4 { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .comp-step.neutral { background: #1e2530; border-left-color: #5a6878; }
  html[data-theme="dark"] .blog-fullhtml .comp-step.ok { background: #1a2a1a; border-left-color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .comp-step.fail { background: #2a1a1a; border-left-color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .comp-step { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .comp-result.errors { background: #2a1515; border-color: #5a2525; color: #e8a0a0; }
  html[data-theme="dark"] .blog-fullhtml .comp-result.success { background: #152a15; border-color: #255a25; color: #a0e8a0; }
  html[data-theme="dark"] .blog-fullhtml .references { border-top-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .references ol { color: #9aa8b8; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #6a7888; border-top-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .math { color: #6aafe6; }
  html[data-theme="dark"] .blog-fullhtml .wh-btn.secondary { background: #2a3545; color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .wh-btn.secondary:hover { background: #354560; }
---

<header class="hero">
    <div class="series-label">Planning in the Era of LLMs — Part 1 of 7</div>
    <h1>Making LLM Agents Actually Plan: A Roadmap</h1>
    <p class="subtitle">Why your LLM agent fails at multi-step tasks, and a 7-part guide to fixing it.</p>
</header>

<article class="blog-container">

    <!-- Series Navigation -->
    <div class="vis-container">
        <div class="series-nav">
            <strong>📚 Planning in the Era of LLMs — Part 1 of 7</strong>
            <div class="nav-desc">A high-level overview of the series: why LLM agents fail at planning, and how the formal planning community is fixing it.</div>
            <div class="nav-links">
                Next: Part 2 — "The Formal Planning Primer" →
            </div>
        </div>
    </div>

    <p class="lead">Your LLM agent is impressive. It writes clean code, summarizes dense papers, drafts emails in your voice. Then you ask it to do something that requires more than one step — and it falls apart.</p>

    <p>You tell it to plan a trip. It books a flight arriving at 11 PM and a dinner reservation at 7 PM the same evening. You ask it to set up a deployment pipeline. It schedules the deploy step before the tests. You ask it to plan a dinner party for eight guests with dietary restrictions. It produces a beautiful timeline where two dishes need the oven at the same time, appetizers finish after the main course, and the "vegan" option contains cheese.</p>

    <p>Each individual step looks reasonable. The flight is real. The restaurant is nice. The recipe is good. But the steps don't fit together. Constraints are violated. Dependencies are ignored. State isn't tracked.</p>

    <p>These aren't language failures. They're <strong>planning</strong> failures.</p>

    <p>LLMs are extraordinarily good at generating plausible-sounding next steps. They are extraordinarily bad at ensuring those steps form a coherent, constraint-satisfying sequence. And if you're building agentic AI systems — anything that coordinates multi-step tasks in the real world — this distinction is the difference between a demo and a product.</p>

    <h2>What the Planning Community Brings to the Table</h2>

    <p>Here's what most people building with LLMs don't know: there is an entire subfield of AI dedicated to exactly this problem. It's called <strong>automated planning</strong> — it has been around for decades, but the rise of LLM-based agents has brought it into the spotlight from a completely new angle.</p>

    <div class="vis-container">
        <div class="callout insight">
            <div class="callout-label">Key Insight</div>
            <p>Formal planners provide mathematical guarantees. A plan returned by a sound planner is <em>guaranteed</em> to be valid — every precondition satisfied, every constraint respected. An optimal planner guarantees the plan is the cheapest or shortest possible. These aren't vibes. They're proofs.</p>
        </div>
    </div>

    <p>Planning solvers take a formal description of a problem — what states exist, what actions are available, what the goal is — and they search for a valid sequence of actions to reach that goal. The best modern solvers handle millions of states efficiently. They've been honed through international competitions and decades of research, and they're now finding a second life as the backbone of reliable agentic systems.</p>

    <p>The one thing they need? A formal description of the problem. The planning community uses a language called <strong>PDDL</strong> (Planning Domain Definition Language) — a precise specification of what's possible and what's desired. Writing PDDL requires expertise. It's not something you hand to a product manager.</p>

    <p>That's where LLMs come in. Not as replacements for planners — but as a bridge between human language and formal planning tools.</p>

    <h2>The Two Paradigms</h2>

    <p>The research on combining LLMs with planning splits into two distinct paradigms, and understanding this split is the key to understanding the whole field.</p>

    <div class="vis-container">
        <div class="paradigm-grid">
            <div class="paradigm-box p1">
                <span class="tag">Paradigm 1</span>
                <h4>PDDL is Given</h4>
                <p style="font-size:0.85rem; margin-bottom:10px;">An expert has already written the formal model. The LLM helps <em>find the plan</em>.</p>
                <ul>
                    <li>LLM generates candidate plans for verification</li>
                    <li>LLM writes heuristic code to guide search</li>
                    <li>LLM produces policies as Python functions</li>
                    <li>Formal tools guarantee correctness</li>
                </ul>
                <p style="font-size:0.82rem; color:#4a6a90; margin-top:10px; margin-bottom:8px;">Covered in Posts 4 &amp; 5</p>
                <div class="flow-row" style="margin-top:12px; padding-top:12px; border-top:1px solid #c8d8e8;">
                    <div class="flow-node formal-rules">Formal Rules<br>(PDDL)</div>
                    <div class="flow-arrow">→</div>
                    <div class="flow-node llm-assists">LLM Assists<br>Planner</div>
                    <div class="flow-arrow">→</div>
                    <div class="flow-node verified-plan">Verified<br>Plan</div>
                </div>
            </div>
            <div class="paradigm-box p2">
                <span class="tag">Paradigm 2</span>
                <h4>Only Natural Language</h4>
                <p style="font-size:0.85rem; margin-bottom:10px;">No PDDL exists. The LLM must <em>create the formal model</em> from an English description, then solve it.</p>
                <ul>
                    <li>LLM extracts types, predicates, and actions from text</li>
                    <li>Multi-agent systems refine and validate PDDL</li>
                    <li>Orchestrator coordinates specialized agents</li>
                    <li>The orchestrator is the key bottleneck</li>
                </ul>
                <p style="font-size:0.82rem; color:#8a5c00; margin-top:10px; margin-bottom:8px;">Covered in Posts 6 &amp; 7</p>
                <div class="flow-row" style="margin-top:12px; padding-top:12px; border-top:1px solid #e0cdb0;">
                    <div class="flow-node nl-input">"Pack 3<br>boxes..."</div>
                    <div class="flow-arrow">→</div>
                    <div class="flow-node llm-generates">LLM Generates<br>Formal Model</div>
                    <div class="flow-arrow">→</div>
                    <div class="flow-node verified-plan">Verified<br>Plan</div>
                </div>
            </div>
        </div>
        <p class="vis-caption">The two paradigms of LLM-Planning integration. Paradigm 2 is harder — and more exciting — because it removes the need for human PDDL expertise entirely.</p>
    </div>

    <p>Paradigm 1 is powerful and already producing strong results. If you have a domain expert who can write PDDL, you can get dramatic improvements by using LLMs to generate heuristics, policies, or candidate plans that formal tools then verify.</p>

    <p>Paradigm 2 is harder and more ambitious. A user describes a task in plain English — "My robot can carry two items, fragile items go on top, it needs to recharge every 30 minutes" — and the system converts that to a formal model, validates it, solves it, and returns a verified plan. No PDDL expertise required. This is the frontier, and it's where the most exciting unsolved problems live.</p>

    <h2>The Evolution</h2>

    <p>The field has moved fast. In just three years, the research question has shifted entirely.</p>

    <div class="vis-container">
        <div class="timeline-track">
            <div class="timeline-node">
                <div class="timeline-dot"></div>
                <div class="timeline-year">2022–2023</div>
                <div class="timeline-question">"Can LLMs plan?"</div>
                <div class="timeline-answer">Mostly no. ~12% success rate.</div>
                <div class="timeline-post-ref">Post 4</div>
            </div>
            <div class="timeline-node">
                <div class="timeline-dot"></div>
                <div class="timeline-year">2023–2024</div>
                <div class="timeline-question">"Can LLMs help planners?"</div>
                <div class="timeline-answer">Yes. Generate + verify works.</div>
                <div class="timeline-post-ref">Post 5</div>
            </div>
            <div class="timeline-node">
                <div class="timeline-dot"></div>
                <div class="timeline-year">2024–2025</div>
                <div class="timeline-question">"English to plans?"</div>
                <div class="timeline-answer">Emerging. Orchestration is key.</div>
                <div class="timeline-post-ref">Post 6</div>
            </div>
            <div class="timeline-node">
                <div class="timeline-dot"></div>
                <div class="timeline-year">2025+</div>
                <div class="timeline-question">"Agentic AI for planning?"</div>
                <div class="timeline-answer">The frontier.</div>
                <div class="timeline-post-ref">Post 7</div>
            </div>
        </div>
        <p class="vis-caption">The rapid evolution of LLM-Planning research. Each phase built on the failures and insights of the previous one.</p>
    </div>

    <p>The first wave asked whether LLMs could plan on their own. Rigorous benchmarks showed they couldn't — frontier models solved about 12% of planning problems correctly. Renaming predicates to meaningless tokens collapsed performance to zero, proving LLMs were doing pattern retrieval, not reasoning.</p>

    <p>The second wave was more productive. Researchers stopped asking LLMs to plan and started asking them to <em>help</em> planners. LLMs generated candidate plans that formal verifiers checked. LLMs wrote Python heuristic functions that guided classical search. Results jumped from 12% to 82% on the same benchmarks.</p>

    <p>The third wave removed the requirement for expert-written PDDL. Multi-agent systems where LLMs translate English to formal models started achieving real results — 100% on some domains, though orchestration failures remained a bottleneck.</p>

    <p>The emerging fourth wave asks: can we build truly agentic systems for planning — systems that learn, adapt, and improve their own coordination strategies? That's the frontier this series builds toward.</p>

    <h2>The Roadmap</h2>

    <p>This series walks through the entire landscape, from foundations to the research frontier. Here's where we're going.</p>

    <div class="vis-container">
        <ul class="roadmap-list">
            <li>
                <span class="post-num">Post 2 — Foundations</span>
                <span class="post-title">The Formal Planning Primer</span><br>
                The math, the language (PDDL), and the computational complexity. Everything you need to follow the rest of the series. Skip if you already know PDDL.
            </li>
            <li>
                <span class="post-num">Post 3 — Classical Algorithms</span>
                <span class="post-title">50 Years of Planning Algorithms</span><br>
                Heuristic search, relaxation, Fast Downward, and the International Planning Competition. What we're integrating <em>with</em>, not replacing.
            </li>
            <li>
                <span class="post-num">Post 4 — The Reality Check</span>
                <span class="post-title">LLMs Try to Plan (It Goes Badly)</span><br>
                PlanBench, Mystery Blocksworld, and the evidence that self-critique makes things worse. The sobering data on why LLMs alone can't plan.
            </li>
            <li>
                <span class="post-num">Post 5 — What Works (Paradigm 1)</span>
                <span class="post-title">The Modern Playbook: LLMs That Help Planners</span><br>
                LLM-Modulo, code generation for heuristics and policies, Thought of Search. What happens when you stop asking LLMs to plan and start asking them to help.
            </li>
            <li>
                <span class="post-num">Post 6 — The Frontier (Paradigm 2)</span>
                <span class="post-title">From English to Plans: The NL-to-PDDL Frontier</span><br>
                NL2Plan, agentic PDDL frameworks, and the orchestrator bottleneck. Describe a task in English, get a verified plan — and why the conductor can't keep up.
            </li>
            <li>
                <span class="post-num">Post 7 — The Research Edge</span>
                <span class="post-title">Agentic AI for Planning</span><br>
                The future of LLM-powered planning systems — building agents that learn to coordinate, adapt, and improve. Where the field goes next.
            </li>
        </ul>
        <p class="vis-caption">The full series arc: from foundations through failures to the systems that actually work — and the open problems at the frontier.</p>
    </div>

    <h2>Meet RoboSort</h2>

    <p>Throughout this series, we'll follow a single running example: a warehouse robot named <strong>RoboSort</strong> working at the <strong>PackBot Warehouse</strong>.</p>

    <p style="font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; font-style: italic; margin-bottom: 8px;">Interactive — click "▶ Auto Demo" to watch a naive LLM plan fail, then see planning fix it</p>

    <div class="vis-container" style="padding: 1em 1em 0.5em;">

        <div class="warehouse-scene" id="warehouse">
            <div class="wh-ceiling"></div>
            <div class="wh-light" style="left: 80px;"></div>
            <div class="wh-light" style="left: 240px;"></div>
            <div class="wh-light" style="left: 400px;"></div>
            <div class="wh-light" style="left: 560px;"></div>
            <div class="wh-floor"></div>
            <div class="wh-status" id="wh-status">
                <span class="status-action" id="status-action">Awaiting orders...</span>
                <span class="status-state" id="status-state">Gripper: empty | Delivered: 0/5</span>
            </div>
            <div class="wh-shelf" style="left: 40px;" id="shelf-a">
                <div class="wh-shelf-label">Shelf A</div>
                <div class="wh-shelf-row"></div><div class="wh-shelf-row"></div><div class="wh-shelf-row"></div>
            </div>
            <div class="wh-shelf" style="left: 200px;" id="shelf-b">
                <div class="wh-shelf-label">Shelf B</div>
                <div class="wh-shelf-row"></div><div class="wh-shelf-row"></div><div class="wh-shelf-row"></div>
            </div>
            <div class="wh-shelf" style="left: 360px;" id="shelf-c">
                <div class="wh-shelf-label">Shelf C</div>
                <div class="wh-shelf-row"></div><div class="wh-shelf-row"></div><div class="wh-shelf-row"></div>
            </div>
            <div class="wh-package pkg-1" id="pkg-1" style="left:55px; bottom:190px;">P1</div>
            <div class="wh-package pkg-2" id="pkg-2" style="left:95px; bottom:190px;">P2</div>
            <div class="wh-package pkg-3" id="pkg-3" style="left:225px; bottom:190px;">P3</div>
            <div class="wh-package pkg-4" id="pkg-4" style="left:375px; bottom:190px;">P4</div>
            <div class="wh-package pkg-5" id="pkg-5" style="left:415px; bottom:190px;">P5</div>
            <div class="wh-dock"><div class="wh-dock-inner">LOADING DOCK</div></div>
            <div class="wh-robot" id="robot" style="left: 40px; bottom: 30px;">
                <div class="wh-robot-body">
                    <div class="wh-robot-eyes"><div class="wh-robot-eye"></div><div class="wh-robot-eye"></div></div>
                    <div class="wh-robot-arm" id="robot-arm"></div>
                </div>
                <div class="wh-robot-wheels"><div class="wh-robot-wheel"></div><div class="wh-robot-wheel"></div><div class="wh-robot-wheel"></div></div>
                <div class="wh-robot-label">RoboSort</div>
            </div>
        </div>

        <div class="comparison-wrapper" style="margin-top: 4px;">
            <div class="comp-panel naive">
                <h4>Naive LLM Plan</h4>
                <div class="comp-step neutral" id="n1">1. Go to Shelf C, pick P4</div>
                <div class="comp-step neutral" id="n2">2. Pick P5 too (already holding P4!)</div>
                <div class="comp-step neutral" id="n3">3. Place both at Dock</div>
                <div class="comp-step neutral" id="n4">4. Shelf A → pick P1, go to Dock... (skips P2)</div>
                <div class="comp-result errors" id="naive-result">
                    <strong>3 constraint violations:</strong>
                    ✗ Picks P5 while holding P4 — gripper full<br>
                    ✗ Places "both" — can only place one at a time<br>
                    ✗ Skips P2 — state not tracked, delivery incomplete<br><br>
                    <em>Plan is invalid. Cannot execute.</em>
                </div>
            </div>
            <div class="comp-panel planned">
                <h4>Planning-Enhanced Agent</h4>
                <div class="comp-step neutral" id="p1">1. Formalize: 1 gripper, pick→move→place</div>
                <div class="comp-step neutral" id="p2">2. Plan: left-to-right minimizes travel</div>
                <div class="comp-step neutral" id="p3">3. Execute: A→P1→Dock, A→P2→Dock, B→P3...</div>
                <div class="comp-step neutral" id="p4">4. Verify: all constraints met, 5/5 delivered</div>
                <div class="comp-result success" id="planner-result">
                    <strong>All constraints satisfied:</strong>
                    ✓ Single gripper — one package per trip<br>
                    ✓ All 5 packages delivered to dock<br>
                    ✓ 10 moves, zero backtracking<br><br>
                    <em>Plan is guaranteed valid and optimal.</em>
                </div>
            </div>
        </div>
        <p class="vis-caption">The naive LLM produces a plan that <em>looks</em> reasonable but violates physical constraints. The planning-enhanced agent formalizes constraints first, then finds a verified, optimal plan.</p>

    </div>

    <div class="wh-controls" style="padding: 4px 0 12px; justify-content: center;">
        <button class="auto-demo-btn" id="wh-auto-btn" onclick="runWarehouseAutodemo()">▶ Auto Demo</button>
        <button class="wh-btn secondary" id="wh-reset-btn" onclick="resetWarehouse()" style="display:none;">↺ Reset</button>
    </div>

    <p>The setup is simple: three shelves holding five packages, one loading dock, and RoboSort with a single gripper. A customer order arrives, and the robot must pick each requested package from its shelf and deliver it to the dock.</p>

    <p>This sounds easy. It isn't. If RoboSort picks packages in the wrong order, it backtracks across the warehouse. If it grabs a package before clearing the one blocking it, it's stuck. If it doesn't account for its single gripper, it tries impossible moves. Every post in this series will revisit this warehouse — growing from 5 packages to 50, adding aisles, conveyor belts, charging stations, and eventually new facilities with novel rules described only in English.</p>

    <div class="vis-container">
        <div class="callout insight">
            <div class="callout-label">Key Insight</div>
            <p>The warehouse robot isn't just a toy example. Every agentic AI system that coordinates multi-step tasks — code generation, workflow automation, travel booking, deployment pipelines — faces the same core problem: sequencing actions under constraints with state tracking. Planning is the general framework. The warehouse just makes it concrete.</p>
        </div>
    </div>

    <h3>Beyond the Warehouse: Planning in Agentic AI</h3>

    <p>RoboSort makes the problem concrete, but every agentic AI system faces the same challenge. If you're building agents that coordinate multi-step tasks, here's how the exact same planning principles apply to your domain.</p>

    <div class="vis-container">
        <div class="agentic-sidebar">
            <div class="sidebar-title">🤖 Agentic AI in the Wild: The Travel Agent That Can't</div>
            <p>Consider an AI travel agent booking a week-long trip under a $3000 budget. It must handle constraints: hotel near the conference venue, flight arriving before 6pm for the welcome dinner, dietary restrictions for restaurants. This is a planning problem — with states (what's booked), actions (book flight, reserve hotel), preconditions (can't book hotel before knowing flight dates), and goals (complete itinerary under budget).</p>
            <p style="margin-top: 10px;">Just like RoboSort, a naive agent books each item independently and ends up with constraint violations — a late flight, a distant hotel, an over-budget dinner. A planning-aware agent formalizes the constraints first, searches within bounds, and verifies before committing. Same principle, different domain.</p>
            <p style="margin-top: 10px;">Every agentic system that coordinates multi-step tasks — coding agents, deployment pipelines, customer service workflows — is solving a planning problem. The formal planning framework gives us the language and tools to do it correctly. Each post in this series includes a sidebar like this one, connecting the planning concepts to real agentic AI applications.</p>
        </div>
    </div>

    <h2>What's Ahead</h2>

    <p>The core thesis of this series is simple: LLMs alone cannot plan reliably, but the combination of LLMs and formal planning tools is extraordinarily powerful. The planning community has built solvers that guarantee correctness. LLMs bring natural language understanding, flexibility, and the ability to bridge the gap between how humans describe problems and how solvers need to receive them. The convergence of these two communities is reshaping what agentic AI can reliably accomplish.</p>

    <p>By the end of these seven posts, you'll understand exactly how to make your LLM agent plan reliably — and why the answer involves a well-established AI subfield that the LLM community has been rapidly integrating over the past few years. You'll know what PDDL is, how heuristic search works, why Tree of Thoughts is expensive and unsound, what LLM-Modulo actually does, how NL2Plan converts English to formal models, and what the frontier of agentic AI for planning looks like.</p>

    <p>Let's get started.</p>

    <div class="next-post">
        <h3>Up Next: Part 2 — The Formal Planning Primer</h3>
        <p>States, actions, goals, and the language that makes them precise. We'll formalize the warehouse robot problem in PDDL, understand why planning is computationally hard, and build the vocabulary you need for the rest of the series. If you already know PDDL, skip straight to Post 3.</p>
    </div>

    <div class="references">
        <h2>References</h2>
        <ol>
            <li>Katz, M., Kokel, H., &amp; Muise, C. (2025). <em>Planning in the Era of Language Models.</em> NeurIPS 2025 Tutorial.</li>
            <li>Valmeekam, K., Marquez, M., Sreedharan, S., &amp; Kambhampati, S. (2023). <em>PlanBench: An Extensible Benchmark for Evaluating Large Language Models on Planning and Reasoning about Change.</em> NeurIPS 2023.</li>
            <li>Kambhampati, S., Valmeekam, K., Guan, L., Stechly, K., Verma, M., Rao, S., Kokel, H., &amp; Katz, M. (2024). <em>LLMs Can't Plan, But Can Help Planning in LLM-Modulo Frameworks.</em> ICML 2024. arXiv: 2402.01817.</li>
            <li>Katz, M., Kokel, H., Srinivas, K., &amp; Sheth, R. (2024). <em>Thought of Search: Planning with Language Models Through the Lens of Efficiency.</em> NeurIPS 2024.</li>
            <li>Gestrin, M., Schreiber, H., &amp; Trevizan, F. (2025). <em>NL2Plan: Robust LLM-Driven Planning from Minimal Text.</em> arXiv: 2405.04215.</li>
            <li>La Malfa, G., et al. (2025). <em>Agentic PDDL: An Agentic Framework for Formal Planning.</em> arXiv: 2512.09629.</li>
            <li>Corrêa, A. B., et al. (2025). <em>LLM-Generated Heuristics for AI Planning.</em> NeurIPS 2025. arXiv: 2503.18809.</li>
            <li>Chen, K., et al. (2025). <em>LMPLAN: Sound Policies from LLMs.</em> RLC 2025. arXiv: 2508.18507.</li>
            <li>Stechly, K., Valmeekam, K., &amp; Kambhampati, S. (2023). <em>GPT-4 Doesn't Know It's Wrong.</em> NeurIPS FMDM Workshop 2023.</li>
        </ol>
    </div>

</article>

<div class="blog-footer">
    <p>Planning in the Era of LLMs — Part 1 of 7</p>
</div>

<script>
var whRunning = false;
var robotState = { x: 40, y: 30, carrying: null, delivered: 0 };

var pkgHome = {
    1: { x: 55, y: 190 },
    2: { x: 95, y: 190 },
    3: { x: 225, y: 190 },
    4: { x: 375, y: 190 },
    5: { x: 415, y: 190 }
};
var dockPos = { x: 540, y: 40 };

function setRobot(x, y) {
    var r = document.getElementById('robot');
    r.style.left = x + 'px'; r.style.bottom = y + 'px';
    robotState.x = x; robotState.y = y;
}
function setPkg(id, x, y, vis) {
    var p = document.getElementById('pkg-' + id);
    p.style.left = x + 'px'; p.style.bottom = y + 'px';
    p.style.opacity = vis ? '1' : '0.3';
}
function setStatus(action, state) {
    document.getElementById('status-action').textContent = action;
    document.getElementById('status-state').textContent = state;
}
function setArm(c) {
    document.getElementById('robot-arm').className = c ? 'wh-robot-arm carrying' : 'wh-robot-arm';
}
function delay(ms) { return new Promise(function(r) { setTimeout(r, ms); }); }

function activateStep(id, cls) {
    var el = document.getElementById(id);
    el.className = 'comp-step active ' + cls;
}

function resetWarehouse() {
    whRunning = false;
    robotState = { x: 40, y: 30, carrying: null, delivered: 0 };
    setRobot(40, 30); setArm(false);
    document.getElementById('status-action').style.color = '';
    document.getElementById('warehouse').style.boxShadow = '';
    for (var i = 1; i <= 5; i++) setPkg(i, pkgHome[i].x, pkgHome[i].y, true);
    setStatus('Awaiting orders...', 'Gripper: empty | Delivered: 0/5');
    for (var j = 1; j <= 4; j++) {
        document.getElementById('n' + j).className = 'comp-step neutral';
        document.getElementById('p' + j).className = 'comp-step neutral';
    }
    document.getElementById('naive-result').style.visibility = 'hidden';
    document.getElementById('planner-result').style.visibility = 'hidden';
    document.getElementById('wh-auto-btn').style.display = '';
    document.getElementById('wh-auto-btn').disabled = false;
    document.getElementById('wh-reset-btn').style.display = 'none';
}

async function runWarehouseAutodemo() {
    if (whRunning) return;
    whRunning = true;
    document.getElementById('wh-auto-btn').disabled = true;
    document.getElementById('wh-reset-btn').style.display = '';

    setStatus('▶ Naive LLM generating plan...', 'No constraint checking');
    document.getElementById('status-action').style.color = '#f38ba8';
    await delay(700);

    activateStep('n1', 'neutral');
    setRobot(375, 100); await delay(400);
    setArm(true); robotState.carrying = 4;
    setPkg(4, 395, 130, true);
    setStatus('Go to Shelf C, PICK(P4)', 'Gripper: P4');
    document.getElementById('status-action').style.color = '#f38ba8';
    await delay(600);

    activateStep('n2', 'fail');
    setRobot(415, 100); setPkg(4, 435, 130, true);
    setStatus('PICK(P5) — ✗ GRIPPER FULL!', 'Cannot hold two packages!');
    document.getElementById('status-action').style.color = '#ff4444';
    document.getElementById('warehouse').style.boxShadow = '0 0 0 3px #ff4444';
    await delay(1100);
    document.getElementById('warehouse').style.boxShadow = '';

    activateStep('n3', 'fail');
    setRobot(540, 30); setPkg(4, 560, 60, true);
    setStatus('Place "both"? — ✗ Only one!', 'Plan is incoherent');
    document.getElementById('status-action').style.color = '#ff4444';
    document.getElementById('warehouse').style.boxShadow = '0 0 0 3px #ff4444';
    await delay(1100);
    document.getElementById('warehouse').style.boxShadow = '';
    setArm(false); robotState.carrying = null;
    setPkg(4, dockPos.x, dockPos.y + 10, false);

    activateStep('n4', 'fail');
    setRobot(55, 100); await delay(300);
    setArm(true); setPkg(1, 75, 130, true);
    setStatus('Picks P1, skips P2 — state lost', 'Delivery will be incomplete');
    document.getElementById('status-action').style.color = '#ff4444';
    document.getElementById('warehouse').style.boxShadow = '0 0 0 3px #ff4444';
    await delay(1100);
    document.getElementById('warehouse').style.boxShadow = '';
    setArm(false); setPkg(1, dockPos.x + 6, dockPos.y + 10, false);

    setStatus('✗ INVALID — 3 violations', 'Cannot execute');
    document.getElementById('status-action').style.color = '#ff4444';
    document.getElementById('naive-result').style.visibility = 'visible';
    await delay(2200);

    robotState = { x: 40, y: 30, carrying: null, delivered: 0 };
    setRobot(40, 30); setArm(false);
    document.getElementById('warehouse').style.boxShadow = '';
    for (var i = 1; i <= 5; i++) setPkg(i, pkgHome[i].x, pkgHome[i].y, true);
    setStatus('▶ Planning-Enhanced Agent...', 'Formalizing constraints');
    document.getElementById('status-action').style.color = '#a6e3a1';
    await delay(1000);

    var d = 0;

    activateStep('p1', 'ok');
    setStatus('Constraint model: 1 gripper, pick→move→place', 'Building...');
    document.getElementById('status-action').style.color = '#a6e3a1';
    await delay(700);

    activateStep('p2', 'ok');
    setStatus('Optimal route: left→right, A→B→C→Dock', 'Planned');
    await delay(600);

    activateStep('p3', 'ok');
    setRobot(55, 100); await delay(250);
    setArm(true); setPkg(1, 75, 130, true);
    setStatus('PICK(P1) → Dock', 'Delivering...'); await delay(300);
    setRobot(530, 30); setPkg(1, 550, 60, true); await delay(200);
    setArm(false); d++; setPkg(1, dockPos.x - 10, dockPos.y, false); await delay(200);
    setRobot(95, 100); await delay(200);
    setArm(true); setPkg(2, 115, 130, true); await delay(200);
    setRobot(545, 30); setPkg(2, 565, 60, true); await delay(200);
    setArm(false); d++; setPkg(2, dockPos.x - 4, dockPos.y, false); await delay(200);
    setRobot(225, 100); await delay(200);
    setArm(true); setPkg(3, 245, 130, true); await delay(200);
    setRobot(555, 35); setPkg(3, 575, 65, true); await delay(200);
    setArm(false); d++; setPkg(3, dockPos.x + 2, dockPos.y, false);
    setStatus('Executing: P1✓ P2✓ P3✓ ...', 'Delivered: ' + d + '/5');
    await delay(200);
    setRobot(375, 100); await delay(200);
    setArm(true); setPkg(4, 395, 130, true); await delay(200);
    setRobot(540, 45); setPkg(4, 560, 75, true); await delay(200);
    setArm(false); d++; setPkg(4, dockPos.x + 8, dockPos.y, false); await delay(150);
    setRobot(415, 100); await delay(200);
    setArm(true); setPkg(5, 435, 130, true); await delay(200);
    setRobot(560, 45); setPkg(5, 580, 75, true); await delay(200);
    setArm(false); d++; setPkg(5, dockPos.x + 14, dockPos.y, false);
    setStatus('All 5 delivered: P1✓ P2✓ P3✓ P4✓ P5✓', 'Delivered: 5/5');
    await delay(400);

    activateStep('p4', 'ok');
    setStatus('✓ DONE — 10 moves, 0 violations, 5/5 delivered', 'Plan verified and optimal');
    document.getElementById('planner-result').style.visibility = 'visible';
}
</script>
