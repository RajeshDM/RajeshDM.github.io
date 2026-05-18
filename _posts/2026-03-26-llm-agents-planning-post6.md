---
layout: fullhtml-post
title: "From English to Plans: The NL-to-PDDL Frontier"
date: 2026-03-26
categories: ["LLMs Automated Planning and Agents"]
tags: ["planning", "llm", "nl-to-pddl"]
description: "NL2Plan, agentic PDDL generation, and the orchestrator bottleneck — when the conductor can't keep up with the orchestra. Part 6 of the Planning in the Era of LLMs series."
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

  .blog-fullhtml footer {
      text-align: center; padding: 32px 20px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.82rem; color: #888; border-top: 1px solid #eee;
  }

  .blog-fullhtml .math { font-family: 'Cambria Math', 'Georgia', serif; font-style: italic; color: #2a4066; }

  .blog-fullhtml .pipeline-flow {
      display: flex;
      align-items: stretch;
      justify-content: center;
      gap: 0;
      margin: 20px 0;
      padding: 10px 0;
  }
  .blog-fullhtml .pipeline-stage {
      padding: 14px 16px;
      border-radius: 10px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.82rem;
      font-weight: 600;
      text-align: center;
      min-width: 110px;
      line-height: 1.4;
      display: flex;
      flex-direction: column;
      justify-content: center;
  }
  .blog-fullhtml .pipeline-stage .stage-label {
      font-size: 0.65rem;
      text-transform: uppercase;
      letter-spacing: 1px;
      margin-bottom: 4px;
      opacity: 0.8;
  }
  .blog-fullhtml .pipeline-arrow {
      font-size: 1.4rem;
      color: #555;
      padding: 0 8px;
      font-weight: 700;
      display: flex;
      align-items: center;
  }
  .blog-fullhtml .stage-nl { background: #2a9d8f; color: white; }
  .blog-fullhtml .stage-extract { background: #6b3fa0; color: white; }
  .blog-fullhtml .stage-pddl { background: #d4740e; color: white; }
  .blog-fullhtml .stage-validate { background: #b22222; color: white; }
  .blog-fullhtml .stage-solve { background: #228b22; color: white; }

  .blog-fullhtml .orchestra-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
      margin: 20px 0;
  }
  .blog-fullhtml .orchestra-agent {
      border-radius: 10px;
      padding: 14px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      text-align: center;
  }
  .blog-fullhtml .orchestra-agent .agent-icon {
      font-size: 1.8rem;
      margin-bottom: 6px;
  }
  .blog-fullhtml .orchestra-agent h5 {
      font-size: 0.82rem;
      margin-bottom: 4px;
  }
  .blog-fullhtml .orchestra-agent p {
      font-size: 0.72rem;
      margin-bottom: 0;
      line-height: 1.4;
  }
  .blog-fullhtml .agent-extractor {
      background: #f5f0ff;
      border: 2px solid #6b3fa0;
  }
  .blog-fullhtml .agent-extractor h5 { color: #6b3fa0; }
  .blog-fullhtml .agent-extractor p { color: #3a2060; }
  .blog-fullhtml .agent-validator {
      background: #fdf2f2;
      border: 2px solid #b22222;
  }
  .blog-fullhtml .agent-validator h5 { color: #b22222; }
  .blog-fullhtml .agent-validator p { color: #5a1010; }
  .blog-fullhtml .agent-fixer {
      background: #fff7ed;
      border: 2px solid #d4740e;
  }
  .blog-fullhtml .agent-fixer h5 { color: #d4740e; }
  .blog-fullhtml .agent-fixer p { color: #7a3f00; }
  .blog-fullhtml .agent-solver {
      background: #f0faf0;
      border: 2px solid #228b22;
  }
  .blog-fullhtml .agent-solver h5 { color: #228b22; }
  .blog-fullhtml .agent-solver p { color: #0a4a0a; }
  .blog-fullhtml .orchestrator-bar {
      background: linear-gradient(90deg, #1b2838, #2a4066);
      color: #e0e8f0;
      border-radius: 10px;
      padding: 12px 20px;
      margin-top: 12px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.85rem;
      font-weight: 600;
      text-align: center;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
  }

  .blog-fullhtml .nl-pddl-transform {
      display: grid;
      grid-template-columns: 1fr 40px 1fr;
      gap: 0;
      margin: 20px 0;
      align-items: stretch;
  }
  .blog-fullhtml .nl-side, .blog-fullhtml .pddl-side {
      border-radius: 10px;
      overflow: hidden;
  }
  .blog-fullhtml .nl-side .panel-header {
      background: #2a9d8f;
      color: white;
      padding: 10px 16px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.8rem;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 1px;
  }
  .blog-fullhtml .pddl-side .panel-header {
      background: #d4740e;
      color: white;
      padding: 10px 16px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.8rem;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 1px;
  }
  .blog-fullhtml .nl-side .panel-body {
      background: #f0fafa;
      border: 2px solid #2a9d8f;
      border-top: none;
      border-radius: 0 0 10px 10px;
      padding: 16px;
      font-size: 0.88rem;
      line-height: 1.7;
      min-height: 280px;
  }
  .blog-fullhtml .pddl-side .panel-body {
      border: 2px solid #d4740e;
      border-top: none;
      border-radius: 0 0 10px 10px;
  }
  .blog-fullhtml .pddl-side .panel-body pre {
      margin: 0;
      border-radius: 0 0 8px 8px;
      font-size: 0.72rem;
      min-height: 280px;
  }
  .blog-fullhtml .transform-arrow {
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 2rem;
      color: #6b3fa0;
      font-weight: 700;
  }

  .blog-fullhtml .bottleneck-chart {
      display: flex;
      flex-direction: column;
      gap: 8px;
      padding: 20px 0;
      font-family: 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .bottleneck-row {
      display: grid;
      grid-template-columns: 160px 1fr 70px;
      align-items: center;
      gap: 12px;
      font-size: 0.85rem;
  }
  .blog-fullhtml .bottleneck-label {
      text-align: right;
      font-weight: 600;
      color: #1b2838;
      font-size: 0.8rem;
  }
  .blog-fullhtml .bottleneck-bar-track {
      height: 24px;
      background: #e8ecf1;
      border-radius: 4px;
      overflow: hidden;
  }
  .blog-fullhtml .bottleneck-bar {
      height: 100%;
      border-radius: 4px;
  }
  .blog-fullhtml .bottleneck-value {
      font-weight: 700;
      font-size: 0.85rem;
  }

  .blog-fullhtml .nl-demo-container {
      width: 960px;
      max-width: calc(100vw - 40px);
      margin-left: 50%;
      transform: translateX(-50%);
      position: relative;
  }
  .blog-fullhtml .nl-demo-stages {
      display: flex;
      flex-direction: column;
      gap: 12px;
      margin-top: 14px;
  }
  .blog-fullhtml .demo-stage {
      border-radius: 10px;
      padding: 16px 20px;
      font-family: 'Helvetica Neue', Arial, sans-serif;
      font-size: 0.85rem;
      transition: all 0.4s ease;
  }
  .blog-fullhtml .demo-stage .stage-header {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 8px;
  }
  .blog-fullhtml .demo-stage .stage-num {
      width: 28px;
      height: 28px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 700;
      font-size: 0.8rem;
      color: white;
      flex-shrink: 0;
  }
  .blog-fullhtml .demo-stage .stage-title {
      font-weight: 700;
      font-size: 0.9rem;
  }
  .blog-fullhtml .demo-stage .stage-content {
      padding-left: 38px;
      font-size: 0.82rem;
      line-height: 1.5;
  }
  .blog-fullhtml .demo-stage.waiting {
      background: #f5f5f5;
      border: 1px solid #ddd;
  }
  .blog-fullhtml .demo-stage.waiting .stage-num { background: #ccc; }
  .blog-fullhtml .demo-stage.active {
      border: 2px solid #2a9d8f;
      background: #f0fafa;
      box-shadow: 0 2px 10px rgba(42, 157, 143, 0.2);
  }
  .blog-fullhtml .demo-stage.active .stage-num { background: #2a9d8f; }
  .blog-fullhtml .demo-stage.done {
      background: #f0faf0;
      border: 1px solid #228b22;
  }
  .blog-fullhtml .demo-stage.done .stage-num { background: #228b22; }
  .blog-fullhtml .demo-stage.error {
      background: #fdf2f2;
      border: 2px solid #b22222;
  }
  .blog-fullhtml .demo-stage.error .stage-num { background: #b22222; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }

  @media (max-width: 700px) {
      .blog-fullhtml .hero h1 { font-size: 1.8rem; }
      .blog-fullhtml .blog-container { padding: 32px 16px 60px; }
      .blog-fullhtml .pipeline-flow { flex-wrap: wrap; gap: 8px; }
      .blog-fullhtml .pipeline-stage { min-width: 80px; font-size: 0.75rem; }
      .blog-fullhtml .orchestra-grid { grid-template-columns: 1fr 1fr; }
      .blog-fullhtml .nl-pddl-transform { grid-template-columns: 1fr; }
      .blog-fullhtml .transform-arrow { transform: rotate(90deg); padding: 8px 0; }
      .blog-fullhtml .nl-demo-container { width: 100%; transform: none; margin-left: 0; }
      .blog-fullhtml .bottleneck-row { grid-template-columns: 100px 1fr 50px; }
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
  html[data-theme="dark"] .blog-fullhtml .auto-demo-btn { background: #2a9d8f; color: #fff; }
  html[data-theme="dark"] .blog-fullhtml .auto-demo-btn:hover { background: #238577; }
  html[data-theme="dark"] .blog-fullhtml .next-post h3 { color: #7eb8da; }
  html[data-theme="dark"] .blog-fullhtml .next-post p { color: #b0c4de; }
  html[data-theme="dark"] .blog-fullhtml .references { border-top-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .references ol { color: #9aa8b8; }
  html[data-theme="dark"] .blog-fullhtml footer { color: #6a7888; border-top-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .math { color: #6aafe6; }
  html[data-theme="dark"] .blog-fullhtml .pipeline-arrow { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .stage-nl { background: #2a9d8f; color: #fff; }
  html[data-theme="dark"] .blog-fullhtml .stage-extract { background: #6b3fa0; color: #fff; }
  html[data-theme="dark"] .blog-fullhtml .stage-pddl { background: #d4740e; color: #fff; }
  html[data-theme="dark"] .blog-fullhtml .stage-validate { background: #b22222; color: #fff; }
  html[data-theme="dark"] .blog-fullhtml .stage-solve { background: #228b22; color: #fff; }
  html[data-theme="dark"] .blog-fullhtml .agent-extractor { background: #1e142e; border-color: #6b3fa0; }
  html[data-theme="dark"] .blog-fullhtml .agent-extractor h5 { color: #b088e0; }
  html[data-theme="dark"] .blog-fullhtml .agent-extractor p { color: #b0a8c8; }
  html[data-theme="dark"] .blog-fullhtml .agent-validator { background: #2a1414; border-color: #b22222; }
  html[data-theme="dark"] .blog-fullhtml .agent-validator h5 { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .agent-validator p { color: #d0a0a0; }
  html[data-theme="dark"] .blog-fullhtml .agent-fixer { background: #2a2010; border-color: #d4740e; }
  html[data-theme="dark"] .blog-fullhtml .agent-fixer h5 { color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .agent-fixer p { color: #d8b888; }
  html[data-theme="dark"] .blog-fullhtml .agent-solver { background: #142a14; border-color: #228b22; }
  html[data-theme="dark"] .blog-fullhtml .agent-solver h5 { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .agent-solver p { color: #a0c8a0; }
  html[data-theme="dark"] .blog-fullhtml .orchestrator-bar { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml .nl-side .panel-header { background: #2a9d8f; color: #fff; }
  html[data-theme="dark"] .blog-fullhtml .pddl-side .panel-header { background: #d4740e; color: #fff; }
  html[data-theme="dark"] .blog-fullhtml .nl-side .panel-body { background: #142a2a; border-color: #2a9d8f; }
  html[data-theme="dark"] .blog-fullhtml .pddl-side .panel-body { border-color: #d4740e; }
  html[data-theme="dark"] .blog-fullhtml .transform-arrow { color: #b088e0; }
  html[data-theme="dark"] .blog-fullhtml .bottleneck-label { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .bottleneck-bar-track { background: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .demo-stage.waiting { background: #1e2530; border-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .demo-stage.waiting .stage-num { background: #4a5565; }
  html[data-theme="dark"] .blog-fullhtml .demo-stage.active { background: #142a2a; border-color: #2a9d8f; }
  html[data-theme="dark"] .blog-fullhtml .demo-stage.active .stage-num { background: #2a9d8f; }
  html[data-theme="dark"] .blog-fullhtml .demo-stage.done { background: #142a14; border-color: #228b22; }
  html[data-theme="dark"] .blog-fullhtml .demo-stage.done .stage-num { background: #228b22; }
  html[data-theme="dark"] .blog-fullhtml .demo-stage.error { background: #2a1414; border-color: #b22222; }
  html[data-theme="dark"] .blog-fullhtml .demo-stage.error .stage-num { background: #b22222; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #6a7888; border-top-color: #2a3545; }
---

<header class="hero">
    <div class="series-label">Planning in the Era of LLMs — Part 6 of 7</div>
    <h1>From English to Plans: The NL-to-PDDL Frontier</h1>
    <p class="subtitle">NL2Plan, agentic PDDL generation, and the orchestrator bottleneck — when the conductor can't keep up with the orchestra.</p>
</header>

<article class="blog-container">

    <!-- Series Navigation -->
    <div class="vis-container">
        <div class="series-nav">
            <strong>📚 Planning in the Era of LLMs — Part 6 of 7</strong>
            <div class="nav-desc">Paradigm 2: no PDDL exists. The system must create the formal model from natural language, validate it, solve it, and return a verified plan. The most ambitious — and most fragile — frontier in LLM-planning research.</div>
            <div class="nav-links">
                ← Part 5: "The Modern Playbook: LLMs That Help Planners" | Next: Part 7 — "Agentic AI for Planning" →
            </div>
        </div>
    </div>

    <!-- ============================== -->
    <!-- SECTION 1: Opening Hook        -->
    <!-- ============================== -->

    <p class="lead">Post 5 showed that when PDDL is given, the combination of LLMs and formal planners achieves remarkable results — 82% accuracy versus 12% for LLMs alone. But it left a critical question unanswered: who writes the PDDL?</p>

    <p>For the RoboSort warehouse, we wrote PDDL by hand in Post 2. That took expertise. We defined <code>robot-at</code>, <code>holding</code>, <code>supports</code>, <code>place-on-piece</code> — every predicate, every action, every precondition. A planning expert could do this for a standard warehouse. But what about the user who simply says:</p>

    <p style="text-align: center; font-style: italic; font-size: 1.15rem; color: #6b3fa0; margin: 24px 0;">"I have a robot in a warehouse with three shelves. It can carry one item. Some pieces need support underneath. Help it build a tower."</p>

    <p>No types. No predicates. No action schemas. Just English. This is <strong>Paradigm 2</strong> — the system must create the formal model from scratch, validate it, solve it, and return a verified plan. It's the most ambitious goal in the LLM-planning research landscape, and it's where the field's most exciting work is happening right now.</p>

    <!-- ============================== -->
    <!-- SECTION 2: The Pipeline        -->
    <!-- ============================== -->

    <h2>The NL-to-Plan Pipeline</h2>

    <p>Converting English to a verified plan requires five stages. Each one is hard. Getting all five right, in sequence, is extraordinarily hard.</p>

    <div class="vis-container">
        <h3 style="text-align:center; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 1rem; color: #1b2838; margin-bottom: 16px;">The NL-to-Plan Pipeline</h3>
        <div class="pipeline-flow">
            <div class="pipeline-stage stage-nl">
                <div class="stage-label">Stage 1</div>
                English<br>Description
            </div>
            <div class="pipeline-arrow">→</div>
            <div class="pipeline-stage stage-extract">
                <div class="stage-label">Stage 2</div>
                Extract Types,<br>Predicates, Actions
            </div>
            <div class="pipeline-arrow">→</div>
            <div class="pipeline-stage stage-pddl">
                <div class="stage-label">Stage 3</div>
                Generate<br>PDDL
            </div>
            <div class="pipeline-arrow">→</div>
            <div class="pipeline-stage stage-validate">
                <div class="stage-label">Stage 4</div>
                Validate &<br>Fix PDDL
            </div>
            <div class="pipeline-arrow">→</div>
            <div class="pipeline-stage stage-solve">
                <div class="stage-label">Stage 5</div>
                Solve &<br>Return Plan
            </div>
        </div>
        <p class="vis-caption">Five stages from English to verified plan. Each stage can fail independently. The pipeline's overall accuracy is the product of per-stage accuracies — if each stage is 90% reliable, the pipeline is only 59% end-to-end.</p>
    </div>
<p>Let's trace the pipeline for our RoboSort warehouse:</p>

    <ol>
        <li><strong>English Description:</strong> "A robot in a warehouse with three shelves (A, B, C) and a build zone. Shelf A has two legs, Shelf B has a beam, Shelf C has a roof and a flag. The robot starts at home and can carry one piece. Legs support beam, beam supports roof, roof supports flag. Build the tower."</li>

        <li><strong>Extract Types, Predicates, Actions:</strong> The LLM must infer that there are <em>locations</em> (shelves, build zone, home), <em>pieces</em> (L1, L2, beam, roof, flag), and <em>predicates</em> like robot-at, piece-at, holding, supports, placed. It must also infer the action schemas: move, pick, place-on-platform, place-on-piece — each with correct preconditions and effects.</li>

        <li><strong>Generate PDDL:</strong> The LLM writes syntactically valid PDDL domain and problem files — the same code we wrote by hand in Post 2.</li>

        <li><strong>Validate & Fix:</strong> A PDDL parser checks syntax. A validator checks semantic consistency. If errors are found, the LLM gets feedback and fixes them.</li>

        <li><strong>Solve & Return:</strong> A classical planner (Fast Downward, etc.) solves the validated PDDL and returns the plan.</li>
    </ol>

    <p>Each stage is a point of failure. The LLM might miss a predicate (stage 2), generate syntactically invalid PDDL (stage 3), or produce semantically correct PDDL that doesn't match the user's intent (stages 2–3). This is why the field converged on <strong>multi-agent systems</strong> — specialized agents for each stage, coordinated by an orchestrator.</p>

    <!-- ============================== -->
    <!-- SECTION 3: NL2Plan             -->
    <!-- ============================== -->

    <h2>NL2Plan: The Multi-Agent Approach</h2>

    <p><strong>NL2Plan</strong> (Gestrin et al., 2025) is the most systematic framework for this problem. Instead of asking a single LLM to do everything, it decomposes the pipeline into specialized agents, each responsible for one stage.</p>

    <div class="vis-container">
        <h3 style="text-align:center; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 1rem; color: #1b2838; margin-bottom: 16px;">NL2Plan: Specialized Agents</h3>
        <div class="orchestra-grid">
            <div class="orchestra-agent agent-extractor">
                <div class="agent-icon">🔍</div>
                <h5>Type Extractor</h5>
                <p>Identifies object types from the description: location, piece, robot</p>
            </div>
            <div class="orchestra-agent agent-extractor">
                <div class="agent-icon">📋</div>
                <h5>Predicate Extractor</h5>
                <p>Derives predicates: robot-at, holding, placed, supports, gripper-empty</p>
            </div>
            <div class="orchestra-agent agent-extractor">
                <div class="agent-icon">⚙️</div>
                <h5>Action Extractor</h5>
                <p>Defines actions with preconditions and effects: move, pick, place</p>
            </div>
            <div class="orchestra-agent agent-validator">
                <div class="agent-icon">✓</div>
                <h5>PDDL Validator</h5>
                <p>Parses, validates syntax and semantics, reports errors</p>
            </div>
        </div>
        <div class="orchestrator-bar">
            🎼 Orchestrator — coordinates agents, routes feedback, decides when to retry or advance
        </div>
        <p class="vis-caption">NL2Plan uses specialized agents for each extraction task. The orchestrator coordinates the pipeline — and, as we'll see, is the weakest link.</p>
    </div>
<p>The key innovation is the decomposition. Instead of one massive prompt ("convert this English to PDDL"), NL2Plan breaks the task into focused queries: "What types of objects exist in this description?" then "Given these types, what predicates describe their relationships?" then "Given these types and predicates, what actions can the robot perform?" Each agent can use few-shot examples and targeted prompts for its specific subtask.</p>

    <h3>RoboSort Through NL2Plan</h3>

    <p>Here's how NL2Plan processes our warehouse description, transforming natural language into the same PDDL we wrote by hand in Post 2:</p>

    <div class="vis-container">
        <div class="nl-pddl-transform">
            <div class="nl-side">
                <div class="panel-header">Natural Language Input</div>
                <div class="panel-body">
                    <p style="margin-bottom: 12px;">"A robot called RoboSort works in a warehouse. There are three shelves (A, B, C) and a build zone. The robot starts at home."</p>
                    <p style="margin-bottom: 12px;">"Shelf A has two leg pieces (L1 and L2). Shelf B has a beam. Shelf C has a roof and a flag."</p>
                    <p style="margin-bottom: 12px;">"The robot can move between locations, pick up one piece at a time, and place pieces in the build zone."</p>
                    <p style="margin-bottom: 12px;">"<strong>Support rules:</strong> Legs support the beam. The beam supports the roof. The roof supports the flag. A piece can only be placed on something that supports it and is already placed."</p>
                    <p style="margin-bottom: 0;">"<strong>Goal:</strong> All five pieces assembled into a tower in the build zone."</p>
                </div>
            </div>
            <div class="transform-arrow">→</div>
            <div class="pddl-side">
                <div class="panel-header">Generated PDDL</div>
                <div class="panel-body">
<pre><code>(define (domain robosort-tower)
  (:requirements :strips :typing)
  (:types location piece)
  (:predicates
    (robot-at ?l - location)
    (piece-at ?p - piece ?l - location)
    (holding ?p - piece)
    (gripper-empty)
    (supports ?lower ?upper - piece)
    (on-platform ?p - piece)
    (placed ?p - piece))

  (:action move
    :parameters (?from ?to - location)
    :precondition (robot-at ?from)
    :effect (and (robot-at ?to)
                 (not (robot-at ?from))))

  (:action pick
    :parameters (?p - piece ?l - location)
    :precondition (and (robot-at ?l)
                       (piece-at ?p ?l)
                       (gripper-empty))
    :effect (and (holding ?p)
                 (not (piece-at ?p ?l))
                 (not (gripper-empty))))
  ...
)</code></pre>
                </div>
            </div>
        </div>
        <p class="vis-caption">NL2Plan transforms natural language into the same PDDL domain we manually wrote in Post 2. The LLM correctly extracts types (location, piece), predicates (robot-at, supports, holding), and actions (move, pick, place) — including the critical support constraint.</p>
    </div>
<div class="callout insight">
        <div class="callout-label">Key Insight</div>
        <p>NL2Plan achieves 100% accuracy on several standard domains — the generated PDDL is functionally identical to expert-written PDDL. The breakthrough is decomposition: by splitting the hard problem ("English → PDDL") into focused subproblems ("English → types," "types → predicates," etc.), each step becomes tractable for current LLMs.</p>
    </div>

    <!-- ============================== -->
    <!-- SECTION 4: Orchestrator        -->
    <!-- ============================== -->

    <h2>The Orchestrator Bottleneck</h2>

    <p>NL2Plan achieves impressive results on standard domains. But it has an Achilles' heel: the <strong>orchestrator</strong>. The orchestrator is the conductor of the multi-agent pipeline — it decides when to call each agent, how to route feedback, when a PDDL file is "good enough" to pass to the solver, and when to give up and restart.</p>

    <p>Here's the problem: the orchestrator is <em>also an LLM</em>. And the orchestration task is itself a planning problem — one that requires sequencing agents, tracking state (which agents have run, what errors remain), and backtracking when a strategy fails. Sound familiar?</p>

    <div class="callout warning">
        <div class="callout-label">The Recursive Problem</div>
        <p>The orchestrator must plan the PDDL generation pipeline. But planning is exactly what LLMs are bad at (Post 4). The system designed to solve the planning bottleneck has a planning bottleneck of its own. The conductor can't keep up with the orchestra.</p>
    </div>

    <p>In practice, this manifests as three failure modes:</p>

    <ol>
        <li><strong>Premature advancement:</strong> The orchestrator declares the PDDL "done" when it still has subtle errors — a missing predicate, an action with an incomplete effect list. The planner then fails or produces an incorrect plan.</li>

        <li><strong>Infinite revision loops:</strong> The validator reports an error, the fixer introduces a new error while fixing the first, the validator catches the new error, and the cycle repeats. The orchestrator doesn't recognize the loop.</li>

        <li><strong>Missing abstractions:</strong> The user's description implies a concept the LLM doesn't extract. For RoboSort, the phrase "pieces need support underneath" implies a <code>supports</code> predicate, a <code>placed</code> predicate, and a precondition on the place action. If the extractor misses any of these, the resulting PDDL allows invalid tower constructions — and the orchestrator may not catch it because the PDDL is syntactically valid.</li>
    </ol>

    <h3>Orchestrator Failure on RoboSort</h3>

    <p>Let's see what happens when NL2Plan processes a subtly ambiguous RoboSort description:</p>

    <div class="vis-container">
        <div style="font-family: 'Helvetica Neue', Arial, sans-serif;">
            <div style="background: #f0fafa; border: 2px solid #2a9d8f; border-radius: 10px; padding: 16px; margin-bottom: 12px;">
                <div style="font-size: 0.72rem; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; color: #2a9d8f; margin-bottom: 6px;">User Description</div>
                <p style="font-size: 0.88rem; margin: 0;">"Build a tower from five parts. Two legs go on the bottom, a beam goes on the legs, a roof on the beam, and a flag on top. The robot carries one part at a time."</p>
            </div>

            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 12px;">
                <div style="background: #f0faf0; border: 1px solid #228b22; border-radius: 8px; padding: 12px;">
                    <div style="font-size: 0.72rem; font-weight: 700; color: #228b22; margin-bottom: 4px;">✓ What NL2Plan Extracts Correctly</div>
                    <ul style="font-size: 0.78rem; margin: 0 0 0 16px; color: #0a4a0a;">
                        <li>Types: location, piece</li>
                        <li>Actions: move, pick, place</li>
                        <li>Objects: L1, L2, beam, roof, flag</li>
                        <li>Gripper-empty constraint</li>
                    </ul>
                </div>
                <div style="background: #fdf2f2; border: 1px solid #b22222; border-radius: 8px; padding: 12px;">
                    <div style="font-size: 0.72rem; font-weight: 700; color: #b22222; margin-bottom: 4px;">✗ What It Misses</div>
                    <ul style="font-size: 0.78rem; margin: 0 0 0 16px; color: #5a1010;">
                        <li>No <code style="font-size:0.75rem;">supports</code> predicate (user said "on the legs" — not formally stated)</li>
                        <li>No <code style="font-size:0.75rem;">placed</code> predicate — can't check if support exists</li>
                        <li>Place action has no support precondition</li>
                    </ul>
                </div>
            </div>

            <div style="background: #fff7ed; border: 2px solid #d4740e; border-radius: 8px; padding: 12px;">
                <div style="font-size: 0.72rem; font-weight: 700; color: #d4740e; margin-bottom: 4px;">⚠ Result: Syntactically Valid, Semantically Wrong PDDL</div>
                <p style="font-size: 0.82rem; color: #7a3f00; margin: 0;">The generated PDDL parses correctly and the planner finds a "solution" — but it places the roof before the beam because nothing in the formal model prevents it. The orchestrator declares success. The plan is wrong.</p>
            </div>
        </div>
        <p class="vis-caption">The orchestrator passes syntactically valid PDDL to the solver — but the PDDL is missing the support constraint. The resulting plan is "valid" according to the broken model but physically impossible. This is the subtlest and most dangerous failure mode.</p>
    </div>
<!-- ============================== -->
    <!-- SECTION 5: Agentic PDDL       -->
    <!-- ============================== -->

    <h2>Agentic PDDL: A Different Approach</h2>

    <p>In parallel with NL2Plan, La Malfa et al. (2025) introduced <strong>Agentic PDDL</strong>, which takes a different approach to the orchestration problem. Instead of a fixed pipeline, it uses a conversational loop: an LLM generates PDDL, a formal tool validates it, and the LLM iteratively refines — but with a critical addition: <strong>the user stays in the loop</strong>.</p>

    <p>When the system encounters ambiguity — "do the legs need to both be placed before the beam, or can one leg suffice?" — it asks the user. This solves the missing-abstraction problem by making implicit constraints explicit through dialogue.</p>

    <p>For RoboSort, this might look like:</p>

    <div class="vis-container">
        <div style="font-family: 'Helvetica Neue', Arial, sans-serif;">
            <div style="background: #f5f0ff; border: 1px solid #d4c5f0; border-radius: 10px; padding: 14px; margin-bottom: 8px;">
                <div style="font-size: 0.72rem; font-weight: 700; color: #6b3fa0; margin-bottom: 4px;">SYSTEM</div>
                <p style="font-size: 0.85rem; color: #3a2060; margin: 0;">I've extracted four actions: move, pick, place-on-platform, and place-on-piece. For place-on-piece, I need to clarify: you said "a beam goes on the legs." Does the beam require <strong>both</strong> legs to be placed first, or just one?</p>
            </div>
            <div style="background: #2a4066; border-radius: 10px; padding: 14px; margin-bottom: 8px;">
                <div style="font-size: 0.72rem; font-weight: 700; color: #7eb8da; margin-bottom: 4px;">USER</div>
                <p style="font-size: 0.85rem; color: #e0e8f0; margin: 0;">Both legs must be placed before the beam.</p>
            </div>
            <div style="background: #f0faf0; border: 1px solid #b5e8b5; border-radius: 10px; padding: 14px;">
                <div style="font-size: 0.72rem; font-weight: 700; color: #228b22; margin-bottom: 4px;">SYSTEM</div>
                <p style="font-size: 0.85rem; color: #0a4a0a; margin: 0;">Understood. Adding preconditions: <code style="font-size:0.78rem;">(supports L1 beam)</code>, <code style="font-size:0.78rem;">(supports L2 beam)</code>, and requiring both <code style="font-size:0.78rem;">(placed L1)</code> and <code style="font-size:0.78rem;">(placed L2)</code> before the beam can be placed. Updated PDDL validates correctly. Solving...</p>
            </div>
        </div>
        <p class="vis-caption">Agentic PDDL keeps the user in the loop. When the system detects ambiguity in support constraints, it asks — rather than guessing. This catches the exact failure mode NL2Plan misses.</p>
    </div>
<div class="callout insight">
        <div class="callout-label">Key Insight</div>
        <p>The orchestration problem is fundamentally a <em>communication</em> problem, not just a planning problem. Natural language is ambiguous. Formal models require precision. The gap between the two is where errors hide. Agentic PDDL bridges this gap through dialogue — asking users to resolve ambiguities rather than guessing. This human-in-the-loop approach trades automation for accuracy.</p>
    </div>

    <div class="agentic-sidebar">
        <div class="sidebar-title">Agentic AI in the Wild: The Requirements-to-Spec Pipeline</div>
        <p>This NL-to-PDDL challenge mirrors a classic software engineering problem: converting requirements documents into formal specifications. Product managers write "the system should handle payments" — but the formal spec needs to know: which payment providers? What happens on failure? Retry or refund? In what currency? The same ambiguity gap that plagues NL-to-PDDL plagues requirements engineering.</p>
        <p style="margin-top: 10px;">The planning community's insight — specialized agents with formal validation and human-in-the-loop clarification — may prove valuable far beyond PDDL generation.</p>
    </div>

    <!-- ============================== -->
    <!-- SECTION 6: Interactive Demo    -->
    <!-- ============================== -->

    <h2>Seeing the Full Pipeline: English to Plan</h2>

    <p>Let's watch the complete NL-to-Plan pipeline process our RoboSort warehouse description — from English input to verified plan output. Each stage shows what happens under the hood.</p>

    <div class="interactive-container">
        <div class="interactive-label">Interactive — click "▶ Run Pipeline" to watch English become a verified plan, stage by stage</div>

        <div style="text-align: center; margin-bottom: 16px;">
            <button class="auto-demo-btn" id="pipelineRunBtn" onclick="runPipelineDemo()">▶ Run Pipeline</button>
            <button class="auto-demo-btn" id="pipelineResetBtn" onclick="resetPipelineDemo()" style="display:none; background:#666;">↺ Reset</button>
        </div>

        <div class="nl-demo-container">
            <div class="nl-demo-stages" id="pipelineStages">
                <div class="demo-stage waiting" id="stage1">
                    <div class="stage-header">
                        <div class="stage-num">1</div>
                        <div class="stage-title">Parse Natural Language</div>
                    </div>
                    <div class="stage-content" id="stage1-content">Waiting...</div>
                </div>
                <div class="demo-stage waiting" id="stage2">
                    <div class="stage-header">
                        <div class="stage-num">2</div>
                        <div class="stage-title">Extract Types & Predicates</div>
                    </div>
                    <div class="stage-content" id="stage2-content">Waiting...</div>
                </div>
                <div class="demo-stage waiting" id="stage3">
                    <div class="stage-header">
                        <div class="stage-num">3</div>
                        <div class="stage-title">Generate PDDL Domain & Problem</div>
                    </div>
                    <div class="stage-content" id="stage3-content">Waiting...</div>
                </div>
                <div class="demo-stage waiting" id="stage4">
                    <div class="stage-header">
                        <div class="stage-num">4</div>
                        <div class="stage-title">Validate & Fix</div>
                    </div>
                    <div class="stage-content" id="stage4-content">Waiting...</div>
                </div>
                <div class="demo-stage waiting" id="stage5">
                    <div class="stage-header">
                        <div class="stage-num">5</div>
                        <div class="stage-title">Solve & Return Plan</div>
                    </div>
                    <div class="stage-content" id="stage5-content">Waiting...</div>
                </div>
            </div>
        </div>
    </div>

    <!-- ============================== -->
    <!-- SECTION 7: Results & Landscape -->
    <!-- ============================== -->

    <h2>The Landscape: Where Paradigm 2 Stands</h2>

    <p>Paradigm 2 results are mixed but improving rapidly. Here's the current picture:</p>

    <div class="vis-container">
        <h3 style="text-align:center; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 1rem; color: #1b2838; margin-bottom: 4px;">Paradigm 2 Results: NL-to-Plan Accuracy</h3>
        <p style="text-align:center; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.78rem; color: #888; margin-bottom: 16px;">End-to-end accuracy on planning domains (English in, valid plan out)</p>
        <div class="bottleneck-chart">
            <div class="bottleneck-row">
                <div class="bottleneck-label">Standard domains<br><span style="font-weight:400; font-size:0.7rem; color:#888;">(Blocksworld, Logistics)</span></div>
                <div class="bottleneck-bar-track">
                    <div class="bottleneck-bar" style="width: 100%; background: linear-gradient(90deg, #228b22, #2ea82e);"></div>
                </div>
                <div class="bottleneck-value" style="color: #228b22;">~100%</div>
            </div>
            <div class="bottleneck-row">
                <div class="bottleneck-label">Medium domains<br><span style="font-weight:400; font-size:0.7rem; color:#888;">(Satellite, Rovers)</span></div>
                <div class="bottleneck-bar-track">
                    <div class="bottleneck-bar" style="width: 65%; background: linear-gradient(90deg, #2a9d8f, #35b8a8);"></div>
                </div>
                <div class="bottleneck-value" style="color: #2a9d8f;">~65%</div>
            </div>
            <div class="bottleneck-row">
                <div class="bottleneck-label">Novel domains<br><span style="font-weight:400; font-size:0.7rem; color:#888;">(unseen in training)</span></div>
                <div class="bottleneck-bar-track">
                    <div class="bottleneck-bar" style="width: 35%; background: linear-gradient(90deg, #d4740e, #e88a30);"></div>
                </div>
                <div class="bottleneck-value" style="color: #d4740e;">~35%</div>
            </div>
            <div class="bottleneck-row">
                <div class="bottleneck-label">Ambiguous descriptions<br><span style="font-weight:400; font-size:0.7rem; color:#888;">(implicit constraints)</span></div>
                <div class="bottleneck-bar-track">
                    <div class="bottleneck-bar" style="width: 15%; background: linear-gradient(90deg, #b22222, #d43030);"></div>
                </div>
                <div class="bottleneck-value" style="color: #b22222;">~15%</div>
            </div>
        </div>
        <p class="vis-caption">NL-to-Plan accuracy varies dramatically by domain familiarity. Standard domains the LLM has seen in training (like Blocksworld) achieve near-perfect results. Novel domains with ambiguous constraints remain hard. Data patterns from Gestrin et al. (2025), La Malfa et al. (2025).</p>
    </div>
<p>The pattern tells a familiar story. When the domain is well-known — Blocksworld, logistics, standard problems from planning competitions — the LLM effectively has the PDDL in its training data and can reproduce it. When the domain is novel, accuracy drops sharply. This is the same pattern-matching vs. reasoning distinction from Post 4's Mystery Blocksworld, now appearing at the model generation level.</p>

    <p>Three open challenges define the frontier:</p>

    <ol>
        <li><strong>Implicit constraints.</strong> The RoboSort support constraint ("legs support beam") is stated explicitly. But many real-world constraints are implicit: "the robot can't carry two items" implies a gripper-empty predicate and a precondition on pick. Extracting these from natural language requires world knowledge, not just text parsing.</li>

        <li><strong>Compositional generalization.</strong> Can a system that has seen Blocksworld and logistics separately solve a problem that combines block-stacking <em>and</em> logistics? Current systems struggle because PDDL generation is treated as pattern matching, not compositional reasoning.</li>

        <li><strong>The orchestrator gap.</strong> The conductor of the multi-agent pipeline is the least reliable component. It must plan the PDDL generation process, handle failures, and decide when to ask the user for clarification vs. when to guess. This meta-planning problem remains largely unsolved.</li>
    </ol>

    <div class="callout insight">
        <div class="callout-label">Key Insight</div>
        <p>Paradigm 2 works well when the domain is familiar — the LLM effectively "remembers" the PDDL. For truly novel domains, the NL-to-PDDL translation requires genuine reasoning about actions, effects, and constraints. This is the hardest open problem in the field: teaching systems to <em>formalize</em> new domains, not just retrieve familiar ones.</p>
    </div>

    <!-- ============================== -->
    <!-- SECTION 8: Both Paradigms      -->
    <!-- ============================== -->

    <h2>Paradigm 1 + Paradigm 2: The Full Picture</h2>

    <p>Let's step back and see how all six posts connect. The series has traced a clear arc from foundations to frontier:</p>

    <div class="vis-container">
        <div style="font-family: 'Helvetica Neue', Arial, sans-serif;">
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 16px;">
                <div style="background: #eff5ff; border: 2px solid #4682b4; border-radius: 10px; padding: 18px;">
                    <div style="font-size: 0.72rem; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; color: #4682b4; margin-bottom: 8px;">Paradigm 1 — PDDL is Given</div>
                    <ul style="font-size: 0.82rem; margin: 0 0 0 16px; color: #2a5080; line-height: 1.6;">
                        <li><strong>Post 2:</strong> Formalize the problem in PDDL</li>
                        <li><strong>Post 3:</strong> Solve with heuristic search</li>
                        <li><strong>Post 4:</strong> LLMs alone fail (~12–30%)</li>
                        <li><strong>Post 5:</strong> LLMs + planners succeed (~82%)</li>
                    </ul>
                    <div style="margin-top: 10px; padding: 8px 12px; background: #d0e0f0; border-radius: 6px; font-size: 0.78rem; color: #2a5080;">
                        <strong>Status:</strong> Strong results. Requires PDDL expertise.
                    </div>
                </div>
                <div style="background: #fff7ed; border: 2px solid #d4740e; border-radius: 10px; padding: 18px;">
                    <div style="font-size: 0.72rem; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; color: #d4740e; margin-bottom: 8px;">Paradigm 2 — Only Natural Language</div>
                    <ul style="font-size: 0.82rem; margin: 0 0 0 16px; color: #7a3f00; line-height: 1.6;">
                        <li><strong>Post 6:</strong> NL2Plan, Agentic PDDL</li>
                        <li>100% on standard domains</li>
                        <li>~35% on novel domains</li>
                        <li>Orchestrator is the bottleneck</li>
                    </ul>
                    <div style="margin-top: 10px; padding: 8px 12px; background: #ffecd0; border-radius: 6px; font-size: 0.78rem; color: #7a3f00;">
                        <strong>Status:</strong> Promising but fragile. Orchestration is unsolved.
                    </div>
                </div>
            </div>
        </div>
        <p class="vis-caption">The two paradigms complement each other. Paradigm 1 is mature and reliable where PDDL exists. Paradigm 2 extends the reach to domains described only in English — but the orchestrator bottleneck limits its reliability on novel problems.</p>
    </div>
<p>The RoboSort warehouse has been our constant through all six posts. We watched it crash under a naive LLM plan (Post 1). We formalized it in PDDL (Post 2). We watched A* find the optimal path through its warehouse floor (Post 3). We watched GPT-4 fail and Mystery Blocksworld expose why (Post 4). We watched LLM-Modulo fix the plan in two iterations (Post 5). And now we've seen NL2Plan generate the PDDL from English (Post 6).</p>

    <p>One question remains: can we build systems that <em>learn</em> to orchestrate better — that adapt their coordination strategies, handle novel domains more reliably, and close the gap between familiar and unfamiliar? That's the subject of the final post.</p>

    <!-- ============================== -->
    <!-- SECTION 9: What's Ahead        -->
    <!-- ============================== -->

    <h2>What's Ahead</h2>

    <p>This post showed the promise and limits of Paradigm 2. NL2Plan and Agentic PDDL demonstrate that English-to-plan is possible — achieving 100% on standard domains. But the orchestrator bottleneck means novel domains and ambiguous descriptions remain unreliable.</p>

    <p>The final post asks the most forward-looking question in the series: can we build <strong>agentic AI systems for planning</strong> — systems that learn from their own failures, adapt their orchestration strategies, and handle novel domains without human intervention? The research frontier is moving fast, and the answers are starting to take shape.</p>

    <!-- Next Post Teaser -->
    <div class="next-post">
        <h3>Up Next: Part 7 — Agentic AI for Planning</h3>
        <p>The future of LLM-powered planning — systems that learn to coordinate, adapt to novel domains, and improve their own planning processes. Meta-learning, self-improving orchestrators, and the convergence of planning with reinforcement learning. Where the field goes next.</p>
    </div>

    <!-- ============================== -->
    <!-- REFERENCES                     -->
    <!-- ============================== -->

    <div class="references">
        <h2>References</h2>
        <ol>
            <li>Gestrin, M., Zuo, N., Stein, M., & Kambhampati, S. (2025). NL2Plan: Robust LLM-Driven Planning from Minimal Text. <em>AAAI 2025</em>.</li>
            <li>La Malfa, E., Mavroudis, E., & Wooldridge, M. (2025). Agentic PDDL: Conversational Generation of Planning Domains. <em>ICAPS 2025</em>.</li>
            <li>Katz, M., Kokel, H., & Muise, C. (2025). Planning in the Era of Language Models. <em>NeurIPS 2025 Tutorial</em>.</li>
            <li>Kambhampati, S., Valmeekam, V., & Stechly, K. (2024). LLM-Modulo: An LLM-Based Framework for Planning with Formal Verification. <em>AAAI 2024</em>.</li>
            <li>Liu, B., Jiang, Y., Zhang, X., et al. (2023). LLM+P: Empowering Large Language Models with Optimal Planning Proficiency. <em>arXiv preprint</em>.</li>
            <li>Xie, Z., Zhang, S., Zhu, Y., et al. (2024). TravelPlanner: A Benchmark for Real-World Planning with Language Agents. <em>ICML 2024</em>.</li>
            <li>Valmeekam, V., Marquez, M., & Kambhampati, S. (2023). PlanBench: An Extensible Benchmark for Evaluating Large Language Models on Planning. <em>NeurIPS 2023</em>.</li>
        </ol>
    </div>

    <!-- Download All -->
</article>

<div class="blog-footer">
    <p>Planning in the Era of LLMs — Part 6 of 7</p>
</div>

<script>
/* === NL-to-Plan Pipeline Demo === */
const pipelineData = [
    {
        title: 'Parse Natural Language',
        content: '<strong>Input:</strong> "A robot in a warehouse with three shelves (A, B, C) and a build zone. Shelf A has two legs (L1, L2). Shelf B has a beam. Shelf C has a roof and a flag. The robot carries one piece at a time. Legs support beam, beam supports roof, roof supports flag. Build the tower."<br><br><strong>Extracted entities:</strong> robot, warehouse, 3 shelves, build zone, 5 pieces, support relations, carry constraint, goal.'
    },
    {
        title: 'Extract Types & Predicates',
        content: '<strong>Types:</strong> location (home, shelf-a, shelf-b, shelf-c, build-zone), piece (L1, L2, beam, roof, flag)<br><br><strong>Predicates:</strong> robot-at(?l), piece-at(?p, ?l), holding(?p), gripper-empty, supports(?lower, ?upper), placed(?p), on-platform(?p)<br><br><em>Note: "carries one piece" → gripper-empty predicate. "Legs support beam" → supports predicate with precondition check.</em>'
    },
    {
        title: 'Generate PDDL Domain & Problem',
        content: '<strong>Domain:</strong> robosort-tower with 4 actions (move, pick, place-on-platform, place-on-piece)<br><strong>Problem:</strong> 5 objects, initial state (all on shelves, robot at home), goal (all placed)<br><br><code style="font-size:0.75rem; background:#1e1e2e; color:#cdd6f4; padding:8px 12px; border-radius:4px; display:block; margin-top:8px; white-space:pre;">(:action place-on-piece\n  :parameters (?p ?support - piece ?l - location)\n  :precondition (and (holding ?p)\n    (robot-at ?l) (placed ?support)\n    (supports ?support ?p))\n  :effect (and (placed ?p)\n    (not (holding ?p)) (gripper-empty)))</code>'
    },
    {
        title: 'Validate & Fix',
        content: '<strong>Syntax check:</strong> ✓ All parentheses balanced, types declared, predicates used consistently.<br><br><strong>Semantic check:</strong> ✓ All action parameters typed. All precondition predicates defined. All effects reference valid predicates.<br><br><strong>Solvability check:</strong> ✓ Planner finds a plan — confirms the model isn\'t trivially unsolvable or over-constrained.<br><br><span style="color:#228b22; font-weight:700;">PDDL validation passed.</span>'
    },
    {
        title: 'Solve & Return Plan',
        content: '<strong>Planner:</strong> Fast Downward (A* + h<sub>FF</sub>)<br><strong>Time:</strong> &lt;0.01 seconds<br><strong>Plan length:</strong> 14 actions (optimal)<br><br><div style="background:#f0faf0; border:1px solid #228b22; border-radius:6px; padding:10px; margin-top:8px;"><strong style="color:#228b22;">Verified Plan:</strong><br>1. move(home, shelf-a)<br>2. pick(L1, shelf-a)<br>3. move(shelf-a, build-zone)<br>4. place-on-platform(L1)<br>5–8. [L2: shelf-a → build-zone]<br>9–11. [Beam: shelf-b → place-on-piece(Beam, L1)]<br>12–13. [Roof: shelf-c → place-on-piece(Roof, Beam)]<br>14. [Flag: shelf-c → place-on-piece(Flag, Roof)]<br><br><span style="color:#228b22; font-weight:700;">✓ All 5 pieces placed. Tower complete.</span></div>'
    }
];

let pipelineStep = -1;

function runPipelineDemo() {
    const btn = document.getElementById('pipelineRunBtn');
    const resetBtn = document.getElementById('pipelineResetBtn');
    btn.disabled = true;
    btn.textContent = 'Running...';
    pipelineStep = 0;
    advancePipeline();
}

function advancePipeline() {
    if (pipelineStep >= 5) {
        document.getElementById('pipelineRunBtn').style.display = 'none';
        document.getElementById('pipelineResetBtn').style.display = 'inline-block';
        return;
    }

    const stage = document.getElementById('stage' + (pipelineStep + 1));
    const content = document.getElementById('stage' + (pipelineStep + 1) + '-content');

    // Mark previous as done
    if (pipelineStep > 0) {
        document.getElementById('stage' + pipelineStep).className = 'demo-stage done';
    }

    stage.className = 'demo-stage active';
    content.innerHTML = pipelineData[pipelineStep].content;

    pipelineStep++;
    setTimeout(advancePipeline, 2000);
}

function resetPipelineDemo() {
    for (let i = 1; i <= 5; i++) {
        document.getElementById('stage' + i).className = 'demo-stage waiting';
        document.getElementById('stage' + i + '-content').textContent = 'Waiting...';
    }
    pipelineStep = -1;
    document.getElementById('pipelineRunBtn').style.display = 'inline-block';
    document.getElementById('pipelineRunBtn').disabled = false;
    document.getElementById('pipelineRunBtn').textContent = '▶ Run Pipeline';
    document.getElementById('pipelineResetBtn').style.display = 'none';
}
</script>
