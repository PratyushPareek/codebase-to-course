# Interactive Elements Reference

Implementation patterns for every interactive element type used in courses. Pick the elements that best serve each module's teaching goal.

## Table of Contents
1. [Interface Snippets](#interface-snippets)
2. [Multiple-Choice Quizzes](#multiple-choice-quizzes)
3. [Drag-and-Drop Matching](#drag-and-drop-matching)
4. [Group Chat Animation](#group-chat-animation)
5. [Message Flow / Data Flow Animation](#message-flow--data-flow-animation)
6. [Interactive Architecture Diagram](#interactive-architecture-diagram)
7. [Layer Toggle Demo](#layer-toggle-demo)
8. ["Spot the Design Flaw" Challenge](#spot-the-design-flaw-challenge)
9. [Scenario Quiz](#scenario-quiz)
10. [Callout Boxes](#callout-boxes)
11. [Pattern/Feature Cards](#patternfeature-cards)
12. [Flow Diagrams](#flow-diagrams)
13. [Permission/Config Badges](#permissionconfig-badges)
14. [Glossary Tooltips](#glossary-tooltips)
15. [Visual File Tree](#visual-file-tree)
16. [Icon-Label Rows](#icon-label-rows)
17. [Numbered Step Cards](#numbered-step-cards)

---

## Interface Snippets

Short, readable code blocks (5-15 lines) that show the *shape* of a component: interfaces, type definitions, config objects, or key function signatures. These make architecture concrete without being a line-by-line walkthrough. Use them when explaining what a component accepts, returns, or exposes.

**HTML:**
```html
<div class="interface-snippet animate-in">
  <div class="snippet-header">
    <span class="snippet-label">CONTRACT</span>
    <span class="snippet-file">src/types/plugin.ts</span>
  </div>
  <pre class="snippet-code"><code>
<span class="code-keyword">interface</span> <span class="code-function">Plugin</span> {
  <span class="code-property">name</span>: <span class="code-keyword">string</span>;
  <span class="code-property">version</span>: <span class="code-keyword">string</span>;
  <span class="code-function">initialize</span>(<span class="code-property">config</span>: <span class="code-function">PluginConfig</span>): <span class="code-keyword">void</span>;
  <span class="code-function">execute</span>(<span class="code-property">context</span>: <span class="code-function">RunContext</span>): <span class="code-function">Promise</span>&lt;<span class="code-function">Result</span>&gt;;
  <span class="code-function">teardown</span>?(): <span class="code-keyword">void</span>;
}
  </code></pre>
  <p class="snippet-caption">This is the contract every plugin must implement. The runner calls <code>initialize</code> once, then <code>execute</code> for each job.</p>
</div>
```

**CSS:**
```css
.interface-snippet {
  border-radius: var(--radius-md);
  overflow: hidden;
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}
.snippet-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #181825;
  padding: var(--space-2) var(--space-4);
  border-bottom: 1px solid rgba(255,255,255,0.06);
}
.snippet-label {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--color-accent-muted);
}
.snippet-file {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  color: #6C7086;
}
.snippet-code {
  background: var(--color-bg-code);
  color: #CDD6F4;
  padding: var(--space-4) var(--space-6);
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  line-height: 1.7;
  margin: 0;
  white-space: pre-wrap;
  word-break: break-word;
  overflow-x: hidden;
}
.snippet-caption {
  background: var(--color-surface-warm);
  padding: var(--space-3) var(--space-5);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  border-top: 3px solid var(--color-accent);
  margin: 0;
}
.snippet-caption code {
  background: rgba(0,0,0,0.06);
  padding: 1px 5px;
  border-radius: 3px;
  font-family: var(--font-mono);
  font-size: 0.9em;
}
```

**Rules:**
- Show original code exactly as-is — never modify or simplify
- Choose naturally short, self-contained definitions (interfaces, types, configs, signatures)
- Don't show function bodies or implementation details — only contracts and shapes
- Always include the file path in the header so the learner can find it
- The caption should explain *why* this snippet matters, not repeat what the code says
- If a snippet isn't readable at a glance (≤15 lines), use a diagram or prose instead

---

## Multiple-Choice Quizzes

For testing understanding with instant feedback. Each question has options, one correct answer, and per-question explanations.

**HTML:**
```html
<div class="quiz-container">
  <div class="quiz-question-block" data-question="q1" data-correct="option-b">
    <h3 class="quiz-question">Question text here?</h3>
    <div class="quiz-options">
      <button class="quiz-option" data-value="option-a" onclick="selectOption(this)">
        <div class="quiz-option-radio"></div>
        <span>Answer A</span>
      </button>
      <button class="quiz-option" data-value="option-b" onclick="selectOption(this)">
        <div class="quiz-option-radio"></div>
        <span>Answer B (correct)</span>
      </button>
      <button class="quiz-option" data-value="option-c" onclick="selectOption(this)">
        <div class="quiz-option-radio"></div>
        <span>Answer C</span>
      </button>
    </div>
    <div class="quiz-feedback" id="q1-feedback"></div>
  </div>

  <button class="quiz-check-btn" onclick="checkQuiz('section-id')">Check Answers</button>
  <button class="quiz-reset-btn" onclick="resetQuiz('section-id')">Try Again</button>
  <div class="quiz-overall-feedback" id="section-overall"></div>
</div>
```

**JS pattern:**
```javascript
window.selectOption = function(btn) {
  // Deselect siblings
  const block = btn.closest('.quiz-question-block');
  block.querySelectorAll('.quiz-option').forEach(o => o.classList.remove('selected'));
  btn.classList.add('selected');
};

window.checkQuiz = function(sectionId) {
  const container = document.querySelector(`#${sectionId} .quiz-container`);
  const questions = container.querySelectorAll('.quiz-question-block');
  let correct = 0;

  questions.forEach(q => {
    const selected = q.querySelector('.quiz-option.selected');
    const feedback = q.querySelector('.quiz-feedback');
    const correctValue = q.dataset.correct;

    if (!selected) {
      feedback.textContent = 'Pick an answer first!';
      feedback.className = 'quiz-feedback show warning';
      return;
    }

    if (selected.dataset.value === correctValue) {
      correct++;
      selected.classList.add('correct');
      feedback.innerHTML = '<strong>Exactly!</strong> ' + getExplanation(q, true);
      feedback.className = 'quiz-feedback show success';
    } else {
      selected.classList.add('incorrect');
      // Highlight the correct one
      q.querySelector(`[data-value="${correctValue}"]`).classList.add('correct');
      feedback.innerHTML = '<strong>Not quite.</strong> ' + getExplanation(q, false);
      feedback.className = 'quiz-feedback show error';
    }

    // Disable further interaction
    q.querySelectorAll('.quiz-option').forEach(o => o.disabled = true);
  });
};
```

**CSS for quiz states:**
```css
.quiz-option {
  display: flex; align-items: center; gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-surface);
  cursor: pointer; width: 100%;
  transition: border-color var(--duration-fast), background var(--duration-fast);
}
.quiz-option:hover { border-color: var(--color-accent-muted); }
.quiz-option.selected { border-color: var(--color-accent); background: var(--color-accent-light); }
.quiz-option.correct { border-color: var(--color-success); background: var(--color-success-light); }
.quiz-option.incorrect { border-color: var(--color-error); background: var(--color-error-light); }
.quiz-option-radio {
  width: 18px; height: 18px; border-radius: 50%;
  border: 2px solid var(--color-border);
  transition: all var(--duration-fast);
}
.quiz-option.selected .quiz-option-radio {
  border-color: var(--color-accent);
  background: var(--color-accent);
  box-shadow: inset 0 0 0 3px white;
}
.quiz-feedback {
  max-height: 0; overflow: hidden; opacity: 0;
  transition: max-height var(--duration-normal), opacity var(--duration-normal);
}
.quiz-feedback.show { max-height: 200px; opacity: 1; padding: var(--space-3); margin-top: var(--space-2); border-radius: var(--radius-sm); }
.quiz-feedback.success { background: var(--color-success-light); color: var(--color-success); }
.quiz-feedback.error { background: var(--color-error-light); color: var(--color-error); }
```

---

## Drag-and-Drop Matching

For matching concepts to descriptions. Supports both mouse (HTML5 Drag API) and touch.

**HTML:**
```html
<div class="dnd-container">
  <div class="dnd-chips">
    <div class="dnd-chip" draggable="true" data-answer="actor-a">Actor A</div>
    <div class="dnd-chip" draggable="true" data-answer="actor-b">Actor B</div>
    <div class="dnd-chip" draggable="true" data-answer="actor-c">Actor C</div>
  </div>
  <div class="dnd-zones">
    <div class="dnd-zone" data-correct="actor-a">
      <p class="dnd-zone-label">Description for Actor A</p>
      <div class="dnd-zone-target">Drop here</div>
    </div>
    <!-- more zones -->
  </div>
  <button onclick="checkDnD()">Check Matches</button>
  <button onclick="resetDnD()">Reset</button>
</div>
```

**JS (mouse + touch):**
```javascript
// MOUSE: HTML5 Drag API
chips.forEach(chip => {
  chip.addEventListener('dragstart', (e) => {
    e.dataTransfer.setData('text/plain', chip.dataset.answer);
    chip.classList.add('dragging');
  });
  chip.addEventListener('dragend', () => chip.classList.remove('dragging'));
});

zones.forEach(zone => {
  const target = zone.querySelector('.dnd-zone-target');
  target.addEventListener('dragover', (e) => { e.preventDefault(); target.classList.add('drag-over'); });
  target.addEventListener('dragleave', () => target.classList.remove('drag-over'));
  target.addEventListener('drop', (e) => {
    e.preventDefault();
    target.classList.remove('drag-over');
    const answer = e.dataTransfer.getData('text/plain');
    const chip = document.querySelector(`[data-answer="${answer}"]`);
    target.textContent = chip.textContent;
    target.dataset.placed = answer;
    chip.classList.add('placed');
  });
});

// TOUCH: Custom implementation (HTML5 drag doesn't work on mobile)
chips.forEach(chip => {
  chip.addEventListener('touchstart', (e) => {
    e.preventDefault();
    const touch = e.touches[0];
    const clone = chip.cloneNode(true);
    clone.classList.add('touch-ghost');
    clone.style.cssText = `position:fixed; z-index:1000; pointer-events:none;
      left:${touch.clientX - 40}px; top:${touch.clientY - 20}px;`;
    document.body.appendChild(clone);
    chip._ghost = clone;
    chip._answer = chip.dataset.answer;
  }, { passive: false });

  chip.addEventListener('touchmove', (e) => {
    e.preventDefault();
    const touch = e.touches[0];
    if (chip._ghost) {
      chip._ghost.style.left = (touch.clientX - 40) + 'px';
      chip._ghost.style.top = (touch.clientY - 20) + 'px';
    }
    // Highlight zone under finger
    const el = document.elementFromPoint(touch.clientX, touch.clientY);
    zones.forEach(z => z.querySelector('.dnd-zone-target').classList.remove('drag-over'));
    if (el && el.closest('.dnd-zone-target')) {
      el.closest('.dnd-zone-target').classList.add('drag-over');
    }
  }, { passive: false });

  chip.addEventListener('touchend', (e) => {
    if (chip._ghost) { chip._ghost.remove(); chip._ghost = null; }
    const touch = e.changedTouches[0];
    const el = document.elementFromPoint(touch.clientX, touch.clientY);
    if (el && el.closest('.dnd-zone-target')) {
      const target = el.closest('.dnd-zone-target');
      target.textContent = chip.textContent;
      target.dataset.placed = chip._answer;
      chip.classList.add('placed');
    }
  });
});
```

---

## Group Chat Animation

iMessage/WeChat-style chat showing components "talking" to each other. Messages appear one by one with typing indicators.

**HTML:**
```html
<div class="chat-window">
  <div class="chat-messages" id="chat-messages">
    <div class="chat-message" data-msg="0" data-sender="actor-a" style="display:none">
      <div class="chat-avatar" style="background: var(--color-actor-1)">A</div>
      <div class="chat-bubble">
        <span class="chat-sender" style="color: var(--color-actor-1)">Actor A</span>
        <p>Hey Background, I need the data for this item.</p>
      </div>
    </div>
    <!-- more messages... -->
  </div>

  <div class="chat-typing" id="chat-typing" style="display:none">
    <div class="chat-avatar" id="typing-avatar">?</div>
    <div class="chat-typing-dots">
      <span class="typing-dot"></span>
      <span class="typing-dot"></span>
      <span class="typing-dot"></span>
    </div>
  </div>

  <div class="chat-controls">
    <button onclick="playChatNext()">Next Message</button>
    <button onclick="playChatAll()">Play All</button>
    <button onclick="resetChat()">Replay</button>
    <span class="chat-progress">0 / N messages</span>
  </div>
</div>
```

**JS:**
```javascript
let chatIndex = 0;
const chatMessages = document.querySelectorAll('#chat-messages .chat-message');

// Actor color/avatar mapping
const actors = {
  'actor-a': { initials: 'A', color: 'var(--color-actor-1)' },
  'actor-b': { initials: 'B', color: 'var(--color-actor-2)' },
  'actor-c': { initials: 'C', color: 'var(--color-actor-3)' },
};

window.playChatNext = function() {
  if (chatIndex >= chatMessages.length) return;
  const msg = chatMessages[chatIndex];
  const sender = msg.dataset.sender;

  // Show typing indicator with correct avatar
  const typing = document.getElementById('chat-typing');
  const avatar = document.getElementById('typing-avatar');
  avatar.textContent = actors[sender].initials;
  avatar.style.background = actors[sender].color;
  typing.style.display = 'flex';

  setTimeout(() => {
    typing.style.display = 'none';
    msg.style.display = 'flex';
    msg.style.animation = 'fadeSlideUp 0.3s var(--ease-out)';
    chatIndex++;
    updateChatProgress();
  }, 800);
};

window.playChatAll = function() {
  const interval = setInterval(() => {
    if (chatIndex >= chatMessages.length) { clearInterval(interval); return; }
    playChatNext();
  }, 1200);
};
```

**CSS for typing dots:**
```css
.typing-dot {
  width: 8px; height: 8px; border-radius: 50%;
  background: var(--color-text-muted);
  animation: typingBounce 1.4s infinite;
}
.typing-dot:nth-child(2) { animation-delay: 0.2s; }
.typing-dot:nth-child(3) { animation-delay: 0.4s; }
@keyframes typingBounce {
  0%, 60%, 100% { transform: translateY(0); }
  30% { transform: translateY(-6px); }
}
```

---

## Message Flow / Data Flow Animation

Step-by-step visualization of data moving between components. User clicks "Next Step" to advance.

**HTML:**
```html
<div class="flow-animation">
  <div class="flow-actors">
    <div class="flow-actor" id="flow-actor-1">
      <div class="flow-actor-icon">A</div>
      <span>Actor 1</span>
    </div>
    <div class="flow-actor" id="flow-actor-2">
      <div class="flow-actor-icon">B</div>
      <span>Actor 2</span>
    </div>
    <div class="flow-actor" id="flow-actor-3">
      <div class="flow-actor-icon">C</div>
      <span>Actor 3</span>
    </div>
  </div>

  <div class="flow-packet" id="flow-packet"></div>

  <div class="flow-step-label" id="flow-label">Click "Next Step" to begin</div>

  <div class="flow-controls">
    <button onclick="flowNext()">Next Step</button>
    <button onclick="flowReset()">Restart</button>
    <span class="flow-progress">Step 0 / N</span>
  </div>
</div>
```

**JS pattern:**
```javascript
const flowSteps = [
  { from: 'actor-1', to: 'actor-2', label: 'User clicks button → Actor 1 detects click event', highlight: 'actor-1' },
  { from: 'actor-1', to: 'actor-2', label: 'Actor 1 sends message to Actor 2', highlight: 'actor-2', packet: true },
  { from: 'actor-2', to: 'external', label: 'Actor 2 calls external API', highlight: 'actor-2', cloud: true },
  // etc.
];

let flowStep = 0;
window.flowNext = function() {
  if (flowStep >= flowSteps.length) return;
  const step = flowSteps[flowStep];

  // Remove previous highlights
  document.querySelectorAll('.flow-actor').forEach(a => a.classList.remove('active'));

  // Highlight current actor
  document.getElementById(`flow-${step.highlight}`).classList.add('active');

  // Animate packet if needed
  if (step.packet) {
    animatePacket(step.from, step.to);
  }

  // Update label
  document.getElementById('flow-label').textContent = step.label;
  flowStep++;
};
```

**CSS for active actor glow:**
```css
.flow-actor.active {
  box-shadow: 0 0 0 3px var(--color-accent), 0 0 20px rgba(217, 79, 48, 0.2);
  transform: scale(1.05);
  transition: all var(--duration-normal) var(--ease-out);
}
```

---

## Interactive Architecture Diagram

A powerful orientation visual for the course. Shows the full system at a glance — components grouped by zone (client, server, data layer, external services, etc.), with click/hover to reveal what each piece does. Strongly encouraged when the project has multiple distinct layers or services. Place early (modules 1-2) so the learner has a mental map before diving into details. Skip only if the project is small/flat enough that a simple list or flow diagram covers the structure.

**Design principles:**
- **Group by zone/layer** — visually cluster components that live together (e.g., "Browser," "API Server," "Database," "External Services"). Use bordered regions with subtle background tints.
- **Use icons or emoji** — each component gets a visual icon that conveys its role at a glance.
- **Show connections** — draw arrows or lines between components that communicate. Use CSS borders, pseudo-elements, or SVG paths. Label the connections where helpful (e.g., "REST API," "WebSocket," "SQL queries").
- **Click to reveal** — clicking a component highlights it and shows a 1-2 sentence description below the diagram. Only one description visible at a time.
- **Responsive** — on mobile, zones should stack vertically rather than sit side-by-side.

**HTML:**
```html
<div class="arch-diagram">
  <div class="arch-zones">
    <div class="arch-zone" style="--zone-color: var(--color-actor-1)">
      <h4 class="arch-zone-label">Browser</h4>
      <div class="arch-components">
        <button class="arch-component" data-desc="Renders the UI and handles user interactions. Built with React." onclick="showArchDesc(this)">
          <div class="arch-icon">🖼️</div>
          <span>Frontend UI</span>
        </button>
        <button class="arch-component" data-desc="Manages client-side state and caches API responses for performance." onclick="showArchDesc(this)">
          <div class="arch-icon">📦</div>
          <span>State Store</span>
        </button>
      </div>
    </div>

    <div class="arch-connector">
      <span class="arch-connector-label">REST API</span>
    </div>

    <div class="arch-zone" style="--zone-color: var(--color-actor-2)">
      <h4 class="arch-zone-label">Server</h4>
      <div class="arch-components">
        <button class="arch-component" data-desc="Handles incoming HTTP requests, validates input, and routes to the right handler." onclick="showArchDesc(this)">
          <div class="arch-icon">🛠️</div>
          <span>API Layer</span>
        </button>
        <button class="arch-component" data-desc="Core business logic — processes data, enforces rules, coordinates between services." onclick="showArchDesc(this)">
          <div class="arch-icon">⚙️</div>
          <span>Service Layer</span>
        </button>
      </div>
    </div>

    <div class="arch-connector">
      <span class="arch-connector-label">SQL Queries</span>
    </div>

    <div class="arch-zone" style="--zone-color: var(--color-actor-3)">
      <h4 class="arch-zone-label">Data</h4>
      <div class="arch-components">
        <button class="arch-component" data-desc="PostgreSQL database storing all persistent application data." onclick="showArchDesc(this)">
          <div class="arch-icon">🗄️</div>
          <span>Database</span>
        </button>
      </div>
    </div>

    <div class="arch-zone" style="--zone-color: var(--color-actor-4)">
      <h4 class="arch-zone-label">External Services</h4>
      <div class="arch-components">
        <button class="arch-component" data-desc="Third-party API for sending transactional emails (welcome, password reset, etc.)" onclick="showArchDesc(this)">
          <div class="arch-icon">📧</div>
          <span>Email Service</span>
        </button>
        <button class="arch-component" data-desc="Cloud object storage for user-uploaded files and generated assets." onclick="showArchDesc(this)">
          <div class="arch-icon">☁️</div>
          <span>File Storage</span>
        </button>
      </div>
    </div>
  </div>

  <div class="arch-description" id="arch-desc">
    <p>👆 Click any component to learn what it does</p>
  </div>
</div>
```

**CSS:**
```css
.arch-diagram {
  margin: var(--space-8) 0;
}
.arch-zones {
  display: flex;
  flex-direction: column;
  gap: var(--space-4);
  align-items: center;
}
.arch-zone {
  background: var(--color-surface);
  border: 2px solid var(--zone-color, var(--color-border));
  border-radius: var(--radius-md);
  padding: var(--space-4) var(--space-6);
  width: 100%;
  max-width: var(--content-width);
}
.arch-zone-label {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--zone-color, var(--color-text-muted));
  margin-bottom: var(--space-3);
}
.arch-components {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-3);
}
.arch-component {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-2) var(--space-4);
  background: var(--color-surface-warm);
  border: 2px solid var(--color-border-light);
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: all var(--duration-fast) var(--ease-out);
  font-family: var(--font-body);
  font-size: var(--text-sm);
}
.arch-component:hover {
  border-color: var(--color-accent-muted);
  box-shadow: var(--shadow-sm);
}
.arch-component.active {
  border-color: var(--color-accent);
  background: var(--color-accent-light);
  box-shadow: 0 0 0 3px var(--color-accent-light), var(--shadow-md);
}
.arch-icon {
  font-size: 1.2em;
}
.arch-connector {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  color: var(--color-text-muted);
}
.arch-connector::before {
  content: '';
  width: 2px;
  height: 16px;
  background: var(--color-border);
}
.arch-connector::after {
  content: '▼';
  font-size: var(--text-xs);
  color: var(--color-border);
  margin-top: -4px;
}
.arch-connector-label {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  margin-top: var(--space-1);
}
.arch-description {
  margin-top: var(--space-4);
  padding: var(--space-4) var(--space-5);
  background: var(--color-surface);
  border-left: 3px solid var(--color-accent);
  border-radius: var(--radius-sm);
  font-size: var(--text-sm);
  min-height: 48px;
  transition: opacity var(--duration-fast);
}

@media (max-width: 480px) {
  .arch-components { flex-direction: column; }
  .arch-component { width: 100%; }
}
```

**JS:**
```javascript
window.showArchDesc = function(el) {
  // Deactivate all components
  document.querySelectorAll('.arch-component').forEach(c => c.classList.remove('active'));
  // Activate clicked component
  el.classList.add('active');
  // Show description
  const desc = document.getElementById('arch-desc');
  desc.innerHTML = '<p>' + el.dataset.desc + '</p>';
  desc.style.borderLeftColor = getComputedStyle(el.closest('.arch-zone')).getPropertyValue('--zone-color') || 'var(--color-accent)';
};
```

**Adapting the diagram to different project types:**
- **Web app** — zones like Browser, API Server, Database, External Services
- **Library/framework** — zones like Public API, Core Engine, Plugins/Extensions, Internal Utilities
- **CLI tool** — zones like CLI Parser, Command Handlers, Core Logic, I/O Layer
- **Data pipeline** — zones like Ingestion, Transform, Storage, Orchestration
- **ML project** — zones like Data Loading, Model Architecture, Training Loop, Inference API

Always adapt zones and component names to match the actual project structure. The example above is a template — replace with real components from the codebase.

---

## Layer Toggle Demo

Shows how different layers of a system build on each other. Use tabs or buttons to switch between views, revealing how each layer adds to the one below it. Adapt the layers to whatever the project uses — this is NOT limited to HTML/CSS/JS.

**Example layer sets by project type:**
- **Web app** — HTML → + CSS → + JS (visual rendering layers)
- **Data pipeline** — Raw Data → Validated → Enriched → Aggregated
- **Library** — Public API → Internal Logic → Storage/IO
- **API server** — Request → Middleware → Handler → Database
- **ML project** — Raw Input → Preprocessed → Model Output → Post-processed

**HTML:**
```html
<div class="layer-demo">
  <div class="layer-tabs">
    <button class="layer-tab active" onclick="showLayer('layer-1')">Layer 1</button>
    <button class="layer-tab" onclick="showLayer('layer-2')">+ Layer 2</button>
    <button class="layer-tab" onclick="showLayer('layer-3')">+ Layer 3</button>
  </div>
  <div class="layer-viewport">
    <div class="layer" id="layer-1" style="display:block">
      <!-- First/base layer view -->
    </div>
    <div class="layer" id="layer-2" style="display:none">
      <!-- Second layer added -->
    </div>
    <div class="layer" id="layer-3" style="display:none">
      <!-- Third layer added -->
    </div>
  </div>
  <p class="layer-description" id="layer-desc">This is the base layer...</p>
</div>
```

**JS:**
```javascript
window.showLayer = function(layerId) {
  // Hide all layers
  document.querySelectorAll('.layer-demo .layer').forEach(l => l.style.display = 'none');
  // Show selected
  document.getElementById(layerId).style.display = 'block';
  // Update active tab
  document.querySelectorAll('.layer-tab').forEach(t => t.classList.remove('active'));
  event.target.classList.add('active');
  // Update description
  const descriptions = {
    'layer-1': 'The base layer — what you start with...',
    'layer-2': 'Adding the second layer transforms...',
    'layer-3': 'The final layer brings everything together...'
  };
  document.getElementById('layer-desc').textContent = descriptions[layerId] || '';
};
```

**CSS:**
```css
.layer-demo {
  border-radius: var(--radius-md);
  overflow: hidden;
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}
.layer-tabs {
  display: flex;
  background: var(--color-surface);
  border-bottom: 2px solid var(--color-border-light);
}
.layer-tab {
  flex: 1;
  padding: var(--space-3) var(--space-4);
  border: none;
  background: transparent;
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  cursor: pointer;
  color: var(--color-text-secondary);
  transition: all var(--duration-fast);
}
.layer-tab.active {
  color: var(--color-accent);
  border-bottom: 2px solid var(--color-accent);
  font-weight: 600;
}
.layer-viewport {
  min-height: 200px;
  padding: var(--space-6);
  background: var(--color-surface-warm);
}
.layer-description {
  padding: var(--space-3) var(--space-5);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  border-top: 1px solid var(--color-border-light);
  margin: 0;
}
```

**Rules:**
- Each tab should show a cumulative view (layer 2 includes layer 1, etc.) so the learner sees what each layer *adds*
- Label tabs with the project-specific layer names, not generic "Layer 1/2/3"
- The description below should explain what changed and why this layer matters

---

## "Spot the Design Flaw" Challenge

Present an architectural decision, configuration, or code pattern with a subtle design flaw. The learner clicks the problematic element. The reveal explains the issue and the better approach. This tests architectural understanding, not syntax knowledge.

**Types of flaws to present:**
- **Architecture flaws** — a component doing too much, wrong layer handling a responsibility, circular dependency
- **Configuration mistakes** — missing timeout, no retry policy, hardcoded secrets, missing index
- **Pattern misuse** — synchronous call where async is needed, polling where a webhook would work, caching without invalidation
- **Scaling issues** — N+1 query, unbounded queue, missing rate limit

**HTML:**
```html
<div class="flaw-challenge">
  <h3>Something's off in this design. Can you spot it?</h3>
  <div class="flaw-options">
    <button class="flaw-option" data-correct="false" onclick="checkFlaw(this)">
      <div class="flaw-icon">📨</div>
      <div>
        <strong>API Gateway</strong>
        <p>Routes requests to the correct service</p>
      </div>
    </button>
    <button class="flaw-option" data-correct="true" onclick="checkFlaw(this)">
      <div class="flaw-icon">🗄️</div>
      <div>
        <strong>User Service queries Order DB directly</strong>
        <p>Fetches order history by calling the orders table</p>
      </div>
    </button>
    <button class="flaw-option" data-correct="false" onclick="checkFlaw(this)">
      <div class="flaw-icon">🔔</div>
      <div>
        <strong>Notification Service</strong>
        <p>Sends emails via external provider</p>
      </div>
    </button>
  </div>
  <div class="flaw-feedback" id="flaw-feedback"></div>
</div>
```

**JS:**
```javascript
window.checkFlaw = function(el) {
  const feedback = el.closest('.flaw-challenge').querySelector('.flaw-feedback');
  const isCorrect = el.dataset.correct === 'true';
  
  if (isCorrect) {
    el.classList.add('correct');
    feedback.innerHTML = '<strong>Good catch!</strong> The User Service is reaching directly into the Order Service\'s database instead of going through its API. This creates tight coupling — if the orders table schema changes, the User Service breaks. The fix: User Service should call the Order Service\'s API instead.';
    feedback.className = 'flaw-feedback show success';
  } else {
    el.classList.add('incorrect');
    feedback.innerHTML = 'This component looks fine — think about which service is crossing a boundary it shouldn\'t...';
    feedback.className = 'flaw-feedback show error';
    setTimeout(() => { el.classList.remove('incorrect'); feedback.className = 'flaw-feedback'; }, 2500);
  }
};
```

**CSS:**
```css
.flaw-options {
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
  margin: var(--space-4) 0;
}
.flaw-option {
  display: flex;
  align-items: center;
  gap: var(--space-4);
  padding: var(--space-4) var(--space-5);
  background: var(--color-surface);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-md);
  cursor: pointer;
  text-align: left;
  transition: all var(--duration-fast);
  font-family: var(--font-body);
}
.flaw-option:hover { border-color: var(--color-accent-muted); }
.flaw-option.correct { border-color: var(--color-success); background: var(--color-success-light); }
.flaw-option.incorrect { border-color: var(--color-error); background: var(--color-error-light); }
.flaw-icon { font-size: 1.5rem; }
.flaw-option p { margin: var(--space-1) 0 0; font-size: var(--text-sm); color: var(--color-text-secondary); }
.flaw-feedback {
  max-height: 0; overflow: hidden; opacity: 0;
  transition: max-height var(--duration-normal), opacity var(--duration-normal);
}
.flaw-feedback.show { max-height: 200px; opacity: 1; padding: var(--space-4); margin-top: var(--space-3); border-radius: var(--radius-sm); }
.flaw-feedback.success { background: var(--color-success-light); color: var(--color-success); }
.flaw-feedback.error { background: var(--color-error-light); color: var(--color-error); }
```
```

---

## Scenario Quiz

"What would a senior engineer do?" — situational questions with explanations.

Same HTML/CSS/JS pattern as Multiple-Choice Quizzes, but with longer scenario descriptions and more detailed explanations. Wrap each question in a scenario context block:

```html
<div class="scenario-block">
  <div class="scenario-context">
    <span class="scenario-label">Scenario</span>
    <p>Your app processes a 3-hour podcast transcript. The API has a 16,000 token limit. What do you do?</p>
  </div>
  <!-- quiz-options here -->
</div>
```

---

## Callout Boxes

"Aha!" moments — universal CS insights. Max 2 per module.

```html
<div class="callout callout-accent">
  <div class="callout-icon">💡</div>
  <div class="callout-content">
    <strong class="callout-title">Key Insight</strong>
    <p>This pattern — splitting responsibilities into focused roles — is one of the most important ideas in software engineering. Engineers call it "separation of concerns."</p>
  </div>
</div>
```

**Variants:**
- `callout-accent`: vermillion left border, light accent background (for CS insights)
- `callout-info`: teal left border, light info background (for "good to know")
- `callout-warning`: red left border, light error background (for common mistakes)

---

## Pattern/Feature Cards

Grid of cards highlighting engineering patterns, tech stack components, or key concepts.

```html
<div class="pattern-cards">
  <div class="pattern-card" style="border-top: 3px solid var(--color-actor-1)">
    <div class="pattern-icon" style="background: var(--color-actor-1)">🔄</div>
    <h4 class="pattern-title">Caching</h4>
    <p class="pattern-desc">Store results to avoid redundant work — like keeping leftovers instead of cooking a new meal every time.</p>
  </div>
  <!-- more cards -->
</div>
```

```css
.pattern-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: var(--space-4);
}
.pattern-card {
  background: var(--color-surface);
  border-radius: var(--radius-md);
  padding: var(--space-6);
  box-shadow: var(--shadow-sm);
  transition: transform var(--duration-normal) var(--ease-out), box-shadow var(--duration-normal);
}
.pattern-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-md);
}
```

---

## Flow Diagrams

**Horizontal flow (desktop):**
```html
<div class="flow-steps">
  <div class="flow-step">
    <div class="flow-step-num">1</div>
    <p>User clicks button</p>
  </div>
  <div class="flow-arrow">→</div>
  <div class="flow-step">
    <div class="flow-step-num">2</div>
    <p>Component A detects click</p>
  </div>
  <div class="flow-arrow">→</div>
  <!-- more steps -->
</div>
```

Arrows rotate to `↓` on mobile via CSS transform.

---

## Permission/Config Badges

For annotating config files, permissions, or settings:

```html
<div class="badge-list">
  <div class="badge-item">
    <code class="badge-code">storage</code>
    <span class="badge-desc">Save data between sessions (like browser bookmarks)</span>
  </div>
  <div class="badge-item">
    <code class="badge-code">activeTab</code>
    <span class="badge-desc">Access the currently open tab (only when the user clicks)</span>
  </div>
</div>
```

```css
.badge-item {
  display: flex; align-items: center; gap: var(--space-4);
  padding: var(--space-3) var(--space-4);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-sm);
  transition: border-color var(--duration-fast);
}
.badge-item:hover { border-color: var(--color-accent-muted); }
.badge-code {
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  background: var(--color-bg-code);
  color: #CBA6F7;
  padding: var(--space-1) var(--space-3);
  border-radius: var(--radius-sm);
  white-space: nowrap;
}
```

---

## Glossary Tooltips

The most important accessibility feature for non-technical learners. Any technical term in the course text should be wrapped in a tooltip that shows a plain-English definition on hover (desktop) or tap (mobile). The learner never has to leave the page or Google anything.

**HTML — mark up terms inline:**
```html
<p>The extension uses a
  <span class="term" data-definition="A service worker is a background script that runs independently of the web page — like a behind-the-scenes assistant that's always on, even when you're not looking at the page.">service worker</span>
  to handle API calls.
</p>
```

**CSS:**
```css
.term {
  border-bottom: 1.5px dashed var(--color-accent-muted);
  cursor: pointer;    /* NOT cursor: help — pointer feels clickable and inviting */
  position: relative;
}
.term:hover, .term.active {
  border-bottom-color: var(--color-accent);
  color: var(--color-accent);
}

/* The tooltip bubble — uses position: fixed and is appended to document.body
   via JS so it is NEVER clipped by ancestor overflow: hidden containers
   (like translation blocks). See JS section below for positioning logic. */
.term-tooltip {
  position: fixed;        /* CRITICAL: fixed, not absolute — prevents clipping */
  background: var(--color-bg-code);
  color: #CDD6F4;
  padding: var(--space-3) var(--space-4);
  border-radius: var(--radius-sm);
  font-size: var(--text-sm);
  font-family: var(--font-body);
  line-height: var(--leading-normal);
  width: max(200px, min(320px, 80vw));
  box-shadow: var(--shadow-lg);
  pointer-events: none;
  opacity: 0;
  transition: opacity var(--duration-fast);
  z-index: 10000;        /* Above everything, including nav */
}
/* Arrow pointing down */
.term-tooltip::after {
  content: '';
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border: 6px solid transparent;
  border-top-color: var(--color-bg-code);
}
.term-tooltip.visible {
  opacity: 1;
}

/* If tooltip goes off-screen top, flip to below */
.term-tooltip.flip {
  bottom: auto;
  top: calc(100% + 8px);
}
.term-tooltip.flip::after {
  top: auto;
  bottom: 100%;
  border-top-color: transparent;
  border-bottom-color: var(--color-bg-code);
}
```

**JS — position: fixed tooltips appended to body (never clipped by overflow):**
```javascript
// Tooltip container — appended to body so it's never clipped
let activeTooltip = null;

function positionTooltip(term, tip) {
  const rect = term.getBoundingClientRect();
  const tipWidth = 300; // approximate
  let left = rect.left + rect.width / 2 - tipWidth / 2;
  // Clamp to viewport
  left = Math.max(8, Math.min(left, window.innerWidth - tipWidth - 8));

  // Try above first
  let top = rect.top - 8;
  tip.style.left = left + 'px';

  // Position above by default, flip below if no room
  document.body.appendChild(tip);
  const tipHeight = tip.offsetHeight;
  if (rect.top - tipHeight - 8 < 0) {
    // Flip below
    tip.style.top = (rect.bottom + 8) + 'px';
    tip.classList.add('flip');
  } else {
    tip.style.top = (rect.top - tipHeight - 8) + 'px';
    tip.classList.remove('flip');
  }
}

document.querySelectorAll('.term').forEach(term => {
  const tip = document.createElement('span');
  tip.className = 'term-tooltip';
  tip.textContent = term.dataset.definition;

  // Hover for desktop
  term.addEventListener('mouseenter', () => {
    if (activeTooltip && activeTooltip !== tip) {
      activeTooltip.classList.remove('visible');
      activeTooltip.remove();
    }
    positionTooltip(term, tip);
    requestAnimationFrame(() => tip.classList.add('visible'));
    activeTooltip = tip;
  });

  term.addEventListener('mouseleave', () => {
    tip.classList.remove('visible');
    setTimeout(() => { if (!tip.classList.contains('visible')) tip.remove(); }, 150);
    activeTooltip = null;
  });

  // Tap for mobile
  term.addEventListener('click', (e) => {
    e.stopPropagation();
    if (activeTooltip && activeTooltip !== tip) {
      activeTooltip.classList.remove('visible');
      activeTooltip.remove();
    }
    if (tip.classList.contains('visible')) {
      tip.classList.remove('visible');
      tip.remove();
      activeTooltip = null;
    } else {
      positionTooltip(term, tip);
      requestAnimationFrame(() => tip.classList.add('visible'));
      activeTooltip = tip;
    }
  });
});

// Close tooltips when clicking elsewhere
document.addEventListener('click', () => {
  if (activeTooltip) {
    activeTooltip.classList.remove('visible');
    activeTooltip.remove();
    activeTooltip = null;
  }
});
```

**Rules:**
- Mark up EVERY technical term on first use in each module (API, DOM, callback, async, endpoint, middleware, etc.)
- Keep definitions to 1-2 sentences max, in everyday language
- Use a metaphor in the definition when it helps — e.g., "A **callback** is like leaving your phone number at a restaurant so they can call you when your table is ready"
- Don't mark the same term twice within the same screen — only on first appearance per module
- The dashed underline should be subtle enough not to distract but visible enough that curious learners discover it

---

## Visual File Tree

Use instead of paragraphs listing "this folder does X, that folder does Y." Much easier to scan.

```html
<div class="file-tree">
  <div class="ft-folder open">
    <span class="ft-name">app/</span>
    <span class="ft-desc">Pages and API routes</span>
    <div class="ft-children">
      <div class="ft-folder">
        <span class="ft-name">api/</span>
        <span class="ft-desc">Backend endpoints the frontend calls</span>
      </div>
      <div class="ft-file">
        <span class="ft-name">layout.tsx</span>
        <span class="ft-desc">The shell that wraps every page</span>
      </div>
    </div>
  </div>
  <div class="ft-folder">
    <span class="ft-name">components/</span>
    <span class="ft-desc">Reusable UI building blocks</span>
  </div>
  <div class="ft-folder">
    <span class="ft-name">lib/</span>
    <span class="ft-desc">Shared logic and utilities</span>
  </div>
</div>
```

```css
.file-tree { font-family: var(--font-mono); font-size: var(--text-sm); }
.ft-folder, .ft-file {
  padding: var(--space-2) var(--space-3);
  border-left: 2px solid var(--color-border-light);
  margin-left: var(--space-4);
}
.ft-folder > .ft-name { color: var(--color-accent); font-weight: 600; }
.ft-folder > .ft-name::before { content: '📁 '; }
.ft-file > .ft-name::before { content: '📄 '; }
.ft-desc {
  color: var(--color-text-secondary);
  font-family: var(--font-body);
  margin-left: var(--space-2);
  font-size: var(--text-xs);
}
.ft-children { margin-left: var(--space-4); }
```

---

## Icon-Label Rows

For listing components, features, or concepts visually. Replaces bullet-point paragraphs.

```html
<div class="icon-rows">
  <div class="icon-row">
    <div class="icon-circle" style="background: var(--color-actor-1)">🖥️</div>
    <div>
      <strong>Frontend (Next.js)</strong>
      <p>What the user sees and interacts with</p>
    </div>
  </div>
  <div class="icon-row">
    <div class="icon-circle" style="background: var(--color-actor-2)">⚡</div>
    <div>
      <strong>API Routes</strong>
      <p>Backend logic that runs on the server</p>
    </div>
  </div>
  <div class="icon-row">
    <div class="icon-circle" style="background: var(--color-actor-3)">🗄️</div>
    <div>
      <strong>Database (Supabase)</strong>
      <p>Where all the data is stored permanently</p>
    </div>
  </div>
</div>
```

```css
.icon-rows { display: flex; flex-direction: column; gap: var(--space-4); }
.icon-row {
  display: flex; align-items: center; gap: var(--space-4);
  padding: var(--space-4);
  background: var(--color-surface);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-sm);
}
.icon-row p { margin: 0; color: var(--color-text-secondary); font-size: var(--text-sm); }
.icon-circle {
  width: 48px; height: 48px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 1.25rem; flex-shrink: 0;
}
```

---

## Numbered Step Cards

For sequences that would otherwise be a numbered paragraph list. Visual, scannable, and each step stands alone.

```html
<div class="step-cards">
  <div class="step-card">
    <div class="step-num">1</div>
    <div class="step-body">
      <strong>User pastes a YouTube URL</strong>
      <p>The frontend captures the URL and extracts the video ID</p>
    </div>
  </div>
  <div class="step-card">
    <div class="step-num">2</div>
    <div class="step-body">
      <strong>API fetches the transcript</strong>
      <p>A server-side route calls an external service to get the video's text</p>
    </div>
  </div>
  <div class="step-card">
    <div class="step-num">3</div>
    <div class="step-body">
      <strong>AI analyzes the content</strong>
      <p>The transcript is sent to an AI model that extracts key moments</p>
    </div>
  </div>
</div>
```

```css
.step-cards { display: flex; flex-direction: column; gap: var(--space-3); }
.step-card {
  display: flex; align-items: flex-start; gap: var(--space-4);
  padding: var(--space-4) var(--space-5);
  background: var(--color-surface);
  border-radius: var(--radius-md);
  border-left: 3px solid var(--color-accent);
  box-shadow: var(--shadow-sm);
}
.step-num {
  width: 32px; height: 32px; border-radius: 50%;
  background: var(--color-accent);
  color: white; font-weight: 700;
  display: flex; align-items: center; justify-content: center;
  font-family: var(--font-display);
  flex-shrink: 0;
}
.step-body p { margin: var(--space-1) 0 0; color: var(--color-text-secondary); font-size: var(--text-sm); }
```
