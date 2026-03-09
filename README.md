# Project Coding Companion

**Author:** Ruben Granet
**Version:** 1.0.0
**License:** MIT

An AI coding companion that coaches you to write better coding prompts before implementing — evaluate, clarify, plan, code, review. Instead of diving straight into code when a request is vague, it first helps you clarify what you actually need — then plans, implements, and reviews the changes.

Works with **Claude Code**, **Claude.ai**, **Cursor**, and **OpenAI Codex**.

---

## What it does

- **Coaches your prompts** — evaluates every coding request against 5 dimensions (context, intent, scope, constraints, acceptance criteria) and asks targeted questions with explanations when something is missing.
- **Respects conversation context** — recognizes continuations ("ok do it", "next step") and doesn't re-ask questions you already answered.
- **Understands repositories** — inspects project structure, identifies the stack, and builds a mental model of the codebase.
- **Plans implementations** — proposes short, step-by-step plans (1–7 steps) and waits for your approval.
- **Implements changes** — reads before writing, makes minimal diffs, preserves existing style.
- **Reviews and summarizes** — lists every file changed, explains why, and suggests tests or follow-up work.
- **Stays safe** — never auto-commits, never runs destructive commands without confirmation.

Works with **any language or stack**: TypeScript, Python, Swift, Kotlin, Rust, Go, Java, Ruby, C#, and more.

---

## When to use it

**Great fit:**

- You have an existing repository and want help adding features, fixing bugs, refactoring, writing tests, or understanding the code.
- You're learning to write better prompts for AI coding assistants.
- You want a disciplined workflow (evaluate prompt → coach → plan → implement → review) instead of ad-hoc code generation.

**Not the best fit:**

- Pure greenfield scaffolding with no existing code.
- Non-code tasks (documents, presentations, spreadsheets).
- Heavy frontend design work driven primarily by aesthetics.

---

## Platform support

This repo ships three files — same instructions, adapted for each platform:

| Platform | File | How it works |
|---|---|---|
| **Claude Code** | `project-coding-companion/SKILL.md` | Installed as a Claude Skill — triggers automatically on coding requests |
| **Claude.ai** | `project-coding-companion/SKILL.md` | Uploaded via Settings → Capabilities → Skills |
| **Cursor** | `.cursorrules` | Placed at the root of your project — applies to all Cursor conversations in that project |
| **OpenAI Codex** | `AGENTS.md` | Placed at the root of your project — Codex reads it as agent instructions |

---

## Folder structure

```
project-coding-companion-skill/        ← This repo
├── README.md                          ← You are here (for humans on GitHub)
├── LICENSE                            ← MIT
├── project-coding-companion/          ← Claude Skill folder
│   └── SKILL.md                       ← Claude Code & Claude.ai
├── .cursorrules                       ← Cursor
└── AGENTS.md                          ← OpenAI Codex
```

> **Note:** The `README.md` lives at the repo root for GitHub readers. It is _not_ inside the skill folder — per Anthropic's Skill specification, the skill folder contains only `SKILL.md` and optional `scripts/`, `references/`, `assets/` directories.

---

## Installation

### Claude Code

1. Clone this repo:
   ```bash
   git clone https://github.com/rgranet/project-coding-companion-skill.git
   ```
2. Place the `project-coding-companion/` folder in your Claude Code skills directory.
3. The skill activates automatically on coding requests.

### Claude.ai (web & desktop)

1. Zip the **skill folder** (not the whole repo):
   ```bash
   zip -r project-coding-companion.zip project-coding-companion/
   ```
2. Go to **Settings → Capabilities → Skills**.
3. Click **Upload skill** and select the zip file.

### Cursor

1. Copy `.cursorrules` from this repo into the **root of your project**:
   ```bash
   cp .cursorrules /path/to/your/project/
   ```
2. Open your project in Cursor — the rules apply automatically to all conversations.

> **Important:** `.cursorrules` must be at the root of the project you're working on, not in a separate skills folder. Each project gets its own copy.

### OpenAI Codex

1. Copy `AGENTS.md` from this repo into the **root of your project**:
   ```bash
   cp AGENTS.md /path/to/your/project/
   ```
2. Codex reads `AGENTS.md` automatically as agent instructions.

> **Important:** Like `.cursorrules`, `AGENTS.md` must be at the root of the project you're working on.

---

## Example workflows

### 1. Vague request → coaching → implementation

> **You:** "Add search to my app"

1. Detects 3+ missing dimensions, enters coaching mode.
2. Asks 3 targeted questions (which screen? what's searchable? real-time or on submit?) with "→ Why this matters" explanations.
3. You answer. Restates the task as a checklist for confirmation.
4. Explores the codebase, plans steps, implements, summarizes changes.
5. Ends with a prompting tip: "Next time, mentioning the target screen saves a round-trip."

### 2. Clear request → straight to code

> **You:** "Add a `isFavorite: Bool` property to the Thought entity, a toggle button in ThoughtDetailView, and a FavoritesListView filtered on `isFavorite == true`. Follow existing MVVM pattern."

1. Evaluates: all 5 dimensions present. "Clear and complete — I have everything I need."
2. Plans 4 steps, implements with minimal diffs, summarizes.

### 3. Explicit prompt coaching

> **You:** "I want to refactor my services layer but I don't know how to describe it. Help me write a good prompt."

1. Asks you to describe the pain points in your own words.
2. Shows a 5-dimension scorecard (✅/❌/⚠️).
3. Proposes a rewritten prompt with all dimensions filled.
4. Explains key improvements so you can apply the same thinking next time.

---

## Prompt examples

```
Add search to my app.
```

```
Help me understand the architecture of this project.
```

```
Fix the bug where deleting a parent entity doesn't cascade to children.
```

```
Help me write a better prompt for refactoring my networking layer.
```

```
Refactor the authentication middleware to use a strategy pattern instead
of the current if/else chain. Keep backward compatibility with existing
callers. Add unit tests for each strategy.
```

---

## Configuration / Customization

The companion is **language- and framework-agnostic**. It discovers conventions from the project itself. You can customize it by:

- **For Claude** — edit `SKILL.md` directly, or add `references/` and `scripts/` directories inside the skill folder. Files in these directories are only loaded when relevant (progressive disclosure).
- **For Cursor** — edit `.cursorrules` at your project root. You can add project-specific rules (e.g., "always use Vitest instead of Jest", "all API routes go through the `createHandler` wrapper").
- **For Codex** — edit `AGENTS.md` at your project root. Same customization approach as Cursor.

Examples of things you can tune: max plan steps, coaching aggressiveness, testing expectations, preferred patterns, language-specific conventions.

---

## Contributing

Contributions are welcome!

1. **Open an issue** to report bugs, suggest trigger phrases, or propose new behaviors.
2. **Submit a PR** — one concern per PR, with a clear description of what and why.
3. **Test your changes** by loading the companion in your platform of choice and running representative prompts.

When contributing, please update **all three files** (SKILL.md, .cursorrules, AGENTS.md) to keep them in sync.

---

## License

MIT — see [LICENSE](./LICENSE).
