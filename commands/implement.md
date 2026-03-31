---
name: implement
description: >
  Run the Implementation phase of the tane-flow workflow. Execute all tasks
  from the approved plan, marking them complete as you go. Use after the user
  has approved the todo list in plan-<slug>.md.
---

# Implementation Phase

## Prerequisites

A `plan-<slug>.md` file should already exist with an approved todo list. If no todo list exists, tell the user to run the Todo List phase first.

## File Naming

Reuse the slug from the existing `plan-<slug>.md` file. If multiple plan files exist, ask the user which one to implement.

## Instructions

Implementation should be boring. Every decision was already made during research and annotation.

Rules:
- **Implement everything in the plan** — do not skip steps or stop for confirmation mid-flow
- **Mark tasks complete** in `plan-<slug>.md` as you go (`- [x]`)
- **Do not add unnecessary comments, docstrings, or type workarounds** — keep code clean
- **Run type checks and linters continuously** — catch problems early, not at the end
- **Do not stop until all tasks and phases are completed**
