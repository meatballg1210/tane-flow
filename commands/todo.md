---
name: todo
description: >
  Run the Todo List phase of the tane-flow workflow. Add a granular task
  breakdown to plan-<slug>.md before implementation begins. Use after the
  annotation cycle is complete and the user has approved the plan.
---

# Todo List Phase

## Prerequisites

A `plan-<slug>.md` file should already exist and the user should have approved it after the annotation cycle.

## File Naming

Reuse the slug from the existing `plan-<slug>.md` file. If multiple plan files exist, ask the user which one needs a todo list.

## Instructions

Before implementation, add a granular task breakdown to `plan-<slug>.md`:

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

**STOP.** Tell the user: "Todo list added to `plan-<slug>.md` — please approve before I implement." Do NOT proceed until the user responds.
