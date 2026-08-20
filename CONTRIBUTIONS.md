# Open-source contributions

A concise ledger of public upstream work, separated by **merged**, **acknowledged**, **open**, and **closed-unmerged** status so nothing is overstated.

_Last verified: 2026-08-20._

## Merged upstream contributions

### Vercel AI SDK — unsafe finish-reason tool execution

- Reported: [vercel/ai#19063](https://github.com/vercel/ai/issues/19063)
- AI SDK 7 fix: [vercel/ai#19066](https://github.com/vercel/ai/pull/19066)
- AI SDK 6 backport: [vercel/ai#19120](https://github.com/vercel/ai/pull/19120)
- AI SDK 5 backport: [vercel/ai#19121](https://github.com/vercel/ai/pull/19121)
- v7 release: [`ai@7.0.70`](https://github.com/vercel/ai/releases/tag/ai%407.0.70)
- Credit: `Co-authored-by: canblmz1` on the merged v7, v6, and v5 fix commits

The report showed that valid side-effecting tool calls could still auto-execute when the enclosing model call ended with `length`, `error`, `content-filter`, or `other`. Vercel classified the report as a high-confidence bug and independently reproduced the defect on AI SDK 7, then on the v6 and v5 release lines.

The resulting fixes use an explicit safe-finish-reason allowlist: automatic tool execution is permitted only after `stop` or `tool-calls`. On streaming paths, execution is deferred until the terminal finish reason is known rather than beginning before that state arrives.

The v7 fix is publicly shipped in `ai@7.0.70`. The v5 and v6 backport PRs are merged; this ledger intentionally does not name specific published v5/v6 patch versions without independent verification.

### Tugtainer — container update lifecycle hooks

- Merged PR: [Quenary/tugtainer#217](https://github.com/Quenary/tugtainer/pull/217)

Implemented configurable container lifecycle hooks across agent, backend, persistence, executor, frontend, documentation, and tests. The design uses two opt-in safety gates (`ALLOW_HOOKS` and agent-side `ALLOW_EXEC`) and defines distinct blocking/report-only semantics for update and rollback hooks.

## Publicly acknowledged security / reliability reports

### Roo Code / Roomote — environment configuration exposure

- Release acknowledgment: [Roomote v0.39.1](https://github.com/RooCodeInc/Roomote/releases/tag/v0.39.1)

Roomote's v0.39.1 changelog publicly credits `@canblmz1` for reporting an unused environment endpoint that could expose raw environment configuration. This is a public responsible-reporting acknowledgment, not a CVE or bounty claim.

### Cline — truncated tool-call repair / execution integrity

- Report: [cline/cline#13001](https://github.com/cline/cline/issues/13001)
- Follow-up integration attempt: [cline/cline#13304](https://github.com/cline/cline/pull/13304) — **closed, not merged**

The report showed that truncated tool-call arguments could be silently repaired into valid-looking JSON before execution. Maintainers independently reproduced the residual container-level truncation case and confirmed that stream-level termination information is needed to distinguish syntactically repairable input from genuinely complete input.

PR #13304 validated one concrete integration approach using `prefix-safe-json` as a raw-stream execution-integrity side channel and included execution-level dispatch tests, but it was closed without merge. It is therefore evidence of a tested integration attempt, **not** an upstream adoption claim.

## Current upstream work

These entries are intentionally listed as **open**, not as merged contributions.

### Node.js

- [nodejs/node#64954 — `fs: fix recursive readdir with buffer encoding`](https://github.com/nodejs/node/pull/64954)
- Status: **open**
- Scope: recursive `readdir` Buffer encoding support across callback, sync, promises, and `withFileTypes` paths, with regression coverage.

### Atomic Agent

- [AtomicBot-ai/atomic-agent#144 — `fix(llm): fail closed on malformed native tool calls`](https://github.com/AtomicBot-ai/atomic-agent/pull/144)
- Status: **open**
- Scope: prevent malformed or ambiguously terminated native OpenAI-compatible tool calls from reaching execution; includes execution-level regression coverage through the real provider and step executor.

### Trendyol Baklava

- [Trendyol/baklava#1220 — `fix(pagination): clean up resize listener on disconnect`](https://github.com/Trendyol/baklava/pull/1220)
- Status: **open**
- Scope: fixes a resize-listener reference leak in `bl-pagination` and adds a regression test that verifies the exact registered handler is removed.

### Vercel AI SDK documentation

- [vercel/ai#18770 — `docs: gate jsonrepair on finishReason in the truncation cookbook`](https://github.com/vercel/ai/pull/18770)
- Status: **open**
- Scope: documents truncation as a different failure mode from malformed JSON and checks `finishReason` before attempting `jsonrepair` in the cookbook example.

## How I approach upstream work

- Reproduce against current upstream before proposing a fix.
- Prefer execution-level or behavior-level tests over event-only assertions.
- Compare failures against a clean baseline before calling them regressions.
- Keep native fixes small when a new dependency would not materially reduce complexity.
- Separate confirmed behavior from inference and avoid inflating security impact.
- Treat side-effect boundaries as integrity problems: valid-looking data is not automatically safe-to-execute data.
