# Open-source contributions

A concise ledger of public upstream work, separated by **merged**, **acknowledged**, and **currently open** status so nothing is overstated.

_Last verified: 2026-08-20._

## Merged upstream contributions

### Vercel AI SDK — unsafe finish-reason tool execution

- Reported: [vercel/ai#19063](https://github.com/vercel/ai/issues/19063)
- Upstream fix: [vercel/ai#19066](https://github.com/vercel/ai/pull/19066)
- Merged commit: [`a828527`](https://github.com/vercel/ai/commit/a8285273ef8b4b2c36cf3cb692706da7f40077d4)
- Credit: `Co-authored-by: canblmz1`

The report showed that valid side-effecting tool calls could still auto-execute when the model call ended with `length`, `error`, `content-filter`, or `other`. Vercel reproduced the behavior on current AI SDK branches and merged a core execution gate that only permits automatic tool execution for safe terminal reasons.

### Tugtainer — container update lifecycle hooks

- Merged PR: [Quenary/tugtainer#217](https://github.com/Quenary/tugtainer/pull/217)

Implemented configurable container lifecycle hooks across agent, backend, persistence, executor, frontend, documentation, and tests. The design uses two opt-in safety gates (`ALLOW_HOOKS` and agent-side `ALLOW_EXEC`) and defines distinct blocking/report-only semantics for update and rollback hooks.

## Publicly acknowledged security / reliability reports

### Roo Code / Roomote — environment configuration exposure

- Release acknowledgment: [Roomote v0.39.1](https://github.com/RooCodeInc/Roomote/releases/tag/v0.39.1)

Roomote's v0.39.1 changelog publicly credits `@canblmz1` for reporting an unused environment endpoint that could expose raw environment configuration. This is a public responsible-reporting acknowledgment, not a CVE or bounty claim.

### Cline — truncated tool-call repair / execution integrity

- Report: [cline/cline#13001](https://github.com/cline/cline/issues/13001)

Reported that truncated tool-call arguments could be silently repaired into valid-looking JSON before execution. Maintainers independently reproduced the residual container-level truncation case and confirmed that a stream-level truncation signal is required to distinguish syntactically repairable input from genuinely complete input. The issue is now closed as completed.

## Current upstream work

These are intentionally listed as **open**, not as merged contributions.

### Node.js

- [nodejs/node#64954 — `fs: fix recursive readdir with buffer encoding`](https://github.com/nodejs/node/pull/64954)
- Status: open
- Scope: recursive `readdir` Buffer encoding support across callback, sync, promises, and `withFileTypes` paths, with regression coverage.

### Atomic Agent

- [AtomicBot-ai/atomic-agent#144 — `fix(llm): fail closed on malformed native tool calls`](https://github.com/AtomicBot-ai/atomic-agent/pull/144)
- Status: open
- Scope: prevent malformed or ambiguously terminated native OpenAI-compatible tool calls from reaching execution; includes execution-level regression coverage.

### Trendyol Baklava

- [Trendyol/baklava#1220 — `fix(pagination): clean up resize listener on disconnect`](https://github.com/Trendyol/baklava/pull/1220)
- Status: open
- Scope: fixes a resize-listener reference leak in `bl-pagination` and adds a regression test that verifies the exact registered handler is removed.

## How I approach upstream work

- Reproduce against current upstream before proposing a fix.
- Prefer execution-level or behavior-level tests over event-only assertions.
- Compare failures against a clean baseline before calling them regressions.
- Keep native fixes small when a new dependency would not materially reduce complexity.
- Separate confirmed behavior from inference and avoid inflating security impact.
- Treat side-effect boundaries as integrity problems: valid-looking data is not automatically safe-to-execute data.
