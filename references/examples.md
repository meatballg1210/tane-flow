# Workflow Examples and Patterns

## Research Phase Prompts

Use emphatic, depth-signaling language. Without these words, the agent will skim at signature level.

```text
read this folder in depth, understand how it works deeply, what it does and all
its specificities. when that's done, write a detailed report of your learnings
and findings in research.md
```

```text
study the notification system in great detail, understand the intricacies of it
and write a detailed research.md document with everything there is to know about
how notifications work
```

```text
go through the task scheduling flow, understand it deeply and look for potential
bugs. there definitely are bugs in the system as it sometimes runs tasks that
should have been cancelled. keep researching the flow until you find all the bugs,
don't stop until all the bugs are found. when you're done, write a detailed report
of your findings in research.md
```

Key language: "deeply", "in great detail", "intricacies", "go through everything", "don't stop until".

## Planning Phase Prompts

```text
I want to build a new feature <name and description> that extends the system to
perform <business outcome>. write a detailed plan.md document outlining how to
implement this. include code snippets
```

```text
the list endpoint should support cursor-based pagination instead of offset. write
a detailed plan.md for how to achieve this. read source files before suggesting
changes, base the plan on the actual codebase
```

### Reference Implementation Trick

For well-contained features where a good implementation exists elsewhere in the codebase or in open source, provide it as a reference:

> "This is how they do sortable IDs. Write a plan.md explaining how we can adopt a similar approach."

> "The users table already has exactly the pagination pattern we need. Use it as the model for the orders table."

The agent performs dramatically better with a concrete reference implementation than designing from scratch.

## Annotation Cycle

### Sending the agent back to review notes

```text
I added a few notes to the document, address all the notes and update the
document accordingly. don't implement yet
```

The explicit "don't implement yet" guard is essential. Without it, the agent may jump to code the moment it thinks the plan is "good enough."

### Typical user annotations

These are inline notes users add directly into plan.md:

- "use drizzle:generate for migrations, not raw SQL"
- "no -- this should be a PATCH, not a PUT"
- "remove this section entirely, we don't need caching here"
- "the queue consumer already handles retries, so this retry logic is redundant. remove it and just let it fail"
- "this is wrong, the visibility field needs to be on the list itself, not on individual items. when a list is public, all items are public. restructure the schema section accordingly"

## Todo List Request

```text
add a detailed todo list to the plan, with all the phases and individual tasks
necessary to complete the plan - don't implement yet
```

## Implementation Command

```text
implement it all. when you're done with a task or phase, mark it as completed in
the plan document. do not stop until all tasks and phases are completed. do not
add unnecessary comments or jsdocs. continuously run typecheck to make sure you're
not introducing new issues.
```

What this encodes:
- **implement it all** -- do everything in the plan
- **mark it as completed in the plan document** -- the plan is the source of truth for progress
- **do not stop** -- don't pause for confirmation mid-flow
- **no unnecessary comments** -- keep code clean
- **continuously run typecheck** -- catch problems early

## Feedback and Correction Patterns

### Terse corrections

- "You didn't implement the deduplicateByTitle function."
- "You built the settings page in the main app when it should be in the admin app, move it."

The agent already has full context of the plan and session — terse corrections are enough.

### Reference existing code

> "This table should look exactly like the users table — same header, same pagination, same row density."

Most features in a mature codebase are variations on existing patterns. Pointing to a reference communicates many implicit requirements without spelling them all out.

### Frontend iteration

- "wider"
- "still cropped"
- "there's a 2px gap"

For visual issues, screenshots communicate faster than long descriptions.

### Revert and narrow scope

Instead of patching endlessly:

> "I reverted everything. Now all I want is to make the list view more minimal — nothing else."

Narrowing scope after a revert works better than trying to rescue a bad direction incrementally.

## Steering Patterns

Even when the agent executes, the user stays in the driver's seat. Common patterns:

- **Cherry-picking from proposals**: accept only the useful parts of a multi-item suggestion
- **Trimming scope**: cut nice-to-haves to avoid scope creep
- **Protecting existing interfaces**: keep critical function signatures stable
- **Overriding technical choices**: force preferred models/libraries/methods when you know what fits the project best
