# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project identity

This is the **study** skill — a Claude Code teaching skill. It lets learners relive the discovery, invention, or dialogue process behind human knowledge across five modes: discoverer (STEM), engineer (tech stacks), dialoguer (humanities debates), observer (social sciences — economics, sociology, cognitive science), deep-diver (classic works). The skill auto-routes `/study [topic]` to the correct mode, drives structured stage-by-stage sessions, and persists all learning output as an Obsidian vault with Git version control.

## Skill deployment

The skill is installed by copying into the Claude Code skills directory:

```bash
cp SKILL.md ~/.claude/skills/study/SKILL.md
cp -r references/ ~/.claude/skills/study/references/
```

The `.claude/settings.local.json` contains permissions for Git operations and file copying needed during development. Remote installation for users is via `npx skills add`.

## Theoretical foundation: 5-stage meta-learning framework

All teaching in this skill is grounded in a universal cognitive grammar — the **five meta-learning stages** that describe how any human mind moves from ignorance to mastery, regardless of discipline:

| Stage | Logic | Core Output |
|-------|-------|-------------|
| **I 动因确立 (Genesis)** | Cognitive dissonance | Problem awareness |
| **II 本体解构 (Ontology)** | Reductionism | Definitions, concepts, boundaries |
| **III 因果建模 (Mechanism)** | Structural functionalism | Mechanisms, algorithms, derivations |
| **IV 辩证审视 (Dialectic)** | Critical realism | Boundary conditions, costs, limitations |
| **V 范式融合 (Synthesis)** | Holism | Knowledge network, transfer capability |

The four teaching modes are domain-specific instantiations — each mode's stages map to these five meta-stages (see mapping tables in each mode file and SKILL.md). The meta-stage constrains what the teacher is allowed to do: Stage I only creates gaps (no definitions), Stage III requires the learner to derive (no spoon-feeding), Stage IV must present alternatives without favoring any.

Textbook chapters serve as the roadmap through the five stages: Ch1 → [I→II→III→IV→V] → Ch2 → [I→II→III→IV→V] → ...

## Architecture: SKILL.md as dispatcher, references/ as executors

**SKILL.md** is the single entry point. It handles:
- Session startup (first-time vs resume via `.study/state.json`)
- Discipline diagnosis → mode routing (two-question classifier)
- The five meta-learning stages (theoretical foundation for all modes)
- The "Five Iron Laws" that apply across all modes
- Output routing (CLI for dialogue only; all teaching content → Obsidian vault)
- Knowledge card splitting (branch choices trigger Write + Edit + index update)
- Compliance checker agent scheduling (lightweight post-output + deep per-stage/10-rounds)

