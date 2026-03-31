---
name: feedback
description: >
  Run the Feedback phase of the tane-flow workflow. Accept terse corrections
  and iterate on the implementation. Use after implementation is underway and
  the user wants to provide corrections or adjustments.
---

# Feedback Phase

## Prerequisites

Implementation should already be underway or complete. A `plan-<slug>.md` file should exist as the reference for the intended behavior.

## File Naming

Reuse the slug from the existing `plan-<slug>.md` file. If multiple plan files exist, ask the user which task they are giving feedback on.

## Instructions

Accept terse corrections. Reference existing code when correcting — most features are variations on existing patterns. For visual or frontend work, the user may attach screenshots and give brief directives ("wider", "still cropped", "there's a 2px gap") — act on these directly.

If you've applied 3+ corrections to the same area without converging:
1. Revert the problematic changes
2. Narrow the scope
3. Re-implement from the plan

Do not patch endlessly. Reverting and re-implementing from the plan is faster and produces cleaner results than incremental fixes on a bad direction.

See `references/examples.md` for feedback, correction, and steering patterns.
