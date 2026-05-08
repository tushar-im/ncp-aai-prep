# NCP-AAI Exam Prep

NVIDIA-Certified Professional: Agentic AI
Exam date: July 5, 2026 | 60 days

---

## Daily Flow

1. **8 PM** — Routine runs automatically, creates `sessions/YYYY-MM-DD/session-study.md`
2. **You sit down** — open Claude Code in this directory
3. **Type `/start-study`** — Claude loads today's session, pulls study-brain context, begins coaching
4. **Answer questions** — fill `sessions/today/answers.md`
5. **Say "done"** — Claude writes `session-notes.md`, updates `progress.md` and `wrong-answers.md`

---

## Structure

```
ncp-aai-prep/
├── CLAUDE.md                        ← persistent context + coaching rules
├── study-plan.md                    ← 60-day week-by-week schedule
├── progress.md                      ← updated after every session
├── wrong-answers.md                 ← auto-updated during sessions
│
├── study-brain/                     ← reference material (markdown only)
│   ├── README.md                    ← how to add files
│   └── nvidia-study-guide.md        ← add after converting PDF
│
├── sessions/                        ← one folder per day
│   ├── YYYY-MM-DD/
│   │   ├── session-study.md         ← pre-built by Routine
│   │   ├── answers.md               ← you fill during session
│   │   └── session-notes.md         ← written by Claude after session
│   ├── weekly-review-YYYY-MM-DD.md  ← written by weekly Routine
│   └── example/                     ← example session to show format
│
├── routines/                        ← Routine prompts (paste into claude.ai/code/routines)
│   ├── daily-session-prep.md        ← weekday evenings at 8 PM
│   └── weekly-review.md             ← Sunday mornings at 9 AM
│
├── notes/                           ← topic notes built during sessions
│   ├── evaluation-tuning.md
│   ├── nvidia-platform.md
│   └── cognition-memory.md
│
└── .claude/
    └── commands/
        ├── start-study.md           ← main daily command
        ├── coach.md                 ← teach a topic
        ├── quiz.md                  ← questions only
        ├── mock.md                  ← timed mock exam
        └── progress.md              ← progress check
```

---

## Commands

```bash
/start-study              # begin today's session (use this daily)
/coach [topic]            # teach a specific concept
/quiz [topic]             # questions only, no explanation first
/mock [section or full]   # timed mock exam
/progress                 # where am I, what's next
/wrong-answers            # review mistake log
```

---

## Setup Checklist

- [ ] Push this repo to GitHub
- [ ] Install context-mode: `/plugin marketplace add mksglu/context-mode`
- [ ] Convert study guide PDF to markdown, add to study-brain/
- [ ] Set up daily Routine at claude.ai/code/routines (see routines/daily-session-prep.md)
- [ ] Set up weekly Routine (see routines/weekly-review.md)
- [ ] Run /start-study on your first session

---

## Priority Study Order
1. Evaluation and Tuning (13%) — biggest gap
2. NVIDIA Platform (7%) — NIM, NeMo, Triton
3. Cognition/Memory (10%) — ReAct, memory types, planning
4. Architecture gaps — knowledge graphs, ReAct mechanics
5. Everything else — reinforce, don't re-learn
