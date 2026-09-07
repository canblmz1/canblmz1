# Open-source contributions

A concise ledger of public upstream work, separated by **merged**, **acknowledged**, **open**, and **closed-unmerged** status so nothing is overstated.

_Last verified: 2026-09-07._

## Merged upstream contributions

### Atomic Agent — native tool-call execution integrity

- Merged PR: [AtomicBot-ai/atomic-agent#144](https://github.com/AtomicBot-ai/atomic-agent/pull/144)
- Merge commit: [`dcf77f180b0f66b8f2d5858afa212638237ac619`](https://github.com/AtomicBot-ai/atomic-agent/commit/dcf77f180b0f66b8f2d5858afa212638237ac619)

The patch closes an execution-integrity failure mode in the native OpenAI-compatible tool path where malformed or ambiguously terminated `function.arguments` could still reach dispatch. Non-empty malformed arguments now fail instead of silently becoming `{}`, pending tool calls require a real terminal signal, Qwen-tagged calls share the same termination-safety decision as native calls, and an undelimited final SSE event is flushed and parsed at EOF.

### Vercel AI SDK — unsafe finish-reason tool execution

- Reported: [vercel/ai#19063](https://github.com/vercel/ai/issues/19063)
- AI SDK 7 fix: [vercel/ai#19066](https://github.com/vercel/ai/pull/19066)
- AI SDK 6 backport: [vercel/ai#19120](https://github.com/vercel/ai/pull/19120)
- AI SDK 5 backport: [vercel/ai#19121](https://github.com/vercel/ai/pull/19121)
- v7 release: [`ai@7.0.70`](https://github.com/vercel/ai/releases/tag/ai%407.0.70)
- Credit: `Co-authored-by: canblmz1` appears on the merged v7, v6, and v5 fix commits.

The report showed that side-effecting tool calls could auto-execute even when the enclosing model call ended with `length`, `error`, `content-filter`, or `other`. The resulting fixes use an explicit safe-finish-reason allowlist and defer streaming execution until the terminal finish reason is known.

### Tugtainer — container update lifecycle hooks

- Merged PR: [Quenary/tugtainer#217](https://github.com/Quenary/tugtainer/pull/217)

Implemented configurable container lifecycle hooks across agent, backend, persistence, executor, frontend, documentation, and tests, including opt-in safety gates and distinct blocking/report-only semantics for update and rollback hooks.

## Publicly acknowledged security / reliability reports

### Roomote — environment configuration exposure

- Release acknowledgment: [Roomote v0.39.1](https://github.com/RooCodeInc/Roomote/releases/tag/v0.39.1)

Roomote's v0.39.1 changelog explicitly thanks `@canblmz1` for reporting an unused environment endpoint that could expose raw environment configuration. This is a public responsible-reporting acknowledgment, not a CVE or bounty claim.

### Cline — truncated tool-call repair / execution integrity

- Report: [cline/cline#13001](https://github.com/cline/cline/issues/13001)
- Follow-up integration attempt: [cline/cline#13304](https://github.com/cline/cline/pull/13304) — **closed, not merged**

The report showed that truncated tool-call arguments could be silently repaired into valid-looking JSON before execution. The follow-up PR tested one concrete integration path but was closed without merge, so it is not presented as an adoption claim.

## Current upstream work

These entries are intentionally listed as **open**, not as merged contributions.

### Node.js

- [nodejs/node#64954 — `fs: fix recursive readdir with buffer encoding`](https://github.com/nodejs/node/pull/64954)
- Scope: recursive `readdir` Buffer encoding support across callback, sync, promises, and `withFileTypes` paths, with regression coverage.

### Apache Maka

- [apache/maka#3434](https://github.com/apache/maka/pull/3434)
- Scope: gate tool execution on raw stream-completion evidence.

### Trendyol Baklava

- [Trendyol/baklava#1220 — `fix(pagination): clean up resize listener on disconnect`](https://github.com/Trendyol/baklava/pull/1220)
- Scope: fixes a resize-listener reference leak in `bl-pagination` and adds regression coverage.

### Continue

- [continuedev/continue#13224](https://github.com/continuedev/continue/pull/13224)
- Scope: correlate id-less interleaved OpenAI tool-call fragments by provider index.

### Vercel AI SDK documentation

- [vercel/ai#18770 — `docs: gate jsonrepair on finishReason in the truncation cookbook`](https://github.com/vercel/ai/pull/18770)
- Scope: documents truncation as a different failure mode from malformed JSON and checks `finishReason` before attempting `jsonrepair` in the cookbook example.

## Closed / unmerged work

Closed work is kept separate from merged contributions and is not presented as adoption.

- [cline/cline#13304](https://github.com/cline/cline/pull/13304) — closed, not merged.
- [sandbaseai/sandbase-harness#73](https://github.com/sandbaseai/sandbase-harness/pull/73) — closed, not merged.

## How I approach upstream work

- Reproduce against current upstream before proposing a fix.
- Prefer execution-level or behavior-level tests over event-only assertions.
- Compare failures against a clean baseline before calling them regressions.
- Keep native fixes small when a new dependency would not materially reduce complexity.
- Separate confirmed behavior from inference and avoid inflating security impact.
- Treat side-effect boundaries as integrity problems: valid-looking data is not automatically safe-to-execute data.
