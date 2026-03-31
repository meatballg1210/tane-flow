---
name: tane-flow
description: >
  Enforces a Research -> Plan -> Annotate -> Todo List -> Implement -> Feedback
  pipeline that separates thinking from typing. Use when the user explicitly types
  /tane-flow. For tasks that would benefit from this workflow (production-grade
  projects from scratch, multi-file changes, architectural refactors, new features,
  performance work, or any task that could conflict with existing codebase patterns),
  ASK the user first whether they want to use the tane-flow workflow before
  activating it. Do NOT activate automatically.
  DO NOT use for: quick questions, explaining code, single-line fixes, or tasks
  where the user has already provided the exact code to write.
---

# Research-Plan-Annotate-Implement Workflow

Never write code until the user has reviewed and approved a written plan. This separation of planning and execution prevents wasted effort, keeps the user in control of architecture decisions, and produces better results than jumping straight to code.

```
Research -> Plan -> Annotate (repeat 1-6x) -> Todo List -> Implement -> Feedback
```

---

## Phase 1: Research

Every task starts with deep reading. Understand the relevant parts of the codebase thoroughly before doing anything else.

1. Read all files related to the task — implementations, callers, tests, and configuration. Not just signatures. Read deeply. Understand the intricacies.
2. Search for existing patterns related to the task — grep for related terms, check for existing utilities, find the closest existing feature. Most features in a mature codebase are variations on something that already exists.
3. Batch related file reads into parallel calls when files are independent. Do not read one file per turn.
4. Write all findings to `research.md`.

`research.md` must include:
- How the relevant system currently works
- Existing patterns, utilities, and abstractions that must be reused
- Reference implementations found — the closest existing feature or pattern
- Constraints, edge cases, and dependencies discovered

`research.md` is the review surface. The user reads it to verify you actually understood the system. If the research is wrong, the plan will be wrong, and the implementation will be wrong. Garbage in, garbage out.

**STOP.** Tell the user: "Research is in `research.md` — please review before I plan." Do NOT proceed until the user explicitly responds.

---

## Phase 2: Plan

Write a detailed implementation plan in `plan.md`. Base every decision on what you found in `research.md`, not on generic best practices.

`plan.md` must include:
- A detailed explanation of the approach
- Code snippets showing the actual changes
- File paths that will be modified
- Considerations and trade-offs
- How the plan builds on existing patterns found during research

When a similar feature or pattern already exists in the codebase, reference it as the implementation model. Point to specific files and functions. When the user provides an external reference — a GitHub link, open-source snippet, or documentation — treat it as an implementation model alongside codebase patterns. The agent performs dramatically better with a concrete reference than designing from scratch.

See `references/examples.md` § "Reference Implementation Trick" for examples.

**STOP.** Tell the user: "Plan is in `plan.md` — please review and add inline notes." Do NOT proceed until the user responds.

---

## Phase 3: Annotation Cycle

This is the most important phase. The user will add inline notes directly into `plan.md`. These notes may correct assumptions, reject approaches, add constraints, or provide domain knowledge you don't have.

When the user says they've added notes:

1. Read `plan.md` carefully
2. Address every note
3. Update the plan accordingly
4. Tell the user: "Plan updated — ready for another review. I won't implement until you say so."

Repeat this cycle until the user is satisfied. Expect 1-6 rounds. Never implement during this phase.

See `references/examples.md` for typical annotation patterns.

---

## Phase 4: Todo List

Before implementation, add a granular task breakdown to `plan.md`:

```
## Todo

### Phase 1: [name]
- [ ] Task 1
- [ ] Task 2

### Phase 2: [name]
- [ ] Task 3
- [ ] Task 4
```

This is the last checkpoint before implementation. The todo list is the progress tracker — mark items complete as you go. Especially valuable in long sessions where the user needs to see exactly where things stand.

**STOP.** Tell the user: "Todo list added to `plan.md` — please approve before I implement." Do NOT proceed until the user responds.

---

## Phase 5: Implementation

Implementation should be boring. Every decision was already made during research and annotation.

Rules:
- **Implement everything in the plan** — do not skip steps or stop for confirmation mid-flow
- **Mark tasks complete** in `plan.md` as you go (`- [x]`)
- **Do not add unnecessary comments, docstrings, or type workarounds** — keep code clean
- **Run type checks and linters continuously** — catch problems early, not at the end
- **Do not stop until all tasks and phases are completed**

---

## Phase 6: Feedback

Once implementation begins, accept terse corrections. Reference existing code when correcting — most features are variations on existing patterns. For visual or frontend work, the user may attach screenshots and give brief directives ("wider", "still cropped", "there's a 2px gap") — act on these directly.

If you've applied 3+ corrections to the same area without converging:
1. Revert the problematic changes
2. Narrow the scope
3. Re-implement from the plan

Do not patch endlessly. Reverting and re-implementing from the plan is faster and produces cleaner results than incremental fixes on a bad direction.

See `references/examples.md` for feedback, correction, and steering patterns.

---

## Principles

1. **Research prevents ignorant changes.** The most expensive failure mode is implementations that work in isolation but break the surrounding system.
2. **The plan prevents wrong changes.** `plan.md` is shared mutable state — review surface, specification, progress tracker, and decision log.
3. **Annotation injects the user's judgment** about the broader system, product direction, engineering culture, and trade-offs.
4. **Reference existing code.** Most features are variations on existing patterns. Point to them instead of designing from scratch.
5. **Implementation should be boring.** The creative work happened during research and annotation.
6. **Stay in one session.** Keep research, planning, and implementation in one continuous conversation. The plan file survives context compaction in full fidelity, and the agent builds cumulative understanding throughout the session.

See `references/philosophy.md` for rationale and anti-patterns.
