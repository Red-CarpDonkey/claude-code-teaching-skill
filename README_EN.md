# Claude Code Teaching Skill

**Don't teach knowledge — let learners relive its discovery, invention, and dialogue.**

Traditional teaching starts from conclusions — open a textbook, chapter one is definitions. But knowledge was never born to be "learned." It was born to solve a dilemma that could no longer be avoided.

This Claude Code Skill puts the learner back at the scene of that dilemma, as the person who must make the choice.

---

## Theoretical Foundation: The Five Meta-Learning Stages

All teaching is grounded in a universal cognitive grammar — the five logical stages through which any human mind moves from ignorance to mastery:

| Stage | Logic | Core Output |
|-------|-------|-------------|
| **I Genesis** | Cognitive dissonance | Problem awareness |
| **II Ontology** | Reductionism | Definitions, concepts, boundaries |
| **III Mechanism** | Structural functionalism | Mechanisms, algorithms, derivations |
| **IV Dialectic** | Critical realism | Boundary conditions, costs, limitations |
| **V Synthesis** | Holism | Knowledge network, transfer capability |

Each stage constrains what the teacher is allowed to do: Stage I only creates gaps (no definitions), Stage III requires the learner to derive (no spoon-feeding), Stage IV must present alternatives without favoring any.

---

## Five Teaching Modes

Each mode is a domain-specific instantiation of the five meta-stages:

| Mode | Domain | Archetype | Core Question |
|------|--------|-----------|---------------|
| **Discoverer** | Math & science | Scientist uncovering nature's secrets | Why is the world this way? |
| **Engineer** | Tech stacks | Inventor solving engineering pain | How to do it more effectively? |
| **Dialoguer** | Humanities debates | Entering a millennia-old conversation | How should we understand and choose? |
| **Observer** | Social sciences (empirical) | Social scientist explaining phenomena | Why does human society work this way? |
| **Deep Diver** | Classic works | Entering an author's mind | What is this author really saying? |

---

## Quick Start

In any directory:

```
/study [what you want to learn]
```

The skill auto-detects the discipline and routes to the right mode. Examples:

```
/study discrete math      → auto-detects as Discoverer mode
/study C++                → auto-detects as Engineer mode
/study philosophy of law  → auto-detects as Dialoguer mode
/study macroeconomics     → auto-detects as Observer mode
/study Zhuangzi           → auto-detects as Deep Diver mode
```

You can also manually specify: "use Engineer mode for graph theory".

---

## Key Features

### Textbook as Roadmap

All five modes support selecting a textbook as the learning roadmap. The chapter list drives the five meta-stages — each chapter completes a full deep learning cycle.

### Concept Introduction: Five-Step Template (Engineer Mode)

```
What is it (1 line) → Usage template (ALL possible cases) → Examples (2-3) → Runnable code → Pain point revealed last
```

The pain point comes last — the learner first experiences the concept hands-on, then looks back and recognizes what problem it elegantly solves.

### Knowledge Point Detection

Cards are no longer triggered by "which stage you're in." Instead, a knowledge point is recognized when the learner (1) passes the Feynman test (explains the essence in one sentence) AND (2) expresses a connection to prior knowledge. Both signals must be the learner's own words.

### Three-Agent Supervision

After every teaching interaction, three independent supervision agents run sequentially to prevent attention mechanism failures:

| Agent | Core Question |
|-------|---------------|
| Format Supervisor | Is it correct? |
| Rule Supervisor | Is it following the rules? |
| Content Reviewer | Is it thorough enough? |

### Question Interruption Protocol

When the learner asks a mid-session question, it's classified (A/B/C/D) and gated by the current meta-stage. Questions about content from later stages are deferred and revisited when the learner reaches that stage — no spoilers.

---

## How to Use

### Interaction During Learning

The CLI terminal is for **dialogue only** — questions, answers, brief guidance. All teaching content (derivations, formulas, diagrams, code, arguments) is written in real-time to an Obsidian vault blackboard file. Open the file in Obsidian to see beautifully rendered LaTeX formulas and mermaid diagrams.

### Knowledge Cards and Cognitive Network

When the learner passes the Feynman test and expresses a connection to prior knowledge, a knowledge card is generated. Each card records four elements: one-sentence definition, connection edge, invocation evidence, and current cognitive depth. Cards use the learner's own words as the core, with bidirectional links forming a personal knowledge network — viewable in Obsidian's graph view.

### Resuming a Previous Session

When tokens run out or you need a new session:

1. Open the **same working directory**
2. Type `/study [same topic]`
3. The skill detects `.study/state.json` and auto-resumes
4. It tells you where you left off and what's still on the blackboard

**No manual steps required.** All learning state lives in the filesystem, not in session memory.

### Obsidian Vault Structure

```
working-directory/
├── 00-总览仪表盘/           # Dashboard
├── 10-理论学习/              # Discoverer mode output
├── 20-工具栈/                # Engineer mode output
├── 30-文科论题/              # Dialoguer mode output
├── 35-社会观察/              # Observer mode output
├── 40-经典专著/              # Deep Diver mode output
├── 50-演示模型/              # Demo code
└── .study/                   # Learning state (gitignored)
    ├── state.json
    ├── errors.md
    └── questions.md
```

Open the working directory in Obsidian to see your complete knowledge graph.

---

## Blackboard Example

After `/study discrete math`, open the blackboard in Obsidian:

> **# Discrete Math · Blackboard**
>
> **Stage 1: The Telegram** — Spring 1736, Königsberg. You are Euler. A letter arrives.
>
> ~~~mermaid
> graph TB
>     A["North Bank"] --- B["Kneiphof Island"]
>     A --- B
>     A --- C["Lomse Island"]
>     B --- C
>     B --- D["South Bank"]
>     B --- D
>     C --- D
> ~~~
>
> **Stage 5: Co-Derivation** — The odd-degree vertex rule:
>
> $$
> \deg(v) \equiv 1 \pmod{2} \quad\Rightarrow\quad \text{must be start or end point}
> $$
>
> **Theorem:** A connected graph has an Eulerian path iff its number of odd-degree vertices is 0 or 2.

---

## Core Principles

1. **Start from a dilemma, not a definition** — begin with a letter, a paradox, an engineering pain, an anomalous phenomenon
2. **Assume the learner knows nothing** — every unfamiliar term is translated before being introduced. Every new concept follows "what → purpose → template → simpler version"
3. **The learner makes the choices** — the skill is a midwife, not an answer key. Never names what the learner discovered for them
4. **Ask before producing** — demos, knowledge cards, reports — always ask "want me to?"
5. **Crystallize learning into a knowledge base** — Obsidian-linked notes + Git version control

---

## Installation

```bash
cp SKILL.md ~/.claude/skills/study/SKILL.md
cp -r references/ ~/.claude/skills/study/references/
```

## License

MIT
