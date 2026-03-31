# Why This Workflow Works

## Why Markdown Files, Not Built-in Plan Mode

Claude Code has a built-in plan mode. This workflow deliberately avoids it in favor of plain `research-<slug>.md` and `plan-<slug>.md` files. The reasons:

- **Full editor control.** You can open the file in your editor, add inline notes at the exact spot where something is wrong, restructure sections, and delete entire blocks. Built-in plan mode gives you a chat interface — you're limited to describing changes verbally.
- **Persistence.** Markdown files are real artifacts on disk. They survive session restarts, context compaction, and can be revisited days later. Built-in plans are ephemeral.
- **Shared mutable state.** The plan becomes a living document that both you and the agent read and write. This is the core mechanic — you annotate, the agent updates, you annotate again. A chat-based plan can't support this loop cleanly.

## Why Deep-Read Language Matters

Without explicit depth signals ("deeply", "in great detail", "intricacies", "go through everything", "don't stop until"), the agent will skim at signature level — read function names, glance at types, and move on. This produces research that looks comprehensive but misses the details that matter: edge cases in existing logic, implicit contracts between modules, caching layers, retry behavior.

The intensifying language is not stylistic. It is a deliberate prompting technique that changes how thorough the agent's reading pass is. See `examples.md` § "Research Phase Prompts" for concrete examples.

## Single Long Session

Keep research, planning, annotation, and implementation in one continuous conversation. The advantages:

- The agent builds cumulative understanding throughout the session. By the time you say "implement it all," it has spent the entire session studying the codebase and refining the plan with your input.
- The `plan-<slug>.md` file survives context auto-compaction in full fidelity — even if earlier messages are compressed, the agent can re-read the file at any point.
- Splitting across sessions loses the accumulated context and forces the agent to re-derive understanding from scratch.

## Anti-Patterns

These are the failure modes this workflow is designed to prevent:

1. **"Prompt → plan mode → fix errors → repeat" loop.** Without written research and annotation cycles, you're letting the agent guess at architecture. This produces code that works in isolation but breaks the surrounding system.
2. **Steering through chat messages instead of structured documents.** Long chat corrections get lost in context. Inline annotations in `plan-<slug>.md` are precise, persistent, and reviewable.
3. **Jumping to code before planning.** The most expensive failure mode. You end up debugging architecture decisions that should have been caught in a plan review.
4. **Verbal summaries instead of written artifacts.** If the agent summarizes its research verbally in the chat, you can't effectively review or annotate it. The whole point of `research-<slug>.md` is that it's a reviewable artifact.
5. **Using built-in plan mode instead of markdown files.** See "Why Markdown Files" above.
