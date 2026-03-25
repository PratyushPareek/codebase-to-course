---
name: codebase-to-course
description: "Turn any codebase into a beautiful, interactive single-page HTML course that teaches how the code works to developers of all experience levels. Use this skill whenever someone wants to create an interactive course, tutorial, or educational walkthrough from a codebase or project. Also trigger when users mention 'turn this into a course,' 'explain this codebase interactively,' 'teach this code,' 'interactive tutorial from code,' 'codebase walkthrough,' 'learn from this codebase,' 'make a course from this project,' or 'help me understand this codebase.' This skill produces a stunning, self-contained HTML file with scroll-based navigation, animated visualizations, and embedded quizzes. The agent adapts depth based on the learner's background — assumes fundamental CS knowledge but surveys the user on advanced topics before diving deep."
---

# Codebase-to-Course

Transform any codebase — app, library, framework, or tool — into a stunning, interactive single-page HTML course. The output is a single self-contained HTML file (no dependencies except Google Fonts) that teaches how the project works at a high level: its purpose, architecture, components, and how to use or deploy it. The course adapts to the learner's experience level through an upfront survey, so developers from junior to senior can all get value from it.

## First-Run Welcome

When the skill is first triggered and the user hasn't specified a codebase yet, introduce yourself and explain what you do:

> **I can turn any codebase into an interactive course that teaches how it works — tailored to your experience level.**
>
> Just point me at a project:
> - **A local folder** — e.g., "turn ./my-project into a course"
> - **A GitHub link** — e.g., "make a course from https://github.com/user/repo"
> - **The current project** — if you're already in a codebase, just say "turn this into a course"
>
> I'll read through the code, figure out how everything fits together, and then ask you a few quick questions about your background and what you're most interested in. From there, I'll generate a beautiful single-page HTML course with architecture diagrams, annotated code walkthroughs, and interactive quizzes — all tailored to what you need to know. The whole thing runs in your browser — no setup needed.

If the user provides a GitHub link, clone the repo first (`git clone <url> /tmp/<repo-name>`) before starting the analysis. If they say "this codebase" or similar, use the current working directory.

## Who This Is For

The target learner is a **developer at any experience level** — from junior devs exploring their first large codebase to senior engineers evaluating an unfamiliar project. They might be onboarding onto a new team, evaluating an open-source library, trying to contribute to a project, or simply curious about how something interesting works under the hood.

**Assume fundamental CS knowledge.** The learner knows what variables, functions, loops, classes, data structures, and basic algorithms are. They understand client-server architecture at a surface level. They know what a database is, what an API is, and roughly how the web works. You do NOT need to explain these basics.

**The codebase may be anything:** an app, a library, a framework, a CLI tool, a plugin, a data pipeline, an ML model, or infrastructure tooling. Do not assume it's a user-facing application — adapt your framing accordingly. A library's "user journey" is its API surface and how consumers integrate it. A CLI tool's journey is the command invocation flow.

**Probe for advanced topic familiarity.** Many codebases use deeper concepts from specific CS domains — OS-level primitives (threads, processes, memory management), advanced software engineering patterns (event sourcing, CQRS, actor model), database internals (B-trees, WAL, query planning), ML/stats concepts (gradient descent, attention mechanisms, Bayesian inference), networking (TCP handshakes, TLS, gRPC), or distributed systems (consensus, partitioning, CRDTs). **Before diving into any of these in the course, the agent must ask the user about their familiarity with the specific advanced concepts the codebase uses.** This happens during the survey phase (Phase 2). Based on their answers, the course either explains these concepts with appropriate depth or treats them as known and focuses on how the codebase applies them.

**The focus is NOT on the programming language itself** unless the user specifically asks. Don't spend course modules explaining language syntax, idioms, or features. The learner can look those up. Instead, focus on what the code *does* — the architecture, the design decisions, the data flow, the component responsibilities.

**Their goals are practical:**
- Get a **high-level understanding** of the project: what it does, what it contains, how it's designed, how the parts fit together
- Understand the **architecture and key design decisions** — why things are structured the way they are
- Know the **main components** and what each one is responsible for
- Understand **how to use, configure, or deploy** the project
- Become **aware enough of the entire project** that they can start exploring the specific component they want to work on
- Learn the **vocabulary of the project** — the domain-specific terms, the naming conventions, the abstractions the authors chose
- For libraries/frameworks: understand the **API surface**, the intended usage patterns, and the extension points

