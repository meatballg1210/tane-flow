---
name: annotate
description: >
  Run the Annotation Cycle phase of the tane-flow workflow. Read the user's
  inline notes in plan-<slug>.md, address every note, and update the plan
  accordingly. Use when the user says they have added notes to the plan file.
---

# Annotation Cycle Phase

## Prerequisites

A `plan-<slug>.md` file should already exist from the Plan phase with the user's inline notes added.

## File Naming

Reuse the slug from the existing `plan-<slug>.md` file. If multiple plan files exist, ask the user which one they annotated.

## Instructions

This is the most important phase. The user has added inline notes directly into `plan-<slug>.md`. These notes may correct assumptions, reject approaches, add constraints, or provide domain knowledge you don't have.

1. Read `plan-<slug>.md` carefully
2. Address every note — do not skip or gloss over any
3. Update the plan accordingly
4. Tell the user: "Plan updated — ready for another review. I won't implement until you say so."

Repeat this cycle until the user is satisfied. Expect 1-6 rounds. Never implement during this phase.

See `references/examples.md` for typical annotation patterns.
