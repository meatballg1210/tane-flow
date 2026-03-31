---
name: plan
description: >
  Run the Plan phase of the tane-flow workflow. Write a detailed implementation
  plan based on research findings. Requires that research has already been
  completed (a research-<slug>.md file should exist). Use after the user has
  reviewed and approved the research.
---

# Plan Phase

## Prerequisites

A `research-<slug>.md` file should already exist from the Research phase. If it doesn't, tell the user to run the Research phase first.

## File Naming

Reuse the slug from the existing `research-<slug>.md` file. If multiple research files exist, ask the user which task they want to plan for.

## Instructions

Write a detailed implementation plan in `plan-<slug>.md`. Base every decision on what you found in `research-<slug>.md`, not on generic best practices.

`plan-<slug>.md` must include:
- A detailed explanation of the approach
- Code snippets showing the actual changes
- File paths that will be modified
- Considerations and trade-offs
- How the plan builds on existing patterns found during research

When a similar feature or pattern already exists in the codebase, reference it as the implementation model. Point to specific files and functions. When the user provides an external reference — a GitHub link, open-source snippet, or documentation — treat it as an implementation model alongside codebase patterns. The agent performs dramatically better with a concrete reference than designing from scratch.

See `references/examples.md` § "Reference Implementation Trick" for examples.

**STOP.** Tell the user: "Plan is in `plan-<slug>.md` — please review and add inline notes." Do NOT proceed until the user responds.