**They are NOT trying to memorize the codebase.** They want a mental map — a confident sense of "I know what this project does, how it's organized, and where to look when I need to work on X." Think of it as the orientation a good senior engineer would give a new team member on their first day.

## Why This Approach Works

Most developers learn a new codebase by reading files one at a time, grepping for keywords, and slowly building a mental model through trial and error. That works eventually, but it's slow and you miss the forest for the trees.

This skill inverts that process: **start with the big picture, then zoom into the parts that matter to you.** The course gives you the orientation that would normally take days of code-reading — the purpose, the architecture, the key components, how they connect, and the design decisions behind them — in a format you can absorb in an hour.

The learner already has *some* context — they've used the tool, read the README, or at least know what the project claims to do. The course meets them there and goes deeper: "You know what this project does. Here's *how* it does it, and *why* it's built this way."

The survey step is critical: by asking what the learner already knows and what they care about, the course avoids wasting time on familiar ground and focuses on the gaps that actually matter. A frontend engineer exploring a backend-heavy project needs different depth than a systems engineer looking at a React app.

The single-file constraint is intentional: one HTML file means zero setup, instant sharing, works offline, and forces tight design decisions.

---

## The Process (4 Phases)

### Phase 1: Codebase Analysis

Before writing course HTML, deeply understand the codebase. Read all the key files, trace the data flows, identify the "cast of characters" (main components/modules), and map how they communicate. Thoroughness here pays off — the more you understand, the better the course.

**What to extract:**
- **Purpose and scope** — What does this project do? What problem does it solve? Is it an app, library, framework, CLI tool, data pipeline, or something else?
- The main "actors" (components, services, modules, packages) and their responsibilities
- The **architecture and high-level design** — how is the project structured? What are the layers? What patterns does it use?
- The primary user/consumer journey — for an app: what happens end-to-end when someone uses it; for a library: what's the API surface and typical integration flow; for a CLI: what's the command invocation flow
- Key APIs, data flows, and communication patterns (internal and external)
- **How to use, configure, and deploy** the project — build system, entry points, configuration, environment setup
- Clever engineering patterns (caching, lazy loading, error handling, concurrency patterns, etc.)
- **Advanced CS concepts used** — identify any deeper concepts from OS, SWE, DBMS, ML/Stats, networking, distributed systems, etc. that the codebase relies on. These will be surfaced during the survey phase.
- The tech stack and why each piece was chosen
- **Extension points** — for libraries/frameworks: how does a consumer extend or customize behavior?

**Figure out what the project does yourself** by reading the README, the main entry points, and the primary interfaces. Don't ask the user to explain the product — figure it out from the code and documentation. The course should open by explaining what the project does in plain language before diving into how it works.

**After completing analysis, present a brief high-level overview** to the user in the chat (not in the HTML course yet). This serves as a sanity check and gives the user context before the survey. The overview should cover:
- What the project is and what it does (1-2 sentences)
- The main components/modules at a glance
- The tech stack
- Any advanced CS concepts the codebase uses that might need explanation depending on the learner's background

### Phase 2: Survey & Curriculum Design

After analysis, the agent conducts a **survey/questionnaire directly in the chat** to tailor the course to the learner. This is NOT optional — always do this before building the course.

#### Step 1: Background & Interest Survey

Conduct the entire survey in a **single message**. Combine concept familiarity checks with interest/background questions so the user only has to respond once. Tailor the questions to what the codebase actually uses.

> A few quick questions so I can tailor the course to you:
>
> **Your background with this project:**
> 1. **What's your goal?** (e.g., contributing to this project, evaluating it for use, understanding the architecture, onboarding, general curiosity)
> 2. **What do you already know about it?** (e.g., "I've used the API but never looked at internals" or "totally new to it")
> 3. **Which of these areas interest you most?** (list the major components/areas discovered in Phase 1 — e.g., "the plugin system," "the query engine," "the auth flow," "the deployment setup")
>
> **Concept familiarity** (only if the codebase uses these — skip irrelevant ones):
> - Are you familiar with [specific advanced concepts the codebase uses, grouped by domain]?
>
> **One last thing:**
> - **Would you like to review and edit the curriculum before I build the course?**

Only ask about advanced concepts the codebase **actually uses** — don't quiz them on irrelevant topics. Keep the whole survey to one screen: 4-8 questions max. Frame questions conversationally, not like an exam.

Adapt the specific questions to what the codebase offers. A library might surface questions about "the public API," "internal architecture," "extension mechanisms," and "testing strategy." An app might surface "the user-facing features," "the backend services," "the data model," and "the deployment pipeline."

