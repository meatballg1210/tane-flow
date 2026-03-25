# How I Use Claude Code

- Source: https://boristane.com/blog/how-i-use-claude-code/
- Date: Feb 10, 2026

## Table of Contents

- [Phase 1: Research](#phase-1-research)
- [Phase 2: Planning](#phase-2-planning)
- [The Annotation Cycle](#the-annotation-cycle)
- [Phase 3: Implementation](#phase-3-implementation)
- [Feedback During Implementation](#feedback-during-implementation)
- [Staying in the Driver’s Seat](#staying-in-the-drivers-seat)
- [Single Long Sessions](#single-long-sessions)
- [The Workflow in One Sentence](#the-workflow-in-one-sentence)

I’ve been using [Claude Code](https://docs.anthropic.com/en/docs/claude-code) as my primary development tool for approx 9 months, and the workflow I’ve settled into is radically different from what most people do with AI coding tools. Most developers type a prompt, sometimes use plan mode, fix the errors, repeat. The more terminally online are stitching together ralph loops, mcps, gas towns (remember those?), etc. The results in both cases are a mess that completely falls apart for anything non-trivial.

The workflow I’m going to describe has one core principle: **never let Claude write code until you’ve reviewed and approved a written plan.** This separation of planning and execution is the single most important thing I do. It prevents wasted effort, keeps me in control of architecture decisions, and produces significantly better results with minimal token usage than jumping straight to code.

## Workflow Loop

**repeat 1–6x**

Research → Plan → Annotate → Todo List → Implement → Feedback & Iterate

---

## Phase 1: Research

Every meaningful task starts with a deep-read directive. I ask Claude to thoroughly understand the relevant part of the codebase before doing anything else. And I always require the findings to be written into a persistent markdown file, never just a verbal summary in the chat.

### Example prompts

```text
read this folder in depth, understand how it works deeply, what it does and all its specificities. when that’s done, write a detailed report of your learnings and findings in research.md
```

```text
study the notification system in great details, understand the intricacies of it and write a detailed research.md document with everything there is to know about how notifications work
```

```text
go through the task scheduling flow, understand it deeply and look for potential bugs. there definitely are bugs in the system as it sometimes runs tasks that should have been cancelled. keep researching the flow until you find all the bugs, don’t stop until all the bugs are found. when you’re done, write a detailed report of your findings in research.md
```

Notice the language: “deeply”, “in great details”, “intricacies”, “go through everything”. This isn’t fluff. Without these words, Claude will skim. It’ll read a file, see what a function does at the signature level, and move on. You need to signal that surface-level reading is not acceptable.

The written artifact (`research.md`) is critical. It’s not about making Claude do homework. It’s the review surface. You can read it, verify Claude actually understood the system, and correct misunderstandings before any planning happens. If the research is wrong, the plan will be wrong, and the implementation will be wrong. Garbage in, garbage out.

This is the most expensive failure mode with AI-assisted coding, and it’s not wrong syntax or bad logic. It’s implementations that work in isolation but break the surrounding system. A function that ignores an existing caching layer. A migration that doesn’t account for the ORM’s conventions. An API endpoint that duplicates logic that already exists elsewhere. The research phase prevents all of this.

---

## Phase 2: Planning

Once the research is reviewed, ask for a detailed implementation plan in a separate markdown file.

### Example prompts

```text
I want to build a new feature <name and description> that extends the system to perform <business outcome>. write a detailed plan.md document outlining how to implement this. include code snippets
```

```text
the list endpoint should support cursor-based pagination instead of offset. write a detailed plan.md for how to achieve this. read source files before suggesting changes, base the plan on the actual codebase
```

The generated plan should include:

- A detailed explanation of the approach
- Code snippets showing the actual changes
- File paths that will be modified
- Considerations and trade-offs

Using a custom `plan.md` instead of built-in plan mode gives full control: editable in your own editor, supports inline notes, and persists as a real project artifact.

### Useful trick

For well-contained features where there is a good open-source implementation, provide a reference implementation along with the request. Example:

> “This is how they do sortable IDs, write a `plan.md` explaining how we can adopt a similar approach.”

Claude performs dramatically better when it has a concrete reference implementation instead of designing from scratch.

---

## The Annotation Cycle

This is the most distinctive part of the workflow.

### Cycle

1. Claude writes `plan.md`
2. You review it in your editor
3. You add inline notes
4. Send Claude back to the document
5. Claude updates the plan
6. Repeat until satisfied
7. Then request the todo list

After Claude writes the plan, open it in your editor and add inline notes directly into the document. These notes can:

- Correct assumptions
- Reject approaches
- Add constraints
- Provide domain knowledge Claude doesn’t have

### Real note examples

- `use drizzle:generate for migrations, not raw SQL`
- `no — this should be a PATCH, not a PUT`
- `remove this section entirely, we don’t need caching here`
- `the queue consumer already handles retries, so this retry logic is redundant. remove it and just let it fail`
- `this is wrong, the visibility field needs to be on the list itself, not on individual items. when a list is public, all items are public. restructure the schema section accordingly`

Then send Claude back with:

```text
I added a few notes to the document, address all the notes and update the document accordingly. don’t implement yet
```

This cycle repeats 1 to 6 times.

The explicit **“don’t implement yet”** guard is essential. Without it, Claude may jump to code the moment it thinks the plan is “good enough.”

### Why this works

The markdown file acts as **shared mutable state** between you and Claude. You can think at your own pace, annotate precisely where something is wrong, and re-engage without losing context.

This is fundamentally different from steering via chat messages. The plan is a structured, complete specification you can review holistically. A chat conversation is something you’d have to scroll through to reconstruct decisions.

Three rounds of “I added notes, update the plan” can transform a generic implementation plan into one that fits perfectly into the existing system.

---

## The Todo List

Before implementation starts, always request a granular task breakdown:

```text
add a detailed todo list to the plan, with all the phases and individual tasks necessary to complete the plan - don’t implement yet
```

This creates a checklist that serves as a progress tracker during implementation. Claude can mark items as completed as it goes, so you can glance at the plan at any point and see exactly where things stand.

Especially valuable in sessions that run for hours.

---

## Phase 3: Implementation

When the plan is ready, issue the implementation command. A strong reusable prompt:

```text
implement it all. when you’re done with a task or phase, mark it as completed in the plan document. do not stop until all tasks and phases are completed. do not add unnecessary comments or jsdocs, do not use any or unknown types. continuously run typecheck to make sure you’re not introducing new issues.
```

### What this encodes

- **implement it all** → do everything in the plan
- **mark it as completed in the plan document** → the plan is the source of truth for progress
- **do not stop until all tasks and phases are completed** → don’t pause for confirmation mid-flow
- **do not add unnecessary comments or jsdocs** → keep the code clean
- **do not use any or unknown types** → maintain strict typing
- **continuously run typecheck** → catch problems early, not at the end

By the time you say “implement it all,” every major decision should already be made and validated. The implementation becomes mechanical, not creative.

That is deliberate: **implementation should be boring**. The creative work happened during research and annotation.

---

## Feedback During Implementation

Once Claude is executing the plan, the role shifts from architect to supervisor. Prompts become much shorter.

### Terse correction examples

- `You didn’t implement the deduplicateByTitle function.`
- `You built the settings page in the main app when it should be in the admin app, move it.`

Claude already has the full context of the plan and the ongoing session, so terse corrections are often enough.

### Frontend iteration examples

- `wider`
- `still cropped`
- `there’s a 2px gap`

For visual issues, screenshots can communicate faster than long descriptions.

### Reference existing code

Example:

> “this table should look exactly like the users table, same header, same pagination, same row density.”

Most features in a mature codebase are variations on existing patterns. Pointing to a reference communicates many implicit requirements without spelling them all out.

### If it goes wrong

Instead of patching endlessly:

> “I reverted everything. Now all I want is to make the list view more minimal — nothing else.”

Narrowing scope after a revert often works better than trying to rescue a bad direction incrementally.

---

## Staying in the Driver’s Seat

Even when Claude executes, you should not give it total autonomy over what gets built. The majority of active steering still happens in `plan.md`.

Claude may propose solutions that are technically correct but wrong for the project:

- Over-engineered approaches
- Public API changes that ripple through the system
- Complex options when a simpler one is better

You still provide judgment about:

- The broader system
- Product direction
- Engineering culture
- Trade-offs that matter now

### Common steering patterns

- **Cherry-picking from proposals**: accept only the useful parts of a multi-item suggestion set
- **Trimming scope**: cut nice-to-haves to avoid scope creep
- **Protecting existing interfaces**: keep critical function signatures stable
- **Overriding technical choices**: force preferred models/libraries/methods when you know what fits the project best

Claude handles mechanical execution. You make the judgment calls.

---

## Single Long Sessions

This workflow is run in a **single long session** rather than splitting research, planning, and implementation across multiple sessions.

A single session might include:

- Deep-reading a folder
- Three rounds of plan annotation
- Full implementation
- Ongoing corrections

The argument here is that long sessions can work well because:

- By the time implementation starts, Claude has already built deep context
- Auto-compaction can preserve enough context to continue
- The `plan.md` document survives as a persistent artifact in full fidelity

At any point, you can point Claude back to the plan.

---

## The Workflow in One Sentence

**Read deeply, write a plan, annotate the plan until it’s right, then let Claude execute the whole thing without stopping, checking types along the way.**

No magic prompts. No elaborate system instructions. No clever hacks.

Just a disciplined pipeline that separates **thinking** from **typing**:

- Research prevents ignorant changes
- The plan prevents wrong changes
- Annotation injects your judgment
- The implementation command enables uninterrupted execution once decisions are locked

If you adopt this workflow, the central artifact becomes the annotated `plan.md` sitting between you and the code.

---