**references/** contains the mode scripts and shared modules. Each mode file is a rigid stage script — the skill follows it sequentially, no skipping or merging:

| File | Role |
|------|------|
| `discoverer.md` | STEM theory: 7 stages (telegram → original-context thinking → cross-discipline toolbox → fork-in-the-road → co-derivation → ripple effect → modern perspective) |
| `engineer.md` | Tech stacks: 7 stages (pain-point telegram → design tradeoffs → minimal concept anchor → micro-lectures + code-as-feedback → code review & refactor → extension & migration → knowledge graph). Stage 3 explicitly labels code as "使用模板" (usage template) per the 4-step methodology |
| `dialoguer.md` | Humanities debates: 6 stages (eternal question → position-first → sages along timeline → user vs sage debate → thought-history positioning → return to the original question). Includes tree-drill-down for broad domains |
| `observer.md` | Social science empirical learning: 5 stages (anomalous phenomenon → variable decomposition → hypothesis & verification → theoretical competition → model application). For economics, sociology, cognitive science, political science — disciplines that explain social phenomena through causal models |
| `deep-diver.md` | Classic works: 5 stages (arrival → plain reading → tracing streams to source → dialogue & savoring → internalization & creation) |
| `translation-layer.md` | Cross-discipline concept translation: follows the 4-step concept introduction methodology (what is it → what's it for → usage template → simpler explanation). Detect unfamiliar terms → translate to known language → ask "need me to expand?" (mini-lecture follows 4-step structure) |
| `rule-supervisor.md` | Rule supervision agent — runs after EVERY user interaction. Three-tier check: (1) Five Iron Laws every time, (2) user question answering quality when user asks, (3) mode framework + knowledge cards at key nodes. Output: PASS/FAIL with fix instructions |
| `format-supervisor.md` | Format supervision agent — runs after EVERY user interaction. 7 format checks. Answers: "对不对?" |
| `rule-supervisor.md` | Rule supervision agent — runs after EVERY user interaction. Three-tier check (iron laws + user questions + framework). Answers: "守不守规矩?" |
| `content-reviewer.md` | Content quality agent — runs after EVERY user interaction. Checks enumeration completeness, laziness detection, code substance, answer completeness. Answers: "够不够?" |
| `compliance-checker.md` | Deep periodic audit — runs at stage boundaries and every 10 rounds. Full 5-item check. References the three per-interaction supervisors |
| `post-output-checker.md` | Legacy lightweight check (4 hard rules). Superseded by the three new supervisors; kept as fallback quick reference |
| `state-schema.md` | Full JSON schema for `.study/state.json` including mode-specific fields, `textbook` field for all four modes, and legacy migration |
| `obsidian-notes.md` | Vault directory structure, file naming, frontmatter templates (with bidirectional `previous`/`next`/`branch_of` links), card templates per mode |
| `demo-models.md` | Demo code generation rules (Python/HTML/MATLAB) — ask before generating, write on-demand, mark tunable params |
| `thought-history-engine.md` | 6-dimension analysis framework for dialoguer mode Stage 5 |
| `question-protocol.md` | User question interruption protocol: classify (A/B/C/D) → meta-stage gate → answer by type → record → return to main thread. Ensures user questions are addressed without derailing the teaching flow or leaking future-stage content |

## Key design patterns (not obvious from single-file reads)

### Output routing (CLI ↔ Obsidian split)

This is the most mechanically important pattern. Every response follows a strict sequence:
1. **Write** full teaching content to `[板书]-实时笔记.md` (formulas, diagrams, derivations, code reviews — everything substantive)
2. **Read** the file back to verify format correctness (mermaid/LaTeX/table integrity)
3. **CLI** outputs only 1–3 sentences of dialogue/questions
4. Never say-it-first-then-write-later; never write an abbreviated blackboard

CLI must convert LaTeX to natural language (say "p squared equals 2 times q squared", not `$p^2 = 2q^2$`).

### Knowledge point definition and card creation

A knowledge point is a node in the cognitive network — a minimal unit of meaning that the learner has formed in their own mind. It must satisfy four conditions simultaneously:

1. **Independently statable** — the user can express it in one complete sentence
2. **Callable** — usable to solve a new problem without context hints
3. **Has a connection edge** — linked to at least one other known concept (causal/comparative/hierarchical/analogical)
4. **User's own words** — Skill did not name or structure it for them

**Trigger**: Card creation triggers when BOTH signals appear: (a) user passes Feynman test (one-sentence essence), AND (b) user expresses a connection to a prior concept. Neither alone is sufficient.

**Probing**: At "探测提醒点" marked in mode files, Skill actively probes with Feynman test + connection inquiry ("How does this relate to [prior concept]?"). If both signals pass → create card. If not → skip, content stays on blackboard.

**Card content**: The user's own words form the core; Skill only formats. Card title is derived from the user's phrasing, never from Skill's naming.

**Inline markers**: Each concept in the blackboard carries a status tag: `[概念名] \`[II|费曼:?|关联:?]\``. When the user passes the Feynman test, the teaching agent Edits it to `费曼:✓`. When the user expresses a connection, `关联:✓`. When both ✓ appear simultaneously, content-reviewer detects it on the next run and fails if no card has been created — ensuring no knowledge point is forgotten. No separate tracker needed; state lives next to content.

### Multi-agent supervision (监管与执行分离)

To prevent attention mechanism failures and laziness, supervision is separated from teaching execution. After EVERY user interaction, three supervision agents run sequentially:

1. **Format Supervisor** (`format-supervisor.md`) — "Is it correct?" 7 format checks. Fix on fail.
2. **Rule Supervisor** (`rule-supervisor.md`) — "Does it follow the rules?" Five Iron Laws + user question answering + mode framework + meta-stage flow. Fix on fail.
3. **Content Reviewer** (`content-reviewer.md`) — "Is it thorough enough?" Enumeration completeness, laziness detection (省略/以此类推/TODO代替实现), code substance, answer completeness. Especially critical for engineer mode's Step 2 template (must enumerate ALL cases). Fill gaps on fail.

A deep periodic audit (`compliance-checker.md`) still runs at stage boundaries. The three supervisors output only `[PASS]` or `[FAIL: ...]` — they don't participate in teaching.

**Division of labor:**

| Agent | Checks | Core question |
|-------|--------|---------------|
| Format | Syntax, rendering, format specs | Is it correct? |
| Rule | Iron laws, framework, discipline | Is it following the rules? |
| Content | Completeness, laziness, substance | Is it thorough enough? |

### Concept introduction methodology (概念引入四步法)

All new concepts follow a mandatory 4-step gradient (defined in SKILL.md, integrated into translation-layer.md):

1. **这是什么 (What is it)** — plain language, one sentence, analogy to known concepts
2. **有什么用途 (What's it for)** — the problem it solves, why it matters
3. **使用模板 (Usage template)** — for applied/engineer topics, a minimal working example
4. **更浅显表达 (Simpler explanation)** — fallback when user says "I don't understand"; always have a simpler version ready

Steps 1-2 are mandatory for every new concept. Step 3 triggers for applied topics. Step 4 triggers only on user confusion signal.

### Universal textbook anchoring (教材锚定)

All four modes now support textbook-based learning (previously engineer-only). After mode confirmation, every mode asks: "Which textbook/tutorial do you want to follow?" The textbook's chapter list becomes the roadmap — each chapter runs the full mode cycle. On chapter completion, the blackboard is extracted to `[板书]-ChN-[章节名].md`, cleared, and all knowledge cards are generated. Choosing "no textbook" keeps the free exploration mode. See SKILL.md "按章节拆分" for the universal chapter-splitting rules.

### State machine

`.study/state.json` is the backbone for session persistence. Key fields: `mode`, `phase` (diagnosis|teaching), mode-specific stage tracking (each mode has its own object with `stage` number), `vault` paths, `split_cards` array, `current_node`, `question_counts`. Legacy format (`type: "tool"`/`"theory"`) is auto-upgraded on first load. State lives in the filesystem so sessions survive token exhaustion with zero manual steps.

### Iron Law 3 enforcement (don't think for the user)

This is the hardest rule to follow and gets explicit checking. Forbidden patterns: "你刚才做的正是…", "你无意中发现了…", "你的论证包含三个层次…", "他的意思是…", "对，而且你还注意到了…". The skill presents forks, sages, and evidence — never names what the user discovered or structures their argument for them.

### Tree drill-down for broad humanities domains

Dialoguer and deep-diver modes detect overly broad topics (e.g. "中国古代哲学") and refuse to start Stage 1. Instead they drill down through the discipline's standard academic taxonomy, presenting mainstream and non-mainstream branches at each level, until reaching a specific debatable thesis.

### Pit-digging (挖坑)

Across all modes, the skill actively identifies common misconceptions and designs scenarios where the learner naturally falls into them. The pattern: plant a subtle trap → let consequences unfold → reveal the principle after the learner recognizes something is wrong. Never warn "there's a trap here."

## File change guardrails

- `SKILL.md` — defines the overall protocol; changes affect all four modes. Contains universal textbook anchoring, chapter splitting, 4-step concept methodology, and multi-agent supervision scheduling
- `references/rule-supervisor.md` + `references/format-supervisor.md` + `references/content-reviewer.md` — supervision agents that run after every interaction. Three distinct checks: correctness, rule adherence, thoroughness
- `references/question-protocol.md` — user question interruption protocol; changes affect how all four modes handle mid-session user questions
- `references/*.md` (mode scripts) — changing a mode file changes its teaching flow; changing a shared module (translation-layer, question-protocol, compliance-checker, obsidian-notes, state-schema) affects all modes
- `skills-lock.json` — auto-generated dependency lock file for the `skills` CLI tool; do not edit manually
- `.claude/settings.local.json` — local dev permissions; not deployed to users
- `.study/` — runtime state directory, gitignored; never commit
