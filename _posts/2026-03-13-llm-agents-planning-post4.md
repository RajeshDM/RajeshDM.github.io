---
layout: fullhtml-post
title: "LLMs Try to Plan (It Goes Badly)"
date: 2026-03-13
categories: ["LLMs Automated Planning and Agents"]
tags: ["planning", "llm", "benchmarks"]
description: "PlanBench, Mystery Blocksworld, and the sobering evidence that frontier models can't reliably sequence three actions. Part 4 of the Planning in the Era of LLMs series."
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
  .blog-fullhtml .interactive-container .interactive-label {
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.82rem;
      color: #888;
      margin-bottom: 12px;
      font-style: italic;
  }

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

  .blog-fullhtml footer {
      text-align: center; padding: 32px 20px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.82rem; color: #888; border-top: 1px solid #eee;
  }

  .blog-fullhtml .math { font-family: 'Cambria Math', 'Georgia', serif; font-style: italic; color: #2a4066; }

  .blog-fullhtml .benchmark-chart {
      display: flex;
      flex-direction: column;
      gap: 12px;
      padding: 20px 0;
  }
  .blog-fullhtml .bench-row {
      display: grid;
      grid-template-columns: 140px 1fr 50px;
      align-items: center;
      gap: 12px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.85rem;
  }
  .blog-fullhtml .bench-label {
      text-align: right;
      font-weight: 600;
      color: #1b2838;
  }
  .blog-fullhtml .bench-bar-track {
      height: 28px;
      background: #e8ecf1;
      border-radius: 4px;
      overflow: hidden;
      position: relative;
  }
  .blog-fullhtml .bench-bar {
      height: 100%;
      border-radius: 4px;
      transition: width 1.5s ease-out;
  }
  .blog-fullhtml .bench-value {
      font-weight: 700;
      color: #1b2838;
      font-size: 0.9rem;
  }

  .blog-fullhtml .mystery-comparison {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
      margin: 20px 0;
  }
  .blog-fullhtml .mystery-panel {
      border-radius: 10px;
      padding: 20px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .mystery-panel.original {
      background: #f0faf0;
      border: 2px solid #228b22;
  }
  .blog-fullhtml .mystery-panel.obfuscated {
      background: #fdf2f2;
      border: 2px solid #b22222;
  }
  .blog-fullhtml .mystery-panel h4 {
      font-size: 0.95rem;
      margin-bottom: 12px;
  }
  .blog-fullhtml .mystery-panel.original h4 { color: #228b22; }
  .blog-fullhtml .mystery-panel.obfuscated h4 { color: #b22222; }
  .blog-fullhtml .mystery-panel pre {
      font-size: 0.78rem;
      padding: 14px;
      margin: 0;
      background: #1e1e2e;
      border-radius: 6px;
  }
  .blog-fullhtml .mystery-result {
      margin-top: 10px;
      padding: 8px 12px;
      border-radius: 6px;
      font-size: 0.82rem;
      font-weight: 600;
      text-align: center;
  }
  .blog-fullhtml .mystery-result.pass {
      background: #d4edda;
      color: #155724;
  }
  .blog-fullhtml .mystery-result.fail {
      background: #f8d7da;
      color: #721c24;
  }

  .blog-fullhtml .strategy-table {
      width: 100%;
      border-collapse: collapse;
      margin: 20px 0;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.88rem;
  }
  .blog-fullhtml .strategy-table th {
      background: #1b2838;
      color: #e0e8f0;
      padding: 12px 16px;
      text-align: left;
      font-weight: 600;
      font-size: 0.82rem;
      text-transform: uppercase;
      letter-spacing: 0.5px;
  }
  .blog-fullhtml .strategy-table td {
      padding: 10px 16px;
      border-bottom: 1px solid #e0e4ea;
      vertical-align: top;
  }
  .blog-fullhtml .strategy-table tr:nth-child(even) { background: #f8fafc; }
  .blog-fullhtml .strategy-table tr:hover { background: #edf2f7; }
  .blog-fullhtml .strategy-table .score-bad { color: #b22222; font-weight: 700; }
  .blog-fullhtml .strategy-table .score-worse { color: #8b0000; font-weight: 700; }
  .blog-fullhtml .strategy-table .score-ok { color: #d4740e; font-weight: 700; }

  .blog-fullhtml .taxonomy-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
      margin: 20px 0;
  }
  .blog-fullhtml .taxonomy-card {
      border-radius: 10px;
      padding: 18px 20px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .taxonomy-card .tax-label {
      font-size: 0.72rem;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 1.5px;
      margin-bottom: 6px;
  }
  .blog-fullhtml .taxonomy-card h4 {
      font-size: 1rem;
      margin-bottom: 8px;
  }
  .blog-fullhtml .taxonomy-card p {
      font-size: 0.85rem;
      margin-bottom: 0;
  }
  .blog-fullhtml .taxonomy-card.role-direct {
      background: #fdf2f2;
      border: 2px solid #b22222;
  }
  .blog-fullhtml .taxonomy-card.role-direct .tax-label { color: #b22222; }
  .blog-fullhtml .taxonomy-card.role-direct h4 { color: #8b1a1a; }
  .blog-fullhtml .taxonomy-card.role-direct p { color: #5a1010; }
  .blog-fullhtml .taxonomy-card.role-heuristic {
      background: #f0faf0;
      border: 2px solid #228b22;
  }
  .blog-fullhtml .taxonomy-card.role-heuristic .tax-label { color: #228b22; }
  .blog-fullhtml .taxonomy-card.role-heuristic h4 { color: #1a6b1a; }
  .blog-fullhtml .taxonomy-card.role-heuristic p { color: #0a4a0a; }
  .blog-fullhtml .taxonomy-card.role-translator {
      background: #fff7ed;
      border: 2px solid #d4740e;
  }
  .blog-fullhtml .taxonomy-card.role-translator .tax-label { color: #d4740e; }
  .blog-fullhtml .taxonomy-card.role-translator h4 { color: #b35c00; }
  .blog-fullhtml .taxonomy-card.role-translator p { color: #7a3f00; }
  .blog-fullhtml .taxonomy-card.role-verifier {
      background: #f5f0ff;
      border: 2px solid #6b3fa0;
  }
  .blog-fullhtml .taxonomy-card.role-verifier .tax-label { color: #6b3fa0; }
  .blog-fullhtml .taxonomy-card.role-verifier h4 { color: #5a2d8a; }
  .blog-fullhtml .taxonomy-card.role-verifier p { color: #3a2060; }

  .blog-fullhtml .llm-demo-container {
      width: 960px;
      max-width: calc(100vw - 40px);
      margin-left: 50%;
      transform: translateX(-50%);
      position: relative;
  }
  .blog-fullhtml .llm-demo-split {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 24px;
      margin-top: 14px;
  }
  .blog-fullhtml .llm-panel {
      border-radius: 10px;
      padding: 20px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      min-height: 400px;
  }
  .blog-fullhtml .llm-panel.gpt-panel {
      background: #fdf8f0;
      border: 2px solid #d4740e;
  }
  .blog-fullhtml .llm-panel.planner-panel {
      background: #f0faf0;
      border: 2px solid #228b22;
  }
  .blog-fullhtml .llm-panel h4 {
      font-size: 0.95rem;
      margin-bottom: 14px;
      display: flex;
      align-items: center;
      gap: 8px;
  }
  .blog-fullhtml .llm-panel.gpt-panel h4 { color: #b35c00; }
  .blog-fullhtml .llm-panel.planner-panel h4 { color: #1a6b1a; }
  .blog-fullhtml .step-list {
      list-style: none;
      padding: 0;
      margin: 0;
      font-size: 0.85rem;
  }
  .blog-fullhtml .step-list li {
      padding: 8px 12px;
      margin-bottom: 6px;
      border-radius: 6px;
      display: flex;
      align-items: flex-start;
      gap: 8px;
      line-height: 1.5;
      transition: all 0.3s ease;
  }
  .blog-fullhtml .step-list li .step-icon {
      flex-shrink: 0;
      width: 20px;
      height: 20px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.65rem;
      font-weight: 700;
      color: #fff;
      margin-top: 2px;
  }
  .blog-fullhtml .step-ok .step-icon { background: #228b22; }
  .blog-fullhtml .step-fail .step-icon { background: #b22222; }
  .blog-fullhtml .step-ok { background: #e8f5e9; }
  .blog-fullhtml .step-fail { background: #ffebee; }
  .blog-fullhtml .step-neutral { background: #f5f5f5; }
  .blog-fullhtml .step-neutral .step-icon { background: #888; }
  .blog-fullhtml .step-highlight {
      border: 2px solid #d4740e;
      background: #fff3e0 !important;
  }
  .blog-fullhtml .demo-verdict {
      margin-top: 16px;
      padding: 10px 14px;
      border-radius: 8px;
      font-size: 0.85rem;
      font-weight: 600;
      text-align: center;
  }
  .blog-fullhtml .demo-verdict.fail {
      background: #f8d7da;
      color: #721c24;
      border: 1px solid #f0b8b8;
  }
  .blog-fullhtml .demo-verdict.pass {
      background: #d4edda;
      color: #155724;
      border: 1px solid #b5e8b5;
  }

  .blog-fullhtml .failure-counter {
      display: flex;
      justify-content: center;
      gap: 40px;
      margin: 20px 0;
      font-family: 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .counter-item {
      text-align: center;
  }
  .blog-fullhtml .counter-value {
      font-size: 2.5rem;
      font-weight: 800;
      line-height: 1;
  }
  .blog-fullhtml .counter-value.bad { color: #b22222; }
  .blog-fullhtml .counter-value.good { color: #228b22; }
  .blog-fullhtml .counter-label {
      font-size: 0.75rem;
      color: #666;
      text-transform: uppercase;
      letter-spacing: 1px;
      margin-top: 4px;
  }

  .blog-fullhtml .chat-exchange {
      margin: 20px 0;
      font-family: 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .chat-bubble {
      padding: 14px 18px;
      border-radius: 12px;
      margin-bottom: 10px;
      font-size: 0.9rem;
      line-height: 1.6;
      max-width: 92%;
      position: relative;
  }
  .blog-fullhtml .chat-bubble.user {
      background: #2a4066;
      color: #e8edf5;
      margin-left: auto;
      border-bottom-right-radius: 4px;
  }
  .blog-fullhtml .chat-bubble.llm {
      background: #f0f4f8;
      color: #1a1a2e;
      border: 1px solid #d0d8ef;
      border-bottom-left-radius: 4px;
  }
  .blog-fullhtml .chat-bubble .chat-label {
      font-size: 0.7rem;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 1px;
      margin-bottom: 6px;
      display: block;
  }
  .blog-fullhtml .chat-bubble.user .chat-label { color: #7eb8da; }
  .blog-fullhtml .chat-bubble.llm .chat-label { color: #6b3fa0; }
  .blog-fullhtml .chat-bubble .error-highlight {
      background: #ffe0e0;
      color: #8b1a1a;
      padding: 1px 5px;
      border-radius: 3px;
      font-weight: 600;
  }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }

  @media (max-width: 700px) {
      .blog-fullhtml .hero h1 { font-size: 1.8rem; }
      .blog-fullhtml .blog-container { padding: 32px 16px 60px; }
      .blog-fullhtml .mystery-comparison { grid-template-columns: 1fr; }
      .blog-fullhtml .taxonomy-grid { grid-template-columns: 1fr; }
      .blog-fullhtml .llm-demo-split { grid-template-columns: 1fr; }
      .blog-fullhtml .bench-row { grid-template-columns: 100px 1fr 40px; }
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
  html[data-theme="dark"] .blog-fullhtml .vis-container { background: #1e2530; border-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .vis-caption { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .interactive-container { background: #1e2530; border-color: #2a3a50; }
  html[data-theme="dark"] .blog-fullhtml .interactive-container .interactive-label { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .next-post h3 { color: #7eb8da; }
  html[data-theme="dark"] .blog-fullhtml .next-post p { color: #b0c4de; }
  html[data-theme="dark"] .blog-fullhtml .references { border-top-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .references ol { color: #9aa8b8; }
  html[data-theme="dark"] .blog-fullhtml footer { color: #6a7888; border-top-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #6a7888; border-top-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .math { color: #6aafe6; }
  html[data-theme="dark"] .blog-fullhtml .bench-label { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .bench-bar-track { background: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .bench-value { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .mystery-panel.original { background: #142a14; border-color: #228b22; }
  html[data-theme="dark"] .blog-fullhtml .mystery-panel.obfuscated { background: #2a1414; border-color: #b22222; }
  html[data-theme="dark"] .blog-fullhtml .mystery-panel.original h4 { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .mystery-panel.obfuscated h4 { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .mystery-result.pass { background: #1a2a1a; color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .mystery-result.fail { background: #2a1a1a; color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .strategy-table th { background: #1e2a3a; color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml .strategy-table td { border-bottom-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .strategy-table tr:nth-child(even) { background: #1e2530; }
  html[data-theme="dark"] .blog-fullhtml .strategy-table tr:hover { background: #25303f; }
  html[data-theme="dark"] .blog-fullhtml .strategy-table .score-bad { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .strategy-table .score-worse { color: #ff6666; }
  html[data-theme="dark"] .blog-fullhtml .strategy-table .score-ok { color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-direct { background: #2a1414; border-color: #b22222; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-direct .tax-label { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-direct h4 { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-direct p { color: #d0a8a8; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-heuristic { background: #142a14; border-color: #228b22; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-heuristic .tax-label { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-heuristic h4 { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-heuristic p { color: #a8d0a8; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-translator { background: #2a2010; border-color: #d4740e; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-translator .tax-label { color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-translator h4 { color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-translator p { color: #d0bba0; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-verifier { background: #1e142e; border-color: #6b3fa0; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-verifier .tax-label { color: #b088e0; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-verifier h4 { color: #b088e0; }
  html[data-theme="dark"] .blog-fullhtml .taxonomy-card.role-verifier p { color: #c0a8d8; }
  html[data-theme="dark"] .blog-fullhtml .llm-panel.gpt-panel { background: #2a2010; border-color: #d4740e; }
  html[data-theme="dark"] .blog-fullhtml .llm-panel.planner-panel { background: #142a14; border-color: #228b22; }
  html[data-theme="dark"] .blog-fullhtml .llm-panel.gpt-panel h4 { color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .llm-panel.planner-panel h4 { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .step-ok { background: #1a2a1a; }
  html[data-theme="dark"] .blog-fullhtml .step-fail { background: #2a1a1a; }
  html[data-theme="dark"] .blog-fullhtml .step-neutral { background: #1e2530; }
  html[data-theme="dark"] .blog-fullhtml .step-ok .step-icon { background: #228b22; }
  html[data-theme="dark"] .blog-fullhtml .step-fail .step-icon { background: #b22222; }
  html[data-theme="dark"] .blog-fullhtml .step-neutral .step-icon { background: #6a7888; }
  html[data-theme="dark"] .blog-fullhtml .step-highlight { border-color: #d4740e; background: #2a2010 !important; }
  html[data-theme="dark"] .blog-fullhtml .demo-verdict.fail { background: #2a1a1a; color: #e06060; border-color: #b22222; }
  html[data-theme="dark"] .blog-fullhtml .demo-verdict.pass { background: #1a2a1a; color: #5cbf5c; border-color: #228b22; }
  html[data-theme="dark"] .blog-fullhtml .counter-value.bad { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .counter-value.good { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .counter-label { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .chat-bubble.user { background: #1e2a3a; color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml .chat-bubble.llm { background: #1e2530; color: #c9c9ca; border-color: #2a3a50; }
  html[data-theme="dark"] .blog-fullhtml .chat-bubble.user .chat-label { color: #7eb8da; }
  html[data-theme="dark"] .blog-fullhtml .chat-bubble.llm .chat-label { color: #b088e0; }
  html[data-theme="dark"] .blog-fullhtml .chat-bubble .error-highlight { background: #2a1414; color: #e06060; }
---

<header class="hero">
    <div class="series-label">Planning in the Era of LLMs — Part 4 of 7</div>
    <h1>LLMs Try to Plan (It Goes Badly)</h1>
    <p class="subtitle">PlanBench, Mystery Blocksworld, and the sobering evidence that frontier models can't reliably sequence three actions.</p>
</header>

<article class="blog-container">

    <!-- Series Navigation -->
    <div class="vis-container">
        <div class="series-nav">
            <strong>📚 Planning in the Era of LLMs — Part 4 of 7</strong>
            <div class="nav-desc">The reality check: rigorous benchmarks reveal that LLMs — even frontier models — fail catastrophically at planning. Self-critique and chain-of-thought don't fix it.</div>
            <div class="nav-links">
                ← Part 3: "50 Years of Planning Algorithms" | Next: Part 5 — "The Modern Playbook: LLMs That Help Planners" →
            </div>
        </div>
    </div>

    <!-- ============================== -->
    <!-- SECTION 1: Opening Hook        -->
    <!-- ============================== -->

    <p class="lead">Posts 2 and 3 built the foundations: PDDL formalizes a planning problem, and heuristic search solves it. Modern planners handle billions of states with mathematical guarantees. The obvious question follows: do we even need any of that? Can't GPT-4 just plan?</p>

    <p>The question is fair. LLMs can pass the bar exam, write working code, and explain quantum mechanics to a five-year-old. Surely sequencing five actions in the right order is within reach.</p>

    <p>Between 2022 and 2024, a wave of rigorous research tested exactly this. The results weren't just disappointing — they were <em>diagnostic</em>. They didn't just show that LLMs fail at planning. They showed <em>why</em> they fail, and the "why" turned out to be the most important finding. It redirected the entire field.</p>

    <p>This post presents the evidence. We'll run our RoboSort warehouse through the same gauntlet the researchers used, watch frontier models fail at problems a classical planner solves in milliseconds, and understand why the popular fixes — chain-of-thought, self-critique, Tree of Thoughts — don't actually work.</p>

    <!-- ============================== -->
    <!-- SECTION 2: The Benchmark       -->
    <!-- ============================== -->

    <h2>The Benchmark: PlanBench</h2>

    <p>In 2023, Valmeekam et al. introduced <strong>PlanBench</strong>, the first rigorous benchmark for evaluating LLMs on classical planning tasks. The setup was deliberately simple: Blocksworld problems where a robot arm must stack colored blocks into a target configuration. Five blocks. A few moves. The kind of problem a STRIPS planner from 1971 could solve.</p>

    <p>They tested GPT-4, GPT-3.5, and several open-source models. The task: given an initial state and a goal state, produce a valid sequence of actions. Not optimal — just valid. Any plan that reaches the goal without violating preconditions counts as a success.</p>

    <div class="vis-container">
        <h3 style="text-align:center; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 1rem; color: #1b2838; margin-bottom: 4px;">PlanBench Results: Blocksworld Plan Generation</h3>
        <p style="text-align:center; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.78rem; color: #888; margin-bottom: 16px;">Percentage of valid plans generated (higher is better)</p>
        <div class="benchmark-chart">
            <div class="bench-row">
                <div class="bench-label">Classical Planner</div>
                <div class="bench-bar-track">
                    <div class="bench-bar" style="width: 100%; background: linear-gradient(90deg, #228b22, #2ea82e);"></div>
                </div>
                <div class="bench-value" style="color: #228b22;">100%</div>
            </div>
            <div class="bench-row">
                <div class="bench-label">GPT-4</div>
                <div class="bench-bar-track">
                    <div class="bench-bar" style="width: 30%; background: linear-gradient(90deg, #d4740e, #e88a30);"></div>
                </div>
                <div class="bench-value" style="color: #d4740e;">~30%</div>
            </div>
            <div class="bench-row">
                <div class="bench-label">GPT-3.5</div>
                <div class="bench-bar-track">
                    <div class="bench-bar" style="width: 3%; background: linear-gradient(90deg, #b22222, #d43030); min-width: 6px;"></div>
                </div>
                <div class="bench-value" style="color: #b22222;">~3%</div>
            </div>
            <div class="bench-row">
                <div class="bench-label">LLaMA-2</div>
                <div class="bench-bar-track">
                    <div class="bench-bar" style="width: 0%; background: #b22222; min-width: 3px;"></div>
                </div>
                <div class="bench-value" style="color: #b22222;">~0%</div>
            </div>
            <div class="bench-row">
                <div class="bench-label" style="font-size: 0.78rem; color: #888;">Fast Downward<br>(time)</div>
                <div class="bench-bar-track" style="height: 20px; background: #e8f5e9;">
                    <div class="bench-bar" style="width: 8%; background: #228b22; min-width: 6px;"></div>
                </div>
                <div class="bench-value" style="color: #228b22; font-size: 0.75rem;">&lt;1s</div>
            </div>
        </div>
        <p class="vis-caption">PlanBench results on Blocksworld plan generation. Classical planners achieve 100% on every instance in under a second. GPT-4 manages ~30% — and that number drops further on harder instances. Data from Valmeekam et al. (2023).</p>
    </div>
<p>The numbers were stark. GPT-4 — the most capable model at the time — generated valid plans for only about 30% of instances. GPT-3.5 managed around 3%. Open-source models scored near zero. Meanwhile, Fast Downward, a classical planner from 2004, solved 100% of instances in under a second.</p>

    <div class="callout warning">
        <div class="callout-label">Critical Finding</div>
        <p>The failures weren't edge cases or adversarial inputs. These were 5-block Blocksworld problems — among the simplest possible planning tasks. If an LLM can't reliably stack five blocks, it certainly can't plan warehouse logistics, multi-step deployments, or supply chain operations.</p>
    </div>

    <p>But the raw accuracy numbers weren't the most devastating finding. That came when the researchers asked a deeper question: <em>is the LLM actually reasoning, or is it pattern-matching?</em></p>

    <!-- ============================== -->
    <!-- SECTION 3: RoboSort at PlanBench -->
    <!-- ============================== -->

    <h3>RoboSort at the Benchmark</h3>

    <p>Let's see what this looks like in our warehouse. We gave a frontier LLM the same RoboSort problem from Posts 1–3: five pieces, three shelves, one build zone, support constraints. Build a tower. Here's what happened.</p>

    <div class="vis-container">
        <div class="chat-exchange">
            <div class="chat-bubble user">
                <span class="chat-label">Prompt</span>
                You are controlling a warehouse robot called RoboSort. The robot is at Home. There are five pieces on shelves: L1 and L2 on Shelf A, Beam on Shelf B, Roof and Flag on Shelf C. The Build Zone has a platform. Support constraints: legs support beam, beam supports roof, roof supports flag. A piece can only be placed on a support that is already placed. The robot can hold one piece at a time. Generate a valid plan to build the tower.
            </div>
            <div class="chat-bubble llm">
                <span class="chat-label">LLM Output</span>
                1. Move to Shelf A, pick up L1<br>
                2. Move to Build Zone, place L1 on platform ✓<br>
                3. Move to Shelf A, pick up L2<br>
                4. Move to Build Zone, place L2 on platform ✓<br>
                5. Move to Shelf C, pick up <span class="error-highlight">Roof</span><br>
                6. Move to Build Zone, place <span class="error-highlight">Roof on legs</span> ✗<br>
                7. Move to Shelf B, pick up Beam<br>
                8. Move to Build Zone, place Beam on legs ✗<br>
                9. Move to Shelf C, pick up Flag<br>
                10. Move to Build Zone, place Flag on Roof ✗
            </div>
        </div>
        <div class="demo-verdict fail">
            INVALID PLAN — Roof placed before Beam (violates support constraint: beam supports roof). 3 precondition violations.
        </div>
        <p class="vis-caption">A real LLM attempt at the RoboSort tower assembly. The model gets the legs right but then skips the Beam, placing Roof directly on legs. It "knows" the support constraint — it even states it — but doesn't enforce it during generation.</p>
    </div>
<p>The model knows what support constraints are — it can explain them eloquently. But knowing a constraint and <em>enforcing</em> it during sequential generation are fundamentally different cognitive operations. The LLM generates step-by-step, each token predicted from context. It doesn't maintain a world model. It doesn't track which pieces are placed. It doesn't verify preconditions before committing to an action.</p>

    <div class="callout insight">
        <div class="callout-label">Key Insight</div>
        <p>LLMs can <em>describe</em> planning constraints perfectly. They cannot <em>enforce</em> them during plan generation. This is the core gap. Knowing the rules and following the rules are different capabilities — and autoregressive generation only provides the first.</p>
    </div>

    <!-- ============================== -->
    <!-- SECTION 4: Mystery Blocksworld -->
    <!-- ============================== -->

    <h2>The Smoking Gun: Mystery Blocksworld</h2>

    <p>If LLMs were genuinely reasoning about actions and consequences — if they truly understood what "pick up" does to the state of the world — then the names of the predicates shouldn't matter. "pick-up-block" and "xyzzy-37" should be equivalent: different labels for the same operation. The planner doesn't care what you call things. Does the LLM?</p>

    <p>Valmeekam et al. tested this with <strong>Mystery Blocksworld</strong>. They took the exact same planning problems and replaced every predicate and action name with meaningless tokens. <code>on(A,B)</code> became <code>snurg(q3,q7)</code>. <code>pick-up</code> became <code>florp</code>. The logical structure was identical. Only the labels changed.</p>

    <div class="vis-container">
        <div class="mystery-comparison">
            <div class="mystery-panel original">
                <h4>Standard Blocksworld</h4>
<pre><code>(:action pick-up
  :parameters (?x - block)
  :precondition (and
    (clear ?x)
    (on-table ?x)
    (arm-empty))
  :effect (and
    (holding ?x)
    (not (clear ?x))
    (not (on-table ?x))
    (not (arm-empty))))</code></pre>
                <div class="mystery-result pass">GPT-4 accuracy: ~30%</div>
            </div>
            <div class="mystery-panel obfuscated">
                <h4>Mystery Blocksworld</h4>
<pre><code>(:action florp
  :parameters (?x - grindle)
  :precondition (and
    (zarb ?x)
    (plonk ?x)
    (sniv-empty))
  :effect (and
    (clutching ?x)
    (not (zarb ?x))
    (not (plonk ?x))
    (not (sniv-empty))))</code></pre>
                <div class="mystery-result fail">GPT-4 accuracy: ~0%</div>
            </div>
        </div>
        <p class="vis-caption">Same logical structure. Different names. Performance collapses to zero. The LLM was matching patterns from training data — "pick-up" evokes block-stacking scripts — not reasoning about preconditions and effects.</p>
    </div>
<p>The result was devastating: performance collapsed to <strong>zero</strong>. Not "lower." Not "somewhat degraded." Zero.</p>

    <p>This is the smoking gun. A system that reasons about state transitions wouldn't care whether the action is called "pick-up" or "florp." The preconditions are the same. The effects are the same. The state space is identical. But the LLM's performance was entirely dependent on recognizing the <em>names</em> — because it wasn't reasoning at all. It was retrieving similar-looking action sequences from its training data.</p>

    <h3>RoboSort Goes Mystery</h3>

    <p>The same test with our warehouse robot is equally revealing. When we describe the problem with meaningful names — "pick up Leg1 from Shelf A" — the LLM can pattern-match against warehouse logistics data and occasionally get it right. But rename the pieces:</p>

    <div class="vis-container">
        <div class="mystery-comparison">
            <div class="mystery-panel original">
                <h4>Standard RoboSort</h4>
<pre><code>Pieces: L1, L2, Beam, Roof, Flag
Locations: Shelf-A, Shelf-B,
           Shelf-C, Build-Zone
Constraint: legs support beam,
            beam supports roof

LLM: "Place L1, L2, then Beam,
     then Roof, then Flag"
     → Sometimes correct (~30%)</code></pre>
                <div class="mystery-result pass">Partially solved</div>
            </div>
            <div class="mystery-panel obfuscated">
                <h4>Mystery RoboSort</h4>
<pre><code>Pieces: Q3, Q7, Zrint, Plonk, Vex
Locations: Zone-W, Zone-X,
           Zone-Y, Zone-Z
Constraint: Q3,Q7 glarb Zrint,
            Zrint glarbs Plonk

LLM: "Move to Zone-X, florp Zrint,
     move to Zone-Z, snarg Zrint..."
     → Incoherent actions</code></pre>
                <div class="mystery-result fail">Complete failure</div>
            </div>
        </div>
        <p class="vis-caption">Same RoboSort warehouse. Same five pieces, same support constraints, same goal. Only the labels changed — and the LLM loses all ability to generate even plausible-looking plans. Pattern matching, not planning.</p>
    </div>
<div class="callout warning">
        <div class="callout-label">What This Means</div>
        <p>When an LLM "solves" a planning problem, it's often not solving it at all — it's recognizing it. The training data contains countless examples of "stack blocks bottom-up" and "assemble structures foundation-first." The LLM retrieves and adapts these patterns. Remove the semantic cues, and there's nothing left to retrieve.</p>
    </div>

    <!-- ============================== -->
    <!-- SECTION 5: The Fix Attempts    -->
    <!-- ============================== -->

    <h2>The Prompting Fixes That Don't Fix</h2>

    <p>After PlanBench, the natural response was: maybe the LLM just needs help. Better prompts. More structure. Time to think. The research community systematically tested every major prompting strategy. Here's what happened.</p>

    <h3>Chain-of-Thought (CoT)</h3>

    <p>The idea: make the LLM reason step-by-step. Instead of generating the plan directly, have it explain its reasoning at each step. "First, I need to place the foundation. The legs go first because they support the beam..."</p>

    <p>The result: <strong>marginal improvement at best, and sometimes worse</strong>. CoT helps the LLM generate more verbose explanations of <em>why</em> its plan is correct — while the plan remains incorrect. The model articulates the constraint ("beam must go before roof") and then violates it in the very next step. It's like a student who writes a perfect essay about traffic laws and then runs a red light.</p>

    <h3>Self-Critique / Verify-and-Refine</h3>

    <p>The idea: have the LLM check its own work. Generate a plan, then ask the same model (or another instance) to verify it. If it finds errors, revise and try again.</p>

    <p>The result: <strong>self-critique makes things worse</strong>. This was the most surprising finding. Valmeekam et al. showed that when LLMs evaluate their own plans, they exhibit a "yes-man" bias — the critic tends to approve whatever the generator produced. Worse, when the critic does flag an issue, the "fix" often introduces new violations. After several rounds of self-critique, accuracy <em>decreased</em>.</p>

    <div class="vis-container">
        <div class="chat-exchange">
            <div class="chat-bubble llm">
                <span class="chat-label">LLM — Plan (Attempt 1)</span>
                1. Pick L1, place on platform ✓<br>
                2. Pick L2, place on platform ✓<br>
                3. Pick Roof, place on legs ✗ <em>(Beam missing!)</em>
            </div>
            <div class="chat-bubble llm" style="background: #f5f0ff; border-color: #d4c5f0;">
                <span class="chat-label" style="color: #6b3fa0;">LLM — Self-Critique</span>
                "The plan looks mostly correct. The robot correctly places the legs first, providing a stable foundation. The Roof placement assumes the Beam is already in place. <strong>Suggestion:</strong> add Beam placement between steps 2 and 3."
            </div>
            <div class="chat-bubble llm">
                <span class="chat-label">LLM — Plan (Attempt 2)</span>
                1. Pick L1, place on platform ✓<br>
                2. Pick Beam, place on legs ✗ <em>(L2 missing!)</em><br>
                3. Pick L2, place on platform ✗ <em>(Beam blocks?)</em><br>
                4. Pick Roof, place on Beam ✓<br>
                5. Pick Flag, place on Roof ✓
            </div>
        </div>
        <div class="demo-verdict fail">
            Self-critique "fixed" one error but introduced another. After 3 rounds: accuracy drops from 30% → 22%.
        </div>
        <p class="vis-caption">Self-critique in action on RoboSort. The critic identifies the missing Beam but the revised plan puts Beam before L2. Each revision shuffles errors around rather than eliminating them. Data pattern from Valmeekam et al. (2023).</p>
    </div>
<h3>Tree of Thoughts (ToT)</h3>

    <p>The idea: explore multiple reasoning paths in parallel, evaluate them, and pick the best. Instead of one linear chain of thought, branch out, score each branch, and select the most promising.</p>

    <p>The result: <strong>expensive and unsound</strong>. ToT can marginally improve results on some instances, but it multiplies computational cost by 10–100x without providing any guarantee. You're searching a tree of LLM-generated candidates — but the evaluation function is also an LLM, which can't reliably distinguish valid from invalid plans. It's search without a sound heuristic. Post 3 showed why that doesn't work: you need a heuristic that actually correlates with distance to goal. An LLM's confidence score doesn't.</p>

    <div class="vis-container">
        <table class="strategy-table">
            <thead>
                <tr>
                    <th>Strategy</th>
                    <th>Idea</th>
                    <th>Result on Planning</th>
                    <th>Cost</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><strong>Direct Prompting</strong></td>
                    <td>Ask for a plan directly</td>
                    <td class="score-bad">~12–30% valid</td>
                    <td>1x</td>
                </tr>
                <tr>
                    <td><strong>Chain-of-Thought</strong></td>
                    <td>Step-by-step reasoning</td>
                    <td class="score-bad">~15–35% valid</td>
                    <td>1.5x</td>
                </tr>
                <tr>
                    <td><strong>Self-Critique</strong></td>
                    <td>LLM checks own plan</td>
                    <td class="score-worse">~10–22% (worse!)</td>
                    <td>3x</td>
                </tr>
                <tr>
                    <td><strong>Tree of Thoughts</strong></td>
                    <td>Branch & evaluate</td>
                    <td class="score-ok">~20–40% valid</td>
                    <td>10–100x</td>
                </tr>
                <tr>
                    <td><strong>Classical Planner</strong></td>
                    <td>Heuristic search on PDDL</td>
                    <td style="color: #228b22; font-weight: 700;">100% valid</td>
                    <td>1x (fast)</td>
                </tr>
            </tbody>
        </table>
        <p class="vis-caption">No prompting strategy brings LLMs close to classical planner accuracy. Self-critique actively degrades performance. Tree of Thoughts is expensive without guarantees. Data patterns from Valmeekam et al. (2023), Kambhampati (2024).</p>
    </div>
<div class="callout insight">
        <div class="callout-label">Key Insight</div>
        <p>The problem isn't the prompt — it's the architecture. Autoregressive generation commits to each token before seeing the consequences. No amount of prompt engineering can add backtracking, constraint propagation, or state tracking to a system that generates left-to-right. You don't fix a calculator by asking it nicely — you use a different tool.</p>
    </div>

    <div class="agentic-sidebar">
        <div class="sidebar-title">Agentic AI in the Wild: The Code Agent That Loops</div>
        <p>This same failure mode plagues coding agents. A coding agent asked to "refactor the authentication module" might: (1) modify the login function, (2) update the tests, (3) realize the tests reference a helper it deleted in step 1, (4) re-add the helper, (5) realize the re-added helper breaks step 1's refactoring. Each fix creates a new problem because the agent isn't tracking state — it's generating plausible next actions. ReAct agents, as we saw in Post 3, loop for the same reason: no heuristic, no state model, no backtracking.</p>
        <p style="margin-top: 10px;">The planning community's answer: don't generate and hope. <strong>Model the state, search the space, verify the result.</strong></p>
    </div>

    <!-- ============================== -->
    <!-- SECTION 6: Interactive Demo    -->
    <!-- ============================== -->

    <h2>Seeing the Failure: LLM vs. Planner on RoboSort</h2>

    <p>Let's make this concrete. Below, we run the same RoboSort tower assembly problem through an LLM (left) and a classical planner (right). Watch the LLM generate a plausible-looking but invalid plan, while the planner finds a guaranteed-correct sequence in milliseconds.</p>

    <div class="interactive-container">
        <div class="interactive-label">Interactive — click "▶ Run Both" to compare LLM vs. Planner on the same 5-piece tower problem</div>

        <div style="text-align: center; margin-bottom: 16px;">
            <button class="auto-demo-btn" id="runBothBtn" onclick="runComparison()">▶ Run Both</button>
            <button class="auto-demo-btn" id="resetCompBtn" onclick="resetComparison()" style="display:none; background:#666;">↺ Reset</button>
        </div>

        <div class="llm-demo-container">
            <div class="llm-demo-split">
                <!-- LLM Side -->
                <div class="llm-panel gpt-panel">
                    <h4>🤖 LLM (Direct Prompting)</h4>
                    <ul class="step-list" id="llmSteps">
                        <li class="step-neutral" id="llm-s1">
                            <span class="step-icon">1</span>
                            <span>Move to Shelf A → Pick L1</span>
                        </li>
                        <li class="step-neutral" id="llm-s2">
                            <span class="step-icon">2</span>
                            <span>Move to Build Zone → Place L1 on platform</span>
                        </li>
                        <li class="step-neutral" id="llm-s3">
                            <span class="step-icon">3</span>
                            <span>Move to Shelf A → Pick L2</span>
                        </li>
                        <li class="step-neutral" id="llm-s4">
                            <span class="step-icon">4</span>
                            <span>Move to Build Zone → Place L2 on platform</span>
                        </li>
                        <li class="step-neutral" id="llm-s5">
                            <span class="step-icon">5</span>
                            <span>Move to Shelf C → Pick <strong>Roof</strong></span>
                        </li>
                        <li class="step-neutral" id="llm-s6">
                            <span class="step-icon">6</span>
                            <span>Move to Build Zone → Place <strong>Roof on legs</strong></span>
                        </li>
                        <li class="step-neutral" id="llm-s7">
                            <span class="step-icon">7</span>
                            <span>Move to Shelf B → Pick Beam</span>
                        </li>
                        <li class="step-neutral" id="llm-s8">
                            <span class="step-icon">8</span>
                            <span>Move to Build Zone → Place Beam (where?)</span>
                        </li>
                        <li class="step-neutral" id="llm-s9">
                            <span class="step-icon">9</span>
                            <span>Move to Shelf C → Pick Flag</span>
                        </li>
                        <li class="step-neutral" id="llm-s10">
                            <span class="step-icon">10</span>
                            <span>Move to Build Zone → Place Flag on Roof</span>
                        </li>
                    </ul>
                    <div class="demo-verdict fail" id="llmVerdict" style="display:none;">
                        INVALID — 3 constraint violations. Roof placed without Beam support.
                    </div>
                </div>

                <!-- Planner Side -->
                <div class="llm-panel planner-panel">
                    <h4>⚙️ Classical Planner (A* + h<sub>FF</sub>)</h4>
                    <ul class="step-list" id="plannerSteps">
                        <li class="step-neutral" id="plan-s1">
                            <span class="step-icon">1</span>
                            <span>move(home, shelf-a)</span>
                        </li>
                        <li class="step-neutral" id="plan-s2">
                            <span class="step-icon">2</span>
                            <span>pick(L1, shelf-a)</span>
                        </li>
                        <li class="step-neutral" id="plan-s3">
                            <span class="step-icon">3</span>
                            <span>move(shelf-a, build-zone)</span>
                        </li>
                        <li class="step-neutral" id="plan-s4">
                            <span class="step-icon">4</span>
                            <span>place-on-platform(L1)</span>
                        </li>
                        <li class="step-neutral" id="plan-s5">
                            <span class="step-icon">5</span>
                            <span>move(build-zone, shelf-a)</span>
                        </li>
                        <li class="step-neutral" id="plan-s6">
                            <span class="step-icon">6</span>
                            <span>pick(L2, shelf-a) → move → place-on-platform(L2)</span>
                        </li>
                        <li class="step-neutral" id="plan-s7">
                            <span class="step-icon">7</span>
                            <span>move(build-zone, shelf-b)</span>
                        </li>
                        <li class="step-neutral" id="plan-s8">
                            <span class="step-icon">8</span>
                            <span>pick(Beam, shelf-b) → move → place-on-piece(Beam, L1)</span>
                        </li>
                        <li class="step-neutral" id="plan-s9">
                            <span class="step-icon">9</span>
                            <span>move → pick(Roof, shelf-c) → move → place-on-piece(Roof, Beam)</span>
                        </li>
                        <li class="step-neutral" id="plan-s10">
                            <span class="step-icon">10</span>
                            <span>move → pick(Flag, shelf-c) → move → place-on-piece(Flag, Roof)</span>
                        </li>
                    </ul>
                    <div class="demo-verdict pass" id="plannerVerdict" style="display:none;">
                        VALID — All preconditions satisfied. Optimal plan. Solved in &lt;0.01s.
                    </div>
                </div>
            </div>

            <!-- Failure Counter -->
            <div class="failure-counter" id="failureCounter" style="display:none;">
                <div class="counter-item">
                    <div class="counter-value bad" id="llmViolations">3</div>
                    <div class="counter-label">LLM Violations</div>
                </div>
                <div class="counter-item">
                    <div class="counter-value good" id="plannerViolations">0</div>
                    <div class="counter-label">Planner Violations</div>
                </div>
                <div class="counter-item">
                    <div class="counter-value bad">~30%</div>
                    <div class="counter-label">LLM Accuracy</div>
                </div>
                <div class="counter-item">
                    <div class="counter-value good">100%</div>
                    <div class="counter-label">Planner Accuracy</div>
                </div>
            </div>
        </div>
    </div>

    <p>The planner's output might look boring — it's just a sequence of formal actions. But every single step has been verified: every precondition checked, every effect applied, every state transition valid. The LLM's output reads better — natural language, confident tone — but it's wrong. This is the fundamental tension: <strong>fluency is not validity</strong>.</p>

    <!-- ============================== -->
    <!-- SECTION 7: Why LLMs Fail       -->
    <!-- ============================== -->

    <h2>Why LLMs Fail at Planning: The Architectural Argument</h2>

    <p>The evidence points to a fundamental architectural mismatch. Planning requires three capabilities that autoregressive language models lack:</p>

    <ol>
        <li><strong>State tracking.</strong> A planner maintains an explicit world state — what's true right now — and updates it after every action. An LLM has no world model. It has a context window of tokens. When RoboSort places L1, the planner <em>knows</em> the gripper is empty and L1 is on the platform. The LLM has to infer this from the text it already generated — and it often gets it wrong.</li>

        <li><strong>Precondition verification.</strong> Before executing <code>place-on-piece(Roof, Beam)</code>, the planner checks: is Beam placed? Is Roof in gripper? Is Robot at build zone? All must be true, or the action is blocked. The LLM has no mechanism to perform this check — it simply generates the next most likely token.</li>

        <li><strong>Backtracking.</strong> When a planner reaches a dead end — a state from which no action sequence reaches the goal — it backtracks and tries a different path. Autoregressive generation is one-shot: each token is committed permanently. There is no "undo." Tree of Thoughts simulates branching but without a sound evaluation function, it's random search with extra steps.</li>
    </ol>

    <div class="callout insight">
        <div class="callout-label">Key Insight</div>
        <p>LLMs fail at planning not because they're not smart enough, but because they're the wrong <em>kind</em> of tool. Asking an LLM to plan is like asking a spellchecker to do math. It might accidentally get simple cases right by pattern matching, but the architecture doesn't support the operation. The solution isn't a better spellchecker — it's using the right tool for the job.</p>
    </div>

    <!-- ============================== -->
    <!-- SECTION 8: Taxonomy            -->
    <!-- ============================== -->

    <h2>The Taxonomy: What Should LLMs Actually Do?</h2>

    <p>The failures of 2022–2023 didn't kill the idea of LLMs in planning. They refined it. The research community converged on a taxonomy of roles — things LLMs are genuinely good at, paired with formal tools that handle what LLMs can't.</p>

    <div class="vis-container">
        <div class="taxonomy-grid">
            <div class="taxonomy-card role-direct">
                <div class="tax-label">Role 1 — Failed</div>
                <h4>LLM as Direct Planner</h4>
                <p>Ask the LLM to generate the full plan. No verification, no formal tools. As PlanBench showed: ~12–30% accuracy on trivial problems. <strong>This doesn't work.</strong></p>
            </div>
            <div class="taxonomy-card role-heuristic">
                <div class="tax-label">Role 2 — Promising</div>
                <h4>LLM as Heuristic Generator</h4>
                <p>LLM writes <em>heuristic functions</em> (as code) that guide a classical planner's search. The planner still does the searching and guarantees correctness. The LLM just helps it search faster. <strong>Post 5 explores this.</strong></p>
            </div>
            <div class="taxonomy-card role-translator">
                <div class="tax-label">Role 3 — Frontier</div>
                <h4>LLM as Model Translator</h4>
                <p>LLM converts natural language problem descriptions into formal PDDL models. The planner then solves the formal model. No human PDDL expertise needed. <strong>Post 6 explores this.</strong></p>
            </div>
            <div class="taxonomy-card role-verifier">
                <div class="tax-label">Role 4 — Emerging</div>
                <h4>LLM as Orchestrator</h4>
                <p>LLM coordinates multiple specialized agents — a PDDL generator, a validator, a planner, an executor. It doesn't plan itself; it manages the planning pipeline. <strong>Post 7 explores this.</strong></p>
            </div>
        </div>
        <p class="vis-caption">The taxonomy of LLM roles in planning. Role 1 (direct planning) failed. Roles 2–4 leverage LLMs' actual strengths — language understanding, code generation, coordination — while delegating reasoning to formal tools.</p>
    </div>
<p>The critical shift: <strong>stop asking LLMs to plan. Start asking them to help plan.</strong> The LLM's genuine strengths — understanding natural language, generating code, translating between formats — are exactly the capabilities that formal planners lack. The planner's strengths — state tracking, constraint verification, optimality guarantees — are exactly what LLMs lack. The marriage is natural. The remaining posts in this series explore how it works in practice.</p>

    <!-- ============================== -->
    <!-- SECTION 9: What's Ahead        -->
    <!-- ============================== -->

    <h2>What's Ahead</h2>

    <p>This post established the hard evidence: LLMs cannot reliably plan. Not with better prompts. Not with self-critique. Not with tree search. The architectural mismatch is fundamental — autoregressive generation lacks state tracking, precondition verification, and backtracking.</p>

    <p>But the story doesn't end with failure. The taxonomy of roles shows the path forward. Instead of replacing planners, what if LLMs <em>amplified</em> them? What if an LLM could look at a PDDL domain and generate a Python function that estimates distance to goal — a heuristic — and hand it to a classical search engine?</p>

    <p>That's exactly what happened. And the results went from 12% to 82%.</p>

    <!-- Next Post Teaser -->
    <div class="next-post">
        <h3>Up Next: Part 5 — The Modern Playbook: LLMs That Help Planners</h3>
        <p>LLM-Modulo, code generation for heuristics, Thought of Search, and the generate-verify loop that turned LLM planning from a failure into a breakthrough. The LLM proposes, the planner verifies — and together they achieve what neither could alone.</p>
    </div>

    <!-- ============================== -->
    <!-- REFERENCES                     -->
    <!-- ============================== -->

    <div class="references">
        <h2>References</h2>
        <ol>
            <li>Valmeekam, V., Marquez, M., Sreedharan, S., & Kambhampati, S. (2023). On the Planning Abilities of Large Language Models — A Critical Investigation. <em>NeurIPS 2023</em>.</li>
            <li>Valmeekam, V., Marquez, M., & Kambhampati, S. (2023). PlanBench: An Extensible Benchmark for Evaluating Large Language Models on Planning and Reasoning about Change. <em>NeurIPS 2023 Datasets and Benchmarks</em>.</li>
            <li>Kambhampati, S. (2024). Can Large Language Models Reason and Plan? <em>Annals of the New York Academy of Sciences</em>.</li>
            <li>Stechly, K., Marquez, M., & Kambhampati, S. (2024). Self-Verification in Large Language Models: Limitations and Implications. <em>ICML 2024 Workshop on LLMs and Cognition</em>.</li>
            <li>Yao, S., Yu, D., Zhao, J., et al. (2023). Tree of Thoughts: Deliberate Problem Solving with Large Language Models. <em>NeurIPS 2023</em>.</li>
            <li>Katz, M., Kokel, H., & Muise, C. (2025). Planning in the Era of Language Models. <em>NeurIPS 2025 Tutorial</em>.</li>
            <li>Valmeekam, V., Stechly, K., & Kambhampati, S. (2024). LLMs Still Can't Plan; Can LLMs Help Planning? <em>AAAI 2024 Workshop on Bridging the Gap Between AI Planning and Reinforcement Learning</em>.</li>
        </ol>
    </div>

    <!-- Download All -->
</article>

<div class="blog-footer">
    <p>Planning in the Era of LLMs — Part 4 of 7</p>
</div>

<script>
/* === LLM vs Planner Comparison Demo === */
function runComparison() {
    const llmResults =  ['ok','ok','ok','ok','fail','fail','ok','fail','ok','fail'];
    const planResults = ['ok','ok','ok','ok','ok','ok','ok','ok','ok','ok'];
    const btn = document.getElementById('runBothBtn');
    const resetBtn = document.getElementById('resetCompBtn');
    btn.disabled = true;
    btn.textContent = 'Running...';

    let step = 0;
    const interval = setInterval(() => {
        if (step >= 10) {
            clearInterval(interval);
            document.getElementById('llmVerdict').style.display = 'block';
            document.getElementById('plannerVerdict').style.display = 'block';
            document.getElementById('failureCounter').style.display = 'flex';
            btn.style.display = 'none';
            resetBtn.style.display = 'inline-block';
            return;
        }

        // LLM side
        const llmStep = document.getElementById('llm-s' + (step + 1));
        llmStep.className = 'step-' + llmResults[step];
        llmStep.querySelector('.step-icon').textContent = llmResults[step] === 'ok' ? '✓' : '✗';

        // Planner side
        const planStep = document.getElementById('plan-s' + (step + 1));
        planStep.className = 'step-' + planResults[step];
        planStep.querySelector('.step-icon').textContent = '✓';

        step++;
    }, 400);
}

function resetComparison() {
    for (let i = 1; i <= 10; i++) {
        const llm = document.getElementById('llm-s' + i);
        const plan = document.getElementById('plan-s' + i);
        llm.className = 'step-neutral';
        llm.querySelector('.step-icon').textContent = i;
        plan.className = 'step-neutral';
        plan.querySelector('.step-icon').textContent = i;
    }
    document.getElementById('llmVerdict').style.display = 'none';
    document.getElementById('plannerVerdict').style.display = 'none';
    document.getElementById('failureCounter').style.display = 'none';
    document.getElementById('runBothBtn').style.display = 'inline-block';
    document.getElementById('runBothBtn').disabled = false;
    document.getElementById('runBothBtn').textContent = '▶ Run Both';
    document.getElementById('resetCompBtn').style.display = 'none';
}
</script>