#### Step 2: Curriculum Design

Based on the survey responses, design the curriculum. Structure the course as 4-8 modules. The arc starts with the big picture and zooms into the areas the user cares about most.

| Module Position | Purpose | What it covers |
|---|---|---|
| 1 | "Here's what this project does and how it's structured" | Purpose, scope, project type (app/lib/tool), high-level architecture overview, tech stack rationale |
| 2 | Meet the components | The main actors, their responsibilities, how the codebase is organized, naming conventions |
| 3 | How the pieces connect | Data flow, communication patterns, key interfaces, dependency relationships |
| 4 | The core mechanism | The central "interesting thing" the project does — the heart of the logic, traced end-to-end |
| 5 | Integration & usage | How to use/consume/deploy it, configuration, API surface (for libs), entry points, extension points |
| 6 | Advanced patterns & design decisions | Caching, error handling, concurrency, performance optimizations — with depth calibrated to survey answers |
| 7 | Deep-dive: user's areas of interest | Focused exploration of the components/areas the user said they care about most |

Not every codebase needs all 7. A simple CLI tool might only need 4 modules. A complex distributed system might need 8. Adapt based on the project's complexity and the user's stated interests. **Prioritize the areas the user expressed interest in** — if they said they care about the plugin system, that should get a dedicated module, not a footnote.

For modules covering advanced CS concepts: if the user indicated unfamiliarity in the survey, include clear explanations with appropriate depth. If they indicated familiarity, treat those concepts as known and focus on how the codebase applies them.

**Each module should contain:**
- 3-6 screens (sub-sections that flow within the module)
- At least one interactive element (quiz, visualization, or animation)
- One or two "aha!" callout boxes with architectural or design insights
- Where helpful, interface or type definition snippets that make architecture concrete (see Content Philosophy → Interface Snippets)
- Where helpful, a metaphor that grounds a complex concept in everyday life — but NEVER reuse the same metaphor across modules, and NEVER default to the "restaurant" metaphor. The best metaphors feel *inevitable* for the concept, not forced.

**Mandatory interactive elements (every course must include ALL of these):**

- **Group Chat Animation** — at least one across the course. iMessage/WeChat-style conversations between components showing how they interact.
- **Message Flow / Data Flow Animation** — at least one across the course. Step-by-step animation of data moving between actors for any request/response, pipeline, or multi-step process.
- **Quizzes** — at least one per module (multiple-choice, scenario, drag-and-drop, or spot-the-bug).
- **Glossary Tooltips** — on technical terms that go beyond CS fundamentals, first use per module.

These four element types are the backbone of every course. Other interactive elements should be added when relevant:
- **Interactive Architecture Diagram** — a clickable/hoverable visual map of the system's main components organized by zones. Strongly encouraged when the project has multiple distinct layers or services (see `references/interactive-elements.md` for a full implementation pattern). Skip only if the project is small/flat enough that a simple list or flow diagram covers the structure.
- Layer toggles, pattern cards, interface snippets, etc.

#### Step 3: Curriculum Review (if requested)

If the user said "yes" to reviewing the curriculum in step 1:

- **If the curriculum is short (≤ 7 modules with brief descriptions):** present it directly as a chat message with the module titles, descriptions, and key topics for each.
- **If the curriculum is long or detailed:** generate a Markdown file (e.g., `course-curriculum.md`) in the workspace with the full curriculum outline and tell the user where to find it.

In either case, ask the user if they want to make changes. Apply any feedback before proceeding to Phase 3.

If the user said "no" to reviewing, proceed directly to Phase 3.

### Phase 3: Build the Course

Generate a single HTML file with embedded CSS and JavaScript. Read `references/design-system.md` for the complete CSS design tokens, typography, and color system. Read `references/interactive-elements.md` for implementation patterns of every interactive element type.

**Build order (task by task):**

1. **Foundation first** — HTML shell with all module sections (empty), complete CSS design system, navigation bar with progress tracking, scroll-snap behavior, keyboard navigation, and scroll-triggered animations. After this step, you should have a working skeleton you can scroll through.

2. **One module at a time** — Fill in each module's content and interactive elements. Don't try to write all 8 modules in one pass — the quality drops. Build Module 1, verify it works, then Module 2, etc.

3. **Polish pass** — After all modules are built, do a final pass for transitions, mobile responsiveness, and visual consistency.

