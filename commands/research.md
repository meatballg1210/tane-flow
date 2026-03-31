---
name: research
description: >
  Run the Research phase of the tane-flow workflow. Deeply read all files related
  to the task, search for existing patterns, and write findings to a research file.
  Use when you want to start a new task or need to build understanding of a system
  before planning changes.
---

# Research Phase

## File Naming

If no slug has been established yet, derive a short, descriptive slug from the user's task (e.g. `auth-refactor`, `add-search`, `fix-pagination`). The slug should be lowercase, hyphen-separated, and 2-4 words max. Confirm the slug with the user before writing the file. If a slug was already established in a prior phase, reuse it.

## Instructions

Every task starts with deep reading. Understand the relevant parts of the codebase thoroughly before doing anything else.

1. Read all files related to the task — implementations, callers, tests, and configuration. Not just signatures. Read deeply. Understand the intricacies.
2. Search for existing patterns related to the task — grep for related terms, check for existing utilities, find the closest existing feature. Most features in a mature codebase are variations on something that already exists.
3. Batch related file reads into parallel calls when files are independent. Do not read one file per turn.
4. Write all findings to `research-<slug>.md`.

`research-<slug>.md` must include:
- How the relevant system currently works
- Existing patterns, utilities, and abstractions that must be reused
- Reference implementations found — the closest existing feature or pattern
- Constraints, edge cases, and dependencies discovered

`research-<slug>.md` is the review surface. The user reads it to verify you actually understood the system. If the research is wrong, the plan will be wrong, and the implementation will be wrong. Garbage in, garbage out.

**STOP.** Tell the user: "Research is in `research-<slug>.md` — please review before I plan." Do NOT proceed to planning until the user explicitly responds.

## Prompting Tips

Use emphatic, depth-signaling language. Without these words, the agent will skim at signature level.

Key language: "deeply", "in great detail", "intricacies", "go through everything", "don't stop until".

See `references/examples.md` § "Research Phase Prompts" for concrete examples.
