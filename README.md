# Codebase to Course

> Forked from [zarazhangrui/codebase-to-course](https://github.com/zarazhangrui/codebase-to-course)

A custom skill for AI coding assistants (GitHub Copilot, Claude Code, etc.) that turns any codebase into a beautiful, interactive single-page HTML course.

**Original intent:** The original skill was designed for "vibe coders" — non-technical people who build with AI tools but have zero CS background, teaching them how code works from scratch.

**What this fork changes:** This version targets **developers of all experience levels** — from juniors exploring their first large codebase to seniors evaluating an unfamiliar project. It assumes fundamental CS knowledge, focuses on high-level understanding (architecture, components, design decisions, data flow) rather than language syntax, and adds an upfront survey so the course adapts to what you already know and what you care about.

### Features

- **Assumes CS fundamentals** — tooltips are reserved for domain-specific, project-specific, and advanced terms, not basics
- **Works for any project type** — apps, libraries, frameworks, CLI tools, data pipelines, ML models
- **Survey-driven** — asks about your background, familiarity with advanced concepts, and which areas you care about before building
- **Curriculum review** — optionally review and edit the course outline before it's generated
- **Architecture-first** — focuses on purpose, components, design decisions, and how to use/deploy the project
- **Interface snippets** — shows clean interfaces, type definitions, and config shapes to make architecture concrete
- **Design flaw challenges** — interactive exercises that test architectural understanding, not syntax knowledge
- **Generalized interactive elements** — layer toggles, architecture diagrams, and visualizations adapted for any project type

## Who is this for?

**Developers at any experience level** who want to quickly understand an unfamiliar codebase.

You might be onboarding onto a new team, evaluating an open-source library, trying to contribute to a project, or simply curious about how something interesting works under the hood. You want a mental map — not a textbook.

**Your goals are practical:**
- Get a high-level understanding of what the project does, how it's structured, and why
- Know the main components and what each one is responsible for
- Understand the architecture and key design decisions
- Learn how to use, configure, or deploy the project
- Become aware enough to start exploring the specific part you want to work on

## What the course looks like

The output is a **single HTML file** — no dependencies, no setup, works offline. It includes:

- **Scroll-based modules** with progress tracking and keyboard navigation
- **Animated visualizations** — data flow animations, group chat between components, architecture diagrams
- **Interactive quizzes** that test application, not memorization ("You want to add a caching layer — where would it go and why?")
- **Interface snippets** — real type definitions and contracts from the codebase, annotated with context
- **Glossary tooltips** — hover domain-specific and advanced terms for definitions (calibrated to your survey answers)
- **Warm, distinctive design** — not the typical purple-gradient AI look

## How to use

This skill is **platform-agnostic** — it works with any AI coding assistant that supports custom skills or instructions. The survey step is a normal chat conversation (the agent asks questions, you reply), so no special UI is required.

### GitHub Copilot

1. Copy the `codebase-to-course` folder into `~/.copilot/skills/` (or your workspace's `.github/copilot/skills/` directory)
2. Open any project in VS Code with Copilot
3. Say: *"Turn this codebase into an interactive course"*

### Claude Code

1. Copy the `codebase-to-course` folder into `~/.claude/skills/`
2. Open any project in Claude Code
3. Say: *"Turn this codebase into an interactive course"*

### Other AI assistants

The skill is defined entirely in Markdown files (`SKILL.md` + `references/`). You can adapt it to any AI coding tool that supports custom instructions by pointing the tool at `SKILL.md` as its system prompt or skill definition.

### Trigger phrases

- "Turn this into a course"
- "Explain this codebase interactively"
- "Make a course from this project"
- "Help me understand this codebase"
- "Interactive tutorial from this code"

## Design philosophy

### Big picture first, then zoom in

Start with what the project does and how it's structured. Then progressively zoom into the areas that matter to the learner. The survey ensures you don't waste time on things you already know.

### Show, don't tell

Every screen is at least 50% visual. Max 2-3 sentences per text block. If something can be a diagram, animation, or interactive element — it shouldn't be a paragraph.

### Focus on architecture, not syntax

The course explains what the code *does* and *why*, not what the language syntax means. Interface snippets show the shape of components — what they accept, return, and expose — not implementation details.

### Quizzes test doing, not knowing

No "What does API stand for?" Instead: "A user reports stale data after switching pages. Where would you look first?" Quizzes test whether you can *use* what you learned to solve a new problem.

### Adapt to the learner

The survey asks about your background, which advanced concepts you know, and what areas of the codebase you care about. The course adjusts depth accordingly — explaining unfamiliar concepts while treating known ones as given.

## Skill structure

```
codebase-to-course/
├── SKILL.md                          # Main skill instructions
└── references/
    ├── design-system.md              # CSS tokens, typography, colors, layout
    └── interactive-elements.md       # Quiz, animation, and visualization patterns
```

---

Originally built by [Zara](https://x.com/zarazhangrui) with Claude Code. This fork adapts the skill for developers of all experience levels.
