# Quiz Template Agent Guide

## Overview
This document provides a comprehensive template for creating interactive MCQ (Multiple Choice Question) quizzes for any subject. The template is a standalone HTML file with embedded CSS and JavaScript, requiring no external dependencies or frameworks.

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
/
├── index.html                          # Main landing page with quiz links
├── [topic]_1_quiz.html  # Quiz file 1
├── [topic]_2_quiz.html  # Quiz file 2
├── ...
└── notes/                              # Optional: Markdown notes
    ├── topic1.md
    ├── topic2.md
    └── ...
```

### Naming Convention
**Format:** `[topic_type]_[number]_quiz.html`

**Examples:**
- `lecture_1_quiz.html`
- `chapter_3_quiz.html`
- `module_5_quiz.html`

---

## Complete HTML Template

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

## Creating an Index Page

### Basic Index Structure
```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>[Subject] – Quizzes</title>
  <style>
    body { font-family: system-ui; max-width: 900px; margin: 40px auto; padding: 20px; }
    h1 { margin-bottom: 30px; }
    .quiz-list { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 15px; }
    .quiz-card { border: 1px solid #ddd; padding: 15px; border-radius: 8px; }
    .quiz-card h3 { margin-top: 0; }
    .quiz-card a { display: inline-block; margin-top: 10px; padding: 8px 16px; background: #2563eb; color: white; text-decoration: none; border-radius: 6px; }
    .quiz-card a:hover { background: #1d4ed8; }
  </style>
</head>
<body>
  <h1>[Subject Code] – Quiz Collection</h1>
  
  <div class="quiz-list">
    <div class="quiz-card">
      <h3>Topic 1: [Topic Name]</h3>
      <p>Description of quiz content</p>
      <a href="subject_topic_1_quiz.html">Start Quiz</a>
    </div>
    
    <div class="quiz-card">
      <h3>Topic 2: [Topic Name]</h3>
      <p>Description of quiz content</p>
      <a href="subject_topic_2_quiz.html">Start Quiz</a>
    </div>
    
    <!-- Add more cards as needed -->
  </div>
</body>
</html>
```

---

## Step-by-Step: Creating a New Quiz

### Step 1: Copy the Template
- Copy the complete HTML template above
- Save as `[topic]_[number]_quiz.html`

### Step 2: Update Metadata
- Change `<title>` tag
- Update header `<h1>` and `.sub` elements
- Modify footer source reference

### Step 3: Add Questions
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

### Step 4: Customize Colors (Optional)
- Adjust `--accent` color in CSS `:root`
- Modify other styling as needed

### Step 5: Test
- Open in browser
- Answer questions to verify logic
- Test "Shuffle", "Reset", and "Reveal all" buttons
- Check on mobile devices

### Step 6: Link from Index
- Add quiz card to your index.html
- Verify link works correctly

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

### Before Publishing
- [ ] All questions have 4 options
- [ ] `correct` indices are accurate (0-3)
- [ ] Explanations are clear and helpful
- [ ] Header information updated
- [ ] Footer sources cited
- [ ] Color scheme appropriate
- [ ] Tested in multiple browsers
- [ ] Mobile responsive verified
- [ ] No spelling/grammar errors
- [ ] Links work in index page

### Quality Standards
- [ ] 15-30 questions per quiz (sweet spot)
- [ ] Mix of difficulty levels
- [ ] Explanations reference source material
- [ ] Distractors (wrong answers) are plausible
- [ ] Questions avoid "all of the above" / "none of the above"
- [ ] Clear, unambiguous phrasing
- [ ] Consistent formatting

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

**Template Creator:** Jk
**Version:** 1.0  
**Last Updated:** February 2024  
**GitHub:**  https://github.com/jacktan-jk

---

## Summary

This template provides everything needed to create professional, interactive MCQ quizzes for any subject. Key advantages:

✨ **Zero dependencies** – No frameworks, libraries, or build tools  
🎨 **Fully customizable** – Easy to modify colors, layout, and behavior  
📱 **Responsive** – Works on all devices  
♿ **Accessible** – Keyboard navigation and semantic HTML  
🔒 **Privacy-focused** – No tracking, no external requests  
📦 **Portable** – Single HTML file per quiz

Start by copying the template, adding your questions, and customizing to match your subject and branding. Happy quiz creating! 🚀