**Critical implementation rules:**
- The file must be completely self-contained (only external dependency: Google Fonts CDN)
- Use CSS `scroll-snap-type: y proximity` (NOT `mandatory` — mandatory traps users in long modules)
- Use `min-height: 100dvh` with `100vh` fallback for sections
- Only animate `transform` and `opacity` for GPU performance
- Wrap all JS in an IIFE, use `passive: true` on scroll listeners, throttle with `requestAnimationFrame`
- Include touch support for drag-and-drop, keyboard navigation (arrow keys), and ARIA attributes

### Phase 4: Review and Open

After generating the course HTML file, open it in the browser for the user to review. Walk them through what was built and ask for feedback on content, design, and interactivity.

---

## Content Philosophy

These principles are what separate a great course from a generic tutorial. They should guide every content decision:

### Show, Don't Tell — Aggressively Visual
People's eyes glaze over text blocks. The course should feel closer to an infographic than a textbook. Follow these hard rules:

**Text limits:**
- Max **2-3 sentences** per text block. If you're writing a fourth sentence, stop and convert it into a visual instead.
- No text block should ever be wider than the content width AND taller than ~4 lines. If it is, break it up with a visual element.
- Every screen must be **at least 50% visual** (diagrams, cards, animations, badges, interface snippets — anything that isn't a paragraph).

**Convert text to visuals:**
- A list of 3+ items → **cards with icons** (pattern cards, feature cards)
- A sequence of steps → **flow diagram with arrows** or **numbered step cards**
- "Component A talks to Component B" → **animated data flow** or **group chat visualization**
- "This file does X, that file does Y" → **visual file tree with annotations** or **icon + one-liner badges**
- Explaining an interface or contract → **annotated interface snippet** with brief inline comments
- Comparing two approaches → **side-by-side columns** with visual contrast

**Visual breathing room:**
- Use generous spacing between elements (`--space-8` to `--space-12` between sections)
- Alternate between full-width visuals and narrow text blocks to create rhythm
- Every module should have at least one "hero visual" — a diagram, animation, or interactive element that dominates the screen and teaches the core concept at a glance

### Interface Snippets — Show the Contracts
When a module explains a component's responsibilities or how pieces connect, showing the actual interface, type definition, config shape, or key function signature from the codebase makes the architecture concrete. These are NOT line-by-line code walkthroughs — they're short, readable snippets (typically 5-15 lines) that reveal the *shape* of a component: what it accepts, what it returns, what it exposes.

**When to include interface snippets:**
- A module describes a component's API surface → show the interface/type definition
- A module explains how two parts communicate → show the shared type or message schema
- A module covers configuration → show the config struct or options object
- A module traces a pipeline → show the key function signatures at each stage

**When NOT to include them:**
- The interface is too large or complex to be readable at a glance — summarize in prose or a diagram instead
- The concept is better explained with a visual (data flow, architecture diagram, group chat) — prefer the visual
- The code is implementation detail that doesn't help the learner understand the architecture

**Rules:**
- Use original code exactly as-is — never modify or simplify. The learner should be able to find the same code in the real file.
- Choose naturally short, self-contained definitions. Every codebase has clean interface moments — find those.
- No horizontal scrollbars. Use `white-space: pre-wrap` so code wraps.
- Add a brief heading or 1-sentence lead-in explaining *why* this snippet matters (e.g., "This is the contract every plugin must implement:").

### One Concept Per Screen
No walls of text. Each screen within a module teaches exactly one idea. If you need more space, add another screen — don't cram.

### Metaphors Where They Help
For advanced or unfamiliar concepts (based on survey answers), introduce them with a metaphor from everyday life. Then immediately ground it: "In this codebase, that looks like..." The metaphor builds intuition; the code grounds it in reality. For concepts the learner already knows, skip the metaphor and go straight to how the codebase applies them.

**Critical: No recycled metaphors.** Do NOT default to "restaurant" for everything — that's the #1 crutch. Each concept deserves its own metaphor that feels natural to *that specific idea*. An event loop is an air traffic controller. Message passing is a postal system. API rate limiting is a nightclub with a capacity limit. Pick the metaphor that makes the concept click, not the one that's easiest to reach for. If you catch yourself using "restaurant" or "kitchen" more than once in a course, stop and rethink.

### Learn by Tracing
Follow what actually happens during a concrete operation — trace the data flow end-to-end. For an app: "When a user submits a form, here's the journey that data takes." For a library: "When you call `client.query()`, here's what happens inside." For a CLI: "When you run `tool build`, here's the pipeline that executes." Start from something the learner can relate to (using the tool, calling the API, running a command) and trace through the internals. This works because the learner has *already experienced the result* — now they're seeing the machinery behind it.

### Make It Memorable
Use "aha!" callout boxes for universal CS insights. Use humor where natural (not forced). Give components personality — they're "characters" in a story, not abstract boxes on a diagram.

### Glossary Tooltips — For Advanced & Domain-Specific Terms
Terms that go beyond fundamental CS knowledge get a dashed-underline tooltip on first use in each module. Hover on desktop or tap on mobile to see a 1-2 sentence definition. The learner shouldn't have to leave the page to Google a term that's specific to the project's domain or tech stack.

**What to tooltip (calibrated to the audience):**
- **Domain-specific terms** — terms unique to the project's problem domain (e.g., "attention head" in an ML project, "WAL" in a database project, "reconciliation" in a React-like framework)
- **Project-specific jargon** — names the codebase invented for its own abstractions (e.g., "Resolver," "Pipeline Stage," "Middleware Chain" as used in this specific project)
- **Advanced CS concepts** — terms from the advanced concept domains (OS, distributed systems, ML, etc.) that the user indicated unfamiliarity with during the survey
- **Acronyms** — tooltip acronyms on first use unless they're universally known (HTTP, URL, API are fine to skip; CRDT, WAL, CQRS need tooltips)
- **Tool/technology names** — specific technologies the learner might not have used (e.g., Redis, gRPC, Kafka, Terraform)

**What NOT to tooltip:** basic CS terms the audience already knows — function, variable, class, array, loop, API, database, server, client, HTTP, JSON, etc. Over-tooltipping fundamentals feels patronizing to developers.

**Tooltip depth should match the survey.** If the user said they're unfamiliar with distributed systems, the tooltip for "eventual consistency" should be a clear 2-sentence explanation. If they said they know it, don't tooltip it at all.

**Cursor:** Use `cursor: pointer` on terms (not `cursor: help`). A pointer feels clickable and inviting.

**Tooltip overflow fix:** Code containers and other elements with `overflow: hidden` will clip tooltips. To fix this, the tooltip JS must use `position: fixed` and calculate coordinates from `getBoundingClientRect()` instead of relying on CSS `position: absolute` within the container. Append tooltips to `document.body` rather than inside the term element. This ensures tooltips are never clipped by any ancestor's overflow.

### Quizzes That Test Application, Not Memory

The goal of learning is practical application — being able to *do something* with what you learned. Quizzes should test whether the learner can use their knowledge to solve a new problem, not whether they can regurgitate a definition.

**What to quiz (in order of value):**
1. **"What would you do?" scenarios** — Present a new situation the learner hasn't seen and ask them to apply what they learned. e.g., "You want to add a 'save to favorites' feature. Which files would you need to change?" This is the gold standard.
2. **Debugging scenarios** — "A user reports X is broken. Based on what you learned, where would you look first?" This tests whether they understood the architecture, not just memorized file names.
3. **Architecture decisions** — "You're building a similar app from scratch. Would you put this logic in the frontend or backend? Why?" Tests whether they understood the *reasoning* behind design choices.
4. **Tracing exercises** — "When a user does X, trace the path the data takes." Tests whether they can follow the flow.

**What NOT to quiz:**
- Definitions ("What does API stand for?") — that's what the glossary tooltips are for
- File name recall ("Which file handles X?") — nobody memorizes file names
- Syntax details ("What's the correct way to write a fetch call?") — this isn't a coding bootcamp
- Anything that can be answered by scrolling up and copying — that tests scrolling, not understanding

**Quiz tone:**
- Wrong answers get encouraging, non-judgmental explanations ("Not quite — here's why...")
- Correct answers get brief reinforcement of the underlying principle ("Exactly! This works because...")
- Never punitive, never score-focused. No "You got 3/5!" — the quiz is a thinking exercise, not an exam
- Wrong answer explanations should teach something new, not just say "wrong, the answer was B"

**How many quizzes:** One per module, placed at the end after the learner has seen all the content. 3-5 questions per quiz. Each question should make the learner pause and *think*, not just pick the obvious answer.

**Deciding what concepts are worth quizzing:** Quiz the things that would actually help someone work with or on the project — architecture understanding ("where does this logic live and why?"), debugging intuition ("what would cause this symptom?"), design tradeoffs ("why did the authors choose X over Y?"), and practical usage ("how would you extend this to support Z?"). If a concept won't help someone navigate, debug, extend, or reason about the codebase, it's not worth quizzing.

---

## Design Identity

The visual design should feel like a **beautiful developer notebook** — warm, inviting, and distinctive. Read `references/design-system.md` for the full token system, but here are the non-negotiable principles:

- **Warm palette**: Off-white backgrounds (like aged paper), warm grays, NO cold whites or blues
- **Bold accent**: One confident accent color (vermillion, coral, teal — NOT purple gradients)
- **Distinctive typography**: Display font with personality for headings (Bricolage Grotesque, or similar bold geometric face — NEVER Inter, Roboto, Arial, or Space Grotesk). Clean sans-serif for body (DM Sans or similar). JetBrains Mono for code.
- **Generous whitespace**: Modules breathe. Max 3-4 short paragraphs per screen.
- **Alternating backgrounds**: Even/odd modules alternate between two warm background tones for visual rhythm
- **Dark code blocks** (for interface snippets): IDE-style with Catppuccin-inspired syntax highlighting on deep indigo-charcoal (#1E1E2E)
- **Depth without harshness**: Subtle warm shadows, never black drop shadows

---

## Gotchas — Common Failure Points

These are real problems encountered when building courses. Check every one before considering a course complete.

### Tooltip Clipping
Code containers and other elements use `overflow: hidden`. If tooltips use `position: absolute` inside the term element, they get clipped by the container. **Fix:** Tooltips must use `position: fixed` and be appended to `document.body`. Calculate position from `getBoundingClientRect()`. This is already specified in the reference files but is the #1 bug that appears in every build.

### Wrong Tooltip Calibration
Two failure modes: **under-tooltipping** advanced terms (assuming the learner knows domain-specific concepts they don't) and **over-tooltipping** basics (explaining what "function" or "API" means to developers who already know). Use the survey answers to calibrate. Tooltip domain-specific terms, project jargon, advanced CS concepts the user flagged as unfamiliar, and technology names they might not have used. Skip fundamental CS vocabulary.

### Walls of Text
The course looks like a textbook instead of an infographic. This happens when you write more than 2-3 sentences in a row without a visual break. Every screen must be at least 50% visual. Convert any list of 3+ items into cards, any sequence into step cards or flow diagrams, any interface explanation into an annotated interface snippet.

### Recycled Metaphors
Using "restaurant" or "kitchen" for everything. Every module needs its own metaphor that feels inevitable for that specific concept. If you catch yourself reaching for the same metaphor twice, stop and find one that fits the concept organically.

### Interface Snippet Misuse
Showing large implementation blocks instead of clean interfaces. Interface snippets should show contracts, type definitions, config shapes, and key signatures — not function bodies or business logic. If a snippet isn't readable at a glance, use a diagram or prose instead. Never modify code — show original snippets exactly as they appear in the codebase.

### Quiz Questions That Test Memory
Asking "What does API stand for?" or "Which file handles X?" — those test recall, not understanding. Every quiz question should present a new scenario the learner hasn't seen and ask them to *apply* what they learned.

### Scroll-Snap Mandatory
Using `scroll-snap-type: y mandatory` traps users inside long modules. Always use `proximity`.

### Module Quality Degradation
Trying to write all modules in one pass causes later modules to be thin and rushed. Build one module at a time and verify each before moving on.

### Missing Interactive Elements
A module with only text and code blocks, no interactivity. Every module needs at least one of: quiz, data flow animation, group chat, architecture diagram, drag-and-drop. These aren't decorations — they make abstract architecture concepts concrete and testable.

### Skipping the Survey
Jumping straight to building the course without asking the user about their background and interests. The survey is NOT optional. Without it, you'll either over-explain things they know or skip things they don't, and you won't prioritize the areas they actually care about. Always run Phase 2 before Phase 3.

### Language-Focused Content
Spending modules explaining language syntax, idioms, or features instead of what the code *does* in the project. Unless the user specifically asked about the language, focus on architecture, design, data flow, and component responsibilities. The code translations should explain intent and role, not syntax.

---

## Reference Files

The `references/` directory contains detailed implementation specs. Read them when you reach the relevant phase:

- **`references/design-system.md`** — Complete CSS custom properties, color palette, typography scale, spacing system, shadows, animations, scrollbar styling. Read this before writing any CSS.
- **`references/interactive-elements.md`** — Implementation patterns for every interactive element: interface snippets, multiple-choice quizzes, drag-and-drop, group chat animations, message flow visualizations, architecture diagrams, layer toggles, design flaw challenges, pattern cards, callout boxes. Read this before building any interactive elements.
