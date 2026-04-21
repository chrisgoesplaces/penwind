# PenWind

A Claude Cowork plugin for intermediate Spanish and Portuguese learners. Students submit a creative story, articulate their intent, and receive structured feedback formatted for teacher review.

Free. No backend. No signup.

---

## What it does

PenWind runs a student's writing through three sequential agents, each with visible reasoning at every step. The session ends with a structured teacher artifact the student can share directly.

**The core principle:** evaluation should follow intent. PenWind asks what a student meant to express before judging whether they expressed it correctly. This mirrors how a good human teacher would approach creative writing — understanding the student's thinking first, then assessing execution.

### The three agents

| Agent | Color | Responsibility |
|-------|-------|----------------|
| `penwind-intent` | 🔵 Blue | Receives the story, confirms language and level, surfaces subjunctive/indicative decision points, asks the student what they intended at each one |
| `penwind-eval` | 🟡 Yellow | Evaluates in three sequential passes: intent alignment, grammar and spelling, style and sophistication |
| `penwind-feedback` | 🟢 Green | Produces the teacher artifact, then opens an optional creative transformation session |

### The three eval passes

1. **Intent alignment** — Was the subjunctive or indicative used correctly given what the student meant? Judgment is against their stated intent, not a single correct answer.
2. **Grammar and spelling** — Full mechanical sweep: conjugations, agreement, spelling, accent marks, punctuation. Student chooses most impactful errors or the complete list.
3. **Style and sophistication** — Opportunities only, not errors. Naturalness, vocabulary precision, cohesion, and vocabulary strength. 2-3 examples per category maximum.

### The teacher artifact

A structured, self-contained summary the student can share with their teacher. Includes the original story, results from all three passes, patterns observed, overall strengths, and a priority focus for the next piece. Also logs whether the student pursued creative exploration after the eval.

### Creative transformation

After the artifact is produced, the student can ask PenWind to transform their story — make it more formal, satirical, melancholic, confident, or anything else they imagine. Every change is shown as original → transformed → explanation of why it works in Spanish or Portuguese specifically. The student learns the technique, not just the output.

---

## Who it's for

- Intermediate Spanish and Portuguese learners (A2–B2)
- Students working with a teacher who wants structured written feedback
- Learners who want more than grammar correction — creative development in the target language

---

## Requirements

- Claude Desktop app (macOS or Windows)
- Paid Claude plan (Pro, Max, Team, or Enterprise)
- Claude Cowork enabled

---

## Installation

1. Open Claude Desktop
2. Go to Settings → Plugins
3. Search for **PenWind**
4. Click Install

---

## How to use

Start with the intent agent and move through in order. Each agent tells you when it's done and how to invoke the next one.

```
@agent-penwind:intent
```

Paste your story when prompted. Answer the language, level, and subjunctive check questions. The agent will surface decision points one at a time.

```
@agent-penwind:eval
```

Confirm your context from the intent agent. The eval runs three passes in sequence with full reasoning visible at each step.

```
@agent-penwind:feedback
```

Your teacher artifact is produced immediately. Then optionally explore creative transformations.

---

## Design decisions

**Why ask intent before evaluating?**
If Claude infers intent first, students anchor to Claude's reading rather than articulating their own. The metacognitive work of saying "I wanted to express a wish here" is part of the learning — not a formality before the real feedback starts.

**Why three separate agents instead of one skill?**
Each agent owns its own context window. This keeps reasoning clean across a long session and makes the architecture legible — three agents with clear, distinct responsibilities is easier to inspect and trust than a single skill trying to hold everything.

**Why 2-3 examples in Pass 3?**
Enough to show a pattern, not enough to overwhelm. Pass 3 is a reward for writing 500 words, not a critique. Capping examples keeps it feeling like opportunity rather than judgment.

**Why no backend?**
Session state lives in the Cowork conversation. The teacher artifact is the record. This keeps the plugin simple, free, and immediately usable without any setup. A future version may add opt-in session logging at the user level via Claude Code hooks.

Keeping sessions local also means they are private by design. A student's writing, their mistakes, and their reasoning never leave their machine unless they choose to share the teacher artifact. There is no database, no account, no data collection. The only thing that exists outside the session is what the student deliberately hands to their teacher.

**Why free?**
PenWind is a proof of concept for human-in-the-loop eval design. The goal is real usage and real teacher artifacts, not revenue.

---

## Plugin structure

```
penwind/
├── .claude-plugin/
│   └── plugin.json
├── agents/
│   ├── intent.md
│   ├── eval.md
│   └── feedback.md
└── skills/
    ├── intent/SKILL.md
    ├── eval/SKILL.md
    └── feedback/SKILL.md
```

Each agent loads its corresponding skill. The skills contain the full reasoning logic. The agents provide context isolation and clear handoffs between steps.

---

## Limitations

- Session state is not persisted between Cowork sessions. If a session ends unexpectedly, the student will need to restart from the intent agent. Because sessions are local and private, there is no recovery mechanism — but there is also no data loss in the traditional sense, since nothing was stored externally to begin with.
- Plugin authors do not currently have access to session-level telemetry. Install counts are available via the marketplace. Teacher artifacts shared back voluntarily are the primary feedback signal for v1.
- PenWind targets A2–B2 learners. Advanced learners (C1–C2) may find the feedback calibration too basic.

---

## Roadmap

**v2 — Teacher/student interchange**
Separate teacher and student agents. Teachers set curriculum context via a Project. Students submit work. Teacher reviews artifacts directly in Cowork.

**v2 — Optional session logging**
User-level hooks in `~/.claude/settings.json` to log session events locally. Documentation will be provided for teachers who want cross-student records.

**v2 — Non-English instruction language**
All coaching and explanations are currently in English, with examples and corrections in the target language. A future version will support instruction in other languages for students whose first language is not English. Likely tied to teacher-set Project context.

**v2 — Additional languages**
French, Italian, German — same architecture, language-specific eval logic.

---

## Background

PenWind was built to demonstrate human-in-the-loop eval design in a domain where intent genuinely precedes correctness. The same architectural pattern — confirm intent, evaluate against that intent, surface reasoning visibly — applies to higher-stakes AI systems where evaluation without understanding intent produces misleading results.

The plugin is free and open source. If you use it with students, feedback is welcome via GitHub issues.

---

Built by [Chris Lincoln](https://chris-lincoln.com)
