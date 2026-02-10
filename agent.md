# Quiz Template Agent Guide

## Table of Contents
1. [Overview & Quick Start](#overview)
2. [Features](#features)
3. [File Structure](#file-structure)
4. [Complete Quiz HTML Template](#complete-quiz-html-template)
5. [Complete Index HTML Template](#complete-index-html-template)
6. [Question Format Specification](#question-format-specification)
7. [Customization Guide](#customization-guide)
8. [Step-by-Step: Creating a New Quiz Collection](#step-by-step-creating-a-new-quiz-collection)
9. [Advanced Features](#advanced-features)
10. [Tips for Different Subjects](#tips-for-different-subjects)
11. [Troubleshooting](#troubleshooting)
12. [Deployment Options](#deployment-options)
13. [Quick Reference Checklist](#quick-reference-checklist)
14. [Examples for Different Subjects](#examples-for-different-subjects)
15. [Summary](#summary)

---

## Overview
This document provides a comprehensive template for creating interactive MCQ (Multiple Choice Question) quizzes for any subject. The template is a standalone HTML file with embedded CSS and JavaScript, requiring no external dependencies or frameworks.

### Quick Start (TL;DR)
1. **Copy** the "Complete Index HTML Template" → save as `index.html`
2. **Copy** the "Complete Quiz HTML Template" → save as `[topic]_1_quiz.html`
3. **Edit** the quiz: add your questions to the `QUESTIONS` array
4. **Update** index.html: add your quiz to the `QUIZZES` object
5. **Open** `index.html` in a browser and test!

Full details and customization options below ⬇️

---

## Features
- ✅ **Instant feedback** – Shows correct/incorrect answers immediately
- 📊 **Score tracking** – Real-time score, progress, and percentage
- 🔒 **One attempt per question** – Questions lock after first answer
- 🔄 **Shuffle mode** – Randomize questions and options
- 👁️ **Reveal all answers** – Preview correct answers without affecting score
- 🎯 **Responsive design** – Works on desktop and mobile
- 💾 **No backend required** – Pure client-side, works offline

---

## File Structure

### Recommended Organization
```
project-root/
├── index.html                          # Main landing page with tabs & notes viewer
├── [topic]_1_quiz.html
├── [topic]_2_quiz.html
├── [topic]_3_quiz.html
├── ...
└── notes/                              # Optional: Markdown notes
    ├── lec1.md
    ├── lec2.md
    └── ...
```

### What You Get
| File | Purpose | Template Location |
|------|---------|------------------|
| `index.html` | Landing page with quiz links, tabs, and notes viewer | See "Complete Index HTML Template" section |
| `*_quiz_html.html` | Individual quiz files with questions | See "Complete Quiz HTML Template" section |
| `notes/*.md` | Optional markdown study notes | User-created content |
| `CNAME` | Optional: For GitHub Pages custom domain | User-configured |

### How It Works Together

```
┌─────────────────────────────────────────────────────────────┐
│                        index.html                           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │   Tab 1      │  │   Tab 2      │  │  Notes Viewer    │  │
│  └──────────────┘  └──────────────┘  └──────────────────┘  │
│         │                 │                     │            │
│         ▼                 ▼                     ▼            │
│  ┌──────────┐      ┌──────────┐         ┌──────────┐       │
│  │ Quiz 1   │      │ Quiz 6   │         │ lec1.md  │       │
│  │ Quiz 2   │      │ Quiz 7   │         │ lec2.md  │       │
│  │ Quiz 3   │      │ Quiz 8   │         │ ...      │       │
│  └──────────┘      └──────────┘         └──────────┘       │
└─────────────────────────────────────────────────────────────┘
          │                 │
          ▼                 ▼
   ┌──────────────┐  ┌──────────────┐
   │ quiz_1.html  │  │ quiz_6.html  │
   │              │  │              │
   │ [Questions]  │  │ [Questions]  │
   │ [Scoring]    │  │ [Scoring]    │
   │ [Shuffle]    │  │ [Shuffle]    │
   └──────────────┘  └──────────────┘
```

**User Journey:**
1. Opens `index.html` in browser
2. Clicks a tab to see quizzes for that section
3. Clicks "Start Quiz" → Opens individual quiz file
4. Takes quiz, sees immediate feedback
5. Returns to index to take another quiz or view notes

### Naming Convention
**Format:** `[topic]_[number]_quiz.html`

**Examples:**
- `lecture_1_quiz.html`
- `chapter_3_quiz.html`
- `module_5_quiz.html`
- `lab_2_quiz.html`

**Why this format?**
- Simple and concise
- Easy to type and remember
- Clear numbering system
- Works well with version control

---

## Complete Quiz HTML Template

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>[SUBJECT CODE] – [Topic Name] MCQ Quiz: [Topic Description]</title>
  <style>
    :root { --maxw: 980px; --accent: #0f766e; --muted:#6b7280; }
    body { font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, "Noto Sans", sans-serif; line-height: 1.45; margin: 0; background: #f7f7f8; color:#111827; }
    header { background: white; border-bottom: 1px solid #e5e7eb; }
    .wrap { max-width: var(--maxw); margin: 0 auto; padding: 18px 16px; }
    h1 { font-size: clamp(20px, 2.6vw, 28px); margin: 0 0 6px; }
    .sub { color: var(--muted); font-size: 14px; }
    .controls { display:flex; gap:10px; flex-wrap:wrap; margin-top:10px }
    button, .btn { cursor:pointer; border:1px solid #e5e7eb; background:white; padding:8px 12px; border-radius:10px; font-weight:600; }
    button.primary { background: var(--accent); color: white; border-color: var(--accent); }
    button:disabled { opacity:.6; cursor:not-allowed }
    .stats { display:flex; gap:18px; align-items:center; margin-top:10px; }
    .pill { background:#eef2ff; color:#3730a3; font-weight:700; padding:6px 10px; border-radius:999px; }
    main { max-width: var(--maxw); margin: 12px auto 60px; padding: 0 16px; }
    .qcard { background:white; border:1px solid #e5e7eb; border-radius:14px; padding:14px; margin:14px 0; box-shadow: 0 1px 0 rgba(0,0,0,.03); }
    .qhead { display:flex; justify-content:space-between; align-items:baseline; gap:6px }
    .qtitle { font-weight:700; margin:0 0 8px }
    .badge { font-size:12px; color:#374151; background:#f3f4f6; border:1px solid #e5e7eb; padding:2px 8px; border-radius:999px }
    .opts { display:grid; gap:8px; margin-top:8px }
    .opt { display:flex; gap:10px; align-items:flex-start; padding:10px; border:1px solid #e5e7eb; border-radius:12px; background:white; cursor:pointer; }
    .opt:hover { border-color:#c7d2fe; }
    .opt.correct { border-color:#16a34a; background:#f0fdf4; }
    .opt.wrong { border-color:#dc2626; background:#fef2f2; }
    .opt.disabled { pointer-events:none; opacity:.8 }
    .letter { width:28px; height:28px; border-radius:999px; border:1px solid #e5e7eb; display:grid; place-items:center; font-weight:700; }
    .exp { margin-top:10px; border-top:1px dashed #e5e7eb; padding-top:10px; color:#111827; }
    .exp strong { color:#065f46 }
    footer { text-align:center; color:#6b7280; padding:30px 12px; }
    details > summary { cursor:pointer; }
  </style>
</head>
<body>
  <header>
    <div class="wrap">
      <h1>[Topic Name] MCQ Quiz — [Topic Description]</h1>
      <div class="sub">[Subject Code] – [Course Name] • [Semester/Term] • Instant feedback • Score tracking • One attempt per question</div>
      <div class="stats" id="stats">
        <span class="pill" id="score">Score: 0</span>
        <span class="pill" id="progress">Answered: 0 / 0</span>
        <span class="pill" id="percent">0%</span>
      </div>
      <div class="controls">
        <button class="primary" id="resetBtn" title="Reset the quiz to try again">Reset quiz</button>
        <button id="revealBtn" title="Reveal correct answers for all questions">Reveal all answers</button>
        <button id="shuffleBtn" title="Shuffle questions and answer options">Shuffle</button>
        <details>
          <summary>How scoring works</summary>
          <div class="sub">You get +1 when you select the correct option on your first click. After you answer a question, it locks to prevent double-counting. "Reveal all" marks all correct answers without affecting your score.</div>
        </details>
      </div>
    </div>
  </header>

  <main id="quiz"></main>

  <footer>
    Based on [Topic/Lecture/Chapter Name] ([source type: e.g., slides, textbook, lecture notes]). This educational quiz is for practice.
  </footer>

  <script>
    const QUESTIONS = [
    // Question 1
    { q: "What is the question text here?",
      options: [
        "First option (if this is correct, set correct: 0)",
        "Second option (if this is correct, set correct: 1)",
        "Third option (if this is correct, set correct: 2)",
        "Fourth option (if this is correct, set correct: 3)"
      ], correct: 0,  // Index of the correct option (0-based)
      why: "Explanation of why this answer is correct. Provide context from the source material."
    },
    // Question 2
    { q: "Another question?",
      options: [
        "Option A",
        "Option B",
        "Option C",
        "Option D"
      ], correct: 2,
      why: "Explanation for the correct answer."
    },
    // Add more questions following the same pattern...
    ];

    // --- Quiz Logic (DO NOT MODIFY) ---
    const quizEl = document.getElementById('quiz');
    const scoreEl = document.getElementById('score');
    const progressEl = document.getElementById('progress');
    const percentEl = document.getElementById('percent');
    const resetBtn = document.getElementById('resetBtn');
    const revealBtn = document.getElementById('revealBtn');
    const shuffleBtn = document.getElementById('shuffleBtn');

    let state = { score: 0, answered: 0 };
    let currentQuestions = [];

    function shuffle(arr) {
      const copy = [...arr];
      for (let i = copy.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [copy[i], copy[j]] = [copy[j], copy[i]];
      }
      return copy;
    }

    function buildQuestions() {
      return shuffle(QUESTIONS).map(q => ({
        q: q.q,
        options: shuffle(q.options.map((txt, i) => ({ txt, isCorrect: i === q.correct }))),
        correct: 0,
        why: q.why,
        __answered: false
      })).map(item => {
        item.correct = item.options.findIndex(o => o.isCorrect);
        item.options = item.options.map(o => o.txt);
        return item;
      });
    }

    function render() {
      quizEl.innerHTML = '';
      currentQuestions.forEach((item, qi) => {
        const card = document.createElement('div');
        card.className = 'qcard';

        const head = document.createElement('div');
        head.className = 'qhead';
        const title = document.createElement('p');
        title.className = 'qtitle';
        title.textContent = `${qi + 1}. ${item.q}`;
        const badge = document.createElement('span');
        badge.className = 'badge';
        badge.textContent = `Q${qi + 1}`;
        head.appendChild(title);
        head.appendChild(badge);
        card.appendChild(head);

        const opts = document.createElement('div');
        opts.className = 'opts';
        item.options.forEach((opt, oi) => {
          const row = document.createElement('div');
          row.className = 'opt';
          row.setAttribute('data-q', qi);
          row.setAttribute('data-o', oi);

          const letter = document.createElement('div');
          letter.className = 'letter';
          letter.textContent = String.fromCharCode(65 + oi); // A, B, C, D...

          const span = document.createElement('span');
          span.textContent = opt;

          row.appendChild(letter);
          row.appendChild(span);
          row.addEventListener('click', onChoose);
          opts.appendChild(row);
        });

        const exp = document.createElement('div');
        exp.className = 'exp';
        exp.hidden = true;
        exp.innerHTML = `<strong>Explanation:</strong> ${item.why}`;

        card.appendChild(opts);
        card.appendChild(exp);
        quizEl.appendChild(card);
      });

      updateStats();
    }

    function onChoose(e) {
      const btn = e.currentTarget;
      const qIndex = +btn.getAttribute('data-q');
      const oIndex = +btn.getAttribute('data-o');
      const card = btn.closest('.qcard');
      const exp = card.querySelector('.exp');

      const data = currentQuestions[qIndex];
      if (data.__answered) return; // lock after first attempt

      const options = card.querySelectorAll('.opt');
      options.forEach(b => b.classList.add('disabled'));

      data.__answered = true;
      state.answered++;

      if (oIndex === data.correct) {
        btn.classList.add('correct');
        state.score++;
      } else {
        btn.classList.add('wrong');
        options[data.correct].classList.add('correct');
      }

      exp.hidden = false;
      updateStats();
    }

    function updateStats() {
      scoreEl.textContent = `Score: ${state.score}`;
      progressEl.textContent = `Answered: ${state.answered} / ${currentQuestions.length}`;
      const pct = currentQuestions.length ? Math.round((state.score/currentQuestions.length)*100) : 0;
      percentEl.textContent = `${pct}%`;
    }

    function revealAll() {
      document.querySelectorAll('.qcard').forEach((card, qi) => {
        const data = currentQuestions[qi];
        const exp = card.querySelector('.exp');
        const options = card.querySelectorAll('.opt');
        options.forEach((b, oi) => {
          b.classList.remove('wrong');
          if (oi === data.correct) b.classList.add('correct');
          b.classList.add('disabled');
        });
        data.__answered = true;
        exp.hidden = false;
      });
    }

    function resetQuiz() {
      state.score = 0; state.answered = 0;
      render();
    }

    function shuffleQuiz() {
      state.score = 0; state.answered = 0;
      currentQuestions = buildQuestions();
      render();
    }

    resetBtn.addEventListener('click', resetQuiz);
    revealBtn.addEventListener('click', revealAll);
    shuffleBtn.addEventListener('click', shuffleQuiz);

    currentQuestions = buildQuestions();
    render();
  </script>
</body>
</html>
```

---

## Complete Index HTML Template

This template creates a landing page that links to all your quizzes with an optional notes viewer.

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>[SUBJECT CODE] – Quizzes & Notes</title>
  <style>
    :root { --maxw:1100px; --accent:#111827; --brand:#2563eb; --muted:#6b7280; }
    * { box-sizing:border-box }
    body { font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, "Noto Sans", sans-serif; line-height:1.5; margin:0; background:#0b1220; color:#e5e7eb; }
    header { border-bottom:1px solid #1f2937; background:linear-gradient(180deg,#0b1220,#0b1220 60%,#0e1628); }
    .wrap { max-width:var(--maxw); margin:0 auto; padding:18px 16px; }
    h1 { margin:0 0 6px; font-size:clamp(20px,2.6vw,28px); }
    .sub { color:#9ca3af; font-size:14px }
    main { max-width:var(--maxw); margin:16px auto 40px; padding:0 16px; display:grid; grid-template-columns: 1.1fr .9fr; gap:18px }
    @media (max-width: 900px) { main { grid-template-columns: 1fr } }
    .card { background:#0f172a; border:1px solid #1f2937; border-radius:14px; padding:14px; box-shadow:0 1px 0 rgba(0,0,0,.35) }
    .grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(260px,1fr)); gap:12px }
    .grid[hidden] { display:none }
    .tabs { display:flex; gap:8px; margin:10px 0 14px; flex-wrap:wrap }
    .tab { border:1px solid #1f2937; background:#0b1328; color:#9ca3af; padding:6px 12px; border-radius:999px; font-weight:600; cursor:pointer }
    .tab.active { background:#1e293b; border-color:#3b82f6; color:#e6f0ff }
    .lec { display:flex; flex-direction:column; gap:10px; padding:12px; border:1px solid #1f2937; border-radius:12px; background:#0b1328 }
    .lec h3 { margin:0; font-size:16px }
    .lec .tag { color:#93c5fd; font-size:12px }
    .btns { display:flex; gap:8px; flex-wrap:wrap }
    a.btn, button.btn { appearance:none; text-decoration:none; cursor:pointer; border:1px solid #1f2937; background:#111827; color:#e5e7eb; padding:8px 12px; border-radius:10px; font-weight:600; font-size:14px }
    a.btn.primary, button.btn.primary { background:#1e293b; border-color:#3b82f6; color:#e6f0ff }
    a.btn:hover, button.btn:hover { border-color:#3b82f6 }
    details { margin-top:10px; color:#9ca3af }
    details > summary { cursor:pointer }
    /* Notes viewer */
    .notes { display:flex; flex-direction:column; gap:10px }
    .notes header { background:transparent; border:0; padding:0 }
    .notes h2 { margin:0 0 4px; font-size:18px }
    .notes .bar { display:flex; gap:8px; align-items:center; flex-wrap:wrap }
    .notes input[type=text] { flex:1; min-width:160px; background:#0b1328; color:#e5e7eb; border:1px solid #1f2937; border-radius:10px; padding:8px 10px }
    .notes article { min-height:280px; max-height:70vh; overflow:auto; padding:12px; border:1px solid #1f2937; border-radius:12px; background:#0b1328 }
    /* Simple markdown styles */
    .md h1,.md h2,.md h3,.md h4,.md h5 { margin:.6em 0 .3em }
    .md h1{font-size:22px}.md h2{font-size:20px}.md h3{font-size:18px}
    .md p { margin:.5em 0 }
    .md code { background:#0a1020; border:1px solid #1f2937; padding:.1em .3em; border-radius:6px; font-family:ui-monospace, SFMono-Regular, Menlo, Consolas, monospace }
    .md pre { background:#0a1020; border:1px solid #1f2937; padding:10px; border-radius:10px; overflow:auto }
    .md ul { margin:.4em 0 .6em 1.2em }
    .md li { margin:.2em 0 }
    /* Fullscreen styles */
    #mdOut:fullscreen { 
      background:#0b1328; 
      border:none; 
      width:100vw; 
      height:100vh; 
      max-height:none; 
      padding:20px; 
      border-radius:0; 
      box-sizing:border-box; 
    }
    #mdOut:fullscreen .md h1{font-size:26px}
    #mdOut:fullscreen .md h2{font-size:22px}
    #mdOut:fullscreen .md h3{font-size:20px}
    #mdOut:fullscreen p{font-size:16px}
  </style>
</head>
<body>
  <header>
    <div class="wrap">
      <h1>[SUBJECT CODE] — Quizzes & Revision Notes</h1>
      <div class="sub">[Subject Name] • [Term/Semester] • Quick links to quizzes • Lightweight Markdown viewer for notes</div>
    </div>
  </header>
  <main>
    <section class="card">
      <h2 style="margin-top:0">Quizzes</h2>
      <div class="tabs" role="tablist">
        <button class="tab active" type="button" data-tab="section1" role="tab" aria-selected="true">Section 1</button>
        <button class="tab" type="button" data-tab="section2" role="tab" aria-selected="false">Section 2</button>
      </div>
      <div class="grid" id="section1Grid" data-panel="section1"></div>
      <div class="grid" id="section2Grid" data-panel="section2" hidden></div>
      <details>
        <summary>File naming convention (keep projects portable)</summary>
        <div style="margin-top:6px">
          Save each quiz HTML beside this index with consistent naming.<br>
          Example: <code>[topic]_1_quiz.html</code>, <code>[topic]_2_quiz.html</code>, etc.<br>
          Place Markdown notes in a <code>notes/</code> folder.
        </div>
      </details>
    </section>
    <section class="card notes">
      <header>
        <h2>Markdown Notes Viewer</h2>
        <div class="bar">
          <input id="mdPath" type="text" placeholder="notes/topic1.md" />
          <button class="btn primary" id="loadBtn" title="Fetch and render the Markdown file">Load</button>
          <input type="file" id="filePick" accept=".md,.txt,.markdown" style="display:none" />
          <button class="btn" id="pickBtn" title="Preview a local .md without hosting">Open .md file…</button>
          <button class="btn" id="fsBtn" title="Toggle fullscreen for the notes viewer">⛶</button>
        </div>
      </header>
      <article id="mdOut" class="md">
        <p style="color:#6b7280">Type a path or pick a file to preview Markdown notes.</p>
      </article>
    </section>
  </main>

  <script>
    // --- Quiz Configuration ---
    const QUIZZES = {
      section1: [
        { num: 1, title: "Topic 1 Title", desc: "Brief description", file: "lecture_1_quiz.html" },
        { num: 2, title: "Topic 2 Title", desc: "Brief description", file: "lecture_2_quiz.html" },
        { num: 3, title: "Topic 3 Title", desc: "Brief description", file: "lecture_3_quiz.html" },
        // Add more quizzes for section 1
      ],
      section2: [
        { num: 4, title: "Topic 4 Title", desc: "Brief description", file: "lecture_4_quiz.html" },
        { num: 5, title: "Topic 5 Title", desc: "Brief description", file: "lecture_5_quiz.html" },
        // Add more quizzes for section 2
      ]
    };

    // --- Tab Switching ---
    const tabs = document.querySelectorAll('.tab');
    const grids = {
      section1: document.getElementById('section1Grid'),
      section2: document.getElementById('section2Grid')
    };

    tabs.forEach(tab => {
      tab.addEventListener('click', () => {
        const target = tab.getAttribute('data-tab');
        tabs.forEach(t => {
          t.classList.remove('active');
          t.setAttribute('aria-selected', 'false');
        });
        tab.classList.add('active');
        tab.setAttribute('aria-selected', 'true');
        
        Object.keys(grids).forEach(key => {
          grids[key].hidden = (key !== target);
        });
      });
    });

    // --- Render Quiz Cards ---
    function renderQuizzes() {
      Object.keys(QUIZZES).forEach(section => {
        const grid = grids[section];
        grid.innerHTML = '';
        
        QUIZZES[section].forEach(quiz => {
          const card = document.createElement('div');
          card.className = 'lec';
          card.innerHTML = `
            <div>
              <h3>${quiz.title}</h3>
              <div class="tag">Quiz ${quiz.num}</div>
            </div>
            <p style="margin:0;color:#9ca3af;font-size:14px">${quiz.desc}</p>
            <div class="btns">
              <a href="${quiz.file}" target="_blank" class="btn primary">Start Quiz</a>
            </div>
          `;
          grid.appendChild(card);
        });
      });
    }

    renderQuizzes();

    // --- Markdown Viewer ---
    const mdPath = document.getElementById('mdPath');
    const mdOut = document.getElementById('mdOut');
    const loadBtn = document.getElementById('loadBtn');
    const pickBtn = document.getElementById('pickBtn');
    const filePick = document.getElementById('filePick');
    const fsBtn = document.getElementById('fsBtn');

    // Simple markdown parser (basic support)
    function parseMarkdown(md) {
      let html = md
        // Headers
        .replace(/^##### (.*$)/gim, '<h5>$1</h5>')
        .replace(/^#### (.*$)/gim, '<h4>$1</h4>')
        .replace(/^### (.*$)/gim, '<h3>$1</h3>')
        .replace(/^## (.*$)/gim, '<h2>$1</h2>')
        .replace(/^# (.*$)/gim, '<h1>$1</h1>')
        // Bold
        .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
        .replace(/__(.*?)__/g, '<strong>$1</strong>')
        // Italic
        .replace(/\*(.*?)\*/g, '<em>$1</em>')
        .replace(/_(.*?)_/g, '<em>$1</em>')
        // Code blocks
        .replace(/```([\s\S]*?)```/g, '<pre><code>$1</code></pre>')
        // Inline code
        .replace(/`([^`]+)`/g, '<code>$1</code>')
        // Links
        .replace(/\[([^\]]+)\]\(([^)]+)\)/g, '<a href="$2" target="_blank">$1</a>')
        // Lists
        .replace(/^\* (.*$)/gim, '<li>$1</li>')
        .replace(/^- (.*$)/gim, '<li>$1</li>')
        // Line breaks
        .replace(/\n\n/g, '</p><p>')
        .replace(/\n/g, '<br>');

      // Wrap lists
      html = html.replace(/(<li>.*<\/li>)/s, '<ul>$1</ul>');
      
      return `<p>${html}</p>`;
    }

    loadBtn.addEventListener('click', async () => {
      const path = mdPath.value.trim();
      if (!path) return;
      
      try {
        const res = await fetch(path);
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        const md = await res.text();
        mdOut.innerHTML = parseMarkdown(md);
      } catch (err) {
        mdOut.innerHTML = `<p style="color:#ef4444">Error loading ${path}: ${err.message}</p>`;
      }
    });

    pickBtn.addEventListener('click', () => filePick.click());

    filePick.addEventListener('change', e => {
      const file = e.target.files[0];
      if (!file) return;
      
      const reader = new FileReader();
      reader.onload = ev => {
        mdOut.innerHTML = parseMarkdown(ev.target.result);
        mdPath.value = file.name;
      };
      reader.readAsText(file);
    });

    fsBtn.addEventListener('click', () => {
      if (document.fullscreenElement) {
        document.exitFullscreen();
      } else {
        mdOut.requestFullscreen().catch(err => {
          console.warn('Fullscreen failed:', err);
        });
      }
    });

    // Allow Enter key to load
    mdPath.addEventListener('keypress', e => {
      if (e.key === 'Enter') loadBtn.click();
    });
  </script>
</body>
</html>
```

### Index.html Customization Guide

#### 1. Update Quiz Configuration

Replace the `QUIZZES` object with your quiz details:

```javascript
const QUIZZES = {
  section1: [
    { 
      num: 1, 
      title: "Introduction to Programming", 
      desc: "Variables, data types, and basic syntax", 
      file: "lecture_1_quiz.html" 
    },
    { 
      num: 2, 
      title: "Control Structures", 
      desc: "If-else, loops, and switch statements", 
      file: "lecture_2_quiz.html" 
    },
  ],
  section2: [
    { 
      num: 8, 
      title: "Advanced Topics", 
      desc: "Recursion and dynamic programming", 
      file: "lecture_8_quiz.html" 
    },
  ]
};
```

#### 2. Customize Tab Names

Modify the tab buttons to match your course structure:

```html
<!-- Change "Section 1" and "Section 2" to your needs. IGNORE THIS AND REMOVE TAB IF THE USER DOES NOT MENTION IT -->
<button class="tab active" data-tab="section1">Part 1: Fundamentals</button>
<button class="tab" data-tab="section2">Part 2: Advanced</button>
```

**Examples:**
- **By Trimester:** "Trimester 1", "Trimester 2"
- **By Unit:** "Unit A", "Unit B", "Unit C"
- **By Topic:** "Theory", "Practice"
- **By Difficulty:** "Beginner", "Intermediate", "Advanced"

#### 3. Update Header Information

```html
<h1>CS 101 — Quizzes & Revision Notes</h1>
<div class="sub">Introduction to Programming • Fall 2024 • Quick links to quizzes • Lightweight Markdown viewer for notes</div>
```

#### 4. Color Theme Options

**Light Theme:**
```css
body { background:#f9fafb; color:#111827; }
.card { background:#ffffff; border:1px solid #e5e7eb; }
.tab { background:#f3f4f6; color:#374151; }
.tab.active { background:#dbeafe; border-color:#2563eb; color:#1e40af }
```

**Blue Theme:**
```css
:root { --brand:#1e40af; }
body { background:#0c1e3a; color:#e5e7eb; }
```

**Purple Theme:**
```css
:root { --brand:#7c3aed; }
.tab.active { border-color:#7c3aed; }
a.btn.primary { border-color:#7c3aed; }
```

#### 5. Add More Sections/Tabs

To add a third section:

```javascript
// 1. Add to QUIZZES object
const QUIZZES = {
  section1: [...],
  section2: [...],
  section3: [  // NEW
    { num: 10, title: "Bonus Topics", desc: "Extra practice", file: "bonus_1.html" }
  ]
};

// 2. Add grid element in HTML
<div class="grid" id="section3Grid" data-panel="section3" hidden></div>

// 3. Add tab button
<button class="tab" data-tab="section3">Section 3</button>

// 4. Update grids object in JS
const grids = {
  section1: document.getElementById('section1Grid'),
  section2: document.getElementById('section2Grid'),
  section3: document.getElementById('section3Grid')  // NEW
};
```

#### 6. Disable Markdown Viewer

If you don't need the notes viewer, remove the second column:

```css
/* Change grid to single column */
main { 
  grid-template-columns: 1fr;  /* Changed from: 1.1fr .9fr */
}
```

Then remove the `<section class="card notes">...</section>` HTML block.

#### 7. Alternative: Simple Light Index

For a minimal light-themed index without tabs:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>[Subject] – Quiz Collection</title>
  <style>
    body { font-family: system-ui; max-width: 900px; margin: 40px auto; padding: 20px; background: #f9fafb; }
    h1 { margin-bottom: 30px; color: #111827; }
    .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 16px; }
    .card { border: 1px solid #e5e7eb; padding: 18px; border-radius: 12px; background: white; }
    .card h3 { margin: 0 0 8px; color: #111827; }
    .card p { color: #6b7280; margin: 0 0 14px; font-size: 14px; }
    .card a { display: inline-block; padding: 8px 16px; background: #2563eb; color: white; text-decoration: none; border-radius: 8px; font-weight: 600; }
    .card a:hover { background: #1d4ed8; }
  </style>
</head>
<body>
  <h1>[Subject Code] – Quiz Collection</h1>
  
  <div class="grid">
    <div class="card">
      <h3>Quiz 1: Topic Name</h3>
      <p>Brief description of quiz content and coverage</p>
      <a href="quiz_1.html">Start Quiz</a>
    </div>
    
    <div class="card">
      <h3>Quiz 2: Topic Name</h3>
      <p>Brief description of quiz content and coverage</p>
      <a href="quiz_2.html">Start Quiz</a>
    </div>
    
    <!-- Add more cards as needed -->
  </div>
</body>
</html>
```

---

## Question Format Specification

### Question Object Structure
```javascript
{
  q: "Question text goes here?",
  options: [
    "Option A text",
    "Option B text", 
    "Option C text",
    "Option D text"
  ],
  correct: 0,  // 0-based index (0=A, 1=B, 2=C, 3=D)
  why: "Explanation of the correct answer with supporting details."
}
```

### Field Descriptions

| Field | Type | Description |
|-------|------|-------------|
| `q` | String | The question text. Can include Unicode characters. |
| `options` | Array | List of 2-6 answer options. Typically 4 options. |
| `correct` | Number | Zero-based index of correct option (0 = first, 1 = second, etc.) |
| `why` | String | Explanation shown after answering. Can include HTML. |

### Best Practices for Questions

1. **Question Text (`q`)**
   - Be clear and concise
   - Avoid ambiguous phrasing
   - Use proper grammar and punctuation
   - Can use Unicode for special characters (e.g., `\u2011` for non-breaking hyphen)

2. **Options**
   - Provide 3-5 options (4 is standard)
   - Make all options plausible
   - Keep similar length to avoid telegraphing answers
   - Randomize correct answer position across questions
   - Use parallel grammatical structure

3. **Explanations (`why`)**
   - Reference source material (slides, pages, chapters)
   - Explain why the correct answer is right
   - Briefly explain why wrong answers are incorrect (optional)
   - Keep it concise but informative

---

## Customization Guide

### 1. Change Color Scheme
Modify the CSS `:root` variables:

```css
:root { 
  --maxw: 980px;        /* Max content width */
  --accent: #0f766e;    /* Primary color (buttons, highlights) */
  --muted: #6b7280;     /* Secondary text color */
}
```

**Popular color schemes:**
- **Blue:** `--accent: #2563eb;`
- **Purple:** `--accent: #7c3aed;`
- **Red:** `--accent: #dc2626;`
- **Green:** `--accent: #059669;`

### 2. Update Header Information
Replace placeholders in the `<header>` section:

```html
<h1>[Topic Name] MCQ Quiz — [Topic Description]</h1>
<div class="sub">[Subject Code] – [Course Name] • [Semester] • ...</div>
```

**Example:**
```html
<h1>Data Structures Quiz — Arrays and Linked Lists</h1>
<div class="sub">CS 201 – Data Structures • Fall 2024 • ...</div>
```

### 3. Update Footer
```html
<footer>
  Based on Chapter 3: Arrays (Textbook pages 45-67). This educational quiz is for practice.
</footer>
```

### 4. Adjust Max Width
Change `--maxw` for narrower/wider layouts:
- Narrow (mobile-first): `--maxw: 720px;`
- Standard: `--maxw: 980px;`
- Wide: `--maxw: 1200px;`

---

## Using the Index Page Template

For a complete, feature-rich index page, see the **Complete Index HTML Template** section above. It includes:
- **Tab navigation** for organizing quizzes into sections/semesters
- **Dynamic quiz cards** generated from JavaScript configuration
- **Markdown notes viewer** with file picker and fullscreen mode
- **Dark theme** optimized for study sessions
- **Fully responsive** design

The simple light alternative in that section provides a minimal static option if you don't need tabs or the notes viewer.

---

## Step-by-Step: Creating a New Quiz Collection

### Step 0: Set Up Your Project Structure
Create your project folder structure:
```
my-subject-quizzes/
├── index.html              (from Complete Index HTML Template)
├── lecture_1_quiz.html
├── lecture_2_quiz.html
├── ...
└── notes/                  (optional)
    ├── topic1.md
    └── topic2.md
```

### Step 1: Create the Index Page
- Copy the **Complete Index HTML Template** from above
- Save as `index.html` in your project root
- Update the `QUIZZES` object with your quiz file names
- Customize tab names, colors, and header text

### Step 2: Copy the Quiz Template
- Copy the **Complete Quiz HTML Template**
- Save as `[topic]_[number]_quiz.html`

### Step 3: Update Quiz Metadata
- Change `<title>` tag
- Update header `<h1>` and `.sub` elements
- Modify footer source reference

### Step 4: Add Questions
Replace the `QUESTIONS` array with your content:

```javascript
const QUESTIONS = [
  { 
    q: "Your first question?",
    options: ["A", "B", "C", "D"],
    correct: 0,
    why: "Explanation here."
  },
  // Add 10-30 questions for a good quiz
];
```

### Step 5: Customize Colors (Optional)
- Adjust `--accent` color in CSS `:root`
- Modify other styling as needed

### Step 6: Test Everything
- Open `index.html` in browser
- Click quiz links to verify they work
- Answer questions to verify logic
- Test "Shuffle", "Reset", and "Reveal all" buttons
- Check on mobile devices
- Test markdown viewer if using notes

### Step 7: Deploy (Optional)
- Upload to GitHub Pages, Netlify, or any web host
- Share the index.html link with students
- All files work offline/locally too

---

## Advanced Features

### 1. Adding Images to Questions
```javascript
{ 
  q: "What data structure is shown? <br><img src='images/tree.png' width='300'>",
  options: ["Array", "Tree", "Graph", "Queue"],
  correct: 1,
  why: "The diagram shows a binary tree structure."
}
```

### 2. Code Snippets in Questions
```javascript
{ 
  q: "What does this code output?<br><pre>for i in range(3):\n    print(i)</pre>",
  options: ["0 1 2", "1 2 3", "0 1 2 3", "Error"],
  correct: 0,
  why: "range(3) generates 0, 1, 2."
}
```

### 3. Math Equations (Basic HTML)
```javascript
{ 
  q: "Solve: x² + 5x + 6 = 0",
  options: ["x = -2, -3", "x = 2, 3", "x = -1, -6", "x = 1, 6"],
  correct: 0,
  why: "Factors to (x+2)(x+3) = 0, so x = -2 or x = -3."
}
```

### 4. Multiple Choice Variations
```javascript
// True/False
{ 
  q: "JavaScript is a compiled language.",
  options: ["True", "False"],
  correct: 1,
  why: "JavaScript is an interpreted language."
}

// Yes/No/Maybe
{ 
  q: "Is recursion always more efficient than iteration?",
  options: ["Always", "Never", "Sometimes", "Depends on the language"],
  correct: 2,
  why: "Efficiency depends on the specific problem and implementation."
}
```

---

## Tips for Different Subjects

### Computer Science / Programming
- Include code snippets
- Test algorithmic thinking
- Cover time/space complexity
- Include debugging questions

### Mathematics
- Use clear notation
- Include step-by-step solutions in explanations
- Vary difficulty levels
- Cover both theory and application

### Science (Biology, Chemistry, Physics)
- Include diagrams where helpful
- Test conceptual understanding
- Include formula-based problems
- Reference experiments or studies

### History / Social Studies
- Focus on key events and dates
- Test cause-and-effect relationships
- Include primary source analysis
- Cover geographical context

### Language Learning
- Include vocabulary questions
- Test grammar rules
- Provide context sentences
- Add pronunciation notes in explanations

---

## Troubleshooting

### Questions don't appear
- Check JavaScript console for errors
- Ensure `QUESTIONS` array is valid JSON
- Verify all questions have required fields

### Shuffle not working
- Ensure page is loaded completely
- Check browser compatibility (use modern browser)
- Verify JavaScript is enabled

### Styling issues
- Verify CSS is in `<style>` tags
- Check for missing closing tags
- Test in different browsers

### Score tracking incorrect
- Don't modify the quiz logic section
- Ensure `correct` index matches option position
- Reset quiz if testing repeatedly

---

## Accessibility Considerations

1. **Keyboard Navigation**: All buttons are keyboard-accessible
2. **Screen Readers**: Use semantic HTML (button, main, header, footer)
3. **Color Contrast**: Ensure text is readable (check AA/AAA standards)
4. **Focus Indicators**: Keep default focus outlines visible

### Accessibility Improvements (Optional)
Add ARIA labels:
```html
<button class="primary" id="resetBtn" 
        aria-label="Reset quiz to beginning"
        title="Reset the quiz to try again">Reset quiz</button>
```

---

## Performance Optimization

### For Large Quizzes (50+ questions)
1. Consider pagination or sections
2. Lazy-load images if used extensively
3. Minimize CSS/JS where possible
4. Test on low-end devices

### File Size Reduction
- Minify CSS/JS for production
- Compress images to WebP/optimized formats
- Remove comments from production code

---

## Version Control & Collaboration

### Recommended Git Workflow
```bash
# Initialize repository
git init

# Add files
git add index.html
git add *_quiz_.html

# Commit with descriptive message
git commit -m "Add quiz for Chapter 3: Data Structures"

# Push to remote
git push origin main
```

### Quiz Metadata (Optional)
Add comment at top of file for tracking:
```html
<!--
  Quiz: Data Structures - Arrays
  Subject: CS 201
  Created: 2024-02-10
  Author: [Your Name]
  Questions: 25
  Version: 1.0
-->
```

---

## License & Attribution

### MIT License Template
```html
<!--
  MIT License
  Copyright (c) [Year] [Your Name]
  
  Permission is hereby granted, free of charge, to any person obtaining a copy
  of this software and associated documentation files (the "Software"), to deal
  in the Software without restriction...
-->
```

### Attribution for Educational Use
```html
<footer>
  Based on [Source Material]. Created by [Your Name] for educational purposes.
  Template: <a href="link-to-template">MCQ Quiz Template</a>
</footer>
```

---

## Examples for Different Subjects

### Example 1: Mathematics Quiz
```javascript
const QUESTIONS = [
  { 
    q: "What is the derivative of f(x) = 3x² + 2x - 5?",
    options: ["6x + 2", "6x - 2", "3x + 2", "6x"],
    correct: 0,
    why: "Using power rule: d/dx(3x²) = 6x, d/dx(2x) = 2, d/dx(-5) = 0. Sum: 6x + 2."
  },
  { 
    q: "If sin(θ) = 0.5, what is θ in degrees (0° ≤ θ ≤ 90°)?",
    options: ["30°", "45°", "60°", "90°"],
    correct: 0,
    why: "sin(30°) = 1/2 = 0.5"
  }
];
```

### Example 2: History Quiz
```javascript
const QUESTIONS = [
  { 
    q: "In which year did World War II end?",
    options: ["1943", "1944", "1945", "1946"],
    correct: 2,
    why: "World War II ended in 1945 with Japan's surrender in September."
  },
  { 
    q: "Who was the first President of the United States?",
    options: ["Thomas Jefferson", "George Washington", "John Adams", "Benjamin Franklin"],
    correct: 1,
    why: "George Washington served as the first U.S. President from 1789-1797."
  }
];
```

### Example 3: Programming Quiz
```javascript
const QUESTIONS = [
  { 
    q: "What is the time complexity of binary search?",
    options: ["O(n)", "O(log n)", "O(n log n)", "O(1)"],
    correct: 1,
    why: "Binary search halves the search space each iteration, resulting in O(log n) complexity."
  },
  { 
    q: "Which keyword creates a constant in JavaScript?",
    options: ["var", "let", "const", "final"],
    correct: 2,
    why: "'const' declares a block-scoped constant variable that cannot be reassigned."
  }
];
```

---

## Quick Reference Checklist

### Index Page Setup
- [ ] `QUIZZES` object configured with all quiz files
- [ ] Tab names customized for your course structure
- [ ] Header and subtitle updated with course info
- [ ] All quiz file paths are correct
- [ ] Quiz descriptions are accurate
- [ ] Color scheme matches your preference
- [ ] Markdown notes folder created (if using)
- [ ] Tested tab switching functionality
- [ ] Verified all quiz links work

### Before Publishing Each Quiz
- [ ] All questions have 3-5 options (typically 4)
- [ ] `correct` indices are accurate (0-based)
- [ ] Explanations are clear and helpful
- [ ] Header information updated (title, subtitle)
- [ ] Footer sources cited correctly
- [ ] Color scheme matches index page
- [ ] Tested in multiple browsers
- [ ] Mobile responsive verified
- [ ] No spelling/grammar errors
- [ ] Quiz listed in index.html QUIZZES object

### Quality Standards
- [ ] 15-30 questions per quiz (sweet spot)
- [ ] Mix of difficulty levels
- [ ] Explanations reference source material
- [ ] Distractors (wrong answers) are plausible
- [ ] Questions avoid "all of the above" / "none of the above"
- [ ] Clear, unambiguous phrasing
- [ ] Consistent formatting across all quizzes
- [ ] File naming follows convention

---

## Support & Resources

### Browser Compatibility
- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+
- ✅ Mobile browsers (iOS Safari, Chrome Android)

### Testing Tools
- [W3C HTML Validator](https://validator.w3.org/)
- [Can I Use](https://caniuse.com/) - Check CSS/JS compatibility
- Browser DevTools for debugging

### Further Customization
For advanced features (timers, analytics, server-side storage), consider:
- Adding localStorage for persistent scores
- Integrating with Google Forms
- Using quiz platforms (Quizlet, Kahoot alternatives)

---

## Deployment Options

### Option 1: GitHub Pages (Free Hosting)
1. Create a GitHub repository
2. Upload all HTML files and notes folder
3. Go to Settings → Pages
4. Select branch (usually `main`) → Save
5. Your site will be live at `https://yourusername.github.io/repo-name/`

**Add custom domain (optional):**
- Create a `CNAME` file with your domain
- Configure DNS settings with your provider

### Option 2: Netlify (Drag & Drop)
1. Go to [netlify.com](https://netlify.com)
2. Drag your project folder to the upload area
3. Done! Auto-deployed with HTTPS

### Option 3: Local/Network Use
- No deployment needed
- Share the folder via USB/email/network drive
- Open `index.html` directly in any browser
- Perfect for offline use or classroom settings

### Option 4: Other Hosting
Works with any static hosting service:
- Vercel
- Cloudflare Pages
- GitLab Pages
- Firebase Hosting
- Your own web server

---

## Changelog Template

Keep track of updates in your quiz files:

```html
<!--
  Changelog:
  v1.0 (2024-02-10) - Initial release, 25 questions
  v1.1 (2024-02-15) - Fixed typo in Q12, added 5 new questions
  v1.2 (2024-02-20) - Updated explanations for clarity
-->
```

---

## Contact & Contributions

**Template Version:** 1.0  
**Last Updated:** February 2026  
**License:** MIT (modify freely for your needs)  
**Project Type:** Educational quiz framework  

Feel free to customize this template for your courses and share with colleagues!

---

## Summary

This guide provides **two complete templates** for creating professional, interactive quiz collections:

1. **Quiz Template** – Individual MCQ quiz pages with instant feedback
2. **Index Template** – Landing page with tabs, quiz links, and markdown viewer

### Key Advantages

✨ **Zero dependencies** – No frameworks, libraries, or build tools  
🎨 **Fully customizable** – Easy to modify colors, layout, and behavior  
📱 **Responsive** – Works on all devices  
♿ **Accessible** – Keyboard navigation and semantic HTML  
🔒 **Privacy-focused** – No tracking, no external requests  
📦 **Portable** – Works offline and online  
🚀 **Ready to deploy** – GitHub Pages, Netlify, or local use  
🎓 **Subject-agnostic** – Works for any topic or course

### Getting Started

1. Copy the **Complete Index HTML Template** (save as `index.html`)
2. Copy the **Complete Quiz HTML Template** (save as `[topic]_N_quiz.html`)
3. Add your questions to the `QUESTIONS` array
4. Configure the `QUIZZES` object in index.html
5. Test locally, then deploy!

### What Makes This Special

Unlike quiz platforms that require accounts, subscriptions, or internet connections:
- ✅ **Own your content** – All files are yours
- ✅ **No ads or tracking** – Complete privacy
- ✅ **Works forever** – No platform dependency
- ✅ **Easy to modify** – Just edit HTML/CSS/JS
- ✅ **Free hosting options** – GitHub Pages, Netlify, etc.

Perfect for educators, students, trainers, and self-learners! 🚀

---

**Happy quiz creating!** 📚✨

---

## Quick Reference Card

Print or bookmark this section for quick access while creating quizzes!

### Essential Code Snippets

**Question Template:**
```javascript
{ 
  q: "Your question text?",
  options: ["Option A", "Option B", "Option C", "Option D"],
  correct: 0,  // 0=A, 1=B, 2=C, 3=D
  why: "Explanation of the correct answer."
}
```

**Adding Quiz to Index:**
```javascript
// In index.html QUIZZES object
{ 
  num: 1, 
  title: "Quiz Title", 
  desc: "Brief description", 
  file: "lecture_1_quiz.html" 
}
```

**Color Customization:**
```css
:root { 
  --accent: #2563eb;  /* Change to your color */
}
```

### Common Customization Points

| What to Change | Where | Example |
|----------------|-------|---------|
| Quiz title | `<h1>` in quiz file | `<h1>Chapter 3 Quiz</h1>` |
| Subject info | `.sub` div in quiz | `CS 101 • Fall 2024` |
| Primary color | `:root --accent` | `#2563eb` (blue) |
| Questions | `QUESTIONS` array | Add/edit question objects |
| Index tabs | Tab buttons in index | `<button>Unit 1</button>` |
| Quiz listings | `QUIZZES` object | Add quiz config objects |

### File Locations Cheat Sheet

```
project-root/
├── index.html              ← Landing page (from "Complete Index HTML Template")
├── lecture_1_quiz.html     ← Quiz files (from "Complete Quiz HTML Template")
├── lecture_2_quiz.html
└── notes/
    └── topic1.md           ← Optional markdown notes
```

### Testing Checklist (Fast)

- [ ] Questions display correctly
- [ ] Clicking answer shows correct/wrong
- [ ] Score updates properly
- [ ] Shuffle randomizes questions
- [ ] Reset clears scores
- [ ] Links work from index
- [ ] Mobile view looks good

### Deployment in 3 Steps

**GitHub Pages:**
```bash
git init
git add .
git commit -m "Add quizzes"
git push
# Enable Pages in repo settings
```

**Or just:** Drag folder to [netlify.com](https://netlify.com)

---

### Need Help?

- **Questions won't show**: Check JavaScript console for errors
- **Links broken**: Verify file names match exactly
- **Styling issues**: Check for missing closing tags
- **Score wrong**: Ensure `correct` index matches option position

---
